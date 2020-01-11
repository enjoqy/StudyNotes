# Centos7常用命令集合

## 1、批量创建java环境的centos服务器

```java
vi /etc/sysconfig/network-scripts/ifcfg-ens33
yum update
yum -y install net-tools libevent gcc unzip zip
yum install -y wget gcc-c++ zlib-devel perl-ExtUtils-MakeMaker
cd /home/halk/
tar -zxvf apache-maven-3.5.0-bin.tar.gz
tar -zxvf apache-tomcat-8.5.47.tar.gz 
tar -zxvf jdk-8u181-linux-x64.tar.gz 
tar -zxvf git-2.9.3.tar.gz 
mv git-2.9.3/ /usr/local/git-2.9.3
mv apache-maven-3.5.0  /usr/local/maven-3.5.0
mv apache-tomcat-8.5.47 /usr/local/tomcat-8.5.47
mv jdk1.8.0_181/ /usr/local/jdk1.8.0_181
vim /etc/profile
# java
JAVA_HOME=/usr/local/jdk1.8.0_181
export PATH=$PATH:$JAVA_HOME/bin
# maven
MAVEN_HOME=/usr/local/maven-3.5.0
export PATH=${MAVEN_HOME}/bin:${PATH}
source /etc/profile
cd /usr/local/git-2.9.3
./configure --prefix=/usr/local
make
make install
firewall-cmd --state
systemctl start firewalld
systemctl enable firewalld
firewall-cmd --list-all
firewall-cmd --add-service=http --permanent
firewall-cmd  --add-port=80/tcp --permanent
firewall-cmd --reload

```

## 2

## 3

## 4

## 5、centos相关命令

### 1、Centos解除端口占用

```java
- 查看所有端口占用
- netstat -ntlp
- 查看端口被哪个进程占用
- lsof -i:端口号
- 杀死被占用端口
- kill -9 端口号
```

### 2、Linux不能执行netstat命令的原因及解决办法

```java
yum install net-tools
```

### 3、重启sshd服务

```java
查看状态：
systemctl status sshd.service

启动服务：
systemctl start sshd.service

重启服务：
systemctl restart sshd.service

开机自启：
systemctl enable sshd.service
```

### 4、linux上的gitblit在后台运行

```java
sudo java -jar gitblit.jar --baseFolder data(crt之后就会结束进程)
nohup java -jar gitblit.jar --baseFolder data &(crt之后进程也不会结束，会在后台运行)
```

### 5、查看命令的具体用法(不知道命令就找男人man)

```java
man -ls  查看ls命令的相关详细信息
man -ps  查看ps命令的相关详细信息
```

### 6、centos常用指令

- date：日期
- cal：日历
  - cal [month] [year]
- ls：显示文件夹
  - ls -l：显示所有的（不含隐藏的），简写为：ll
  - ls -al: 显示所有的，包括隐藏的
- bc: 计算器
  - 加 +、减—、乘*、除/、指数^、余数%
  - quit：退出计算器
  - bc预设仅输出整数，如果要输出小数点下位数，那么就必须要执行 scale=number ，number 就是小数点后几位

### 7、解决端口占用

```java
查看指定服务端口
lsof -i :80
kill -9 17665

查看指定服务端口
netstat -ap | grep 80
kill -9 17665
```

### 8、修改时区

- 查询服务器时间
  - timedatectl
- 修改时区为Asia/Shanghai
  - timedatectl  set-timezone Asia/Shanghai

### 9、修改文件夹所属用户

- sudo chown  用户名  文件夹名

### 10、将目录下的所有文件与子目录皆设为任何人可读取、可写、可执行:

- sudo chmod -R a+rwx  /usr/local/tomcat/*

## 6、git相关命令

### 1、git生成新的SSH Key

```java
ssh-keygen -t rsa -C "xxxxxxx@163.com"
```

### 2、找到git的位置

```java
终端命令:which -a git
或者：whereis git
```

### 3、使用git命令放弃本地修改，让云端强制覆盖本地代码

- git pull 强制覆盖本地的代码方式
  - git fetch --all
  - git reset --hard origin/master
  - git pull
- 如果你在其他分支上：
  - git fetch --all
  - git reset --hard origin/<branch_name>
  - git pull

```java
说明：

git fetch从远程下载最新的，而不尝试合并或rebase任何东西。

然后git reset将主分支重置为您刚刚获取的内容。 --hard选项更改工作树中的所有文件以匹配origin/master中的文件。
```

#### 4、git回退指定版本

```java
回退命令：

$ git reset --hard HEAD^ 回退到上个版本
$ git reset --hard HEAD~3 回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id 退到/进到 指定commit的sha码

强推到远程：

$ git push origin HEAD --force
```

##### 5、node-sass安装报错node-sass@4.12.0 postinstall: `node scripts/build.js`

```java
先 npm install -g cnpm --registry=https://registry.npm.taobao.org下载cnpm

然后用cnpm去安装 cnpm install node-sass --save

dev终于跑起来了
```





## 7

## 8

## 9

## 10

## 11

## 12、Ubuntu修改hosts文件

- 打开host文件
  - vim /etc/hosts
- 重启网络。
  - /etc/init.d/networking restart



##  15、查看tomcat日志

```java
1、先到tomcat的logs目录下我这边是：/usr/local/apache-tomcat-7.0.73/logs
2、tail -f catalina.out
3、这样，前端有请求时候，就会输出log

Ctrl+c 是退出tail命令。
```

## 16、待定

## 17、文件查看命令

```java
less：文件查看命令（推荐使用）
	空格查看下一行，方向键上下查看上一行或下一行，pgup上翻，pgdn下翻
	查找关键字 /+关键字  n往下翻
	
head：显示文件内容（正向）
	-n 指定显示的行数
	
tail：显示文件内容（反向）
	-n 指定显示的行数
	-f 动态显示文件末尾的内容

cat: 查看文件文件内容命令
	-n 带有行号
tac：倒着查看文件内容
	
more：文件查看命令
	f、空格可以翻页，b可以向上翻
	
```

## 18、创建文件命令

```java
touch: 创建文件命令
	示例：touch aa 创建文件aa
		touch aa bb 中间加空格表示创建两个文件，aa、bb
	
```

## 19、创建文件软连接

```java
ln: 创建文件软连接
	-s [原文件] [目标文件]
	示例：
		ln -s /etc/issue /tmp/issue.soft
		创建文件/etc/issue的软连接/tmp/issue.soft
```

## 20、jar后台运行，指定端口

```java
sudo nohup java -jar xxx.jar --server.port=8080
```
## 21、安装xz解压缩工具

```java
下载地址：https://sourceforge.net/projects/lzmautils/
安装：
tar -xf xz-5.0.3.tar
cd xz-5.0.3
./configure
make
make install
使用：
xz -d xxxx.tar.xz
tar -xvf xxxx.tar
```

## 22、安装node

```java
下载、解压缩
配置环境变量：
export NODE_HOME=/usr/local/node-v12.14.1
export PATH=$PATH:$NODE_HOME/bin
```

## 23、安装postwoman

```java
https://github.com/liyasthomas/postwoman.git
npm install
npm run dev
```

