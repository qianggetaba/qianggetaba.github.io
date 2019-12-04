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

pi 100 5000000, Pi is 3.14159312800000000000


datanode 日志
ls hadoop-2.7.7/logs/
rm hadoop-2.7.7/logs/*

:8088/cluster 查看job进度，state一直为accepted是有问题的
左边的cluster--applications--accepted可以看各个状态的任务
比如finished，点击第一列的job id可以进去看日志



单机开发环境
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
ssh localhost

sudo apt-get install ssh rsync

nano etc/hadoop/hadoop-env.sh
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_221

# 测试运行
mkdir input
cp etc/hadoop/*.xml input
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.6.jar grep input output 'dfs[a-z.]+'
cat output/*

rm -rf output
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar wordcount input output


# jar
idea maven proj
<properties>
        <hadoop.version>2.7.7</hadoop.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>


        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
    </dependencies>

WordCount.java<<<
package ddddd;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

  public static class TokenizerMapper 
       extends Mapper<Object, Text, Text, IntWritable>{
    
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();
      
    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }
  
  public static class IntSumReducer 
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values, 
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length < 2) {
      System.err.println("Usage: wordcount <in> [<in>...] <out>");
      System.exit(2);
    }
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    for (int i = 0; i < otherArgs.length - 1; ++i) {
      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
    }
    FileOutputFormat.setOutputPath(job,
      new Path(otherArgs[otherArgs.length - 1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
<<<
使用右侧的maven，clean，package
C:\Users\mjoys\proj\hadoopmy\target

使用类全名，不然main ClassNotFoundException
bin/hadoop jar jar/hadoopmy-1.0.jar ddddd.WordCount input output

自定义排序
自定义包装类 implements WritableComparable，里面有比较方法compareTo
mapper读取行，写入以自定义对象为key的k-v,context.write(自定义对象,NullWritable.get());，hadoop会自动调用自定义保证类排序

分区
多个reduce，每个reduce生成一个结果文件
自定义分区类 extends Partitioner里面getPartition()第三个参数为分区数量,返回结果只能为0,1,2,3等，表示分区号，因为源码里返回值计算 return (key.hashCode() & Integer.MAX_VALUE) % numReduceTasks;
job.setNumReduceTasks(3) reduce数量即为分区数量
