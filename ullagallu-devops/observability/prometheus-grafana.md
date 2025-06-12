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
