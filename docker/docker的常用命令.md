

#### Docker的常用命令

##### 帮助命令

-----

```shell
docker version		 # 显示docker的版本信息
docker info			 # 显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help	# 万能命令

```

帮助文档的地址：https://docs.docker.com/engine/reference/commandline/docker/



##### 镜像命令

-----

###### docker images		查看所有本地的主机上的镜像

```shell
[root@VM_0_10_centos ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              bf756fb1ae65        10 months ago       13.3kB

# 解释
REPOSITORY	镜像的仓库源
TAG			镜像的标签
IMAGE ID	镜像的id
CREATED		镜像的创建时间
SIZE		镜像的大小

# 可选项
  -a, --all             # 列出所有的镜像
  -q, --quiet           # 只显示镜像的id
  
```

----

###### docker search 搜索镜像

```shell
[root@VM_0_10_centos ~]# docker search mysql
NAME                              DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   10115               [OK]                
mariadb                           MariaDB is a community-developed fork of MyS…   3715                [OK]                

# 可选项，通过搜索来过滤
 -filter=STARS=3000		# 搜索出来的神经性就是STARS大于3000的
```

----

###### docker pull 下载镜像

```shell
# 下载镜像 docker pull mysql
[root@VM_0_10_centos ~]# docker pull mysql
Using default tag: latest	# 如果不写 tag，默认就是 latest
latest: Pulling from library/mysql	
bb79b6b2107f: Pull complete 	# 分层下载，docker image的核心 联合文件系统
49e22f6fb9f7: Pull complete 
842b1255668c: Pull complete 
9f48d1f43000: Pull complete 
c693f0615bce: Pull complete 
8a621b9dbed2: Pull complete 
0807d32aef13: Pull complete 
a56aca0feb17: Pull complete 
de9d45fd0f07: Pull complete 
1d68a49161cc: Pull complete 
d16d318b774e: Pull complete 
49e112c55976: Pull complete 
Digest: sha256:8c17271df53ee3b843d6e16d46cff13f22c9c04d6982eb15a9a47bd5c9ac7e2d	# 签名
Status: Downloaded newer image for mysql:latest	
docker.io/library/mysql:latest	# 真实地址

# 等价于它
docker pull mysql
docker pull docker.io/library/mysql:latest

# 指定版本下载
docker pull mysql:5.7
```

----

###### docker rmi	删除镜像

```shell
[root@VM_0_10_centos ~]# docker rmi -f 镜像id		# 删除指定的镜像
[root@VM_0_10_centos ~]# docker rmi -f 镜像id 镜像id	#删除多个镜像
[root@VM_0_10_centos ~]# docker rmi -f $(docker images -aq)		#删除全部的镜像
```



##### 容器命令

------

说明：我们有了镜像才可以创建容器，linux，下载一个centos镜像。

```shell
docker pull centos
```

###### 新建容器并启动

```shell
docker run [可选参数] image

# 参数说明
--name="Name"	容器名字	tomcat01	tomcat02，用来区分容器
-d				后台运行
-it				使用交互方式运行，进入容器查看内容
-p				指定容器的端口	-p 8080:8080
	-p	主机端口:容器端口（常用）
	-p	容器端口
-P				随机指定端口


# 测试、启动并进入容器
[root@VM_0_10_centos ~]# docker run -it centos /bin/bash
[root@f30048161779 /]# ls	# 查看容器内的centos，基础版本，很多命令都是不完善的

# 从容器中退回主机
[root@f30048161779 /]# exit
exit
[root@VM_0_10_centos ~]# 
```

###### 列出所有的运行的容器

```shell
# docker ps 命令
    	 # 列出当前正在运行的容器
-a  	 # 列出当前正在运行的容器+带出历史过的容器
-n=？	# 显示最近创建的容器
-q		 # 只显示容器的编号

```

###### 退出容器

```shell
exit	# 直接容器停止并退出
Ctrl + P + Q # 容器不停止退出 ,（注意大写模式）
```

###### 删除容器

```shell
docker rm 容器id					# 删除指定的容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)	  # 删除所有的容器
docker ps -a -q|xargs docker rm   # 删除所有的容器
```

###### 启动和停止容器的操作

```shell
docker start 容器id		# 启动容器
docker restart 容器id		# 重启容器
docker stop	容器id		# 停止当前正在运行的容器
docker kill 容器id		# 强制停止当前的容器
```



##### 常用的其他命令

-----

###### 后台启动容器

