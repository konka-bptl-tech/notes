- Google SRE Book

1. Install Helm chart of newrelic for k8s infra monitoring
dnf list all | grep nginx

Prometheus is a TSDB based on time you can get the data at give point of time what is the value
Prometheus focus on collecting data rather than visualization
The default configuaration for storing maximum days is 2weeks
If TSDB have many no of days data DB is very slow 

Grafana mimir

maintainning static configuration is very difficult
so configure dynamic configuration

black box monitoring = end users can test the application
white box monitoring = cpu,ram,memory, no of requests, latency=time to respond to our request

up
node_memory_MemAvailable_bytes / 1024
 - job_name: 'ec2-instances'
    ec2_sd_configs:
      - region: us-east-1
        filters:
          - name: "tag:Monitoring"
            values: ["true"]


1860

APM monitoring

How can i manage my own repos instead of donwloading from public repos

for APM we are using Jaegar open source edition for enterprise newrelic

100 - avg by (instance,name) (rate(node_cpu_seconds_total{node="idle"})[5m]) * 100

100 - (node_memory_MemFree_bytes / node_memory_MemTotal_bytes) * 100

100 - (node_filesystems_avail_bytes{mountpoint="/"}  * 100 / node_filesystems_size_bytes{mountpoint="/"})

sum by (instance,name) (rate(node_network_receive_bytes_total[1m])) + sum by (instance,name) (irate(node_network_transmit_bytes_total[1m]))

4 Golden Rules
1. Latency
2. Errors
3. Traffic
 - can be done via ELK
4. Saturation[Prometheus]

EL/FK
Elastic Search = Repo which store logs
Kibana = visualization tool for Elastic Search
Logstash = filtering the logs before storing into elasticsearch
FluentBit = Light weight log collector

