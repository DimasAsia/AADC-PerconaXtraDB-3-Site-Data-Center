# Architecture

## Topologi Umum
- 3 Site Data Center (DC A, DC B, DC C)
- Tiap site memiliki 1 atau 2 node PXC (total 5 node)
- 1 Node khusus untuk **PMM Server + Garbd**
- Komunikasi antar-site melalui jaringan private VPN (latency 5–15 ms)

<img width="690" height="677" alt="percona drawio" src="https://github.com/user-attachments/assets/272d7228-ea15-4136-ac47-9ca9ef62d7fa" />

### Spesifikasi VM
| Komponen | CPU | RAM | Storage | OS | Fungsi |
|-----------|-----|------|----------|------|---------|
| DB Node | 4 vCPU | 8 GB | 300 GB | Ubuntu 20.04 | Database Cluster |
| PMM + Garbd | 4 vCPU | 4 GB | 200 GB | Ubuntu 20.04 | Monitoring & Quorum |
| LoadGen | 4 vCPU | 8 GB | 100 GB | Ubuntu 20.04 | Sysbench & JMeter |

---

### Catatan
- Tidak menggunakan **ProxySQL/HAProxy**
- Semua koneksi langsung ke node PXC.
- SSL aktif di semua komunikasi client–server dan antar-node.

