# 狂神Docker笔记从入门到入坑指南


# Docker-狅神说

![image-20211019185855625](Docker.jpg)

**隔离：Docker核心思想！打包装箱！每个环境相互隔离。**

**Docker通过隔离机制，可以将服务器利用到极致！**

**本质：所有的技术都是出现了需要我们去解决，才去学习**





2014.4.9Docker1.0发布！

Docker：隔离，镜像（只包含最核心的环境！）

VM：虚拟机是一个完整的系统！

Docker是基于go语言开发，开源项目！

官网：http://www.docker.com

文档（非常详细）：http://docs.docker.com

仓库：http://hub.docker.com



**Docker能干嘛**

之前的虚拟机技术

虚拟出一套硬件，运行一个完整的操作系统！

缺点：

1. 资源占用多
2. 冗余步骤多
3. 启动速度慢



**容器化技术**

- **容器化技术不是模拟一个完整的操作系统！**容器内的应用直接运行在宿主主机上的，容器是没有自己的内核，也没用虚拟出硬件所以很轻便
- 每个容器间是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响！

### DevOps（开发、运维）

**应用更快速的交付和部署**

- 传统：一堆帮助文档，安装程序。。。
- Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用Docker之后，我们部署应用就像搭积木一样。

**更简单的系统运维**

在使用Docker之后，我们的开发，测试环境都是高度一致。

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例，服务器的性能可以被压榨到极致





# 只要学不死，就往死里学！





## Docker安装

### Docker基本组成：

![Docker 架构详解– 每天5分钟玩转容器技术（7） – Jimmy&#39;s Blog](https://tva1.sinaimg.cn/large/008i3skNgy1gvkvhyyrhoj61ee0qegqf02.jpg)

- 镜像(iamge)：通过这个模板来创建容器服务，

docker镜像就好比是一个模板，可以通过这个模板来创建服务，Tomcat镜像\==>>run==>>Tomcat1容器（提供服务器），通过这个镜像可以创建多个容器（最终运行或者项目运行就是在容器中的）。

- 容器(container)：

Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建的。

启动、停止、删除，基本命令！

可以把Docker理解为一个极简的Linux系统。

- 仓库(repository)：

仓库就是存放镜像的地方！ 

仓库分为共有仓库和私有仓库！





### Docker安装

```shell
环境查看
uname -a
系统版本
cat /etc/os-release
```

查看安装文档

1. 卸载旧的Docker
2. 安装需要的安装包
3. 设置镜像的仓库
4. 更新软件包
5. 安装Docker相关的（docker-ce社区版、docker-ee企业版）
6. 启动docker
7. 使用`docker version`查看安装成功与否
8. 查看下载镜像`docker images`

卸载docker：

1. 卸载依赖
2. 删除资源`rm rf /var/lib/docker`默认工作路径

### 阿里云镜像加速

1. 登录阿里云
2. 找到镜像加速
3. 配置使用`参考阿里云文档`

**ubuntu**

```
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://8wpthd6g.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

官网地址：https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

```shell
加速器地址
https://8wpthd6g.mirror.aliyuncs.com
https://8wpthd6g.mirror.aliyuncs.com

1. 安装／升级Docker客户端
对于10.10.3以下的用户 推荐使用Docker Toolbox

Mac安装文件：http://mirrors.aliyun.com/docker-toolbox/mac/docker-toolbox/

对于10.10.3以上的用户 推荐使用Docker for Mac

Mac安装文件：http://mirrors.aliyun.com/docker-toolbox/mac/docker-for-mac/

2. 配置镜像加速器
针对安装了Docker Toolbox的用户，您可以参考以下配置步骤：

创建一台安装有Docker环境的Linux虚拟机，指定机器名称为default，同时配置Docker加速器地址。

docker-machine create --engine-registry-mirror=https://8wpthd6g.mirror.aliyuncs.com -d virtualbox default
查看机器的环境配置，并配置到本地，并通过Docker客户端访问Docker服务。

docker-machine env default
eval "$(docker-machine env default)"
docker info
针对安装了Docker for Mac的用户，您可以参考以下配置步骤：

在任务栏点击 Docker Desktop 应用图标 -> Perferences，在左侧导航菜单选择 Docker Engine，在右侧输入栏编辑 json 文件。将

https://8wpthd6g.mirror.aliyuncs.com加到"registry-mirrors"的数组里，点击 Apply & Restart按钮，等待Docker重启并应用配置的镜像加速器。

例如："registry-mirrors":["https://8wpthd6g.mirror.aliyuncs.com"]

3. 相关文档
Docker 命令参考文档

Dockerfile 镜像构建参考文档
```





![image-20211019211341206](https://tva1.sinaimg.cn/large/008i3skNgy1gvkxieat0kj60n00fnmxv02.jpg)





### 底层原理

**docker是怎么工作的？**

docker是一个Client-Server结构的系统，docker的守护进程运行在主机上。通过socket从客户端访问！

![image-20211019211913453](https://tva1.sinaimg.cn/large/008i3skNgy1gvkxo67yxij60vs0d13za02.jpg)

**docker为什么比VM快**

![image-20211019212211331](https://tva1.sinaimg.cn/large/008i3skNgy1gvkxra6k4cj60j50bo0tg02.jpg)

1. docker有着比虚拟机更少的抽象层

2. docker利用的实宿主机的内核，vm需要Guest OS。

	所以说，新建一个容器的时候，docker不需要像虚拟机一样重新加载一个操作系统，避免引导。虚拟机是加载Guest OS，分钟级别的，而docker是利用宿主机的操作系统，省略了这一复杂的过程，秒级的。





### Docker的常用命令

#### 帮助命令

```shell
docker version		#显示docker版本信息
docker info			#显示docker的系统信息，包括镜像和容器的数量
docker --help		#查看所有命令
docker 命令 --help	#万能命令
```

#### 镜像命令（增删改查）

##### docker images查看所有本地docker镜像命令

```shell
❯ docker images
REPOSITORY          TAG       IMAGE ID       CREATED       SIZE
docker101tutorial   latest    ab474d56b12d   8 hours ago   28.3MB
alpine/git          latest    612b988140be   9 days ago    27.4MB
REPOSITORY：		镜像的仓库源
TAG：			镜像的标签
IMAGE ID：		镜像的id
CREATED：		镜像的创建时间
SIZE：			镜像的大小


❯ docker images --help

Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]

