# Containers: Docker Deprecation and Apptainer

*Last updated: 2026-09-02*

[English](apptainer.md) | [中文](apptainer.zh-CN.md)

> [!IMPORTANT]
> **Docker access for regular users will be removed by default at the end of
> September 2026 (from 2026-09-30).** Migrate Docker-based workloads to
> [Apptainer](https://apptainer.org/docs/user/main/) before that date.

Apptainer runs most Docker/OCI images unchanged and is designed for shared GPU
and HPC machines: containers run as **your own user**, with no daemon and no
group membership that grants root-equivalent access. That is the reason for the
change — `docker` group membership is effectively root on the host, and a
Docker-based workload is also invisible to per-user memory and GPU accounting,
which is how single users have taken whole servers down.

## What changes

| | Before | After 2026-09-30 |
|---|---|---|
| Regular users | `docker` (rootless or via `docker` group) | `apptainer` |
| Root Docker daemon | Available on some hosts | Removed for regular users |
| Existing images | Local Docker image store | Pull or convert to `.sif` (see below) |
| Exceptions | — | Granted case by case, on request in advance |

Apptainer is already installed on the shared compute nodes. Check it:

```bash
apptainer --version
```

If the command is missing on a machine you are authorized to use, contact the
administrator.

## Migration cheat sheet

| Docker | Apptainer |
|---|---|
| `docker pull ubuntu:24.04` | `apptainer pull ubuntu.sif docker://ubuntu:24.04` |
| `docker run -it IMG bash` | `apptainer shell IMG.sif` |
| `docker run IMG cmd` | `apptainer exec IMG.sif cmd` |
| `docker run --gpus all …` | `apptainer exec --nv IMG.sif …` |
| `-v /host:/ctr` | `--bind /host:/ctr` |
| `--env K=V` | `--env K=V` (or `APPTAINERENV_K=V` in the shell) |
| `-w /work` | `--pwd /work` |
| `docker build -t img .` | `apptainer build img.sif img.def` |
| `docker run -d` / `compose` | `apptainer instance start` (see *Long-running services*) |

### Pull and run an image

```bash
# Convert any Docker/OCI image into a single-file .sif
apptainer pull pytorch.sif docker://pytorch/pytorch:2.5.1-cuda12.4-cudnn9-runtime

# GPU job: --nv exposes the NVIDIA driver and devices
apptainer exec --nv pytorch.sif python train.py

# Interactive shell
apptainer shell --nv pytorch.sif
```

Private registries:

```bash
apptainer remote login --username YOUR_USER docker://registry.example.com
apptainer pull img.sif docker://registry.example.com/team/img:tag
```

### Filesystem behaviour (the biggest difference from Docker)

Apptainer mounts your `$HOME`, `/tmp`, and the current directory into the
container by default, and runs as your UID. So:

- You usually **do not need `-v`** for your own data — it is already there.
- The container filesystem is **read-only**. Do not `pip install` into the image
  at runtime; install into a virtualenv/conda env in your home directory, or
  bake the packages into the image with a definition file.
- Anything the container writes to your home counts against your home quota.

Use `--bind` for paths outside the defaults, and `--contain`/`--no-home` when you
deliberately want isolation:

```bash
apptainer exec --nv --bind /shared/ssd/mydata:/data pytorch.sif python train.py
```

### Cache and image storage — mind your quota

`apptainer pull` and `build` cache layers under `~/.apptainer/cache`, which is
charged to your **home-directory quota** and can reach tens of GiB. Point the
cache and temporary build space at shared storage:

```bash
# add to ~/.bashrc
export APPTAINER_CACHEDIR=/shared/ssd/$USER/apptainer/cache
export APPTAINER_TMPDIR=/shared/ssd/$USER/apptainer/tmp
```

Clear it when you are done: `apptainer cache clean`.

### Writable state

`.sif` images are immutable. For workloads that must write inside the container
image, use an overlay rather than converting to a sandbox:

```bash
apptainer overlay create --size 4096 overlay.img       # 4 GiB, MiB units
apptainer exec --overlay overlay.img img.sif touch /opt/marker
```

### Building your own image

Write a definition file and build it:

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

Unprivileged builds work for most definition files. If a build needs privileges
that are unavailable on the host, build the image on a machine you control (or
in CI), then copy the resulting `.sif` over — a `.sif` is a single file and needs
nothing installed to run. Contact the administrator if neither route works.

### Long-running services

There is no `docker compose` equivalent. Run background services as named
instances:

```bash
apptainer instance start --nv img.sif myservice
apptainer instance list
apptainer exec instance://myservice curl localhost:8080
apptainer instance stop myservice
```

Instances are **your processes** — they die with your session unless started
under `systemd --user`, and they remain subject to the port rules in the
[Network Policy](network.md) and to your CPU, memory, and GPU quotas.

## What does not carry over

Plan around these before the deadline:

- **Root inside the container.** You are always your own UID; `USER root` in an
  image does not make you root on the host. Anything needing genuine root
  (kernel modules, `mount`, `iptables`, privileged devices) will not work.
- **Docker networking.** No bridge networks, no `--publish` port mapping, no
  service discovery by container name. Containers share the host network, so
  bind to a port you are allowed to use and address services as `localhost:PORT`.
- **Docker volumes and `docker cp`.** Use plain host directories and normal file
  copies.
- **`docker compose`, Swarm, and other multi-container orchestration.**
- **Images built with a Dockerfile at runtime.** Convert the Dockerfile to a
  definition file, or push the built image to a registry and pull it as `.sif`.

## Requesting an exception

Docker access may still be granted where it is technically necessary and cannot
reasonably be reproduced with Apptainer. Contact the administrators **in
advance** — before 2026-09-30 — with:

1. The server and your username.
2. The Docker-specific feature you depend on, and why Apptainer cannot provide it.
3. The expected duration of the exception.

Exceptions are granted at the administrator's discretion, may be time-limited,
and may be revoked at any time for security or operational reasons. Docker
privileges also remain subject to the escalation schedule in the
[Terms of Use](../README.md#docker-induced-violations).

## See also

- [Getting Started](getting-started.md)
- [Network Policy](network.md)
- [Rootless Docker](docker.md) — deprecated, kept for reference until the cutover
- [Apptainer user guide](https://apptainer.org/docs/user/main/)
