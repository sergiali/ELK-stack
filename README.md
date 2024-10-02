# ELK-Stack
in this repo we are implement elastic search cluster with kibana, logstash, filebeats to read nginx access logs. and then send and config them to Grafana for monitoring(in the next repository).

- installation procces
install elk.deb file on all of the nodes (don't start yet those)

download elasticsearch.deb file from link below on node 1, 2 ,3
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.15.2-amd64.deb

download kibana.deb file from link below on node 1
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.15.2-amd64.deb

download filebeat.deb file from link below on node 2 
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-8.15.2-amd64.deb

download logstash.deb file from link below on node 3
wget https://artifacts.elastic.co/downloads/logstash/logstash-8.15.2-amd64.deb


- now edit elasticsearch yaml file to set and initialize cluster like the elasticsearch.yml file uploaded here.