List images

Options（可选项）:
  -a, --all             #列出所有的镜像 /Show all images (default hides intermediate images)
      --digests         #Show digests
  -f, --filter filter   #Filter output based on conditions provided
      --format string   #Pretty-print images using a Go template
      --no-trunc        #Don't truncate output
  -q, --quiet           # 只显示镜像的ID/Only show image IDs

```



#### docker search搜索镜像

```shell
❯ docker search mysql
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11551     [OK]
mariadb                           MariaDB Server is a high performing open sou…   4395      [OK]
mysql/mysql-server                Optimized MySQL Server Docker images. Create…   854                  [OK]

# 可选项，通过收藏来过滤
--filter=STARS=3000		#搜索出来的镜像stars大于3000的
❯ docker search mysql --filter=STARS=10000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   11551     [OK]
```

#### docker pull下载镜像

```shell
docker pull 镜像名[:tag]
❯ docker pull mysql
Using default tag: latest		#如果不写tag，默认下载latest
latest: Pulling from library/mysql
b380bbd43752: Pull complete		#分层下载，docker image 的核心，联合文件系统
f23cbf2ecc5d: Pull complete
30cfc6c29c0a: Pull complete
b38609286cbe: Pull complete
8211d9e66cd6: Pull complete
2313f9eeca4a: Pull complete
7eb487d00da0: Pull complete
4d7421c8152e: Pull complete
77f3d8811a28: Pull complete
cce755338cba: Pull complete
69b753046b9f: Pull complete
b2e64b0ab53c: Pull complete
Digest: sha256:6d7d4524463fe6e2b893ffc2b89543c81dec7ef82fb2020a1b27606666464d87		# 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest		#真实地址

# 等价关系
docker pull mysql
docker pull docker.io/library/mysql:latest
```



#### `docker rmi`删除镜像

```shell
docker rmi -f id/name	#删除指定镜像
docker rmi -f 容器ID 容器ID 容器ID....	#删除多个容器
docker rmi -f $(docker images -aq)	#递归删除镜像（全部删除）
```



#### 阿里云镜像加速配置（详情查看阿里云官网镜像加速项）



#### Run流程

![image-20211021205535724](https://tva1.sinaimg.cn/large/008i3skNgy1gvn8884bhrj60n40fb0t902.jpg)



#### 容器命令

说明：有了镜像才可以创建容器，Linux，下载一个centos镜像来测试学习

```shell
docker pull centos
```

#### 新建容器并启动

```shell
docker run [可选参数] image

# 参数说明：
--name=“NAME”	容器名字 Tomcat01、Tomcat02，用来区分容器
-d				后台方式运行
-it				使用交互方式运行，进入容器查看内容
-p				指定容器端口 -p 8080：8080
	-p				IP：主机端口：容器端口
	-p				主机端口：容器端口
	-p				容器端口
	容器端口
-p				随机指定端口

# 测试,启动并进入容器
❯ docker run -it centos /bin/bash
[root@c254a32cd19c /]# ls		#查看容器内centos，属于基础版本，很多命令都是不完善的
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var
dev  home  lib64  media       opt  root  sbin  sys  usr
# 从容器中退回主机
exit

