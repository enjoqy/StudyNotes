# 大数据装逼项目

## 1、爬取数据

## 2、数据处理

## 3、大数据可视化

## 配置Linux服务器步骤

### 1、查看Linux服务器的IP地址，方便用工具连接

- 查看ip：ip add

- 修改文件：vi /etc/sysconfig/network-scripts/ifcfg-ens33
- 更改文件：ONBOOT = yes
- 重启网络服务：sudo service network restart 

### 2、上传文件到Linux，/home/hacker/DB/

- 通过工具上传jdk、hadoop
- 解压tgz

  - tar -xvf ../software/jdk-8u181-linux-x64.tar.gz
  - tar -xvf ../software/hadoop-2.6.0-cdh5.15.0.tar.gz
- 创建目录
  - mkdir -p /xxx/yy 若不加 -p，且原本 xxx目录不存在，则产生错误。

### 3、添加环境变量
  - 修改配置文件：vi /etc/profile
    - JAVA_HOME=/home/hacker/DB/jdk1.8.0_181
    - export PATH=$PATH:$JAVA_HOME/bin
    - HADOOP_HOME=/home/hacker/DB/hadoop-2.6.0-cdh5.15.0
    - export PATH=$PATH:$HADOOP_HOME/bin
  - 通过source命令重新加载/etc/profile文件
    - source /etc/profile

### 3、添加tomcat环境变量

```java
JAVA_OPTS="-Xms512m -Xmx1024m -Xss1024K -XX:PermSize=512m -XX:MaxPermSize=1024m"
export TOMCAT_HOME=/home/hacker/apache-tomcat-8.5.47
export CATALINA_HOME=/home/hacker/apache-tomcat-8.5.47
export JRE_HOME=/home/hacker/jdk1.8.0_181/jre
export JAVA_HOME=/home/hacker/jdk1.8.0_181

export CATALINA_HOME=/usr/local/apache-tomcat-8.5.47 
export CLASSPATH=.:$JAVA_HOME/lib:$CATALINA_HOME/lib
export PATH=$PATH:$CATALINA_HOME/bin
```

### 3、添加maven环境变量

```java
vi /etc/profile

将下面这两行代码拷贝到文件末尾并保存

MAVEN_HOME=/home/hacker/apache-maven-3.5.0
export PATH=${MAVEN_HOME}/bin:${PATH}
```




### 4.修改hadoop-env.sh

- 进入到文件目录：cd /home/hacker/DB/hadoop-2.6.0-cdh5.15.0/etc/hadoop/
- 修改文件：vi hadoop-env.sh   
  - /home/hacker/DB/jdk1.8.0_181
  - export JAVA_HOME=/home/hacker/DB/jdk1.8.0_181

### 5、IP映射（给IP起别名）

- 修改文件： sudo vi /etc/hosts
  -  192.168.1.143 hacker
  - 或者：hostnamectl set-hostname  主机名
- 在windows下修改ip映射
  - 找C盘——C:\Windows\System32\drivers\etc\hosts
  - 添加： 192.168.15.144  hacker

### 6、修改域名——防止bug

- 修改文件：sudo vi /etc/sysconfig/network
- HOSTNAME=hacker

### 5、开放对外的端口

- 查询指定端口是否已开
  -  firewall-cmd --query-port=8080/tcp
  - 提示 yes，表示开启；no表示未开启。

### 7、关闭安全校验+关闭防火墙（仅仅实验环境！！！生产环境不需要！！！）

- ctenos7查看查看防火墙的状态

  - firewall-cmd --state

- 开启防火墙

  - systemctl start firewalld

- 关闭防火墙

  - systemctl stop firewalld

- 禁止firewall开机启动

  - systemctl disable firewalld

- 设置firewall开机启动

  - systemctl enable firewalld

- 查看端口的开放情况：

  - firewall-cmd --list-all

  ```java
  public (active)
    target: default
    icmp-block-inversion: no
    interfaces: ens33
    sources: 
    services: ssh dhcpv6-client
    ports: 80/tcp 
    protocols: 
    masquerade: no
    forward-ports: 
    source-ports: 
    icmp-blocks: 
    rich rules: 
  //ports: 80/tcp 表示开放的端口号
  //services: ssh dhcpv6-client 表示 ssh 服务是放行的
  ```

- 开放`http` `80` 端口：

  - firewall-cmd  --add-service=http  --permanent
  - firewall-cmd  --add-port=80/tcp --permanent
  - 命令末尾的`--permanent`表示用久有效，不加这句的话重启后刚才开放的端口就又失效了。
  - 重启防火墙：
    - firewall-cmd --reload

