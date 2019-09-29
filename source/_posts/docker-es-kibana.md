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

# es监控，先进入es容器，配置跨域
docker exec -it 42d639a08934 /bin/bash
echo "http.cors.enabled: true" >>/usr/share/elasticsearch/config/elasticsearch.yml
echo "http.cors.allow-origin: \"*\"" >>/usr/share/elasticsearch/config/elasticsearch.yml
docker restart 42d639a08934

docker run --name es-head5 -p 9100:9100 -d docker.io/mobz/elasticsearch-head:5
```