```

### 列出所有运行的容器

```shell
docker ps 命令
	#列出当前正在运行的容器
	-a	#列出当前正在运行的容器	+	带出历史运行的容器
	-n=?	#显示最近创建的容器
	-q		#只显示容器的编号

# 查看正在运行的容器
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 查看正在运行过的容器	+	带出历史运行的容器
❯ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED         STATUS                     PORTS     NAMES
c254a32cd19c   centos    "/bin/bash"   6 minutes ago   Exited (0) 9 seconds ago             magical_rubin

# 显示最近创建的两个容器
❯ docker ps -a -n=2
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS                     PORTS     NAMES
3741d7e69642   centos    "/bin/bash"   5 seconds ago    Exited (0) 2 seconds ago             beautiful_mayer
c254a32cd19c   centos    "/bin/bash"   11 minutes ago   Exited (0) 5 minutes ago             magical_rubin

# 只显示所有容器编号id
❯ docker ps -aq
3741d7e69642
c254a32cd19c

```





#### 退出容器

```shell
exit	# 直接退出容器并停止
Ctrl + P + Q #不停止容器退出
```



### 删除容器

```shell
docker rm 容器id					#删除指定容器，不能删除正在运行的容器，如果要强制删除 rm -f
docker rm -f $(docker ps -aq)		#删除所有容器
docker ps -a -q|xargs docker rm		#删除所有容器
```

### 启动和停止容器的操作

```shell
docker start 容器id		#启动容器
docker restart 容器id		#重启容器
docker stop 容器id		#停止当前正在运行的容器
docker skill 容器id		#强制停止正在运行的容器

```

## 常用其他命令

#### 后台启动容器不进入

```shell
# 命令 docker run -d 镜像名
❯ docker run -d centos
de25d2ecdadfd02c95df96875e0534781bfde6d873bc83e186d5373d203bbc3d

~
❯ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

# 问题docker ps ，发现centos停止了

#常见的坑：docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止

#Nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

#### 查看日志命令

```shell
docker logs -f -t --tail	容器，没有日志

# 自己编写一段shell脚本
"while true;do echo helloeorld;sleep 1;done"

❯ docker run -d centos /bin/sh -c "while true;do echo helloeorld;sleep 1;done"
62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13

❯ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS     NAMES
62d27d98112c   centos    "/bin/sh -c 'while t…"   2 seconds ago   Up 1 second             heuristic_nightingale


#显示日志
-tf			#显示日志
--tail number		#要显示日志条数
docker logs -tf --tail 10 id	#显示最新的10条日志

```

#### 查看容器中进程信息

```shell
docker top 容器id

❯ docker top 62d27d98112c
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2876                2849                0                   14:09               ?                   00:00:00            /bin/sh -c while true;do echo helloeorld;sleep 1;done
root                3446                2876                0                   14:18               ?                   00:00:00            /usr/bin/coreutils --coreutils-prog-shebang=sleep /usr/bin/sleep 1

```

#### 查看镜像元数据

```shell
#命令
docker inspect 容器id

docker inspect --help


❯ docker inspect --help

Usage:  docker inspect [OPTIONS] NAME|ID [NAME|ID...]

Return low-level information on Docker objects

Options:
  -f, --format string   Format the output using the given Go template
  -s, --size            Display total file sizes if the type is container
      --type string     Return JSON for specified type


❯ docker inspect 62d27d98112c
[
    {
        "Id": "62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13",
        "Created": "2021-10-21T14:09:34.827771095Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo helloeorld;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 2876,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-10-21T14:09:35.05148062Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13/hostname",
        "HostsPath": "/var/lib/docker/containers/62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13/hosts",
        "LogPath": "/var/lib/docker/containers/62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13/62d27d98112c05b2413d5b0163f9a6f933677a03e066431fe6b10b7020544f13-json.log",
        "Name": "/heuristic_nightingale",
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
                "LowerDir": "/var/lib/docker/overlay2/82a3885422dec54f175a090a322f97334a83b0b34169829d751435e56b1369c1-init/diff:/var/lib/docker/overlay2/ce6c2f444ff9950adef64d136203ca6c9c376a61da513d82372e85c4535cd1aa/diff",
                "MergedDir": "/var/lib/docker/overlay2/82a3885422dec54f175a090a322f97334a83b0b34169829d751435e56b1369c1/merged",
                "UpperDir": "/var/lib/docker/overlay2/82a3885422dec54f175a090a322f97334a83b0b34169829d751435e56b1369c1/diff",
                "WorkDir": "/var/lib/docker/overlay2/82a3885422dec54f175a090a322f97334a83b0b34169829d751435e56b1369c1/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "62d27d98112c",
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
                "while true;do echo helloeorld;sleep 1;done"
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
            "SandboxID": "86afd4a6f37bd8754a8b6649530de457844c6972db16fe3764e907c50f301611",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/86afd4a6f37b",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "7146c99cc63b90167a894d18884d2e8db88cec7b1cb4cc1321261221ea76c932",
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
                    "NetworkID": "5b042119547f4235b08f3fcd70ec5c183ae4437d4efef7dca0ad3795d33ff254",
                    "EndpointID": "7146c99cc63b90167a894d18884d2e8db88cec7b1cb4cc1321261221ea76c932",
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

#### 进入当前正在运行的容器

```shell
#我们通常都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bashshell

