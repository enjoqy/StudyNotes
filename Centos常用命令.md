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

## 2.Windows相关命令

### 1.查看端口占用，并解除

```shell
查看所有的端口使用情况
netstat -aon

查找指定端口
netstat -ano|findstr "80"

根据pid结束相关端口的进程
taskkill -f /pid 20732

```



## 3、搭建redis集群

安装redis，推荐使用最新版本，6.0之后的版本需要使用最新的gcc，可以使用如下命令：

1、安装centos-release-scl

```
sudo yum install centos-release-scl
```

2、安装devtoolset，注意，如果想安装7.*版本的，就改成devtoolset-7-gcc*，以此类推

```
sudo yum install devtoolset-8-gcc*
```

3、激活对应的devtoolset，所以你可以一次安装多个版本的devtoolset，需要的时候用下面这条命令切换到对应的版本

```
scl enable devtoolset-8 bash
```

大功告成，查看一下gcc版本

```
gcc -v

这条激活命令只对本次会话有效，重启会话后还是会变回原来的4.8.5版本,要想随意切换可   百度
```

redis集群至少需要**3个master节点**，此步骤是搭建3个master，每个master配1个slave节点，共计6个redis节点，全部在部署在一台机器上，使用不同的配置、不同的端口号，搭建伪集群模式

### 1、在/usr/local/下创建文件夹redis-cluster，然后创建6个文件夹

```tex
# mkdir -p /usr/local/redis-cluster
# mkdir 8001
# mkdir 8002
# mkdir 8003
# mkdir 8004
# mkdir 8005
# mkdir 8006
```

### 2、把redis.conf配置文件copy至8001文件夹下（其他的同理），并修改如下配置

- daemonize  yes
- port  8001 （分别对每个机器的端口号进行设置）
- bind  192.168.25.154  （绑定当前机器的ip，方便redis集群定位机器）
- dir  /usr/local/redis-cluster/8001/  （指定数据文件存放位置）
- cluster-enabled  yes  （启用集群模式）
- cluster-config-file  nodes-8001.conf  （这里800x最好和端口号port对应）
- cluster-node-timeout  5000   集群之间互相确认连接是否超时
- appendonly  yes  开启写入操作日志

### 3、每个文件夹下面的配置文件的端口号需要一一对应

可以用命令批量替换：:%s/源字符串/目的字符串/g

### 4、redis5.0之前的需要使用ruby命令，5.0之后的不需要此步骤

```tex
yum install ruby
yum install rubygems
gem install redis --version 3.0.0  (安装redis和ruby的接口)
```

### 5、分别启动6个redis实例，然后检查是否启动成功

```tex
/usr/local/redis-6.0.0/src/redis-server  /usr/local/redis-cluster/800*/redis.conf

ps -ef | grep redis
```

### 6、5.0之前的版本使用redis-trib.rb启用集群

```tex
./redis-trib.rb create --replicase 1 192.168.25.157:8001 192.168.25.157:8002 192.168.25.157:8003 192.168.25.157:8004 192.168.25.157:8005 192.168.25.157:8006

1表示 主节点个数/从节点个数=1 
```

5.0之后的版本可以这么干

```tex
redis-cli --cluster create 192.168.25.157:8001 192.168.25.157:8002 192.168.25.157:8003 192.168.25.157:8004 192.168.25.157:8005 192.168.25.157:8006 --cluster-replicas 1

1表示 主节点个数/从节点个数=1 
```

### 7、验证集群

- 客户端连接命令： 
  - redis-cli -c -h 192.168.25.154  -p 8001
  - -c  表示集群模式
- 使用命令查看集群相关节点信息：
  - info replication 
  - cluster info  查看集群信息
  - cluster nodes  查看节点列表
- 关闭集群需要逐个关闭：
  - ./redis-cli   -c  -h 192.168.25.164 -p 8001 shutdown
- 当集群无法启动时，删除临时的数据文件，再次重新启动每一个redis服务，然后重新构造集群环境



## 4、

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
- chown leyou:leyou 文件名；  修改文件为乐优用户乐优组
- chmod 755 文件名； 修改文件权限

### 10、将目录下的所有文件与子目录皆设为任何人可读取、可写、可执行:

