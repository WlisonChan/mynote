## Docker安装

#### Docker的基本组成

**镜像（image）：**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务。通过一个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）

**容器（container）：**

docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。

启动，停止，删除，基本命令！

目前就可以把这个容器理解为就是一个简易的linux系统

**仓库（repository）：**

仓库就是存放镜像的地方！

仓库分为公有仓库和私有仓库！



#### 安装Docker

----

>环境准备

1、CentOS 7

2、远程服务器交互 xshell

> 环境查看

```shell
# 系统内核是3.10 以上
[root@VM_0_10_centos ~]# uname -r
3.10.0-862.el7.x86_64
```

```shell
#系统版本
[root@VM_0_10_centos ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

>安装

帮助文档

```shell
# 1、卸载旧的版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

# 2、需要的安装包
yum install -y yum-utils

# 3、设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo #默认是从国外的！
    
yum-config-manager \
    --add-repo \
	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
	
# 更新yum软件包索引
[root@VM_0_10_centos ~]# yum makecache fast

# 4、安装docker相关 docker-ce 社区 ee 企业版
yum install docker-ce docker-ce-cli containerd.io

# 5、启动docker
systemctl start docker

# 6、使用命令查看启动情况
docker version

# 7、 hello-world
docker run hello-world

# 8、查看一下下载的这个hello-world镜像
docker images

[root@VM_0_10_centos ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        10 months ago       13.3kB

```

了解：卸载docker命令

```shell
# 1、卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

# 2、删除资源
rm -rf /var/lib/docker

# /var/lib/docker	docker的默认工作路径
```



#### 阿里云镜像加速

----