#测试（方式一）
❯ docker exec -it 62d27d98112c /bin/bash
[root@62d27d98112c /]#
[root@62d27d98112c /]# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 14:09 ?        00:00:00 /bin/sh -c while true;do echo he
root      1173     0  0 14:29 pts/0    00:00:00 /bin/bash
root      1224     1  0 14:29 ?        00:00:00 /usr/bin/coreutils --coreutils-p
root      1225  1173  0 14:29 pts/0    00:00:00 ps -ef
[root@62d27d98112c /]#

#方式二
docker attach 容器id
❯ docker attach 62d27d98112c
正在执行的代码。。。。。


# docker exec		进入容器后打开一个新的终端，可以在里面操作（常用）
# docker attach		进入容器正在运行的终端，不会启动新的进程
```

#### 从容器拷贝文件到主机

```shell
docker cp 容器id：容器内路径 目的的主机路径

#测试
❯ docker run -it centos /bin/bash
[root@a7afe7361b93 /]# cd /home/
[root@a7afe7361b93 home]# ls
[root@a7afe7361b93 home]# touch test.cpp
[root@a7afe7361b93 home]# exit
exit

~ 51s
❯ docker ps -a
CONTAINER ID   IMAGE     COMMAND       CREATED              STATUS                            PORTS     NAMES
a7afe7361b93   centos    "/bin/bash"   About a minute ago   Exited (0) 16 seconds ago                   unruffled_mclean
0df1127221bc   centos    "/bin/bash"   About a minute ago   Exited (127) About a minute ago             jovial_fermi

~
❯ docker cp a7afe7361b93:/home/test.cpp ~/Desktop

~
❯ cd Desktop

~/Desktop
❯ ls
 Linux基础学习计划                test.cpp
 Linux基础学习计划.xmind          ubuntu-20.04.3-desktop-amd64.iso
WorkSpace                         网络中心Linux学习计划.pdf




#拷贝是一个手动的过程，未来我们可以使用 -v 卷的技术，可以实现自动同步 /home /home


```







## 小结

![image-20211022200042512](https://tva1.sinaimg.cn/large/008i3skNgy1gvoc9fs2baj60t70kc77902.jpg)





## 作业练习

#### docker安装Nginx

```shell
#1、搜索镜像
docker search 镜像名
在dockerhub上搜索,可以看到详细信息
❯ docker search nginx
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
nginx                             Official build of Nginx.                        15690     [OK]
jwilder/nginx-proxy               Automated Nginx reverse proxy for docker con…   2085                 [OK]
richarvey/nginx-php-fpm           Container running Nginx + PHP-FPM capable of…   818                  [OK]


#2、下载镜像
docker pull 镜像名[:tag]
❯ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
b380bbd43752: Already exists
fca7e12d1754: Pull complete
745ab57616cb: Pull complete
a4723e260b6f: Pull complete
1c84ebdff681: Pull complete
858292fd2e56: Pull complete
Digest: sha256:644a70516a26004c97d0d85c7fe1d0c3a67ea8ab7ddf4aff193d9f301670cf36
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest


#3、启动镜像
docker run -d --name Nginx01 -p 3344：80 nginx
# -d 后天运行
# --name 给容器起个名字
# -p 宿主机端口：容器内部端口

❯ docker run -d --name ngnix01 -p 3344:80 nginx
baa28e5e5c581b95a55e7fa74ee95a22bc9f3d0ddb25801bb907a9cb848714ea


#4、运行测试
❯ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
baa28e5e5c58   nginx     "/docker-entrypoint.…"   7 seconds ago   Up 6 seconds   0.0.0.0:3344->80/tcp   ngnix01

~
❯ curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

#	进入容器
❯ docker exec -it Nginx /bin/bash
```

端口

1. 从外网访问docker需要经过的防火墙，都要放行才能访问

![1](https://tva1.sinaimg.cn/large/008i3skNgy1gxyjw1qxzlj30s00l5wfl.jpg)

思考问题：我们每次改动nginx的配置文件都要进入容器内部？十分的麻烦，我要是可以在容器外部提供一个映射路径，达到在容器修改文件名，容器内部就可以自动修改？	-v	数据卷！



**docker 装Tomcat**



```shell
# 官方的使用
docker run -it --rm tomcat:9.0
# 我们之前的启动都是以后台的方式启动，停止了容器之后，容器还是可以看到的

