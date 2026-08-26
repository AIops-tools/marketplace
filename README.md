<!-- mcp-name: io.github.AIops-tools/marketplace -->
# AIops-tools plugin marketplace

Governed infrastructure ops for self-hosted and mid-sized estates. Each plugin
bundles a skill and its MCP server, so one install gives an agent both the
procedure and the tools.

```bash
/plugin marketplace add AIops-tools/marketplace
/plugin install proxmox-aiops@aiops-tools
```

## What "governed" means here

Every state change is recorded through the same path whether it arrives over MCP
or the CLI — there is no unaudited entry point. Writes that can be reversed
record an undo token capturing the state *before* the change, and a runaway
breaker stops a loop that has gone wrong. The audit database lives on your
machine (`~/.<tool>/audit.db`); nothing is sent anywhere.

Authorization is deliberately **not** part of this. Whether an agent may write is
your agent's decision or your account's permissions — give it a read-only account
and writes are refused by the server, where that belongs.

## Plugins

| Plugin | Platform |
|---|---|
| `proxmox-aiops` | Proxmox VE — VMs/LXC, snapshots, cluster, storage, backup, HA |
| `veeam-aiops` | Veeam Backup & Replication — jobs, restore, repositories, sessions |
| `k8s-aiops` | Kubernetes (k3s/EKS/GKE/AKS) — workloads, config, storage, rollout |
| `network-aiops` | NAPALM multi-vendor devices + NetBox |
| `truenas-aiops` | TrueNAS SCALE — pools, datasets, snapshots, disks, alerts |
| `nutanix-aiops` | Nutanix Prism Central v4 |
| `ceph-aiops` | Ceph — OSD/PG/pool/RBD/CephFS/RGW, recovery, capacity |
| `inference-aiops` | GPU inference clusters (vLLM + Ray Serve) |
| `monitoring-aiops` | SolarWinds Orion, PRTG, Zabbix |
| `compliance-aiops` | Reads the other tools' audit trails → sealed, framework-mapped evidence + OSCAL |
| `ai-guardian` | Local LLM (Ollama) observability, policy and a transparent capture proxy |
| `endpoint-aiops` | Managed endpoint fleets — logon storms, patch/config drift |
| `fabric-aiops` | Controller-layer networking (Meraki, Catalyst Center, CVP, UniFi) |
| `postgres-aiops` | PostgreSQL — slow queries, bloat, blocking locks, replication |
| `observability-aiops` | Prometheus + Grafana |
| `firewall-aiops` | OPNsense + pfSense |
| `container-host-aiops` | Docker + Portainer (not Kubernetes) |
| `mysql-aiops` | MySQL + MariaDB |
| `proxy-aiops` | Traefik + Caddy + HAProxy |
| `queue-aiops` | Redis + RabbitMQ |
| `minio-aiops` | MinIO — capacity, exposure, lifecycle, object lock (WORM), IAM |
| `xcpng-aiops` | XCP-ng via Xen Orchestra |
| `identity-aiops` | Keycloak + Authentik |
| `cicd-aiops` | GitLab + Gitea |

## Requirements

Each plugin's MCP server is fetched with `uvx`, so [uv](https://docs.astral.sh/uv/)
must be installed. The server is pinned to the exact package version the plugin
declares, so an audit row can always be traced back to the code that wrote it.

Every tool also ships a CLI (`proxmox-aiops --help`) that goes through the same
governed path.

## Honest status

The tools are verified against real servers where the hardware or an account was
available — 19 of 24 at the time of writing, with each repo's
`docs/VERIFICATION.md` recording what was actually proven and what is still only
covered by mocks. Pointing one of these at a real server has produced a defect
the mocks could not see every single time, so please read that file for the tool
you plan to use rather than assuming test count means correctness.

Missing a platform or an operation? Open an issue on the tool's own repository.

MIT licensed.
