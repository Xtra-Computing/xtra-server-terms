# Network Policy

This page describes the network and firewall configuration on the Xtra Computing Server.

> [!WARNING]
> Port availability is subject to NUS School of Computing firewall policies, which may change without prior notice. Always refer to the official NUS firewall documentation for the latest rules: https://dochub.comp.nus.edu.sg/cf/tech/network/firewall

## Open Ports

### System-Reserved Ports

These ports are used by server infrastructure and are **not available** for user services.

| Port | Protocol | Purpose |
|------|----------|---------|
| 22 | TCP | SSH access (rate limited) |
| 111 | TCP/UDP | autofs |
| 2049 | TCP/UDP | NFS |
| 2379 | TCP | etcd client (usage reporting) |
| 2380 | TCP | etcd peer (usage reporting) |
| 4000 | TCP | Cgroup Exporter (RAM monitoring) |
| 4001 | TCP | Node Exporter (CPU and general monitoring) |
| 4003 | TCP | NVIDIA GPU Exporter (GPU monitoring) |

### User-Available Ports

Users may run services on ports within the following ranges:

| Port Range | Protocol | Notes |
|------------|----------|-------|
| 4000-4100 | TCP/UDP | SoC allowed range. Avoid system-reserved ports listed above. |
| 10000-30000 | TCP/UDP | General-purpose range for user services. |

Ports outside these ranges are blocked by default.

## User Responsibilities

- Only bind ports within the user-available ranges listed above.
- Do not interfere with system-reserved ports or services.
- Terminate services and release ports when no longer needed.
- If you need a port outside the allowed ranges, contact the administrator. Approval depends on NUS firewall policy.