docker run -it --rm		# 一般用来测试，用完即删

# 下载在启动
docker pull tomcat
# 启动运行
docker images
docker run -d -p 9999:8080 --name Tomcat tomact
# 测试访问没有问题
docker exec -it Tomcat /bin/bash
# 发现问题
#	1.Linux命令少了
#	2.没有webapps
# 原因：阿里云的镜像的原因。默认是最小的镜像，所有不必要的都剔除掉了。
# 保证最小可运行环境
把webapps.dist下面的所有文件拷贝到webapp下，刷新页面就可以访问到Tomcat的默认项目了
root@9a5e16fb9b52:/usr/local/tomcat# cp -r  webapps.dist/* webapps/
root@9a5e16fb9b52:/usr/local/tomcat# ls webapps
ROOT  docs  examples  host-manager  manager
```

思考问题：我们以后部署项目，如果每次都进入容器是不是十分麻烦？我要是可以在容器外部提供一个映射路径，webapps，我们在外部放置项目，就可以自动同步到内部就好了！



**作业练习：**

部署es-kibana		es(elasticsearch)

```shell
# es 暴露的端口很多
# es 十分的耗内存
# es 的数据一般到要放置到安全目录挂载
# --net somenetwork	网络配置

# 官方命令
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:tag
#启动
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.6.2
# 启动了Linux就很卡
docker stats #	查看CPU状态
# es 是十分耗内存的，1.XG
# 测试一下es是否成功了
❯ curl localhost:9200
{
  "name" : "f04995e24d6b",
  "cluster_name" : "docker-cluster",
  "cluster_uuid" : "p8H9dizAQ7a081Kj-bgqHg",
  "version" : {
    "number" : "7.6.2",
    "build_flavor" : "default",
    "build_type" : "docker",
    "build_hash" : "ef48eb35cf30adf4db14086e8aabd07ef6fb113f",
    "build_date" : "2020-03-26T06:34:37.794943Z",
    "build_snapshot" : false,
    "lucene_version" : "8.4.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

![image-20220102000236453](https://tva1.sinaimg.cn/large/008i3skNgy1gxym8y9x17j30yr03m3z6.jpg)

```shell
# 成功了就赶紧关闭，增加内存的限制,修改配置文件 -e 环境修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xms512m" elasticsearch:7.6.2	# 内存使用范围限制：64-512M
```

**作业：使用es连接kibana**

docker网络原理

![image-20220102001547604](https://tva1.sinaimg.cn/large/008i3skNgy1gxymmo8r3mj30q50cqq3b.jpg)

#### 可视化

- portainer（先用这个）

```shell
docker run -d -p 8088:9000\
--restart=always -v /var/run/docker.sock --privileged=true portainer/portainer
```

- Rancher(CI/CD再用)



**什么是portainer？**

Docker图形化界面管理工具！提供一个后台面板供我们操作！

访问测试：外网ip:8088

### Docker镜像讲解

**镜像是什么**？

镜像是一种轻量级，可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含某个软件所需要的所有内容，包括代码、运行时、库、环境变量和配置环境。



所有的应用，直接打包docker镜像，就可以直接跑起来！

如何得到应用：

- 从远程仓库下载
- 朋友拷贝给你
- 自己制作一个Dockerfile

#### Docker镜像加载原理

**UnionFS(联合文件系统)**

![image-20220102003831631](https://tva1.sinaimg.cn/large/008i3skNgy1gxynac3jabj30ur08z0uv.jpg)

**Docker镜像加载原理**

![image-20220102003933687](https://tva1.sinaimg.cn/large/008i3skNgy1gxynbee3voj30wl0pa43y.jpg)

#### 分层理解

.........

特点：

Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部！

这一层就是我们通常说的容器层，容器之下的都叫镜像层！

![image-20220102005402418](https://tva1.sinaimg.cn/large/008i3skNgy1gxynqgzjkcj30xv0ba402.jpg)

#### 如何提交一个自己的镜像

**commit镜像**

```shell
docker commit 提交容器成为一个新的副本

# 命令和git原理类似
docker commit -m "message" -a="作者" 容器id 目标镜像名:[TAG]
```

学习方式说明：理解概念，但一定要实践，最后实践和理论相结合搞定这个知识。

如果想保存当前容器的状态，就可以通过commit来提交，获得一个镜像

到这里就算入门docker了

### 容器数据卷

需求：数据可以持久化存储

卷技术：目录的挂载，将我们容器内的目录，挂载到Linux上面，容器中产生的数据，同步到本地

总结：容器的持久化和同步操作！容器间也可以数据同步！

#### 使用数据卷

方式一：直接使用命令来挂载 `-v`

```shell
docker run -it -v 主机目录：容器目录

# 测试
docker run -it -v /home/iuv/docker_test:/home ubuntu /bin/bash

docker inspect id	# 查看容器的详细信息
```

好处：我们以后修改只需要在本地修改即可，容易内会自动同步！

实战：安装MySQL

思考；MySQL的数据持久化的问题

```shell
# 获取镜像
docker pull mysql：5.7
# 运行容器,需要数据挂载！ # 安装启动MySQL，需要配置密码，这是需要注意的点
docker run -d -p 3333;3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=password --name MySQL mysql:5.7
# 官方测试
docker run --name MySQL -e MYSQL_ROOT_PASSWORD=password -d mysql:tag

```

##### 具名和匿名挂载

```shell
# 匿名挂载
-v 容器内路径
docker run -d -p --name nginx01 -v /etc/nginx nginx

# 查看所有的volume（卷）的情况
docker volume ls
❯ docker volume ls
DRIVER    VOLUME NAME
local     5a0d5757b0f3c5992dec7cb42640e84aadbd1efa57843adf74dc0d9c24447e38
local     5b06bda70a507481eac4866b7947570f1c1959ae49000f0e15946fa27852cf63
local     708bbb952dc960c7cce2a182a4a815ec3030c66649d7104ce388d2d89d5fcf6c
local     a34994374cf865e100a30de904b0792bab5eb292f869ec0cf32faa646e9b07c4
local     bdee2c1f4446808797d76dfd4887b5664aed7b3cf15623e157919656699c818f
local     f98cf9d8412e0a14de50580c8159ec83cbc4873bd44b021df52cbc898a65af0a
local     f55715ede0dbd227ee5baf13b790e3df420f5cb89db156e14e6c8142816bfcc4

# 具名挂载(-P:随机端口)
>docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx nginx
# 通过 -v 卷名：容器内路径
# 所有的docker容器内的卷，没有指定目录的情况下都是在/var/lib/docker/volumes/xxx/_data
# 我们通过具名挂载可以方便找到我们的一个卷，大多数情况使用的是具名挂载
```

##### 如何确定是具名 挂载还是匿名挂载，还是指定路径挂载：

```shell
-v 容器内路径	# 匿名挂载
-v 卷名：容器内路径		# 具名挂载
-v /宿主机路径：容器内路径		# 指定路径挂载
```

**拓展**

```shell
# 通过 -v 容器内路径:ro/rw 改变读写权限
ro		readonly	# 只读
rw		readwrite	# 可读可写
# 一旦设置了容器权限，容器对我们挂载出来的内容就有了限定！
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:rw nginx

# ro 只能通过宿主机来操作，容器内部是无法操作！

```



### 初识DockerFile（方式二）

Dockerfile就是用来构建docker镜像的构建文件！（命令脚本）

通过这个脚本（Dockerfile）可以生成一个镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层！

```shell
# Dockerfile（指令都是大写）
❯ cat Dockerfile
FROM centos

volume ["voluem01","volume02"]	# 匿名挂载

cmd echo "--------end--------"
cmd /bin/bash

# docker build
❯ docker build -f ./Dockerfile -t iuv/cnetos:v1 .
[+] Building 27.3s (5/5) FINISHED
 => [internal] load build definition from Dockerfile                       0.0s
 => => transferring dockerfile: 132B                                       0.0s
 => [internal] load .dockerignore                                          0.0s
 => => transferring context: 2B                                            0.0s
 => [internal] load metadata for docker.io/library/centos:latest           2.3s
 => [1/1] FROM docker.io/library/centos@sha256:a27fd8080b517143cbbbab9df  25.0s
 => => resolve docker.io/library/centos@sha256:a27fd8080b517143cbbbab9dfb  0.0s
 => => sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee 762B / 762B  0.0s
 => => sha256:a1801b843b1bfaf77c501e7a6d3f709401a1e0c83863037 529B / 529B  0.0s
 => => sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc 2.14kB / 2.14kB  0.0s
 => => sha256:a1d0c75327776413fa0db9ed3adcdbadedc95a66 83.52MB / 83.52MB  21.0s
 => => extracting sha256:a1d0c75327776413fa0db9ed3adcdbadedc95a662eb1d360  3.7s
 => exporting to image                                                     0.0s
 => => exporting layers                                                    0.0s
 => => writing image sha256:b129de596a04cfd85bd8497408b0f793eff7993b73e67  0.0s
 => => naming to docker.io/iuv/cnetos:v1                                   0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them


~/Desktop/WorkSp镜像
# 查看docker镜像
❯ docker images
REPOSITORY         TAG       IMAGE ID       CREATED        SIZE
nginx              latest    605c77e624dd   4 days ago     141MB
ubuntu             latest    ba6acccedd29   2 months ago   72.8MB
iuv/cnetos         v1        b129de596a04   3 months ago   231MB
careywong/subweb   latest    1c9bdfb315fe   7 months ago   25.4MB

# 启动镜像
❯ docker run -it b129de596a04 /bin/bash
[root@00814a155b3d /]# ls
bin  etc   lib	  lost+found  mnt  proc  run   srv  tmp  var	   volume02
dev  home  lib64  media       opt  root  sbin  sys  usr  voluem01
[root@00814a155b3d /]# ls -al
total 64
drwxr-xr-x   1 root root 4096 Jan  3 16:53 .
drwxr-xr-x   1 root root 4096 Jan  3 16:53 ..
-rwxr-xr-x   1 root root    0 Jan  3 16:53 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root  360 Jan  3 16:53 dev
drwxr-xr-x   1 root root 4096 Jan  3 16:53 etc
drwxr-xr-x   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root 4096 Sep 15 14:17 lost+found
drwxr-xr-x   2 root root 4096 Nov  3  2020 media
drwxr-xr-x   2 root root 4096 Nov  3  2020 mnt
drwxr-xr-x   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 225 root root    0 Jan  3 16:53 proc
dr-xr-x---   2 root root 4096 Sep 15 14:17 root
drwxr-xr-x  11 root root 4096 Sep 15 14:17 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Jan  3 16:53 sys
drwxrwxrwt   7 root root 4096 Sep 15 14:17 tmp
drwxr-xr-x  12 root root 4096 Sep 15 14:17 usr
drwxr-xr-x  20 root root 4096 Sep 15 14:17 var
drwxr-xr-x   2 root root 4096 Jan  3 16:53 voluem01	# 这个目录是我们生成镜像的时候自动挂载的，数据卷目录
drwxr-xr-x   2 root root 4096 Jan  3 16:53 volume02	# 这个目录是我们生成镜像的时候自动挂载的，数据卷目录
```

这个卷和外部一定有一个同步的目录

```shell
❯ docker inspect 00814a155b3d

"Mounts": [
            {
                "Type": "volume",
                "Name": "548c26c0b72a771358f6f87264514f5ed03b5d7e29422cdba3f5adde79e5ee78",
                "Source": "/var/lib/docker/volumes/548c26c0b72a771358f6f87264514f5ed03b5d7e29422cdba3f5adde79e5ee78/_data",
                "Destination": "voluem01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "2ba58f6cfae8f7a7e1a806abace30955cb89bed02d46f96a4d7ee72ada3e5fea",
                "Source": "/var/lib/docker/volumes/2ba58f6cfae8f7a7e1a806abace30955cb89bed02d46f96a4d7ee72ada3e5fea/_data",
                "Destination": "volume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

这种方式我们未来使用的十分多，因为我们通常会自己构建自己的容器！

假设构建镜像时没有挂载卷，就要手动镜像挂载 -v 卷名:容器内路径！

### 数据卷容器

例如：两个MySQL同步数据

`参数：--volume-from可以实现容器间的数据共享（数据属于双向拷贝）`

删除容器，只要还有一个容器在使用数据就不会被删除

多个MySQL实现数据共享

```shell
docker run -d -p 3310:3306 -v /etc/mysql/conf.d -v /var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql02 --volumes-from mysql01 mysql:5.7
```

**结论：**

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。

但是一旦持久化到本地，本地的数据就不会被删除

## Dockerfile

**Dockerfile介绍**

Dockerfile就是用来构建docker镜像的构建文件！（命令脚本）

通过这个脚本（Dockerfile）可以生成一个镜像，镜像是一层一层的，脚本一个个的命令，每个命令都是一层！

**构建步骤：**

1. 编写一个Dockerfile文件
2. `docker build`构建成为一个镜像
3. `docker run`运行镜像
4. `docker push`发布镜像（DockerHub、阿里云镜像仓库）

### Dockerfile构建过程

**基础知识：**

1. 每个保留关键字（指令）都是必须是大写字母

2. 执行顺序：从上到下

3. `#`表示注释

4. 每个指令都会创建提交一个新的镜像层，并提交！

	![Docker 实践：使用Dockerfile 构建自己的centos - SegmentFault 思否](https://tva1.sinaimg.cn/large/008i3skNgy1gy1n41ai3hj307j056dfq.jpg)

Dockerfile：构建文件，定义了一切的步骤，相当于源代码

DockerImages：通过Dockerfile构建生成的镜像，最终发布和运行的产品

docker容器：容器就是镜像运行起来提供服务器



#### Dockerfile的指令（命令）

```shell
FROM		# 基础镜像，一切从这里开始构建
MAINTAINER	# 镜像是谁写的，姓名+邮箱
RUN			# 镜像构建的时候要运行的命令
ADD			# copy文件
WORKDIR		# 镜像的工作目录
VOLUME		# 挂载目录
EXPOSE		# 暴露端口
CMD			# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD		# 当构建一个被继承Dockerfile这个时候就会运行ONBUILD的指令，触发指令。
COPY		# 类似ADD，将我们的文件拷贝到镜像中
ENV			# 构建的时候设置的环境变量

# 以上都是常用命令
```

![2](https://tva1.sinaimg.cn/large/008i3skNgy1gy1nfjnll5j308m04j74g.jpg)



#### 实战测试

Docker Hub中99%镜像都是从这个基础镜像过来的`FROM scratch`，然后配置需要的软件和配置来进行的构建

**创建一个自己的centos**

==Dockerfile==

```dockerfile
FROM centos
MAINTAINER iuv<me@iuuovvi.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "=======end======="
CMD /bin/bash

```

==docker build==

```shell
docker build -f Dockerfile -t mycentos:0.1 .	# 不要忘记最后面的"."

docker build -f Dockerfile文件路径 -t 镜像名:[tag]
```

==docker run==

```shell
docker run -it mycentos:0.1
```

 `docker history id`可以查看镜像变更历史

```shell
# nginx
❯ docker history 605c77e624dd
IMAGE          CREATED       CREATED BY                                      SIZE      COMMENT
605c77e624dd   5 days ago    /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>      5 days ago    /bin/sh -c #(nop)  STOPSIGNAL SIGQUIT           0B
<missing>      5 days ago    /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>      5 days ago    /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:09a214a3e07c919a…   4.61kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:0b866ff3fc1ef5b0…   1.96kB
<missing>      5 days ago    /bin/sh -c #(nop) COPY file:65504f71f5855ca0…   1.2kB
<missing>      5 days ago    /bin/sh -c set -x     && addgroup --system -…   61.1MB
<missing>      5 days ago    /bin/sh -c #(nop)  ENV PKG_RELEASE=1~bullseye   0B
<missing>      5 days ago    /bin/sh -c #(nop)  ENV NJS_VERSION=0.7.1        0B
<missing>      5 days ago    /bin/sh -c #(nop)  ENV NGINX_VERSION=1.21.5     0B
<missing>      2 weeks ago   /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>      2 weeks ago   /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>      2 weeks ago   /bin/sh -c #(nop) ADD file:09675d11695f65c55…   80.4MB

```

>CMD 和 ENTRYPOINT 区别

```shell
CMD			# 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	# 指定这个容器启动的时候要运行的命令，可以追加命令
```

Dockerfile中很多的命令都是十分相似的，我们需要了解他们的区别，我们最好的学习就是对比他们的然后测试效果！

#### 实战：Tomcat镜像

1. 准备镜像文件Tomcat压缩包，jdk的压缩包！

2. 编写Dockerfile文件，官方命名`Dockerfile`,`build`会自动寻找这个文件，就不需要`-f`来指定了！

	```dockerfile
	FROM centos
	MAINTAINER iuv<me@iuuovvi.com>
	
	COPY README.md /usr/local/README.md
	
	ADD jdk-8ull-linux-x64.tar.gz /usr/local/
	ADD apach-tomcat-9.0.22.tar.gz /usr/local/
	
	RUN yum -y install vim
	
	ENV MYPATH /usr/local
	WORKDIR $MYPATH
	
	ENV JAVA_HOME /usr/local/jdk1.8.0_11
	ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
	ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.22
	ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.22
	ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
	
	EXPOSE 8080
	
	CMD /usr/local/apache-tomcat-9.0.22/bin/starup.sh && tail -F /usr/local/apache-tomcat-9.0.22/bin/logs/catalina.out
	
	```

	

3. 构建镜像

	```shell
	docker build -t diytomcat . 
	```

4. 启动镜像

5. 访问测试

6. 发布项目（由于做了卷挂载，我们直接在本地编写项目就可以发布）

#### 发布镜像

1. 地址：https://hub.docker.com

2. 在自己的服务器上提交自己的镜像

	```shell
	❯ docker login --help
	Log in to a Docker registry or cloud backend.
	If no registry server is specified, the default is defined by the daemon.
	
	Usage:
	  docker login [OPTIONS] [SERVER] [flags]
	  docker login [command]
	
	Available Commands:
	  azure       Log in to azure
	
	Flags:
	  -h, --help              Help for login
	  -p, --password string   password
	      --password-stdin    Take the password from stdin
	  -u, --username string   username
	
	Use "docker login [command] --help" for more information about a command.
	
	
	❯ docker login -u iuvi
	Password:
	Login Succeeded
	
	Logging in with your password grants your terminal complete access to your account.
	For better security, log in with a limited-privilege personal access token. Learn more at https://docs.docker.com/go/access-tokens/
	
	```

3. 登录后就可以提交自己的镜像,`docker push`



**阿里云镜像服务**

.......

**小结：**

![image-20220105233510017](https://tva1.sinaimg.cn/large/008i3skNgy1gy37xoe2nuj30lt0i3q44.jpg)





## Docker网络