```shell
# 命令 docker run -d 镜像名！
[root@VM_0_10_centos ~]# docker run -d centos

# 问题docker ps ，发现 centos停止了

# 常见的坑，docker 容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，没有程序了
```

###### 查看日志

```shell
docker logs

[root@VM_0_10_centos ~]# docker logs -f -t --tail 10 容器id 

# 显示日志
-tf				# 显示日志
--tail number 	# 要显示日志条数
[root@VM_0_10_centos ~]# docker logs -f -t --tail 10 1013f524e9b0 
```

###### 查看容器中进程信息 ps

```shell
docker top 容器id
[root@VM_0_10_centos ~]# docker top $(docker ps -q) 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                26172               26156               0                   17:13               pts/0               00:00:00            /bin/bash
```

###### 查看镜像的元数据

```shell
# 命令
docker inspect 容器id

# 测试
[root@VM_0_10_centos ~]# docker inspect 1013f524e9b0
[
    {
        "Id": "1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24",
        "Created": "2020-10-30T09:06:06.569681247Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 26172,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-10-30T09:13:45.176186653Z",
            "FinishedAt": "2020-10-30T09:12:20.694461637Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24/hostname",
        "HostsPath": "/var/lib/docker/containers/1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24/hosts",
        "LogPath": "/var/lib/docker/containers/1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24/1013f524e9b0152ba07a7242cef6c201fb043459b47dfbf2dcd5ca65476d2e24-json.log",
        "Name": "/laughing_shaw",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/86a8f6cb1fbccc967db583d8258a70286ee4db894bdb64b9c9fffe7fc5eb7b63-init/diff:/var/lib/docker/overlay2/77e22a3af6c43fac31f9cb09bb5a74a4c0bd52de9f69122080379a8b98590e96/diff",
                "MergedDir": "/var/lib/docker/overlay2/86a8f6cb1fbccc967db583d8258a70286ee4db894bdb64b9c9fffe7fc5eb7b63/merged",
                "UpperDir": "/var/lib/docker/overlay2/86a8f6cb1fbccc967db583d8258a70286ee4db894bdb64b9c9fffe7fc5eb7b63/diff",
                "WorkDir": "/var/lib/docker/overlay2/86a8f6cb1fbccc967db583d8258a70286ee4db894bdb64b9c9fffe7fc5eb7b63/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "1013f524e9b0",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "bcaabd31d1b0e3d4073029f7696e96979c2d2124da275d22b61fd162a259f2ef",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/bcaabd31d1b0",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "a3f12a069e7522f71cc0e2c18e2485495fe2f3d5ccdd96cab448e103a2c89291",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "c5dc6117b9866848a13c7f238820dc43f7d90b71d1912c8a677cb69119548dcd",
                    "EndpointID": "a3f12a069e7522f71cc0e2c18e2485495fe2f3d5ccdd96cab448e103a2c89291",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

###### 进入当前正在运行的容器

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bashshell

# 测试
[root@VM_0_10_centos ~]# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
1013f524e9b0        centos              "/bin/bash"         12 minutes ago      Up 5 minutes                            laughing_shaw
[root@VM_0_10_centos ~]# docker exec -it 1013f524e9b0 /bin/bash
[root@1013f524e9b0 /]# ll
bash: ll: command not found
[root@1013f524e9b0 /]# ls 
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@1013f524e9b0 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 09:13 pts/0    00:00:00 /bin/bash
root        14     0  0 09:19 pts/1    00:00:00 /bin/bash
root        29    14  0 09:19 pts/1    00:00:00 ps -ef

# 方式二
docker attach 容器id

# 测试
[root@VM_0_10_centos ~]# docker attach 7cda923eff6c
hahahah
hahahah
......
(当前正在执行的代码)

# docker exec		# 进入容器后开启一个新的终端，可以在里面操作（常用）
# docker attach		# 进入容器正在执行的终端，不会启动新的进程！
```

###### 从容器内拷贝文件到主机上

```shell
docker cp 容器id:容器内路径 目的的主机路径

# 测试 将文件拷贝出来到主机上
[root@VM_0_10_centos opt]# docker cp 096dab144b3e:/opt/test/t.txt /opt
[root@VM_0_10_centos opt]# ls
community-mysql-script  containerd  kafka_2.12-2.3.0  kafka_2.12-2.3.0.tgz  project  rh  test  t.txt
[root@VM_0_10_centos opt]# cat t.txt 
sdjlasdjasdlsadjlas:wq

[root@VM_0_10_centos opt]# 

# 拷贝是一个手动过程，未来我们使用 -v 卷的技术，可以实现，自动同步
```