### 8、ssh免密登录(生产环境也需要操作)——集群内部节点免密操作

- 生成密钥
  - ssh-keygen -t rsa --> 只按回车键，不需要输入任何字符，密钥只需要生成一次
- 分发密钥
  - ssh-copy-id 用户名@IP地址
- ssh 用户名 --> 不需要输入密码 --> 验证

### 9、伪分布式--配置

#### 1、搭建伪分布式

- etc -- 配置文件，用于设置
- bin -- 执行一些通用的命令，eg : hadoop hdfs
- sbin -- 系统的管理命令（.cmd为命令）
- share -- doc（官方帮助文档）

#### 2、需要配置的文本

- core-site.xml 核心，入口
- httpfs-site.xml hdfs相关的配置
-  mapred-site.xml MapReduce
-  yarn-site.xml yarn资源管理器

#### 3、配置的基本语法

```xml
<properties>
<name> 配置选项</name>
<value>值</value>
<description>描述 注释</description>
<final>配置是只读，该选项在应用层无法修改</final>
</properties>
```

#### 4、部署简易伪分布式

##### 1、修改文件：vi core-site.xml

```xml
<configuration>
<!--hdfs入口,namenode节点。应用层请求都是通过这个节点
namenode http协议 50070 通过浏览器访问端口
namenode rcp协议 9000(端口号) 编程通信的接口
-->
<property>
<name>fs.defaultFS</name>
<value>hdfs://hacker:9000</value>
</property>
<!-- 由于hadoop默认情况下metadata是存放在/tmp目录下，这个目录不适合存放metadata --> 
<!--元数据位置重新定向-->
<property>
<name>hadoop.tmp.dir</name>
<value>/home/hacker/DB/hadoop-2.6.0-cdh5.15.0/metadata</value>
</property>
    
</configuration>
```

##### 2、修改文件：vi hdfs-site.xml

```xml
<configuration>
<!--副本数量，伪分布式 只需要一个就行了-->
<property>
<name>dfs.replication</name>
<value>1</value> <!--副本个数-->
</property>
<!-- 如果secondaryname开启，那么sn节点是一定不能和namenode放到一个节点 -->
<property>
<name>dfs.namenode.secondary.http-address</name>
<value>hacker:50090</value>
</property>
<property>
<name>dfs.namenode.secondary.https-address</name>
<value>hacker:50091</value>
</property>
 <!--关闭用户的检查-->
<property>
<name>dfs.permissions.enabled</name>
<value>false</value>
</property>    
</configuration>
```

##### 3、修改文件：vi slaves

- 添加：本地ip设置的别名，删除localhost

##### 4、格式化

- ——[注]：格式化只能一次
  - hdfs namenode -format
- 成功标志

```xml
18/09/25 09:54:53 INFO common.Storage: Storage directory /tmp/hadoop-candle/dfs/name has been
successfully formatted.
```

##### 5、启动hdfs

- 切换到/hadoop根目录/sbin目录 ./start-dfs.sh

- 成功标志

  - 输入jps命令

  ```xml
  8913 DataNode
  9062 SecondaryNameNode
  8796 NameNode
  ```

##### 6、hdfs系统启动命令简介

```xml
hadoop-daemon.sh 主要启动或者关闭单一进程
hadoop-daemons.sh 如果是集群,那么就是启动或者关闭集群中所有的相同的进程
hadoop-daemon.sh start datanode 在namenode节点中启动
hadoop-daemons.sh start datanode 启动集群所有的datanode进程

./start-all.sh 启动所有 不推荐
./stop-all.sh 关闭所有
./start-dfs.sh 启动hdfs

./stop-dfs.sh 关闭hdfs
./start-yarn.sh yarn
./stop-yarn.sh
```

##### 7、尝试文件系统的下载和上传

```xml
hdfs dfs -mkdir -p /user/sue/mr 创建目录
hdfs dfs -put test.txt /user/sue/mr/ 上传本地文本 test.txt

HDFS shell命令
 ——没有cd命令 所有文件的路径都是绝对路径
hdfs dfs 用于执行hdfs shell命令
windows D:/xxx/xxx
linux路径 /xxx/xxx/xx
hdfs文件系统 /xxx/xx/xx
对于hdfs路径 使用RPC协议
String destPath = "hdfs://sue:9000/user/sue"
[注]：hdfs 默认"home"目录 /user/sue
```

##### 8、重启虚拟机

- 成功标志 [sue@sue sbin]

##### 9、重新对hdfs进行格式化

```xml
——由于我们更改了metadate路径
hdfs namenode -format 格式化
```

##### 10、启动hdfs

- sbin目录 ./start-dfs.sh







