# 容器：Docker 停用与 Apptainer 迁移

*最后更新：2026-09-02*

<sub>Approved by Junyi Hou and Hongshi Tan.</sub>

[English](apptainer.md) | [中文](apptainer.zh-CN.md)

> [!IMPORTANT]
> **自 2026 年 9 月底（2026-09-30）起，普通用户默认不再拥有 Docker 权限。**
> 请在该日期之前将基于 Docker 的工作负载迁移到
> [Apptainer](https://apptainer.org/docs/user/main/)。

Apptainer 可以直接运行绝大多数 Docker/OCI 镜像，且专为共享 GPU 与 HPC 环境设计：
容器以**你自己的用户身份**运行，没有守护进程，也不需要加入任何等同于 root 的用户组。
这正是本次调整的原因——`docker` 用户组权限实际上等同于主机 root，而且 Docker 中的
负载无法被按用户的内存与 GPU 统计看到，过去已多次出现单个用户把整台服务器拖垮的情况。

## 变更内容

| | 变更前 | 2026-09-30 之后 |
|---|---|---|
| 普通用户 | `docker`（rootless 或通过 `docker` 组） | `apptainer` |
| root Docker 守护进程 | 部分主机可用 | 普通用户不再可用 |
| 已有镜像 | 存放在本地 Docker 镜像库 | 拉取或转换为 `.sif`（见下） |
| 例外 | — | 需提前申请，逐例审批 |

共享计算节点上已经安装了 Apptainer，可以先确认：

```bash
apptainer --version
```

如果在你有权使用的机器上找不到该命令，请联系管理员。

## 迁移对照表

| Docker | Apptainer |
|---|---|
| `docker pull ubuntu:24.04` | `apptainer pull ubuntu.sif docker://ubuntu:24.04` |
| `docker run -it IMG bash` | `apptainer shell IMG.sif` |
| `docker run IMG cmd` | `apptainer exec IMG.sif cmd` |
| `docker run --gpus all …` | `apptainer exec --nv IMG.sif …` |
| `-v /host:/ctr` | `--bind /host:/ctr` |
| `--env K=V` | `--env K=V`（或在 shell 中设置 `APPTAINERENV_K=V`） |
| `-w /work` | `--pwd /work` |
| `docker build -t img .` | `apptainer build img.sif img.def` |
| `docker run -d` / `compose` | `apptainer instance start`（见*长期运行的服务*） |

### 拉取并运行镜像

```bash
# 把任意 Docker/OCI 镜像转换成单文件 .sif
apptainer pull pytorch.sif docker://pytorch/pytorch:2.5.1-cuda12.4-cudnn9-runtime

# GPU 任务：--nv 会挂载 NVIDIA 驱动与设备
apptainer exec --nv pytorch.sif python train.py

# 交互式 shell
apptainer shell --nv pytorch.sif
```

私有 registry：

```bash
apptainer remote login --username YOUR_USER docker://registry.example.com
apptainer pull img.sif docker://registry.example.com/team/img:tag
```

### 文件系统行为（与 Docker 差别最大的地方）

Apptainer 默认把你的 `$HOME`、`/tmp` 和当前目录挂载进容器，并以你的 UID 运行，因此：

- 访问自己的数据通常**不需要 `-v`**，它们本来就在容器里。
- 容器文件系统是**只读**的。不要在运行时往镜像里 `pip install`；应把包安装到 home
  目录下的 virtualenv/conda 环境，或用定义文件在构建镜像时装好。
- 容器写入 home 的任何内容都会计入你的 home 目录配额。

默认挂载之外的路径用 `--bind`；需要刻意隔离时用 `--contain` / `--no-home`：

```bash
apptainer exec --nv --bind /shared/ssd/mydata:/data pytorch.sif python train.py
```

### 缓存与镜像存放位置——注意配额

`apptainer pull` 和 `build` 会把层缓存在 `~/.apptainer/cache`，这部分占用**home 目录
配额**，很容易达到几十 GiB。建议把缓存和构建临时目录指向共享存储：

```bash
# 加到 ~/.bashrc
export APPTAINER_CACHEDIR=/shared/ssd/$USER/apptainer/cache
export APPTAINER_TMPDIR=/shared/ssd/$USER/apptainer/tmp
```

用完及时清理：`apptainer cache clean`。

### 可写状态

`.sif` 镜像不可修改。如果负载必须在镜像内写入，请使用 overlay，而不是把镜像展开成
sandbox 目录：

```bash
apptainer overlay create --size 4096 overlay.img       # 4 GiB，单位是 MiB
apptainer exec --overlay overlay.img img.sif touch /opt/marker
```

### 自己构建镜像

编写定义文件并构建：

```apptainer
Bootstrap: docker
From: pytorch/pytorch:2.5.1-cuda12.4-cudnn9-runtime

%post
    pip install --no-cache-dir transformers datasets

%environment
    export HF_HOME=/data/hf

%runscript
    exec python "$@"
```

```bash
apptainer build myjob.sif myjob.def
```

大多数定义文件都可以在无特权的情况下构建。如果某个构建需要主机上不具备的权限，可以
在自己可控的机器（或 CI）上构建，再把生成的 `.sif` 拷过来——`.sif` 是单个文件，运行时
不需要额外安装任何东西。两种方式都行不通时请联系管理员。

### 长期运行的服务

Apptainer 没有 `docker compose` 的等价物。后台服务请以命名实例方式运行：

```bash
apptainer instance start --nv img.sif myservice
apptainer instance list
apptainer exec instance://myservice curl localhost:8080
apptainer instance stop myservice
```

实例就是**你自己的进程**：除非通过 `systemd --user` 启动，否则会随会话退出而结束；
同时仍然受 [网络策略](network.zh-CN.md) 中的端口规则以及 CPU、内存、GPU 配额约束。

## 无法平移的功能

请在截止日期前提前规划：

- **容器内的 root 权限。** 你始终是自己的 UID；镜像里的 `USER root` 不会让你成为主机
  root。任何真正需要 root 的操作（加载内核模块、`mount`、`iptables`、特权设备）都不可用。
- **Docker 网络。** 没有 bridge 网络、没有 `--publish` 端口映射、不能按容器名做服务发现。
  容器共享主机网络，请绑定到允许使用的端口，并以 `localhost:PORT` 访问服务。
- **Docker volume 与 `docker cp`。** 改用主机目录和普通的文件拷贝。
- **`docker compose`、Swarm 等多容器编排。**
- **运行时用 Dockerfile 构建镜像。** 请把 Dockerfile 改写成定义文件，或把构建好的镜像
  推到 registry 后再拉成 `.sif`。

## 申请例外

对于确有技术必要、且无法用 Apptainer 合理替代的场景，仍可申请保留 Docker 权限。请在
**2026-09-30 之前**提前联系管理员，并说明：

1. 服务器名称与你的用户名。
2. 依赖的 Docker 专有功能，以及为什么 Apptainer 无法满足。
3. 预计需要的时长。

例外由管理员酌情批准，可能设有期限，并可因安全或运维原因随时撤销。Docker 权限同样
适用[使用条款](../README.zh-CN.md#docker-引发的违规)中的累进处理机制。

## 相关文档

- [快速开始](getting-started.zh-CN.md)
- [网络策略](network.zh-CN.md)
- [Rootless Docker](docker.md) —— 已停用，在切换完成前保留供参考
- [Apptainer 用户手册](https://apptainer.org/docs/user/main/)
