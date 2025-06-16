# Prometheus & Grafana
1. Download the extract and mv package as prometheus
  https://prometheus.io/download/
  wget  wget https://github.com/prometheus/prometheus/releases/download/v2.53.4/prometheus-2.53.4.linux-amd64.tar.gz
  tar -xvzf prometheus-2.53.4.linux-amd64.tar.gz
  mv prometheus-2.53.4.linux-amd64 prometheus
2. Create Service file
   
mkdir -p /home/ec2-user/prometheus/data --> storage for prometheus

sudo nano /etc/systemd/system/prometheus.service

```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/prometheus/prometheus \
--config.file=/home/ec2-user/prometheus/prometheus.yml \
--storage.tsdb.path=/home/ec2-user/prometheus/data \
--web.console.templates=/home/ec2-user/prometheus/consoles \
--web.console.libraries=/home/ec2-user/prometheus/console_libraries

Restart=on-failure

[Install]
WantedBy=multi-user.target
```

    sudo systemctl daemon-reexec[refreshes prometheus service]
    sudo systemctl daemon-reload
    sudo systemctl enable prometheus
    sudo systemctl start prometheus
    sudo systemctl status prometheus
    journalctl -u prometheus -xe

9090  = prometheus
9100  = Node exporter

# Node Exporter Downloads
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvzf node_exporter-1.9.1.linux-amd64.tar.gz
mv node_exporter-1.9.1.linux-amd64 node_exporter
rm -rf node_exporter-1.9.1.linux-amd64.tar.gz
sudo nano /etc/systemd/system/node_exporter.service
```bash
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/node_exporter/node_exporter
Restart=on-failure

[Install]
WantedBy=default.target
```
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

curl http://localhost:9100/metrics


# Alert Manager

wget https://github.com/prometheus/alertmanager/releases/download/v0.28.1/alertmanager-0.28.1.linux-amd64.tar.gz
tar -xvzf alertmanager-0.28.1.linux-amd64.tar.gz
sudo mv alertmanager-0.28.1.linux-amd64 alertmanager

sudo nano /etc/systemd/system/alertmanager.service
[Unit]
Description=Prometheus Alertmanager
After=network.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/alertmanager/alertmanager \
  --config.file=/home/ec2-user/alertmanager/alertmanager.yml \
  --storage.path=/home/ec2-user/alertmanager/data
Restart=on-failure

[Install]
WantedBy=multi-user.target

sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager
sudo systemctl status alertmanager

curl http://localhost:9093


<!-- To configure **Slack** to receive Prometheus alerts via **Alertmanager**, you need to create an **Incoming Webhook** in Slack. Here's a complete step-by-step guide:

---

### 🛠️ Step 1: Create a Slack App with Incoming Webhook

1. **Go to Slack API apps**:
   [https://api.slack.com/apps](https://api.slack.com/apps)

2. **Click "Create New App"**

   * Choose “From scratch”
   * Give it a name like `Prometheus Alertmanager`
   * Pick your Slack workspace and click **Create App**

3. **Enable Incoming Webhooks**

   * In the app settings menu (left panel), click **Incoming Webhooks**
   * Click the toggle to **"Activate Incoming Webhooks"**

4. **Add a Webhook URL**

   * Scroll down and click **"Add New Webhook to Workspace"**
   * Select a channel (e.g., `#alerts`) where you want alerts to appear
   * Click **Allow**

5. **Copy the Webhook URL**
   It will look like this:

   ```
   https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX
   ```

---

### 🧩 Step 2: Add Webhook URL to Alertmanager

Now, in your `alertmanager.yml`:

```yaml
receivers:
  - name: 'slack-notifications'
    slack_configs:
      - api_url: 'https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX'
        channel: '#alerts'
        send_resolved: true
```

---

### 🔄 Step 3: Reload Alertmanager

After saving your config file:

```bash
sudo systemctl restart alertmanager
```

Or if you're running it manually:

```bash
./alertmanager --config.file=alertmanager.yml
```

---

### ✅ Step 4: Test It

1. Trigger an alert in Prometheus (e.g., simulate `up == 0`)
2. You should receive a notification in your selected Slack channel

---

Let me know if you want to **customize the Slack message format** (e.g., add emojis, colors, or attachments). -->

