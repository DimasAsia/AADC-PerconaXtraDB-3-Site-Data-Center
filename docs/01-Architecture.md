## Architecture Rationale

The active-active multi–data center architecture was selected to eliminate
single points of failure and ensure continuous service availability. Each
node operates as both a reader and a writer, allowing the system to remain
operational even if one or more nodes or sites become unavailable.

This design prioritizes availability and consistency, which are critical
requirements for enterprise and regulated environments.

## Key Design Decisions
- Synchronous replication using Galera to guarantee strong data consistency
- Equal node roles to avoid primary node dependency and reduce failover complexity
- SSL/TLS enabled for secure inter-node and client communication
- Logical separation between monitoring services and database workloads

## Trade-offs
- Higher latency compared to asynchronous replication models
- Increased resource consumption under high write concurrency
- Additional tuning required for wide-area network (WAN) environments

These trade-offs are acceptable for enterprise systems where data integrity
and availability take precedence over raw throughput.

## General Topology
- Three Data Centers (DC A, DC B, DC C)
- Each site hosts 1–2 Percona XtraDB Cluster (PXC) nodes  
  *(5 database nodes in total)*
- Dedicated node for **Percona Monitoring and Management (PMM)** and  
  **Galera Arbitrator (garbd)**
- Inter-site communication established over a private VPN
- Average inter-site latency: **5–15 ms**

<img width="670" height="657" alt="Percona XtraDB Cluster – Multi-DC Topology" src="https://github.com/user-attachments/assets/272d7228-ea15-4136-ac47-9ca9ef62d7fa" />

## Virtual Machine Specifications
| Component | CPU | RAM | Storage | OS | Role |
|-----------|-----|------|----------|------|---------|
| Database Node | 4 vCPU | 8 GB | 300 GB | Ubuntu 20.04 LTS | PXC Database Cluster |
| PMM + Garbd | 4 vCPU | 4 GB | 200 GB | Ubuntu 20.04 LTS | Monitoring & Quorum |
| Load Generator | 4 vCPU | 8 GB | 100 GB | Ubuntu 20.04 LTS | Sysbench & JMeter |

---

### Architectural Notes
- No external load balancer (HAProxy / ProxySQL) was used
- Applications and load-testing tools connect directly to PXC nodes
- SSL/TLS is enabled for:
  - Client-to-database communication
  - Inter-node replication traffic
