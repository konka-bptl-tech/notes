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