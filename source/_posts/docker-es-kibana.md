---
title: docker-es-kibana
date: 2019-09-27 15:04:14
tags:
categories:
 - docker
---

```
docker run --name elasticsearch5.6.11 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node"  -d elasticsearch:5.6.11
curl 127.0.0.1:9200  # 需要等一会
http://192.168.245.134:9200/
docker run --name kibana5.6.11 -e ELASTICSEARCH_URL=http://192.168.245.134:9200 -p 5601:5601 -d kibana:5.6.11
http://192.168.245.134:5601

docker run --name logstash7.0.0 -p 5044:5044 -p 9600:9600 -d docker.elastic.co/logstash/logstash:7.0.0
docker cp logstash7.0.0:/usr/share/logstash/config .
docker rm -f logstash7.0.0

```
