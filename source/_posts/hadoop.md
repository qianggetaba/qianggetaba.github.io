---
title: hadoop
date: 2019-09-30 15:43:19
tags:
---

vm install debian standard,grpghic need ram 1G，ustc mirror

boot into vm,root login

apt-get install openssh-server
systemctl enable ssh --now
ip -4 addr

normal user login via ssh

su -
nano /etc/sudoers
Defaults        env_keep = "http_proxy https_proxy ftp_proxy DISPLAY XAUTHORITY"
li      ALL=(ALL:ALL) ALL
exit

export http_proxy=http://192.168.1.37:1080

sudo apt-get upgrade

sudo apt install default-jdk
java -version

wget http://us.mirrors.quenda.co/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz
tar xf hadoop-2.7.7.tar.gz

sudo nano /etc/profile
export JAVA_HOME=/usr/lib/jvm/default-java
export CLASSPATH=.:$JAVA_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin
export HADOOP_HOME=/home/li/hadoop-2.7.7
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin


nano hadoop-2.7.7/etc/hadoop/core-site.xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>  
    <property>  
       <name>fs.default.name</name>  
       <value>hdfs://cluster-master:9000</value>  
   </property>
</configuration>

cp hadoop-2.7.7/etc/hadoop/mapred-site.xml.template hadoop-2.7.7/etc/hadoop/mapred-site.xml
nano hadoop-2.7.7/etc/hadoop/mapred-site.xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>  
    <property>  
       <name>mapreduce.framework.name</name>  
       <value>yarn</value>  
   </property>
</configuration>

nano hadoop-2.7.7/etc/hadoop/hdfs-site.xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>  
        <name>dfs.replication</name>  
        <value>1</value>  
    </property>  
    <property>   
     <name>dfs.permissions</name>   
     <value>false</value>   
  </property>  
   <property>  
       <name>dfs.namenode.name.dir</name>  
       <value>file:///home/li/data/hdfs/nn</value>  
   </property>  
   <property>  
        <name>fs.checkpoint.dir</name>  
        <value>file:///home/li/data/hdfs/snn</value>  
    </property>  
    <property>  
        <name>fs.checkpoint.edits.dir</name>  
        <value>file:///home/li/data/hdfs/snn</value>  
    </property>  
       <property>  
       <name>fs.datanode.data.dir</name>  
       <value>file:///home/li/data/hdfs/dn</value>  
   </property>
</configuration>

nano hadoop-2.7.7/etc/hadoop/yarn-site.xml
<?xml version="1.0"?>
<configuration>
    <property>
        <name>yarn.resourcemanager.hostname</name>
    <value>cluster-master</value>
    </property>
    <property>  
        <name>yarn.nodemanager.aux-services</name>  
        <value>mapreduce_shuffle</value>  
   </property>  
   <property>  
        <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>    
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>  
   </property>
</configuration>

nano hadoop-2.7.7/etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/default-java

ssh-keygen -t rsa
cat .ssh/id_rsa.pub >>.ssh/authorized_keys
chmod 600 .ssh/authorized_keys

ssh localhost

sudo systemctl poweroff -i 关机快照，链接克隆3个，hadoop1,2,3

启动三个克隆,normal login, ip -4 addr

ssh normal user login all

sudo hostnamectl set-hostname cluster-master

sudo nano /etc/hosts
192.168.245.135 cluster-master
192.168.245.137 cluster-slave1
192.168.245.136 cluster-slave2

2,rm .ssh/id_rsa*

master
ssh cluster-master
ssh cluster-slave1
ssh cluster-slave2
ssh 0.0.0.0

nano hadoop-2.7.7/etc/hadoop/slaves
cluster-slave1
cluster-slave2

hdfs namenode -format

start-dfs.sh
start-yarn.sh

stop-yarn.sh
stop-dfs.sh

http://192.168.245.135:50070  HDFS
http://192.168.245.135:8088   yarn

:50070的datanodes会显示slave
hdfs fsck /
hdfs dfs -ls /
hdfs dfs -mkdir -p /user/zyh
hdfs dfsadmin -report  datanode报告

test
yarn jar hadoop-2.7.7/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar pi 4 10


datanode 日志
ls hadoop-2.7.7/logs/
rm hadoop-2.7.7/logs/*

:8088/cluster 查看job进度，state一直为accepted是有问题的
左边的cluster--applications--accepted可以看各个状态的任务
比如finished，点击第一列的job id可以进去看日志
