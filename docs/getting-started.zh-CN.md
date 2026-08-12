# Xtra Computing 服务器新人使用指南

*最后核对：2026-08-12*

[English](getting-started.md) | [中文](getting-started.zh-CN.md)

本文帮助新同学从申请账号走到成功建立 SSH 会话，仅介绍普通用户接入。

> [!IMPORTANT]
> 获得 Xtra 账号不代表自动获得所有机器的权限。请仅使用管理员明确分配或授权的服务器。

## 从申请到可用的完整步骤

1. 阅读并接受[服务器使用条款](../README.zh-CN.md)，特别是 GPU、存储、备份、
   安全和账号到期规则。
2. 填写 [Xtra 服务器账号申请表](https://forms.gle/Wf8qbNeuSPS2ia8u6)。
3. 等待管理员确认。管理员会向您发送一封确认邮件包含 hostname、用户名、密码。
4. 使用 SSH 登录，并完成本文的首次登录检查。
5. 为重要数据保留独立备份，为长任务保存 checkpoint。服务器不保证数据持久性，
   也不保证计算任务不中断。

## 服务器地址

请优先使用清单发布的 DNS 域名，不要保存数字 IP，因为
`*.ddns.comp.nus.edu.sg` 背后的地址可能变化。

### SoC 私网服务器

| 服务器 | SSH 域名 | 资源或用途 | 说明 |
|---|---|---|---|
| `xtra3090` | `xtra3090.ddns.comp.nus.edu.sg` | 8 × RTX 3090 |  |
| `xtrah100` | `xtrah100.ddns.comp.nus.edu.sg` | 4 × H100 | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtrah200` | `xtrah200.ddns.comp.nus.edu.sg` | 4 × H200 | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtraa100` | `xtraa100.ddns.comp.nus.edu.sg` | 8 × HGX A100 80 GB | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtraa6k01` | `xtraa6k01.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtraa6k02` | `xtraa6k02.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtraa6k03` | `xtraa6k03.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtraa6k04` | `xtraa6k04.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | 需要 SoC VPN 或 NUS Wi-Fi。 |
| `xtra4x3090` | `xtra4x3090.ddns.comp.nus.edu.sg` | 清单未记录资源 | 使用前向管理员确认是否适合。 |
| `xtra-v80-0` | `xtra-v80-0.ddns.comp.nus.edu.sg` | 清单未记录资源 | 使用前向管理员确认是否适合。 |
| `xtra-v80-1` | `xtra-v80-1.ddns.comp.nus.edu.sg` | 清单未记录资源 | 使用前向管理员确认是否适合。 |
| `xacchead` | `xacchead.ddns.comp.nus.edu.sg` | HACC 入口；11 个 FPGA/AMD GPU 节点 | 请填写 [FPGA 服务器账号申请表](https://forms.gle/fvfPgJypd1sSWzHm8) |


## 负责任地使用组内资源

同时确认以下基本规则：

- 每位用户默认可同时使用最多 2 块 GPU，无需额外申请。空闲 GPU 可机会性使用，但在其他用户需要时应主动释放。
- 博士生的 home 配额通常为 512 GiB，其他用户通常为 256 GiB。
- 如果服务器上存在 `/shared/hdd` 和 `/shared/ssd`，它们是共享存储。RAID 不是备份，重要数据必须另存一份独立副本。
- 不得进行未经授权的扫描、绕过监控或配额、共享账号，或把凭据写入代码和 shell 历史。
