
# 🔍 System Monitoring with Prometheus and Grafana

Monitor an AWS EC2 Linux instance using Prometheus for metrics collection and Grafana for visualization. This setup also includes Node Exporter for gathering system-level metrics.

---

## 📌 Project Overview

- **Type**: DevOps Mini-Project  
- **Technologies**: Docker, Prometheus, Grafana, Node Exporter, AWS EC2  
- **Goal**: Set up a real-time system monitoring stack on a Linux server using containerized services.

---

## 🧰 Prerequisites

- AWS EC2 instance (Ubuntu 20.04)
- Docker & Docker Compose installed
- Security Group allows: `22`, `9090`, `3000`, `9100`

---

## ⚙️ Project Structure

```
prometheus-grafana-monitoring/
├── docker-compose.yml
└── prometheus.yml
```

---

## 🧾 Setup Instructions

### 1. SSH into EC2

```bash
 ssh -i "Prometheus.pem" ubuntu@ec2-13-218-62-109.compute-1.amazonaws.com
```

### 2. Install Docker & Docker Compose

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo usermod -aG docker ubuntu
```

Logout and re-login to apply Docker group permission.

---

### 3. Clone the Project

```bash
git clone https://github.com/venky2350/prometheus-grafana-monitoring.git
cd prometheus-grafana-monitoring
```

---

### 4. Configuration Files

#### `docker-compose.yml`

```yaml
version: '3'

services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
    ports:
      - "9090:9090"

  node-exporter:
    image: prom/node-exporter
    ports:
      - "9100:9100"

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
    volumes:
      - grafana-storage:/var/lib/grafana

volumes:
  grafana-storage:
```

#### `prometheus.yml`

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

---

### 5. Start the Stack

```bash
docker-compose up -d
```

---

## ✅ Access Services

| Service       | URL                            |
|---------------|---------------------------------|
| Prometheus    | `http://<ec2-ip>:9090`         |
| Node Exporter | `http://<ec2-ip>:9100/metrics` |
| Grafana       | `http://<ec2-ip>:3000`         |

---

## 📊 Grafana Setup

1. Login (Default: `admin` / `admin`)
2. Add Data Source → Prometheus → `http://prometheus:9090`
3. Import Dashboard → ID: `1860` (Node Exporter Full)

---

## 🚨 Optional: Alerts

You can configure alerts in Grafana:
- Go to: **Alerting → Contact Points**
- Add email/webhook/Slack alert

---

## 🧹 Stopping the Stack

```bash
docker-compose down
```

---

## 📎 Author

**Yendoti Venkateswarlu**  
MCA Graduate | DevOps Trainee  
GitHub: [venkateswarluyendoti](https://github.com/venkateswarluyendoti)

---

## 🏷️ Tags

`#DevOps` `#Prometheus` `#Grafana` `#Monitoring` `#AWS` `#Docker`
