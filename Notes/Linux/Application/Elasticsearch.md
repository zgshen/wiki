# Elasticsearch

`vi /etc/elasticsearch/elasticsearch.yml`

```json
#外部访问，配置为虚拟机IP或者0.0.0.
#network.host: 192.168.0.1
network.host: 0.0.0.0
discovery.seed_hosts: ["0.0.0.0"]
```