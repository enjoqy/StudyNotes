# Centos7常用命令集合

## 1、重启sshd服务

```
查看状态：
systemctl status sshd.service

启动服务：
systemctl start sshd.service

重启服务：
systemctl restart sshd.service

开机自启：
systemctl enable sshd.service
```

## 2、Linux不能执行netstat命令的原因及解决办法

- yum install net-tools

## 3、Centos解除端口占用

```
- 查看所有端口占用
- netstat -tln
- 查看端口被哪个进程占用
- lsof -i:端口号
- 杀死被占用端口
- kill 端口号
```

## 4、linux上的gitblit在后台运行

- sudo java -jar gitblit.jar --baseFolder data(crt之后就会结束进程)
- nohup java -jar gitblit.jar --baseFolder data &(crt之后进程也不会结束，会在后台运行)

## 5、找到git的位置

```java
终端命令:which -a git
或者：whereis git
```

## 6、git生成新的SSH Key

```java
ssh-keygen -t rsa -C "xxxxxxx@163.com"
```

## 7、查看具体命令的详细信息

- man -ls  查看ls命令的相关详细信息
- man -ps  查看ps命令的相关详细信息

## 8、指令

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

## 9、修改时区

- 查询服务器时间
  - timedatectl
- 修改时区为Asia/Shanghai
  - timedatectl  set-timezone Asia/Shanghai

## 10、使用git命令放弃本地修改，让云端强制覆盖本地代码

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

## 11、修改文件夹所属用户

- sudo chown  用户名  文件夹名

## 12、Ubuntu修改hosts文件

- 打开host文件
  - vim /etc/hosts
- 重启网络。
  - /etc/init.d/networking restart

## 13、将目录下的所有文件与子目录皆设为任何人可读取、可写、可执行:

- sudo chmod -R a+rwx  /usr/local/tomcat/*

## 14、解决端口占用

- ```lsof -i :80css
  查看指定服务端口
  lsof -i :80
  kill -9 17665
  
  查看指定服务端口
  netstat -ap | grep 80
  kill -9 17665
  ```

##  15、查看tomcat日志

```java
1、先到tomcat的logs目录下我这边是：/usr/local/apache-tomcat-7.0.73/logs
2、tail -f catalina.out
3、这样，前端有请求时候，就会输出log

Ctrl+c 是退出tail命令。
```

## 16、git回退指定版本

```git
回退命令：

$ git reset --hard HEAD^ 回退到上个版本
$ git reset --hard HEAD~3 回退到前3次提交之前，以此类推，回退到n次提交之前
$ git reset --hard commit_id 退到/进到 指定commit的sha码

强推到远程：

$ git push origin HEAD --force
```

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

