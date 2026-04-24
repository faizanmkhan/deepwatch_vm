# 🔍 deepwatch-vm

> Watching what's happening deep inside your VM — in real time.

A lightweight, containerized monitoring stack that scrapes, stores, and visualizes your VM's system metrics using **Prometheus**, **Node Exporter**, and **Grafana** — all wired together with Docker Compose.

---

## 📸 What It Does

- Collects CPU, memory, disk, and network metrics from the VM
- Stores metrics in Prometheus with persistent storage
- Visualizes everything in a live Grafana dashboard
- Fully containerized — spins up with a single command

---

## 🧱 Stack

| Tool | Role | Port |
|---|---|---|
| Node Exporter | Collects VM system metrics | `9100` |
| Prometheus | Scrapes and stores metrics | `9090` |
| Grafana | Visualizes metrics | `3000` |

---

## 📁 Project Structure

```
deepwatch-vm/
├── docker-compose.yaml      # Defines all services
├── prometheus.yaml          # Scrape config for Prometheus
├── dashboards/
│   └── node-exporter.json  # Pre-built Grafana dashboard
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- Docker
- Docker Compose

### Run It

```bash
git clone https://github.com/faizanmkhan/deepwatch-vm.git
cd deepwatch-vm
docker compose up -d
```

### Access the Services

| Service | URL |
|---|---|
| Grafana | `http://<vm-ip>:3000` |
| Prometheus | `http://<vm-ip>:9090` |
| Node Exporter | `http://<vm-ip>:9100/metrics` |

---

## 📊 Setting Up the Dashboard

1. Open Grafana at `http://<vm-ip>:3000`
2. Login with `admin / admin`
3. Go to **Connections → Data Sources → Add data source**
4. Select **Prometheus** and set URL to `http://prometheus:9090`
5. Click **Save & Test**
6. Go to **Dashboards → Import**
7. Upload `dashboards/1860_rev45.json` or use ID `1860`

---

## 🧪 Testing the Setup

Stress the CPU to verify metrics are flowing:

```bash
# Install stress
sudo apt install stress -y

# Max out all CPUs for 60 seconds
stress --cpu $(nproc) --timeout 60
```

Watch the CPU graph spike in Grafana in real time.

---

## 🔍 Useful Prometheus Queries

```promql
# CPU Usage %
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100)

# Memory Usage %
100 - ((node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes) * 100)

# Disk Usage %
100 - ((node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100)
```

---

## 🛑 Stopping the Stack

```bash
docker compose down
```

Data is persisted in Docker volumes — metrics and dashboards survive restarts.

---

## 📌 Notes

- Scrape interval is set to `15s` — metrics update every 15 seconds
- Grafana data (dashboards, users) is persisted via `grafana-data` volume
- Prometheus metrics are persisted via `prometheus-data` volume