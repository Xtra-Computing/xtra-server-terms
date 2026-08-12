# Getting Started with Xtra Computing Servers

*Last reviewed: 2026-08-12*

[English](getting-started.md) | [中文](getting-started.zh-CN.md)

This guide takes new users from applying for an account to establishing a
working SSH session. It covers regular user access only.

> [!IMPORTANT]
> An Xtra account does not automatically grant access to every machine. Use only
> servers explicitly assigned or authorized by the administrator.

## End-to-end onboarding

1. Read and accept the [Terms of Use](../README.md), especially the rules on
   GPUs, storage, backups, security, and account expiration.
2. Submit the [Xtra server account application](https://forms.gle/Wf8qbNeuSPS2ia8u6).
3. Wait for confirmation. The administrator will send you an email containing
   the hostname, username, and password.
4. Log in using SSH and complete the first-login checks in this guide.
5. Keep independent backups of important data and checkpoint long-running jobs.
   Server storage and uninterrupted computation are not guaranteed.

## Server addresses

Prefer the published DNS hostnames over numeric IP addresses because the
addresses behind `*.ddns.comp.nus.edu.sg` may change.

### SoC private servers

| Server | SSH hostname | Resource or role | Notes |
|---|---|---|---|
| `xtra3090` | `xtra3090.ddns.comp.nus.edu.sg` | 8 × RTX 3090 |  |
| `xtrah100` | `xtrah100.ddns.comp.nus.edu.sg` | 4 × H100 | Requires SoC VPN or NUS Wi-Fi. |
| `xtrah200` | `xtrah200.ddns.comp.nus.edu.sg` | 4 × H200 | Requires SoC VPN or NUS Wi-Fi. |
| `xtraa100` | `xtraa100.ddns.comp.nus.edu.sg` | 8 × HGX A100 80 GB | Requires SoC VPN or NUS Wi-Fi. |
| `xtraa6k01` | `xtraa6k01.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | Requires SoC VPN or NUS Wi-Fi. |
| `xtraa6k02` | `xtraa6k02.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | Requires SoC VPN or NUS Wi-Fi. |
| `xtraa6k03` | `xtraa6k03.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | Requires SoC VPN or NUS Wi-Fi. |
| `xtraa6k04` | `xtraa6k04.ddns.comp.nus.edu.sg` | 2 × A6000 48 GB | Requires SoC VPN or NUS Wi-Fi. |
| `xtra4x3090` | `xtra4x3090.ddns.comp.nus.edu.sg` | Resource not recorded in the inventory | Confirm suitability with the administrator before use. |
| `xtra-v80-0` | `xtra-v80-0.ddns.comp.nus.edu.sg` | Resource not recorded in the inventory | Confirm suitability with the administrator before use. |
| `xtra-v80-1` | `xtra-v80-1.ddns.comp.nus.edu.sg` | Resource not recorded in the inventory | Confirm suitability with the administrator before use. |
| `xacchead` | `xacchead.ddns.comp.nus.edu.sg` | HACC entry; 11 FPGA/AMD GPU nodes | Submit the [FPGA server account application](https://forms.gle/fvfPgJypd1sSWzHm8). |

## Responsible use of group resources

Please also observe these basic rules:

- Each user may use up to two GPUs concurrently by default without additional
  approval. Idle GPUs may be used opportunistically, but should be released
  proactively when other users need them.
- Home-directory quotas are normally 512 GiB for PhD students and 256 GiB for
  other users.
- `/shared/hdd` and `/shared/ssd`, where present, are shared storage. RAID is not
  a backup; keep an independent copy of important data.
- Do not run unauthorized scans, bypass monitoring or quotas, share accounts,
  or store credentials in code or shell history.
