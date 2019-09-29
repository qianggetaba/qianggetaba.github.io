---
title: docker-logstash
date: 2019-09-29 14:46:31
tags:
categories:
 - docker
---

```
// pipeline/logstash.conf
input {
        tcp {
                port => 5000
        }
}

## Add your filters / logstash plugins configuration here

output {
        elasticsearch {
                hosts => "192.168.245.134:9200"
        }
}
```

```
//查看默认配置
docker run --name logstash7.0.0 -p 5044:5044 -p 9600:9600 -d docker.elastic.co/logstash/logstash:7.0.0
docker cp logstash7.0.0:/usr/share/logstash/config .
docker rm -f logstash7.0.0


docker run --name logstash7.0.0 -v /home/qgtb/pipeline/:/usr/share/logstash/pipeline/ -p 5000:5000 -p 9600:9600 -d docker.elastic.co/logstash/logstash:7.0.0
```

springboot
```
//pom.xml
        <dependency>
            <groupId>net.logstash.logback</groupId>
            <artifactId>logstash-logback-encoder</artifactId>
            <version>5.1</version>
        </dependency>
```
```
//application.yml
logging:
  config: classpath:logback-spring.xml
```
```
//logback-spring.xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <include resource="org/springframework/boot/logging/logback/base.xml" />

    <appender name="LOGSTASH" class="net.logstash.logback.appender.LogstashTcpSocketAppender">
        <destination>192.168.245.134:5000</destination> //logstash ip和暴露的端口
        <encoder charset="UTF-8" class="net.logstash.logback.encoder.LogstashEncoder" />
    </appender>

    <root level="warn">
        <appender-ref ref="LOGSTASH" />
        <appender-ref ref="CONSOLE" />
    </root>
</configuration>
```
```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

private Logger log = LoggerFactory.getLogger(App.class.getName());
```