- sudo chmod -R a+rwx  /usr/local/tomcat/*

### 11.查看进程命令

- ps -ef | grep

```java
ps命令将某个进程显示出来
grep命令是查找
中间的|是管道命令 是指ps命令与grep同时执行
PS是LINUX下最常用的也是非常强大的进程查看命令
grep命令是查找，是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。
grep全称是Global Regular Expression Print，表示全局正则表达式版本，它的使用权限是所有用户。

以下这条命令是检查java 进程是否存在：ps -ef |grep java
字段含义如下：
UID       PID       PPID      C     STIME    TTY       TIME         CMD

zzw      14124   13991      0     00:38      pts/0      00:00:00    grep --color=auto dae


UID      ：程序被该 UID 所拥有
PID      ：就是这个程序的 ID 
PPID    ：则是其上级父程序的ID
C          ：CPU使用的资源百分比
STIME ：系统启动时间
TTY     ：登入者的终端机位置
TIME   ：使用掉的CPU时间。
CMD   ：所下达的是什么指令
```

### 12.查看内存使用和磁盘空间使用情况

- 查看内存使用情况
  - free -h
- 查看磁盘空间使用情况
  - df -h

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

### 4、git回退指定版本

```java
回退命令：

$ git reset --hard HEAD^ 回退到上个版本
$ git reset --hard HEAD~3 回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id 退到/进到 指定commit的sha码

强推到远程：

$ git push origin HEAD --force
```

### 5、node-sass安装报错node-sass@4.12.0 postinstall: `node scripts/build.js`

```java
先 npm install -g cnpm --registry=https://registry.npm.taobao.org下载cnpm

然后用cnpm去安装 cnpm install node-sass --save

dev终于跑起来了
```

### 6、git知识梳理

#### 1、git专用名词

- Workspace：工作区

- Index / Stage：暂存区

- Repository：仓库区（或本地仓库）

- Remote：远程仓库

```java
本地分支关联远程：git branch --set-upstream-to=origin/beta beta
```

####   2、新建代码库

```tex
# 在当前目录新建一个Git代码库
$ git init
# 新建一个目录，将其初始化为Git代码库
$ git init [project-name]
# 下载一个项目和它的整个代码历史
$ git clone [url]
```

#### 3、配置

Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

```tex
# 显示当前的Git配置
$ git config --list
# 编辑Git配置文件
$ git config -e [--global]
# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

#### 4、增加/删除文件

```tex
# 添加指定文件到暂存区
$ git add [file1] [file2] ...
# 添加指定目录到暂存区，包括子目录
$ git add [dir]
# 添加当前目录的所有文件到暂存区
$ git add .
# 对于同一个文件的多处变化，可以实现分次提交
$ git add -p
# 删除工作区文件，并且将这次删除放入暂存区
$ git rm [file1] [file2] ...
# 停止追踪指定文件，但该文件会保留在工作区
$ git rm --cached [file]
# 改名文件，并且将这个改名放入暂存区
$ git mv [file-original] [file-renamed]
```

#### 5、代码提交

```tex
# 提交暂存区到仓库区
$ git commit -m [message]
# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]
# 提交工作区自上次commit之后的变化，直接到仓库区
$ git commit -a
# 提交时显示所有diff信息
$ git commit -v
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]
# 重做上一次commit，并包括指定文件的新变化
$ git commit --amend [file1] [file2] ...
```

#### 6、分支

```tex
# 列出所有本地分支
$ git branch
# 列出所有远程分支
$ git branch -r
# 列出所有本地分支和远程分支
$ git branch -a
# 新建一个分支，但依然停留在当前分支
$ git branch [branch-name]
# 新建一个分支，并切换到该分支
$ git checkout -b [branch]
# 新建一个分支，指向指定commit
$ git branch [branch] [commit]
# 新建一个分支，与指定的远程分支建立追踪关系
$ git branch --track [branch] [remote-branch]
# 切换到指定分支，并更新工作区
$ git checkout [branch-name]
# 切换到上一个分支
$ git checkout -
# 建立追踪关系，在现有分支与指定的远程分支之间
$ git branch --set-upstream [branch] [remote-branch]
# 合并指定分支到当前分支
$ git merge [branch]
# 选择一个commit，合并进当前分支
$ git cherry-pick [commit]
# 删除分支
$ git branch -d [branch-name]
# 删除远程分支
$ git push origin --delete [branch-name]
$ git branch -dr [remote/branch]
```

#### 7、标签

```tex
# 列出所有tag
$ git tag
# 新建一个tag在当前commit
$ git tag [tag]
# 新建一个tag在指定commit
$ git tag [tag] [commit]
# 删除本地tag
$ git tag -d [tag]
# 删除远程tag
$ git push origin :refs/tags/[tagName]
# 查看tag信息
$ git show [tag]
# 提交指定tag
$ git push [remote] [tag]
# 提交所有tag
$ git push [remote] --tags
# 新建一个分支，指向某个tag
$ git checkout -b [branch] [tag]
```

#### 8、查看信息

```tex
# 显示有变更的文件
$ git status
# 显示当前分支的版本历史
$ git log
# 显示commit历史，以及每次commit发生变更的文件
$ git log --stat
# 搜索提交历史，根据关键词
$ git log -S [keyword]
# 显示某个commit之后的所有变动，每个commit占据一行
$ git log [tag] HEAD --pretty=format:%s
# 显示某个commit之后的所有变动，其"提交说明"必须符合搜索条件
$ git log [tag] HEAD --grep feature
# 显示某个文件的版本历史，包括文件改名
$ git log --follow [file]
$ git whatchanged [file]
# 显示指定文件相关的每一次diff
$ git log -p [file]
# 显示过去5次提交
$ git log -5 --pretty --oneline
# 显示所有提交过的用户，按提交次数排序
$ git shortlog -sn
# 显示指定文件是什么人在什么时间修改过
$ git blame [file]
# 显示暂存区和工作区的差异
$ git diff
# 显示暂存区和上一个commit的差异
$ git diff --cached [file]
# 显示工作区与当前分支最新commit之间的差异
$ git diff HEAD
# 显示两次提交之间的差异
$ git diff [first-branch]...[second-branch]
# 显示今天你写了多少行代码
$ git diff --shortstat "@{0 day ago}"
# 显示某次提交的元数据和内容变化
$ git show [commit]
# 显示某次提交发生变化的文件
$ git show --name-only [commit]
# 显示某次提交时，某个文件的内容
$ git show [commit]:[filename]
# 显示当前分支的最近几次提交
$ git reflog
```

#### 9、远程同步

```tex
# 下载远程仓库的所有变动
$ git fetch [remote]
# 显示所有远程仓库
$ git remote -v
# 显示某个远程仓库的信息
$ git remote show [remote]
# 增加一个新的远程仓库，并命名
$ git remote add [shortname] [url]
# 取回远程仓库的变化，并与本地分支合并
$ git pull [remote] [branch]
# 上传本地指定分支到远程仓库
$ git push [remote] [branch]
# 强行推送当前分支到远程仓库，即使有冲突
$ git push [remote] --force
# 推送所有分支到远程仓库
$ git push [remote] --all
```

#### 10、撤销

```tex
# 恢复暂存区的指定文件到工作区
$ git checkout [file]
# 恢复某个commit的指定文件到暂存区和工作区
$ git checkout [commit] [file]
# 恢复暂存区的所有文件到工作区
$ git checkout .
# 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
$ git reset [file]
# 重置暂存区与工作区，与上一次commit保持一致
$ git reset --hard
# 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
$ git reset [commit]
# 仅仅是撤回commit操作，您写的代码仍然保留。
$ git reset --soft HEAD^
# 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
$ git reset --hard [commit]
# 重置当前HEAD为指定commit，但保持暂存区和工作区不变
$ git reset --keep [commit]
# 新建一个commit，用来撤销指定commit
# 后者的所有变化都将被前者抵消，并且应用到当前分支
$ git revert [commit]
# 暂时将未提交的变化移除，稍后再移入
$ git stash
$ git stash pop
```

#### 11、其他

```tex
# 生成一个可供发布的压缩包
$ git archive
```



## 7.npm相关命令

### 1.安装通过cnpm安装控制台无反应，可通过更改npm url来实现安装，直接更换更换成淘宝的源

- npm config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org/)，来替代cnpm的安装
- 配置后可通过下面方式来验证是否成功
  - npm config get registry

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

## 16

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

