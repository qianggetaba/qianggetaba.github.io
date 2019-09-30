---
title: hadoop
date: 2019-09-30 15:43:19
tags:
---

vm install debian standard，ustc mirror


apt-get install openssh-server
systemctl enable ssh --now

normal user login via ssh

su -
nano /etc/sudoers
Defaults        env_keep = "http_proxy https_proxy ftp_proxy DISPLAY XAUTHORITY"
li    ALL=(ALL:ALL) ALL

export http_proxy=http://192.168.1.37:1080

sudo apt-get upgrade

sudo apt install default-jre
java -version

wget http://us.mirrors.quenda.co/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz
tar xf hadoop-2.7.7.tar.gz

/etc/profile
export JAVA_HOME=/usr/lib/jvm/default-java
export CLASSPATH=.:$JAVA_HOME/lib
export PATH=$PATH:$JAVA_HOME/bin
export HADOOP_HOME=/home/li/hadoop-2.7.7
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

re login

nano hadoop-2.7.7/etc/hadoop/core-site.xml
<configuration>  
    <property>  
       <name>fs.default.name</name>  
       <value>hdfs://cluster-master:9000</value>  
   </property>
</configuration>

cp hadoop-2.7.7/etc/hadoop/mapred-site.xml.template hadoop-2.7.7/etc/hadoop/mapred-site.xml
nano hadoop-2.7.7/etc/hadoop/mapred-site.xml
<configuration>  
    <property>  
       <name>mapreduce.framework.name</name>  
       <value>yarn</value>  
   </property>
</configuration>

nano hadoop-2.7.7/etc/hadoop/hdfs-site.xml
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
<configuration>
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

关机快照，链接克隆3个，hadoop1,2,3

启动三个克隆,normal login, ip -4 addr

1,ssh 登录其他两个，正常
2,3删除id_rsa，rm .ssh/id_rsa*，hosts,192.168.245.135 cluster-master

1，sudo nano /etc/hosts
192.168.245.135 cluster-master
192.168.245.137 cluster-slave1
192.168.245.136 cluster-slave2

ping test
ssh test

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

test
yarn jar hadoop-2.7.7/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar pi 4 10


datanode 日志，slave
nano /home/li/hadoop-2.7.7/logs/hadoop-li-datanode-debian1.log