# Day 1 — SIEM Layer: Wazuh + ELK via Docker Compose

**Owner:** Vaishnavi (Network Architect / SIEM)  
**Branch:** `feature/siem-wazuh`  
**Status:** ✅ Deployed and accessible

---

## What Was Built

The SIEM layer of the Mini SOC — a fully containerized Wazuh + ELK stack running on the host machine via Docker Compose (single-node, version 4.7.5). The dashboard is live at `https://localhost:443` and is ready to receive agent connections from VM3.

---

## Tool Justification

| Tool | Role | Why This, Not Another |
|---|---|---|
| **Wazuh Manager** | Log collection, rule-based alert engine | Open-source, Docker-deployable, no external internet dependency after image pull — SRM network compatible |
| **Wazuh Indexer** | Data storage and indexing (Elasticsearch-based) | Bundled with official Wazuh Docker Compose; no separate Elasticsearch setup required |
| **Wazuh Dashboard** | Web UI for SOC visibility (Kibana-based) | Pre-integrated with indexer; exposes all modules (Security Events, Integrity Monitoring, Policy Monitoring) out of the box |
| **Docker Compose** | Orchestration | Single `docker compose up -d` spins up the entire stack; deterministic, reproducible, zero manual container wiring |

---

## Architecture

```
Host Machine (Windows)
└── Docker Engine
    └── single-node (Docker Compose group)
        ├── wazuh.manager-1      → Log ingestion, rule matching, alert generation
        ├── wazuh.indexer-1      → Stores and indexes all alert/log data
        └── wazuh.dashboard-1   → Browser UI at https://localhost:443
              ↑
        (TLS-encrypted internal communication via generated SSL certs)
              ↑
        VM3 — Wazuh Agent  [pending Day 2 enrollment]
```

---

## Setup Steps (Reproduce This)

### 1. Pre-download Docker images (do this first — before network congestion)
```bash
docker compose pull
```
Images pulled:
- `wazuh/wazuh-manager:4.7.5` — 546s
- `wazuh/wazuh-indexer:4.7.5` — 1102s  
- `wazuh/wazuh-dashboard:4.7.5` — 802s

### 2. Generate SSL certificates
```bash
docker compose -f generate-indexer-certs.yml run --rm generator
```
Generates four certificate sets — Admin, Indexer, Manager, Dashboard — placed in `config/wazuh_indexer_ssl_certs/`. Mandatory before starting the stack.

### 3. Start the stack
```bash
docker compose up -d
```
Creates 14 persistent Docker volumes and starts all 3 containers.

### 4. Access the dashboard
```
https://localhost:443
Default credentials: admin / SecretPassword (change post-setup)
```

---

## Resource Usage (Verified)

| Metric | Value |
|---|---|
| CPU | 1.63% / 1200% (12 CPUs) |
| RAM | 1.64 GB / 7.42 GB |
| Disk | 7.52 GB / 1006.85 GB |

Stack runs efficiently without overloading the host, leaving headroom for VMs running concurrently.

---

## Current State

| Component | Status |
|---|---|
| Wazuh Dashboard | ✅ Live at https://localhost:443 |
| Wazuh Manager | ✅ Running |
| Wazuh Indexer | ✅ Running |
| SSL/TLS between containers | ✅ Configured |
| Agent enrolled (VM3) | ⏳ Day 2 |
| First live alert in dashboard | ⏳ Day 2 (post Nmap scan) |

---

## Screenshots

| # | Description |
|---|---|
| [Screenshot 1](../../screenshots/) | Wazuh Dashboard live at localhost:443 |
| [Screenshot 2](../../screenshots/) | Docker Desktop — all 3 containers running |
| [Screenshot 3](../../screenshots/) | CMD — SSL cert generation output |
| [Screenshot 4](../../screenshots/) | CMD — docker compose pull + up -d output |

---

## SRM Network Constraint Handling

Wazuh + ELK was selected specifically because it is **fully Docker-deployable with zero external internet dependency after the initial image pull**. All images were pre-downloaded in the morning before network congestion. The entire SIEM stack runs on localhost with no external port exposure required.
