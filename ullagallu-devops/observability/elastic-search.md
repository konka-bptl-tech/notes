# Elastic Search 
1. Install Java for Ealstic Search
2. yum install elasticsearch -y
vim /etc/elasticsearch/elasticsearch.yml
network.host: 0.0.0.0
http.port: 9200
discovery.type: sigle-node

systemctl restart elasticsearch

curl localhost:9200

yum install kibana -y
vim /etc/kibana/kibana.yml
server.host: "0.0.0.0"
server.port: 5601
elasticsearch.hosts: ["http://localhost:9200"]

systemctl restart kibana

- install filebeat
- modify configuration file
  enable true
  loagstach ip
  and restart
Goto Kibana --> Discover --> Index Management[see all logs shipped by filebeat]
                         --> Index Patterns --> create index pattern --> name: filebeat
                                                                     --> Timestamp field: @timestamp --> create index pattern


- Install logstash
- 