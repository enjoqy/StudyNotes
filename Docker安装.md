# Docker安装

## 1、安装步骤

- Centos6.8安装步骤
  - yum install -y epel-release
  - yum install -y docker-io
  - 安装后的配置文件：/etc/sysconfig/docker
  - 启动Docker后台服务：service docker start
  - docker version验证
- Centos7安装步骤
  - yum  install  -y  yum-utils  device-mapper-persistent-data  lvm2
  - yum-config-manager  --add-repo  https://download.docker.com/linux/centos/docker-ce.repo
  - yum  install  docker-ce  docker-ce-cli  containerd.io
  - systemctl  start  docker
  - docker  run  hello-world
- 使用阿里云镜像加速配置
  - Centos6.8操作
    - /etc/sysconfig/docker
    - 修改：other_args="--registry-mirror=https://你自己的账号加速信息.mirror.aliyuncs.com"
    - service restart docker
    - 检验：ps -ef| grep docker
  - Centos7操作
    - 您可以通过修改daemon配置文件/etc/docker/daemon.json
    - {
      "registry-mirrors": ["https://s30ey3iq.mirror.aliyuncs.com"]
      }
    - systemctl daemon-reload
    - systemctl restart docker
    - 检验：ps -ef| grep docker

## 2、Docker命令

- docker --version  查看版本
- docker version  查看版本详细信息
- docker info  查看docker相关的详细信息
- docker --help  查看帮助信息
- docker images  列出本机所有的镜像
  - -a  列出本地所有镜像（含中间映像层）
  - -q  只显示镜像ID
  - --digests  显示镜像的摘要信息
  - --no-trunc  显示完整的镜像信息
- docker search  tomcat  到dockerhub上搜索镜像
  - --no-trunc  显示完整的镜像描述
  - -s  列出收藏数不小于指定值的镜像
  - --automated  只列出自动构建类型的镜像
- docker pull  tomcat  拉取tomcat镜像
- docker rmi  tomcat  删除tomcat镜像
  - -f  强制删除镜像
  - $(docker  images  -qa)  删除本地所有的镜像，清库
- docker  run
  - -it  镜像名称   以交互模式运行一个镜像
  - -d  镜像名称   ：在后运行一个镜像（后台模式）
    - docker  run  -d  centos  /bin/sh  -c  "while true; do echo hello world; sleep 5; done"
      - 后台运行centos容器，并且每隔5秒打印一次hello world
- docker  logs  -f  -t  --tail  容器ID  ：查看容器日志（-t是加入时间戳，-f跟随最新的日志打印，--tail数字显示最后多少条）
- docker  ps  查看当前正在运行的镜像容器
  - -a  列出当前所有正在运行的容器+历史上运行的
  - -l  显示最近创建的容器
  - -n  显示最近n个创建的容器
  - -q  静默模式，只显示容器编号
  - --no-trunc  不截断输出
- docker  inspect  容器ID  ：以json字符串的方式查看容器内部细节
- docker  top  容器ID  ：查看容器运行的进程
- docker  exec -it  容器ID baseShell  ：重新进入容器打开新的终端，并且可以启动新的线程
- docker  attach  容器ID  ：直接进入容器已经启动的终端，不会启动新的进程
- 退出容器
  - 容器停止退出
    - exit  
    - exit  -f  强制退出
  - 容器不停止退出
    - ctrl + P + Q
- docker  start  镜像名称  ：启动容器
- docker  restart  镜像名称  ：重启容器
- docker  stop  镜像名称  ：停止容器
- docker  kill  镜像名称  ：强制停止容器

## 3、Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?解决办法

```java
systemctl daemon-reload

systemctl restart docker.service
```

























