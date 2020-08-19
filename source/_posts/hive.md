---
title: hive 3.1.2 高可用安装
date: 2020-08-19 14:33:14
author: 相飞
comments:
- true
tags:
- hive
- bigdata
categories:
- hive
- bigdata


---


#### 机器
|  vip   | ip   |  host  | OS |
|  ----   | ----  | ---- |  ----  |
| 192.168.151.200 |  192.168.226.64   | hadoop-cluster-1   |  centos 7  |
| 192.168.151.200 |  192.168.226.65    | hadoop-cluster-2 |  centos 7  |
| 192.168.151.200 |  192.168.226.63    | hadoop-cluster-3 |  centos 7  |
|     |  192.168.226.66   |  hadoop-datanode01  |  centos 7  |
|     |  192.168.226.67   |  hadoop-datanode02  |  centos 7  |
|     |  192.168.226.68   |  hadoop-datanode03  |  centos 7  |
|     |  192.168.226.69   |  hadoop-datanode04  |  centos 7  |
|     |  192.168.213.59   |  mariadb-galera-cluster |  centos 7 |
|     |  192.168.213.65   |  mariadb-galera-cluster |  centos 7 |
|     |  192.168.213.58   |  mariadb-galera-cluster |  centos 7 |


#### cluster node

- hiveserver2



### tar包下载

 - https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz



### cluster 配置
 
#### add hive_home_path

```
# /etc/profile
export HIVE_HOME=/data/tools/apache-hive-3.1.2-bin
export PATH=$HIVE_HOME/bin:$PATH

```
#### 创建hive dir


```
[root@hadoop-cluster-2 bin]# ./hadoop  fs -mkdir       /tmp
2020-08-19 07:00:55,671 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
[root@hadoop-cluster-2 bin]# 
[root@hadoop-cluster-2 bin]# ./hadoop  fs -mkdir          /user/hive/warehouse
2020-08-19 07:01:12,608 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
mkdir: `hdfs://mycluster/user/hive': No such file or directory
[root@hadoop-cluster-2 bin]# ./hadoop  fs -mkdir       -p   /user/hive/warehouse
2020-08-19 07:01:35,783 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
[root@hadoop-cluster-2 bin]# ./hadoop  fs  -chmod g+w   /tmp
2020-08-19 07:02:04,907 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
[root@hadoop-cluster-2 bin]# ./hadoop  fs -chmod g+w   /user/hive/warehouse
2020-08-19 07:02:20,989 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
[root@hadoop-cluster-2 bin]# 

```

#### 安装 mariadb clint



```
[root@hadoop-cluster-2 conf]# yum -y install mariadb mariadb-dev mariadb-server
```

#### 修改hive remote 配置
 - touch apache-hive-3.1.2-bin/conf/hive-site.xml 

```
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# cat conf/hive-site.xml 
<?xml version="1.0" encoding="UTF-8" standalone="no"?><?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
        <property>
                <name>javax.jdo.option.ConnectionURL</name>
                <value>jdbc:mysql://192.168.213.59:3306/hive</value>
        </property>

        <property>
                <name>javax.jdo.option.ConnectionDriverName</name>
                <value>com.mysql.jdbc.Driver</value>
        </property>

        <property>
                <name>javax.jdo.option.ConnectionUserName</name>
                <value>root</value>
        </property>

        <property>
                <name>javax.jdo.option.ConnectionPassword</name>
                <value>123456</value>
        </property>

        <property>
                <name>hive.metastore.schema.verification</name>
                <value>false</value>
        </property>
	<property> 
   		 <name>hive.cli.print.current.db</name>
		 <value>true</value>
	</property>
	<property> 
	         <name>hive.cli.print.header</name>
	         <value>true</value>
	</property>
	<property>
       		 <name>hive.server2.thrift.port</name>
     		 <value>9090</value>
	</property>

    	<property>
       		<name>hive.server2.thrift.bind.host</name>
       		<value>192.168.213.178</value>
     	</property>

</configuration>


```

 - hive-env.sh

```
# export HIVE_CONF_DIR=

# Folder containing extra libraries required for hive compilation/execution can be controlled by:
# export HIVE_AUX_JARS_PATH=
export HIVE_HOME=/data/tools/apache-hive-3.1.2-bin
export HIVE_CONF_DIR=/data/tools/apache-hive-3.1.2-bin/conf
export HADOOP_HOME=/data/tools/hadoop-3.3.0
export HIVE_AUX_JARS_PATH=/data/tools/apache-hive-3.1.2-bin/lib
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# 

