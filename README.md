# Grafana Monitoring Stack

Complete Monitoring Setup using:

- Node Exporter
- Prometheus
- Grafana

## Architecture

Ubuntu Server
   ↓
Node Exporter (System Metrics)
   ↓
Prometheus (Collect & Store Metrics)
   ↓
Grafana (Visualization Dashboard)

---

# STEP 0: System Update

```bash
sudo apt update && sudo apt upgrade -y
```

---

# STEP 1: Check Server Information

```bash
hostname -I
lsb_release -a
```

---

# STEP 2: Firewall Configuration

```bash
sudo ufw allow 3000
sudo ufw allow 9090
sudo ufw allow 9100
sudo ufw enable
```

Ports:

| Service | Port |
|---|---|
| Grafana | 3000 |
| Prometheus | 9090 |
| Node Exporter | 9100 |

---

# STEP 3: Install Node Exporter

## Download

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.11.1/node_exporter-1.11.1.linux-amd64.tar.gz
```

## Extract

```bash
tar xvf node_exporter-1.11.1.linux-amd64.tar.gz
```

## Enter Directory

```bash
cd node_exporter-1.11.1.linux-amd64
```

## Run

```bash
./node_exporter
```

## Run in Background

```bash
nohup ./node_exporter > node.log 2>&1 &
```

## Stop Process

```bash
ps aux | grep node_exporter
kill -9 PID
```

## Verify

Open browser:

```bash
http://YOUR-IP:9100/metrics
```

---

# STEP 4: Install Prometheus

## Download

```bash
wget https://github.com/prometheus/prometheus/releases/download/v3.11.3/prometheus-3.11.3.linux-amd64.tar.gz
```

## Extract

```bash
tar -xvzf prometheus-*.tar.gz
```

## Enter Directory

```bash
cd prometheus-3.11.3.linux-amd64
```

## Run Test

```bash
./prometheus --config.file=prometheus.yml
```

## Run in Background

```bash
nohup ./prometheus --config.file=prometheus.yml > prom.log 2>&1 &
```

## Stop Process

```bash
ps aux | grep prometheus
kill -9 PID
```

## Verify

```bash
http://YOUR-IP:9090
```

Check:

```bash
Status → Targets → UP
```
<img width="1902" height="437" alt="3" src="https://github.com/user-attachments/assets/c7dbca14-5de9-496f-8cf3-49bab3e86ff8" />

---

# STEP 5: Install Grafana

## Install Dependencies

```bash
sudo apt install -y apt-transport-https software-properties-common wget
```

## Add Repository

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -

echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

## Install Grafana

```bash
sudo apt update
sudo apt install grafana -y
```

## Start Service

```bash
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

## Open Grafana

```bash
http://YOUR-IP:3000
```

Default Login:

```bash
admin / admin
```

---

# STEP 6: Connect Prometheus to Grafana

Go to:

```text
Connections → Data Sources → Add Prometheus
```

Prometheus URL:

```bash
http://YOUR-IP:9090
```

Click:

```text
Save & Test
```

---

# STEP 7: Import Dashboard

Go to:

```text
Dashboards → Import
```

Dashboard ID:

```text
1860
```

Select:

```text
Prometheus → Import
```

---

# STEP 8: Verification

## Node Exporter

```bash
http://YOUR-IP:9100/metrics
```

## Prometheus

```bash
http://YOUR-IP:9090/targets
```

## Grafana

```bash
http://YOUR-IP:3000
```

---

# Useful Commands

## Check Running Processes

```bash
ps aux | grep prometheus
ps aux | grep node_exporter
```

## Kill Process

```bash
kill -9 PID
```

---

# Technologies Used

- Prometheus
- Node Exporter
- Grafana
- Ubuntu Server

---

# Dashboard Used

Grafana Dashboard ID:

```text
1860
```

---

# Future Improvements

- AlertManager Integration
- Slack/Telegram Alerts
- Docker Deployment
- Systemd Service Setup
- SSL/HTTPS Configuration
