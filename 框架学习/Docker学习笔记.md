# Docker学习笔记

## 一、Docker概述

### 1.1、Docker

官网：[https://www.docker.com/](https://www.docker.com/) 

文档地址：[https://docs.docker.com/](https://docs.docker.com/)  文档超级详细！

仓库地址：[https://www.docker.com/products/docker-hub/](https://www.docker.com/products/docker-hub/) 

## 二、Docker安装 

### 2.1、基本组成

**镜像（image）：**

docker镜像就好比是一个模板，可以通过这一个模板来创建容器服务，tomcat镜像====> run ====>tomcat1容器（提供服务），通过镜像可以创建多个容器（最终服务运行和项目运行就是在容器中的）。

**容器（container）：**

Docker利用容器技术，独立运行一个或者一组应用，它是通过来创建的。

启动，停止，删除，基本命令！

目前就可以把容器理解为就是一个简易的Linux系统

**仓库（repository）：**

仓库就是存放镜像的地方！

仓库分公有仓库和私有仓库

Docker Hub （默认是国外的）

阿里云......都有容器的服务

### 2.2、安装Docker

> 环境准备

1、会Linux基础

2、CentOS 7

3、使用XShell连接远程服务器

> 环境查看

```shell
# 系统内核是3.10以上的
uname -r

3.10.0-1160.53.1.el7.x86_64
```

```shell
# 系统版本
cat /etc/os-release

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

> 安装

帮助文档：

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
    https://download.docker.com/linux/centos/docker-ce.repo # 默认是国外的下载速度慢

yum-config-manager \
    --add-repo \
	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 推荐使用阿里云的
	
# 更新yum软件包索引
yum makecache fast
 
# 4、安装docker相关的内容 docker-ce 社区版 ee 企业版
yum install docker-ce docker-ce-cli containerd.io

# 5、启动docker
systemctl start docker

# 使用docker version 查看是否安装成功
```

![1648548095872](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281614871.png)

```shell
# 7、测试hello world
docker run hello-world
```

![1648548657552](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281614380.png)

```shell
# 8、查看一下下载的这个 hello-world 镜像
docker images
[root@chenstudy /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB
```

![1648548752522](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281615444.png)

卸载docker:

```shell
# 1、卸载依赖
yum remove docker-ce docker-ce-cli containerd.io

# 2、删除docker运行环境
 rm -rf /var/lib/docker
 rm -rf /var/lib/containerd
 # /var/lib/docker docker的默认资源路径
```

### 2.3、阿里云镜像加速

1、镜像加速器位置

![1648554047030](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281615286.png)

2、配置使用

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://jxnjyjvm.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```



## 三、Docker常用命令

### 3.1、帮助命令

```shell
docker version			# 显示docker的版本信息		
docker info				# 显示docker的系统信息，包括镜像和容器的数量
docker 命令 --help	   # 万能帮助命令
```

帮助文档地址：[https://docs.docker.com/reference/](https://docs.docker.com/reference/) 

### 3.2、镜像命令

**docker images**查看所有本地主机上的镜像

```shell
[root@chenstudy ~]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   6 months ago   13.3kB

# 解释
REPOSITORY 		镜像的仓库源
TAG 		    	镜像的标签
IMAGE ID			镜像的id
CREATED				镜像的创建时间
SIZE					镜像的大小

# 参数和选项
-a, --all               # 列出所有的镜像
-q, --quiet           	# 只显示镜像的id
```

**docker search镜像**

```shell
docker search mysql
NAME                             DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                            MySQL is a widely used, open-source relation…   12321     [OK]       
mariadb                          MariaDB Server is a high performing open sou…   4739      [OK]       

# 可选项
--filter=STARS=5000  # 搜索出来的镜像就是STARTS大于5000的
```

**docker pull下载镜像**

```shell
# 下载镜像 docker pull 镜像名 [:tag]
docker pull mysql
Using default tag: latest 			# 不写tag,默认就是latest
latest: Pulling from library/mysql
72a69066d2fe: Pull complete 		# 分层下载，docker images 的核心  联合文件系统
93619dbc5b36: Pull complete 
99da31dd6142: Pull complete 
626033c43d70: Pull complete 
37d5d7efb64e: Pull complete 
ac563158d721: Pull complete 
d2ba16033dad: Pull complete 
688ba7d5c01a: Pull complete 
00e060b6d11d: Pull complete 
1c04857f594f: Pull complete 
4d7cfa90e6ea: Pull complete 
e0431212d27d: Pull complete 
Digest: sha256:e9027fe4d91c0153429607251656806cc784e914937271037f7738bd5b8e7709 # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest	# 真实地址

# 等价于它
docker pull mysql
docker.io/library/mysql:latest

# 指定版本下载
docker pull mysql:5.7
5.7: Pulling from library/mysql
72a69066d2fe: Already exists 
93619dbc5b36: Already exists 
99da31dd6142: Already exists 
626033c43d70: Already exists 
37d5d7efb64e: Already exists 
ac563158d721: Already exists 
d2ba16033dad: Already exists 
0ceb82207cd7: Pull complete 
37f2405cae96: Pull complete 
e2482e017e53: Pull complete 
70deed891d42: Pull complete 
Digest: sha256:f2ad209efe9c67104167fc609cca6973c8422939491c9345270175a300419f94
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

![1648620820878](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1648620820878.png)

**docker rmi 删除镜像**

```shell
docker rmi -f 容器id # 删除指定的容器
docker rmi -f 容器id 容器id 容器id 容器id # 删除多个容器
docker rmi -f $(docker images -aq) # 删除全部的容器
```



### 3.3、容器命令

**说明：我们有了镜像才可以去创建容器，Linux， 下载一个centos镜像来学习**

```shell
docker pull centos
```

**新建容器并启动**

```shell
docker run [可选参数] image

# 参数说明
--name="Name"	容器名字，用来区分容器
-d			    后台方式运行
-it				使用交互方式运行，进入容器查看内容
-p				指定容器的端口 -p 8080:8080
	-p ip:主机的端口:容器端口
	-p 主机的端口:容器端口（常用）
	-p 容器端口
	容器端口
-P				随机指定端口

# 测试，启动并进入容器
[root@chenstudy ~]\# docker run -it centos /bin/bash   
[root@3834df926404 /]\# ls	# 查看容器内的centos
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# 退出命令
exit

```

**列出所有运行的容器**

```shell
# docker ps命令
	# 列出当前正在运行的容器
-a 	# 列出当前正在运行的容器+带出历史运行过的容器
-n=?	# 显示最近创建的容器
-q	# 只显示容器的编号

[root@chenstudy /]\# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@chenstudy /]\# docker ps -a
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                     PORTS     NAMES
3834df926404   centos         "/bin/bash"   5 minutes ago   Exited (0) 2 minutes ago             quizzical_tharp
9fada536b1fe   feb5d9fea6a5   "/hello"      26 hours ago    Exited (0) 26 hours ago              hopeful_hopper
[root@chenstudy /]\# 

```

**退出容器**

```shell
exit	# 直接容器停止并退出
Ctrl + P + Q # 容器不停止并退出
```



**删除容器**

```shell
docker rm 容器id					# 删除指定的容器
docker rm -f $(docker ps -aq)	   # 删除所有的容器
docker ps -a -q | xargs docker rm  # 删除所有的容器
```

**启动和停止容器**

```shell
docker start 容器id			# 启动容器
docker restart 容器id			# 重启容器
docker stop 容器id			# 停止当前正在运行的容器
docker kill 容器id			# 强制停止当前容器
```

### 3.4、常用的其他命令

**后台启动容器**

```shell
# 命令 docker run -d 镜像名
[root@chenstudy ~]\# docker run -d centos

# 问题 docker ps，发现centos停止了
# 常见的坑docker使用后台运行，就必须要有一个前台进程，就会自动停止
# nginx,容器启动后，发现自己没有提供服务，就会立刻停止，
```

**查看日志命令**

```shell
docker logs -f -t --tail n 容器没有日志

# 自己写一段shell脚本
docker run -d centos /bin/sh -c "while true; do echo chen; sleep 1; done"

[root@chenstudy ~]\# docker ps
CONTAINER ID   IMAGE
9b991f32cba8   centos

# 显示日志
-tf			   # 显示日志
--tail number	# 要显示日志条数
[root@chenstudy ~]\# docker logs -tf --tail 10 9b991f32cba8
```

**查看容器中的进程信息**

```shell
# 命令  docker top 容器id
[root@chenstudy ~]\# docker top 9b991f32cba8
UID                 PID                 PPID                C                   STIME      TTY 
root                315                 32010               0                   21:13       ? 
root                32010               31991               0                   21:02       ? 
```

**查看镜像的元数据**

```shell
# 命令 docker inspect 容器id
# 测试
[root@chenstudy ~]# docker inspect 9b991f32cba8
[
    {
        "Id": "9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050",
        "Created": "2022-03-30T13:02:15.650788989Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true; do echo chen; sleep 1; done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 32010,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-03-30T13:02:15.934493767Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050/hostname",
        "HostsPath": "/var/lib/docker/containers/9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050/hosts",
        "LogPath": "/var/lib/docker/containers/9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050/9b991f32cba8bd80c622ac7414a9b744744d10ca05f325f59837e2b7785d1050-json.log",
        "Name": "/quizzical_fermi",
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
            "CgroupnsMode": "host",
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
                "LowerDir": "/var/lib/docker/overlay2/299315b57b2c2edc53ab207384a51f4ddc579fb2ee5683b19c1c36c2d9099427-init/diff:/var/lib/docker/overlay2/bcd5233179f60c1c0cf1a53d8554e84df16fda95f2b16a76c3fb386a171722a3/diff",
                "MergedDir": "/var/lib/docker/overlay2/299315b57b2c2edc53ab207384a51f4ddc579fb2ee5683b19c1c36c2d9099427/merged",
                "UpperDir": "/var/lib/docker/overlay2/299315b57b2c2edc53ab207384a51f4ddc579fb2ee5683b19c1c36c2d9099427/diff",
                "WorkDir": "/var/lib/docker/overlay2/299315b57b2c2edc53ab207384a51f4ddc579fb2ee5683b19c1c36c2d9099427/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "9b991f32cba8",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "while true; do echo chen; sleep 1; done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "2ca6e7fe19f40dc335a4fd06fd72842c34a0c9fc2854d3ab7f4b0446318299ad",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/2ca6e7fe19f4",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "bc1147f1fbfad915997facf6043f6046b4ed20f8207ab72f5668c3145d1d1925",
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
                    "NetworkID": "d4cc38e384438f34beb0d1494aeb907bb75ba9cf2c1162f5f3f46cd479d019f5",
                    "EndpointID": "bc1147f1fbfad915997facf6043f6046b4ed20f8207ab72f5668c3145d1d1925",
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
[root@chenstudy ~]# 
```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台的方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bashShell

# 测试
[root@chenstudy ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED       STATUS       PORTS     NAMES
9b991f32cba8   centos    "/bin/sh -c 'while t…"   2 hours ago   Up 2 hours             quizzical_fermi
[root@chenstudy ~]# docker exec -it 9b991f32cba8 /bin/bash
[root@9b991f32cba8 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@9b991f32cba8 /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 13:02 ?        00:00:02 /bin/sh -c while true; do echo chen; sleep 1; done
root      7622     0  0 15:09 pts/0    00:00:00 /bin/bash
root      7666     1  0 15:09 ?        00:00:00 /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1
root      7667  7622  0 15:09 pts/0    00:00:00 ps -ef

# 方式二
docker attach 容器id
# 测试
docker attach 9b991f32cba8

# docker exec		# 进入容器后开启一个新的终端，可以在里面操作（常用）
# docker attach		# 进入容器正在执行的终端，不会启动新的进程！
```

**从容器内拷贝文件到主机上**

```shell
docker cp 容器id:容器内路径  目的主机路径
# 测试

# 查看当前主机目录下
[root@chenstudy ~]# cd /home
[root@chenstudy home]# ls
[root@chenstudy home]# touch chen.java
[root@chenstudy home]# ls
chen.java
[root@chenstudy home]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS         PORTS     NAMES
feb2ec189436   centos    "/bin/bash"   2 minutes ago   Up 2 minutes             musing_meitner
# 进入docker容器内部
[root@chenstudy home]# docker attach feb2ec189436
[root@feb2ec189436 /]# cd /home
[root@feb2ec189436 home]# ls
# 在容器内新建一个文件
[root@feb2ec189436 home]# touch test.java
[root@feb2ec189436 home]# ls
test.java
[root@feb2ec189436 home]# exit
exit
[root@chenstudy home]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@chenstudy home]# docker ps -a
CONTAINER ID   IMAGE     COMMAND          CREATED         STATUS                         
feb2ec189436   centos    "/bin/bash"      4 minutes ago   Exited (0) 15 seconds ago           
7cc7c544ee34   centos    "5d0da3dc9764"   4 minutes ago   Created 
# 将这个文件拷贝出来到主机上
[root@chenstudy home]# docker cp feb2ec189436:/home/test.java /home
[root@chenstudy home]# ls
chen.java  test.java
[root@chenstudy home]# 

# 拷贝是手动过程，未来我们使用 -v 卷的技术，可以实现，自动同步
```

### 小结

![img](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/202206281615890.png) 

### Docker安装Nginx和Tomcat

> Docker 安装Nginx 

```shell
# 1、搜索镜像
docker search nginx
# 2、下载镜像
docker pull nginx
# 3、启动镜像、测试
# -d 后台运行
# --name 给容器命名
# -p 宿主机端口:容器内部端口
[root@chenstudy ~]#  docker images
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        latest    605c77e624dd   3 months ago   141MB
centos       latest    5d0da3dc9764   6 months ago   231MB
[root@chenstudy ~]# docker run -d --name nignx01 -p 8888:80 nginx
9d219c6953482f900862b6cace896fd23c3902a731747ba9622b7455724d2ae4
[root@chenstudy ~]# docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED          STATUS          PORTS         NAMES
9d219c695348   nginx     "/docker-entrypoint.…"   44 seconds ago   Up 43 seconds   0.0.0.0:8888->80/tcp, :::8888->80/tcp   nignx01
[root@chenstudy ~]# curl localhost:8888

# 进入容器
[root@chenstudy ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS            
9d219c695348   nginx     "/docker-entrypoint.…"   8 minutes ago   Up 8 minutes 
PORTS 								  NAMES
0.0.0.0:8888->80/tcp, :::8888->80/tcp   nignx01
[root@chenstudy ~]# docker exec -it nignx01 /bin/bash
root@9d219c695348:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@9d219c695348:/# cd /etc/nginx
root@9d219c695348:/etc/nginx# ls
conf.d	fastcgi_params	mime.types  modules  nginx.conf  scgi_params  uwsgi_params
root@9d219c695348:/etc/nginx# 

```

> Docker 安装Tomcat

```shell
# 官方的使用
docker run -it --rm tomcat:9.0

# 我们之前的启动都是后台启动，停止了容器之后，容器还可以查到docker run -it --rm 一般用来测试

# 先下载再启动
docker pull tomacat:9.0

# 启动运行
docker run -d -p 8888:8080 --name tomcat01 tomcat
# 测试访问没有问题
[root@chenstudy ~]# docker run -d -p 8888:8080 --name tomcat01 tomcat
# 进入容器
a49fcebf9d45e2bc54225a431a09bd2af7120469dbe7fbc89073cf90a618fcfc
[root@chenstudy ~]# docker exec -it tomcat01 /bin/bash
# 发现问题：1、Linux命令少了，2、没有webapps。阿里云镜像的原因，默认是最小的镜像
# 保证最小的可运行环境
```

> 部署es+kibana

```shell
# es 暴露的端口有很多，es十分耗内存，es的数据一般需要放置到安全目录！挂载
# --net somenetwork		网络配置

# 启动 elasticsearch
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.17.1

# 启动就卡住了 docker status 查看 CPU 的使用情况

# 查看docker stats

# 测试es是否能够成功
[root@chenstudy ~]# clear
[root@chenstudy ~]# curl localhost:9200
{
  "name" : "eafdbc6f7434",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "_Be_dHgMQpqwll0t77WFiA",
  "version" : {
    "number" : "7.17.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "e5acb99f822233d62d6444ce45a4543dc1c8059a",
    "build_date" : "2022-02-23T22:20:54.153567231Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

![1648691599224](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1648691599224.png)

```shell
# 赶紧关闭，增加内存限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch02 -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xmx256m" elasticsearch:7.17.1
# 查看 docker stats
```

![1648704663603](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md202206281615162.png)

```shell
# 测试是否启动成功
[root@chenstudy ~]# curl localhost:9200 
{
  "name" : "9b8a00629df9",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "xNu8NLAtRsavS1uYJJVNhw",
  "version" : {
    "number" : "7.17.1",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "e5acb99f822233d62d6444ce45a4543dc1c8059a",
    "build_date" : "2022-02-23T22:20:54.153567231Z",
    "build_snapshot" : false,
    "lucene_version" : "8.11.1",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```



### 可视化

- portainer(先用这个)

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

- Rancher(CI/CD 再用)

**什么是 portainer ？**

Docker 图形化界面管理工具！提供一个后台界面供我们操作

```shell
docker run -d -p 8088:9000 \
--restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true portainer/portainer
```

访问测试：http://ip:8088

![1648705910441](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281617103.png)

选择挂载本地的：

![1648706067599](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/Md1648706067599.png)

登录之后的界面：

![1648706097895](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281617894.png)

## 四、Docker镜像

### 4.1、镜像是什么？

镜像是一种轻量级、可执行的独立软件包，用来打包运行环境和基于运行环境的开发软件，它包含运行某个软件运行所需的所有内容，包括代码、运行时、库、环境变量和配置文件

所有的应用，直接打包docker镜像，就可以直接跑起来！

**如何得到镜像：**

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个镜像DockerFile

### 4.2、Docker镜像加载原理

>  UnionFS（联合文件系统）

Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交，来一层层的叠加。
同时可以将不同目录，挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）。
Union文件系统是Docker镜像的基础。
镜像可以通过分层来进行继承，基于基础镜像（没有父镜像概念），可以制作各种具体的应用镜像。
总结：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

>  Docker镜像加载原理

Docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统UnionFS（联合文件系统）。

分为两个部分：

- bootfs（boot file system）：主要包含bootloader和kernel（Linux内核），bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，而在Docker镜像的最底层也是bootfs这一层，这与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后，整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

  即：系统启动时需要的引导加载，这个过程会需要一定时间。就是黑屏到开机之间的这么一个过程。电脑、虚拟机、Docker容器启动都需要的过程。在说回镜像，所以这一部分，无论是什么镜像都是公用的。

- rootfs（root file system）：rootfs在bootfs之上。包含的就是典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如Ubuntu，Centos等等。

  即：镜像启动之后的一个小的底层系统，这就是我们之前所说的，容器就是一个小的虚拟机环境，比如Ubuntu，Centos等，这个小的虚拟机环境就相当于rootfs。

  ![img](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281617112.png) 	

  > 平时我们安装进虚拟机的CentOS系统都是好几个G，为什么Docker这里才200M？ 

![1648707022796](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1648707022796.png)

对于一个精简的OS系统，rootfs可以很小，只需要包含最基本的命令、工具和程序库就可以了，因为底层直接用Host（宿主机）的kernel（也就是宿主机或者服务器的boosfs+内核），自己只需要提供rootfs就可以了。

由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs部分。

虚拟机是分钟级别的，容器是秒级

### 4.3、分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载日志的输出，可以看到是一层一层的再下载！

思考：为什么Docker镜像要采用这种分层的结构呢？

最大的好处，我觉得莫过于资源共享了！比如有多个镜像都从相同的Base镜像构建而来，那么宿主机只需在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层的方式可以通过docker image inspect 命令

```shell
{
	// ......
    "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:2edcec3590a4ec7f40cf0743c15d78fb39d8326bc029073b41ef9727da6c851f",
                "sha256:9b24afeb7c2f21e50a686ead025823cd2c6e9730c013ca77ad5f115c079b57cb",
                "sha256:4b8e2801e0f956a4220c32e2c8b0a590e6f9bd2420ec65453685246b82766ea1",
                "sha256:529cdb636f61e95ab91a62a51526a84fd7314d6aab0d414040796150b4522372",
                "sha256:9975392591f2777d6bf4d9919ad1b2c9afa12f9a9b4d260f45025ec3cc9b18ed",
                "sha256:8e5669d8329116b8444b9bbb1663dda568ede12d3dbcce950199b582f6e94952"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
}
```

理解：

所有的 Docker镜像都起始于一个基础镜像层，当进行修改或培加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于 Ubuntu Linux16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加 Python包，
就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创健第三个镜像层该像当前已经包含3个镜像层，如下图所示（这只是一个用于演示的很简单的例子）。

![img](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/Md202206281617243.png) 

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。下图中举了一个简单的例子，每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。 

![1](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1.png)

文种情況下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中

Docker通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统

Linux上可用的存储引撃有AUFS、 Overlay2、 Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于 Linux中对应的
件系统或者块设备技术，井且每种存储引擎都有其独有的性能特点。

Docker在 Windows上仅支持 windowsfilter 一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW [1]。

下图展示了与系统显示相同的三层镜像。所有镜像层堆并合井，对外提供统一的视图
![2](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618014.png)

> 特点

Docker 镜像都是只读的，当容器启动时，一个新的可写层加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

### 4.4、commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m="提交的描述信息" -a="作者" 容器id, 目标镜像名,[TAG]
```

实战测试：

```shell
# 1、启动一个默认的tomcat
docker run -it -p 8080:8080 tomcat
# 2、发现这个默认的tomcat是没有webapps应用，镜像的原因，官方的镜像默认webapps下面是没有文件的

# 3、自己拷贝进去基本的文件

# 4、将我们操作过的容器通过commit提交为一个镜像！我们以后就使用我们修改过的镜像即可，这就是我们自己的一个修改过的镜像
```

![1648806392702](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618056.png)

## 入门Docker结束，开启进阶学习

## 五、容器数据卷

### 5.1、什么是容器数据卷

数据？如果数据都在容器中，那么我们容器删除，数据就会丢失！**需求：数据持久化**

MySQL，容器删了，就相当于删库跑路了，**需求：MySQL数据可以存储在本地**

容器之间可以有一个数据共享的技术，Docker中产生的数据，同步到本地

### 5.2、使用数据卷

>方式一：直接使用命令来挂载 -v

```shell
docker run -it -v 主机目录，容器目录

# 测试
docker run -it -v /home/ceshi:/home centos /bin/bash

# 启动之后，我们可以通过docker inspect 查看容器的详细信息
```

![1648862741448](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618627.png)

测试文件同步

![1648863110625](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618639.png)

### 5.3、实战MySQL

思考：MySQL的数据持久化问题！

```shell
# 获取镜像
[root@chenstudy home]# docker pull mysql:5.7

# 运行容器时需要做数据挂载 安装启动MySQL时需要设置密码的这是要注意的
# 官方的 docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 启动
[root@chenstudy home]# docker run -d -p 8088:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=ZJch1328. --name mysql1 mysql:5.7

```

### 5.4、具名挂载和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx

# 查看所有的 volume 的情况
[root@chenstudy mysql]# docker volume ls
local     79c37ba8f722b547003d715c8295eb44481d6bd927c77842123e4fe08406d764
# 这里发现，这种就是匿名挂载， 我们在 -v只写了容器内路径，没有写容器外的路径

# 具名挂载
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx


[root@chenstudy mysql]# docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
8d8c7ce383aafe40b5fbd346de2380ec2e221c392f7dfc6873b16ffa17ec72a1
[root@chenstudy mysql]# docker volume ls
DRIVER    VOLUME NAME
local     juming-nginx
```

![1648869722639](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618460.png)

所有的docker内的卷，没有指定目录的情况下都是在`/var/lib/docker/volumes/xxx`

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况下在使用**具名挂载**

```shell
# 如何确定时具名挂载还是匿名挂载，还是指定路径挂载
-v 容器内路径 		# 匿名挂载
-v 卷名：容器内路径	  # 具名挂载
-v /宿主级的路径:容器内路径  # 指定路径挂载
```

扩展：

```shell
# 通过-v容器内路径:ro|rw
ro:	readonly		# 只读
rw:	readwrite		# 可读可写

# 一旦设置了容器的权限，容器对我们挂载出来的内容就有限定了！
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx 
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx 
```

### 5.5、初始Dockerfile

Dockerfile就是用来构建docker镜像的构建文件！命令脚本！

通过脚本可以生成镜像，镜像时一层一层的，脚本时一个个的命令，每个命令是一层！

```shell
# dockerfile文件 文件名随机， 建议Dockerfile
# 文件内容指令(大写)	参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "-----end-----"
CMD /bin/bash

# 这里的每个命令，就是镜像的一层
```

![1648883192391](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618351.png)

```shell
# 启动自己写的镜像
docker run -it 3d2683f12a3d /bin/bash
```

![1648883561355](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618753.png)

### 5.6、数据卷容器

多个mysql同步数据！

![1649119573350](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618706.png)



![1649119817799](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281618621.png)

![1649120127956](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619272.png)

```shell
# 测试，可以删除docker01,查看一下docker02和docker03是否可以访问这个文件
# 测试依旧可以访问
```



多个mysql实现数据共享

```shell
[root@chenstudy home]# docker run -d -p 8088:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=ZJch1328. --name mysql1 mysql:5.7

[root@chenstudy home]# docker run -d -p 8088:3306 -e MYSQL_ROOT_PASSWORD=ZJch1328. --name mysql2 --volumes-from mysql2 mysql:5.7

# 这个时候，可以实现两个容器数据同步！
```



## 六、DockerFile

### 6.1、DockerFile介绍

dockerfile是用来构建docker镜像的文件！命令参数脚本

构建步骤：

1、编写一个dockerfile文件

2、docker build构建成为一个镜像

3、docker run 运行镜像

4、docker push 发布镜像（Docker Hub、阿里云镜像仓库）

### 6.2、DockerFile构建过程

**基础知识：**

1、每个保留关键字（指令）都必须是大写字母

2、执行从上到下顺序执行

3、# 表示注释

4、每个指令都会创建提交一个新的镜像层，并提交

![1649124071074](https://edu-1328.oss-cn-hangzhou.aliyuncs.com/img/1649124071074.png)

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件十分简单！

步骤：开发，部署，运维...缺一不可

DockerFile:构建文件，定义了一切的步骤，源代码

DockerImages:通过DockerFile构建生成的镜像，最终发布和运行的产品

Docker容器：容器就是镜像运行起来提供服务器

### 6.3、DockerFile指令

```shell
FROM 			# 基础镜像,一切这里开始构建
MAINTAINER		# 镜像是谁写的，姓名+邮箱
RUN				#镜像构建时需要运行的命令
ADD				# 步骤，tomcat镜像，这个tomcat压缩包！添加内容
WORKDIR			# 镜像的工作目录 
VOLUME			# 挂载的目录
EXPOSE			# 暴露端口配置
CMD				# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT		# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD			# 当构建一个被继承的DockerFile 这个时候就会运行ONBUILD指令
COPY			# 类似ADD，将我们的文件拷贝到镜像中
ENV				# 构建的时候设置环境变量！
```

![1649124606683](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619914.png)

### 6.4、实战测试

Docker Hub中99%的镜像都是从这个基础镜像过来的FROM scratch，然后配置需要的软件和配置来进行构建

![1649125810610](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619117.png)

> 创建自己的centos

```shell
# 编写DockerFile的文件
FROM centos

MAINTAINER haochen<1065501674@qq.com>

ENV MYPATH /usr/local

WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools


EXPOSE 80

CMD echo $MYPATH

CMD "------end------"
CMD /bin/bash

# 2、通过这个文件构建镜像
# 命令 docker build -f dockerfile文件路径 -t 镜像名:[tag]
Successfully built 22dc06d87f2d
Successfully tagged mycentos:latest

# 测试运行
```



> CMD 和 ENTRYPOINT 的区别

```shell
CMD				# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT		# 指定这个容器启动的时候要运行的命令，可以追加命令
```

### 6.5、实战：tomcat镜像

1、准备镜像文件 tomcat压缩包，jdk压缩包！

![1649230617526](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619405.png)

2、编写dockerfile文件,官方命名`Dockerfile`build会自动寻找这个文件，就不需要-f指定了

```shell
FROM centos:7
MAINTAINER xiaofan<594042358@qq.com>
 
COPY readme.txt /usr/local/readme.txt
 
ADD jdk-8u73-linux-x64.tar.gz /usr/local/
ADD apache-tomcat-9.0.37.tar.gz /usr/local/
 
RUN yum -y install vim
 
ENV MYPATH /usr/local
WORKDIR $MYPATH
 
ENV JAVA_HOME /usr/local/jdk1.8.0_73
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.37
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.37
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
 
EXPOSE 8080
 
CMD /usr/local/apache-tomcat-9.0.37/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.37/bin/logs/catalina.out
 
```

1、构建镜像

```shell
docker build -t diytomcat .
```

2、启动镜像

```shell
docker run -d -p 8088:8080 --name chentomcat -v /home/chen/tomcat/test:/usr/local/apache-tomcat-9.0.62/webapps/test -v /home/chen/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.62/logs diytomcat
```



## 七、Docker网络原理

### 7.1、理解Docker0

清空所有环境

> 测试

![1649236696183](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619665.png)

三个网络

```shell
# 问题：docker 是如何处理容器网络访问的?
```

![1649236948773](https://myblog-1312029826.cos.ap-nanjing.myqcloud.com/myImg/202206281619314.png)

```shell
#[root@chenstudy tomcat]# docker run -d -P --name tomcat01 tomcat
# 查看容器的内部网络地址 ip addr
```





