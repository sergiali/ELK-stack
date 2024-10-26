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


- 2 command below all done on master node

#~ systemctl start elasticsearch

generate TOKEN  #~ ./elasticsearch-create-enrollment-token -s node

- run 2 command bellow on slave nodes

#~ ./elasticsearch-reconfigure-node --enrollment-token $TOKEN

then edit elasticsearch.yml file on nodes 

#~ systemctl restart elasticsearch

- is time to kibana : 

reset kibana users password to "kibana":  #~ /usr/share/elasticsearch/bin/elasticsearch-reset-password -u kibana -i

- install kibana .deb file on node 1:   #~ dpkg -i kibana.deb

generate .key and .crt file from p12 key in 4 steps bellow:

save password from bellow:  

1. #~ /usr/share/elasticsearch/bin/elasticsearch-keystore show xpack.security.http.ssl.keystore.secure_password  

create and expose .key and .crt file from p12 file:  

2. #~ openssl pcks12 -in /etc/elasticsearch/certs/http.p12 -out server.crt -clcerts -nokeys 

3. #~ openssl pcks12 -in /etc/elasticsearch/certs/http.p12 -out server.key -nocerts -nodes

make ready keys to usage of kibana app:

4. #~ chown root:kibana /etc/kibana/server.* chmod g+r /etc/kibana/server.*

config kibana.yml file like the kibana.yml uploaded here.

restart kibana:  #~ systemctl restart kibana

- the way you can test your cluster and see the totally status on terminal :

nodes: #~ curl -k -u elastic "https://37.32.12.116:9200/_cat/nodes?v"          kibana: "https://37.32.12.116:5601"      cluster health: "https://37.32.12.116:9200/_cluster/health"

- generate keys for Logstash-filebeat

#~ openssl genrsa -out ca.key 2048

#~ openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt


- create logstash config file and write configs like the logstash.conf file uploaded here

#~ openssl genrsa -out logstash.key 2048

#~ openssl req -sha512 -new -key logstash.key -out logstash.csr -config logstash.conf

#~ openssl x509 -in ca.crt -text -noout  -serial

#~ echo "(serial-number)" > serial

#~ openssl x509 -days 3650 -req -sha512 -in logstash.csr -CAserial serial -CA ca.crt -CAkey ca.key -out logstash.crt -extensions v3_req -extfile logstash.conf

#~ mv logstash.key logstash.key.pem && openssl pkcs8 -in logstash.key.pem -topk8 -nocrypt -out logstash.key
