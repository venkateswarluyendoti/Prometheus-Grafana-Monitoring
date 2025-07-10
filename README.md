# Prometheus-Grafana-Monitoring

## System Monitoring with Prometheus and Grafana

## Project Objective:

Monitor a Linux EC2 instance using Prometheus and visualize metrics using Grafana, with Node Exporter as the system metrics collector.

## Prerequisites:

- AWS EC2 Ubuntu instance
- Docker + Docker Compose installed
- Open ports: 22, 9090, 3000

## Step-by-Step Implementation:

1. Launch & Connect to EC2:

- Open required ports in Security Group.
- SSH: ssh -i "key.pem" ubuntu@your-ec2-public-ip

2. Install Docker & Docker Compose:
   sudo apt update
   sudo apt install docker.io docker-compose -y
   sudo usermod -aG docker ubuntu

3. Create Project Directory:
   mkdir prometheus-grafana-monitoring && cd
   prometheus-grafana-monitoring

4. Create Files:
   ### docker-compose.yml:

---

version: '3'

services:
prometheus:
image: prom/prometheus
volumes: - ./prometheus.yml:/etc/prometheus/prometheus.yml
ports: - "9090:9090"

node-exporter:
image: prom/node-exporter
ports: - "9100:9100"

grafana:
image: grafana/grafana
ports: - "3000:3000"
volumes: - grafana-storage:/var/lib/grafana

volumes:
grafana-storage:

## prometheus.yml:

global:
scrape_interval: 15s

scrape_configs:

- job_name: 'prometheus'
  static_configs:

  - targets: ['localhost:9090']

- job_name: 'node-exporter'
  static_configs:
  - targets: ['node-exporter:9100']

5. Start All Services: docker-compose up -d
6. Verify Services:

- Prometheus: http://your-ip:9090
- Grafana: http://your-ip:3000 (admin/admin)
- Node Exporter: http://your-ip:9100/metrics

7. Grafana Setup:

- Add Prometheus data source: http://prometheus:9090
- Import Dashboard ID: 1860

8. Optional Alerts:

- Use Grafana Alerting > Contact Points


  To Stop:
  --------
docker-compose down