```

### 启动hiveserver2


```
[root@hadoop-cluster-2 bin]# cat start.sh 
nohup ./hiveserver2 2>&1 >/dev/null&
[root@hadoop-cluster-2 bin]# 

```


### error

- java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument

```
which: no hbase in (/data/tools/apache-hive-3.1.2-bin/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
2020-08-19 15:54:32: Starting HiveServer2
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/data/tools/apache-hive-3.1.2-bin/lib/log4j-slf4j-impl-2.10.0.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/data/tools/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Exception in thread "main" java.lang.NoSuchMethodError: com.google.common.base.Preconditions.checkArgument(ZLjava/lang/String;Ljava/lang/Object;)V
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1380)
	at org.apache.hadoop.conf.Configuration.set(Configuration.java:1361)
	at org.apache.hadoop.mapred.JobConf.setJar(JobConf.java:536)
	at org.apache.hadoop.mapred.JobConf.setJarByClass(JobConf.java:554)
	at org.apache.hadoop.mapred.JobConf.<init>(JobConf.java:448)
	at org.apache.hadoop.hive.conf.HiveConf.initialize(HiveConf.java:5141)
	at org.apache.hadoop.hive.conf.HiveConf.<init>(HiveConf.java:5099)
	at org.apache.hadoop.hive.common.LogUtils.initHiveLog4jCommon(LogUtils.java:97)


```

> hadoop和hive中guava jar包可能版本冲突，删除低版本使用高版本的jar包



```
[root@hadoop-cluster-2 hadoop-3.3.0]# pwd
/data/tools/hadoop-3.3.0
[root@hadoop-cluster-2 hadoop-3.3.0]# cd ../hbase-2.3.0
[root@hadoop-cluster-2 hbase-2.3.0]# find -name \*.jar | grep gu
./lib/jersey-guava-2.25.1.jar
./lib/guice-3.0.jar
./lib/guice-servlet-3.0.jar
./lib/guava-11.0.2.jar
./lib/commons-configuration-1.6.jar
[root@hadoop-cluster-2 hbase-2.3.0]# pwd
/data/tools/hbase-2.3.0
[root@hadoop-cluster-2 hbase-2.3.0]# pwd
/data/tools/hbase-2.3.0
[root@hadoop-cluster-2 hbase-2.3.0]# cd /data/tools/hadoop-3.3.0/
[root@hadoop-cluster-2 hadoop-3.3.0]# find -name \*.jar | grep gua
./share/hadoop/common/lib/listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar
./share/hadoop/common/lib/guava-27.0-jre.jar
./share/hadoop/yarn/csi/lib/guava-20.0.jar
./share/hadoop/hdfs/lib/listenablefuture-9999.0-empty-to-avoid-conflict-with-guava.jar
./share/hadoop/hdfs/lib/guava-27.0-jre.jar
[root@hadoop-cluster-2 hadoop-3.3.0]# 
[root@hadoop-cluster-2 hadoop-3.3.0]# pwd
/data/tools/hadoop-3.3.0
[root@hadoop-cluster-2 hadoop-3.3.0]# cd /data/tools/apache-
apache-hive-3.1.2-bin/        apache-hive-3.1.2-bin.tar.gz  apache-zookeeper-3.6.0-bin/   
[root@hadoop-cluster-2 hadoop-3.3.0]# cd /data/tools/apache-hive-3.1.2-bin
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# mv lib/guava-19.0.jar  lib/guava-19.0.jar.bak
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# cp /data/tools/hadoop-3.3.0/share/hadoop/yarn/csi/lib/guava-20.0.jar lib/
\[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# 
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# 
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# ./hiveserver2 
-bash: ./hiveserver2: No such file or directory
[root@hadoop-cluster-2 apache-hive-3.1.2-bin]# cd bin
[root@hadoop-cluster-2 bin]# ./hiveserver2 
which: no hbase in (/data/tools/apache-hive-3.1.2-bin/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
2020-08-19 16:01:32: Starting HiveServer2
SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/data/tools/apache-hive-3.1.2-bin/lib/log4j-slf4j-impl-2.10.0.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/data/tools/hadoop-3.3.0/share/hadoop/common/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.apache.logging.slf4j.Log4jLoggerFactory]
Hive Session ID = edac1d17-208b-4a6e-b79e-af51f84a2d29

```

> 配置galera cluster ,其他2 台机器连接自己的thrift2 server 和数据库配置
