---
title: Docker学习笔记
date: 2024-01-18 16:31:29
tags:
- Docker
---
## 1.Docker概述

#### 1.1 Docker 为什么会出现？

1. 一款产品从开发到上线，一般至少有开发 /上线 两套环境！每套环境的配置都不同。

2. 开发与运维之间的难题：我在自己电脑上可以运行，版本更新导致服务不可用……

3. 环境配置十分麻烦，每一台机器都要部署环境（集群Redis、ES、Hadoop……）
4. 发布一个项目（jar包 + Redis\Mysql\jdk\ES...），项目带上环境安装打包
5. 之前在服务器上配置一个应用环境 Redis、jdk、ES、Hadoop，配置十分麻烦，且不能够跨平台

Docker给以上问题提出了解决方案！！

流程：java项目 -- jar+环境 -- 打包项目带上环境（镜像） --  发布到Docker仓库：商店 -- 下载发布的镜像 -- 直接运行即可

Docker的思想来源于集装箱，通过隔离机制，可以将服务器利用到极致。

#### 1.2 Docker历史

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240104155418100.png" alt="image-20240104155418100" style="zoom:50%;" />

Docker是基于Go语言开发的。

官网：https://www.docker.com/

文档：https://docs.docker.com/get-started/overview/

DockerHub：https://hub.docker.com/

#### 1.3 Docker能做什么？

== 虚拟机技术 ==

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240104160406126.png" alt="image-20240104160406126" style="zoom:50%;" />

**虚拟机技术存在的缺点：**

1.资源占用非常多

2.冗余步骤多

3.启动很慢



== 容器化技术 ==

<span style="color:red;">容器化技术不是模拟一个完整的操作系统！！</span>

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240104161511514.png" alt="image-20240104161511514" style="zoom:50%;" />

比较Docker和虚拟机技术的不同：

+ 传统虚拟机，虚拟出一套硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件
+ 容器内的应用是直接运行在宿主机的内核，容器是没有自己的内核，也没有虚拟硬件，所以轻便
+ 每个容器间是相互隔离的，每个容器内都有一个属于自己的文件系统，互不影响

== DevOps（开发、运维）==

**应用更快速交付和部署**

传统：一堆文件，安装程序

Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩缩容**

使用Docker后，部署应用像是搭积木一样

**更简单的系统运维**

在容器化之后，开发和测试环境高度一致

**更高效的计算资源利用**

Docker是内核级别的虚拟化

## 2.Docker安装

#### 2.1 Docker的基本组成

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/1912698-20201229222645994-415295107.png" alt="img" style="zoom:50%;" />

**镜像（image）**：Docker镜像就好比是一个模板，可通过这个模板来创建容器服务。通过一个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中）。

**容器（container）**：Docker利用容器技术，独立运行一个或者一组应用，通过镜像来创建。

**仓库（repository）**：存放镜像的地方。仓库分为公有仓库和私有仓库。

#### 2.2 安装

1.环境查看

```shell
uname -r
```

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240104165041325.png" alt="image-20240104165041325" style="zoom:50%;" />

```shell
cat /etc/os-release
```

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240104165225937.png" alt="image-20240104165225937" style="zoom:50%;" />

2.安装

```shell
## 1.卸载旧版本Docker
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

## 2.需要的安装包
sudo yum install -y yum-utils

## 3.设置镜像的仓库
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo （国内镜像地址）

## 4.安装Docker引擎 ce-社区版  ee-企业版
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


## tips:更新yum软件包索引
yum makecache fast

## 5.启动Docker
sudo systemctl start docker

## 6.方法一：判断是否安装成功
docker -v
## 6.方法二：判断是否安装成功
sudo docker run hello-world

## 7.查看安装的hello-world镜像
docker images

## 8.卸载Docker
sudo yum remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
sudo rm -rf /var/lib/docker #Docker的默认工作路径：/var/lib/docker
sudo rm -rf /var/lib/containerd
```

#### 2.3 run流程和Docker原理

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240105104710214.png" alt="image-20240105104710214" style="zoom:50%;" />

#### 2.4 底层原理

**Docker是怎么工作的？**

Docker是一个Client- Server结构的系统，Docker的守护进程运行在主机上，通过Socket从客户端访问。Docker Server接收到Docker Client的指令，就会执行这个命令。

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240105111556497.png" alt="image-20240105111556497" style="zoom:50%;" />

**Docker为什么比VM快？**

+ Docker有着比虚拟机更少的抽象层
+ Docker利用的是宿主机的内核,而不需要加载操作系统OS内核：*当新建一个容器时，docker不需要和虚拟机一样重新加载一个操作系统内核，进而避免引寻、加载操作系统内核返回等比较费时费资源的过程。*

![img](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/bec855a4f0634c52a80ee604e760234a-20240105111842805.png)

![image-20240105112010205](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240105112010205.png)

## 3.Docker命令

### 3.1帮助命令

```shell
docker version      # 显示docker版本信息
docker info         # 显示docker系统信息，包括镜像和容器
docker 命令 --help   # 帮助命令
```

### 3.2镜像命令

```shell
docker images  # 查看本地主机上的镜像
```

![image-20240105112846850](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240105112846850.png)

```shell
docker search 镜像名称   # 搜索镜像
-----
docker pull 镜像名称     # 下载镜像，`docker pull`默认拉取最新版的镜像
docker pull 镜像名称:版本号 #下载指定版本镜像，例如：docker pull mysql:5.7
------
docker rmi -f 容器ID    # 删除指定容器 
docker rmi -f 容器ID 容器ID 容器ID   # 删除多个容器
docker rmi -f $(docker images -aq) # 删除全部容器
```

### 3.3容器命令

说明：有了镜像才可以创建容器。

以下载一个CentOS镜像来测试学习

```shell
docker pull centos
# 新建容器并启动
docker run [可选参数] image
##参数说明：
  --name="Name" 容器名称 tomcat01，tomcat02，用来区分容器
  -d            后台方式运行
  -it           使用交互方式运行
  -p            指定容器的端口 -p 8080:8080
  	-p ip:主机端口:容器端口
  	-p 主机端口:容器端口 （最常使用方式）
  	-p 容器端口
  	容器端口
## 测试：启动并进入容器
[root@localhost ~]# docker run -it centos /bin/bash
[root@9dcaa54373de /]# ls  # 查看容器内的centos 
[root@9dcaa54373de /]# exit #退出容器
```

**列出所有运行的容器**

```shell
docker ps      # 列出当前正在运行的容器
docker ps -a   # 列出所有运行的容器，包括历史运行过的容器
docker ps -aq  # 只列出所有运行的容器的编号，包括历史运行过的容器
```

**退出容器**

```shell
exit  # 直接退出容器
Ctrl + p + q  # 容器不停止退出
```

**删除容器**

```shell
docker rm 容器ID    # 删除指定容器，不能删除正在运行的容器。如果要强制删除使用rm -f
docker rm -f $(docker ps -aq) # 删除全部容器
docker ps -a -q|xargs docker rm  # 删除全部容器
```

**启动和停止容器**

```shell
docker start 容器ID     # 启动容器
docker restart 容器ID   # 重启容器
docker stop 容器ID      # 停止当前正在运行容器
docker kill 容器ID      # 强制停止当前容器 
```

### 3.4常用其他命令

后台启动容器

```shell
# 命令 docker run -d 镜像名
docker run -d centos
#--问题是docker ps 后发现centos停止了
##--常见的坑：docker 容器使用后台运行，就必须要有一个前台进程。Docker发现没有应用，就会自动停止
```

查看日志命令

```shell
docker logs -tf --tail 显示日志条数  # 容器，没有日志

# 自己编写一段shell脚本
docker run -d centos /bin/bash -c "while true;do echo Hello World;sleep 1;done"

# 查看正在运行的容器
docker ps
---
CONTAINER ID   IMAGE     COMMAND                   CREATED         STATUS         PORTS     NAMES
e2e8c5e2d761   centos    "/bin/bash -c 'while…"   5 seconds ago   Up 4 seconds             cool_neumann
---
# 显示日志
-tf   # 显示日志
-tail number  # 显示日志条数
docker logs -tf --tail 10 e2e8c5e2d761
```

查看容器中的进程信息

```shell
docker top 容器id
```

查看镜像元数据

```shell
docker inspect 容器id
```

进入当前正在运行的容器

```shell
# 通常容器都是使用后台方式运行的，有时候需要进入到容器，修改一些配置
# 方式一
docker exec -it 容器id bashshell
docker exec -it e2e8c5e2d761 /bin/bash

# 方式二
docker attach 容器id  # 显示正在执行的当前代码

## 二者区别：
docker exec   # 进入容器后开启一个新的终端，可以在里面进行操作
docker attach  # 进入容器正在执行的专断，不会启动新的进程
```

从容器内拷贝文件到主机上

```shell
docker cp 容器id:容器内路径 目的主机路径

-----
## 示例：
# 进入容器
[root@localhost ~]# docker exec -it e2e8c5e2d761 /bin/bash
[root@e2e8c5e2d761 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
# 在容器中创建test.java
[root@e2e8c5e2d761 /]# touch test.java
[root@e2e8c5e2d761 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  test.java  tmp  usr  var
[root@e2e8c5e2d761 /]# pwd
/
[root@e2e8c5e2d761 /]# mv test.java /home
[root@e2e8c5e2d761 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@e2e8c5e2d761 /]# cd /home
[root@e2e8c5e2d761 home]# ls
test.java
[root@e2e8c5e2d761 home]# exit
exit
[root@localhost ~]# ls
anaconda-ks.cfg  Desktop  initial-setup-ks.cfg  公共  模板  视频  图片  文档  下载  音乐
# 将容器中的test.java拷贝到主机上
[root@localhost Desktop]# docker cp e2e8c5e2d761:/home/test.java /home
Successfully copied 1.54kB to /home
[root@localhost Desktop]# cd /home
[root@localhost home]# ls
mysql  qinwei  test.java
```

### 3.5 小结

![img](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/70.png)

**作业练习**

**1.使用Docker安装Nginx**

```shell
# 1.搜索Nginx
docker search nginx
# 2.下载镜像
docker pull nginx
# 3.运行测试
docker run -d --name nginx01 -p 3344:80 nginx
##参数说明：
### -d 后台运行
### --name 给容器重命名
### -p 宿主机端口:容器内部端口
```

端口暴露概念：

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240108141601418.png" alt="image-20240108141601418" style="zoom: 33%;" />

**🤔️思考问题**：对于部署在容器中的Nginx，每次修改配置都需要进入到容器中去修改，能否在容器外部对容器内的Nginx配置文件进行修改？

**2.使用Docker安装tomcat**

```shell
# 方式一：官方的使用
docker run -it --rm tomcat:9.0
# 我们之前的启动都是后台启动，停止容器后，还可以查询到。docker run -it --rm，一般用来测试，用完就删除
----------------
# 方式二
# 下载tomcat
docker pull tomcat:9.0
# 运行tomcat
docker run -d -p 3355:8080 --name tomcat01 tomcat:9.0
# 进入容器查看tomcat目录
docker exec -it tomcat01 /bin/bash

##发现问题：1.Linux命令少了；2.webapps目录下没有内容。原因：默认使用的是最小的镜像，所有不必要的内容都进行了剔除。保证最小可运行环境即可。

# 可通过复制webapps.dist的内容到websapps实现访问内容呈现
cp -r webapps.dist/* webapps
```

**🤔️思考问题**：以后部署项目，如果每次都进入到容器中十分麻烦，如果可以在容器外部提供一个映射路径，在外部放置项目，自动同步到内部就好了！

**3.部署es + kibana**

存在问题1：

+ es 暴露的端口很多
+ es十分耗内存
+ es的数据一般需要放置在安全目录，通过挂载方式使用

```shell
# 启动 elasticsearch
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:7.8.0
## --net somenetwork 网络配置

# 增加内存限制，修改配置文件 -e 环境配置修改
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" -e ES_JAVA_OPTS="-Xms64m -Xms512m" elasticsearch:7.8.0

# 测试是否运行成功
curl localhost:9200
```

![image-20240109101056368](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240109101056368.png)

查看docker的CPU状态：`docker stats`

**🤔️思考问题**：使用kibana连接es?思考网络如何才能连接过去 

![image-20240109101535771](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240109101535771.png)

## 4.Docker可视化面板

**portainer**

portainer是Doker的图形化界面管理工具，提供一个后台面板，方便操作。

```shell
# 安装
docker run -d -p 9000:9000 -v /var/run/docker.sock:/var/run/docker.sock -v /dockerData/portainer:/data --restart=always --name portainer portainer/portainer
```

访问：http:ip地址:9000【首次登录需要设置密码】

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240109103445122.png" alt="image-20240109103445122" style="zoom:50%;" />

PS：平时不会使用。

## 5.Docker镜像讲解
### 5.1 什么是镜像？
镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码、运行时、库、环境变量和配置文件。
所有应用，直接打包docker镜像，就可以直接跑起来！
如何得到镜像：
+ 远程仓库下载
+ 自行制作
+ 拷贝别人制作 

### 5.2 UnionFS（联合文件系统）

UnionFS（联合文件系统）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）。Union文件系统是Docker镜像的基础，镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

特征：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

**镜像加载原理**

docker的镜像实际上由一层一层的文件系统组成，这种层级文件系统就是上述的UnionFS。接着，在内部又分为2部分：

+ bootfs(boot file system)：docker镜像的最底层是bootfs，主要包含bootloader（加载器）和kernel（内核）。bootloader主要是引导加载kernel，linux刚启动时会加载bootfs文件系统。这一层与典型的linux/Unix系统一样，包含bootloader和kernel。当boot加载完成后，整个内核就在内存中了，此时内存的使用权已由bootfs转交给了内核，此时系统也会卸载bootfs。
  *这里的加载，可以理解为，我们windows电脑开机时候，从黑屏到进入操作系统的过程。*

+ rootfs(root file system)：在bootfs之上，包含的就是典型linux系统中的`/dev、/proc、/bin、/etc`等标准目录和文件。
  rootfs就是各种不同的操作系统发行版，比如Ubuntu、Centos等等。

![img](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/475fbb6fbeb7478da1bc38e94ef1b567~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

![image-20240109110323515](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240109110323515.png)

理解：

所有Docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容的时候，就会在当前镜像层之上，创建新的镜像。

举一个简单的例子，加入基于Ubuntu Linux 16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加Python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像层当前已经包含3个镜像层，如下图所示。

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240109111525949.png" alt="image-20240109111525949" style="zoom: 67%;" />

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要。

🌰例子：每个镜像层包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![image-20240110101432615](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110101432615.png)

上图中的镜像层跟之前图中的略有区别，主要目的是便于展示文件。

下图中展示了一个稍微复杂的三层镜像，在外部看来整个镜像只有6个文件，这是因为最上层中的文件7是文件5的一个更新版本。

![image-20240110102007568](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110102007568.png)

这种情况下，上层镜像层中的文件覆盖了底层镜像层中的文件。这样就使得文件的更新版本作为一个新镜像层添加到镜像当中。

Docker 通过存储引擎（新版本采用快照机制）的方式来实现镜像层堆栈，并保证多镜像层对外展示为统一的文件系统。

Linux上可用的存储引擎有AUFS、Overlay2、Device Mapper、Btrfs以及ZFS。顾名思义，每种存储引擎都基于Linux中对应的 文件系统或者块设备技术，并且每种存储引擎都有其独有的性能特点。

Docker在Windows 上仅支持 windowsfilter一种存储引擎，该引擎基于NTFS文件系统之上实现了分层和CoW［1］。 

下图展示了与系统显示相同的三层镜像。所有镜像层堆叠并合并，对外提供统一的视图。

![image-20240110102334495](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110102334495.png)

> 特点：Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层就是我们通常说的容器层，容器之下的都叫镜像层。

![image-20240110103749189](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110103749189.png)

### 5.3 commit镜像

```shell
# docker commit 提交容器称为一个新的副本，命令和git原理一致
docker commit -m="提交的描述信息" -a="作者" 容器id 目标镜像名:[tag]
```

实战流程：

```shell
# 1.启动一个默认的tomcat 
docker run -d -p 8080:8080 tomcat:9.0
# 2.发现这个tomcat的webapps中没有内容，拷贝进去基本文件
docker exec -it cd75bd66e53d /bin/bash
cp -r webapps.dist/* webapps
exit
# 3.将操作过的容器通过commit提交为一个镜像
docker commit -m="add webapps application" -a="qinwei" cd75bd66e53d tomcat_temp01
```

![image-20240110105847098](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110105847098.png)

## 6.容器数据卷
### 6.1 什么是容器数据卷
数据存在于容器中，删除容器之后，数据就会丢失！

<span style="color:red">需求：数据持久化</span>

容器之间数据共享的技术——Docker容器中产生的数据，同步到本地——这就是卷技术。目录的挂载，将容器内的目录挂载到Linux上面。

![image-20240110143341757](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110143341757.png)

**容器数据卷的作用是容器持久化和数据同步操作，容器间也是可以数据共享的。**

 ### 6.2 使用数据卷

##### 方式一：使用命令挂载

```shell
docker run -it -v 主机目录:容器目录

# 通过命令查看容器相关信息
docker inspect f780ed985de9

```

![image-20240110144735141](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110144735141.png)

【此时主机内的/home/test目录和docker容器内/home目录是同步的关系】

测试：
1.停止容器
2.宿主机上修改文件
3.再次启动容器查看文件内容 -- 文件内容已经同步

![image-20240110145834449](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110145834449.png)

##### 实战：安装Mysql

思考：Mysql的数据持久化

```shell
# 1.安装Mysql
docker pull mysql
# 2.运行容器，需要做数据挂载；注意：安装启动mysql需要配置密码
docker run -d -p 3310:3306 -v /home/mysql_docker/conf:/etc/mysql/conf.d -v /home/mysql_docker/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql
# 参数说明：
## -d 后台运行  -p 端口映射  -v 卷挂载  -e 环境配置

# 启动成功之后，在本地使用数据库管理软件进行测试
# Navicat-连接到服务器3307------3307和容器内的3306映射

```

假设将容器删除，发现挂载到本地的数据卷依旧没有丢失，这就实现了容器数据持久化功能！

##### 具名挂载与匿名挂载

```shell
# 匿名挂载：
-v 容器内路径
docker run -d -P --name nginx01 -v /etc/nginx nginx
## 查看所有volume的情况
docker volume ls

```

![image-20240110160706616](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110160706616.png)

【这里发现，VOLUME NAME是一串无规律的字符串，这种就是匿名挂载，因为在`-v`后只写了容器内路径，没有写容器外路径】

```shell
# 具名挂载
docker run -d -P --name nginx02 -v juming_nginx:/etc/nginx nginx
## 查看所有volume的情况
docker volume ls
```

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110161021355.png" alt="image-20240110161021355" style="zoom:50%;" />

【通过`-v 卷名:容器内路径`可以实现具名挂载】

查看jumping_nginx的具体位置

```shell
docker volume inspect juming_nginx
```

![image-20240110161334501](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110161334501.png)

✅如何确定是具名挂载还是匿名挂载，还是指定路径挂载？

![image-20240110164021959](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240110164021959.png)

拓展：

```shell
# 通过-v 容器内路径，ro rw 改变读写权限
ro   # read-only ，只读
rw   # read-write，可读写

# 一旦设置了容器权限，容器对我们挂载出来的内容就有限定了
docker run -d -P --name nginx02 -v juming_nginx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v juming_nginx:/etc/nginx:rw nginx

# ro 只要看到ro就说明这个路径只能通过宿主机改变，容器内部无法改变
```

## 7.DockerFile

### 7.1 初识DockerFile

DockerFile是用来构建docker镜像的构建文件！本质上就是一个命令脚本。

通过这个脚本就可以生成镜像，镜像是一层一层的，脚本的命令是一个一个的，每个命令都是一层。

```shell
# 创建一个dockerfile文件，名字可以随意 建议 Dockerfile
# 文件中的内容 指令（大写）
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "----end------"
CMD /bin/bash

# 这里的每个命令，就是镜像的一层
```

启动自己的容器

![image-20240111143913314](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111143913314.png)

这两个数据卷一定在外部存在同步目录。

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111144117812.png" alt="image-20240111144117812" style="zoom:50%;" />

在容器volume01中创建test.txt文件（用于测试同步目录是否存在）

![image-20240111144344738](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111144344738.png)

查看容器的信息

```shell
docker inspect e3ca6f259ea4
```

![image-20240111150742545](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111150742545.png)

(在本机上实验未成功)

### 7.2 数据卷容器

例子：多个mysql同步数据

![image-20240111154405704](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111154405704.png)

命令：`docker run -it --name docker02 --volumes-from docker01 qw/centos:1.0`

通过`--volumes-from`创建的`docker02` 可以同步`docker01`中数据卷内容的变化。

可以通过`--volumes-from`创建的多个容器，及时删除其中一个容器，数据卷volume依旧存在，不会被删除

![image-20240111163103740](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111163103740.png)

![image-20240111163309494](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240111163309494.png)

结论：容器之间配置的信息传递，数据卷容器的生命周期一直持续到没有容器使用为止。一旦持久化到本地，这个时候，本地的数据是不会删除的。

### 7.3 DockerFile

**DockerFile介绍**

DockerFile是用来构建docker镜像的构建文件！本质上就是一个命令脚本。

构建步骤：

1.编写 dockerfile 文件

2.docker build构建成为一个镜像

3.docker run运行镜像

4.docker push发布镜像（dockerhub、阿里云镜像仓库）

👍 **查看官方是如何做的**：

![image-20240112094117083](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240112094117083.png)

![image-20240112094603528](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240112094603528.png)

很多官方的镜像都是基础包，很多功能都没有，我们通常会自己搭建自己的镜像。

**DockerFile构建过程**

基础知识：

1.每个保留关键字（指令）都必须是大写字母

2.执行时按照从上到下顺序执行

3.`#`表示注释

4.每一个指令都会创建提交一个新的镜像层并提交

![image-20240112100406980](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240112100406980.png)

步骤：开发、部署、运维

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品

Docker容器：容器就是镜像运行起来提供服务的

**DockerFile指令**

``` shell
FROM          # 基础镜像，一切从这里开始构建
MAINTAINER    # 镜像是谁写的：姓名+邮箱
RUN           # 镜像构建的时候需要运行的命令
ADD           # 添加内容
WORKDIR       # 镜像的工作目录
VOLUME        # 挂载的目录
EXPOSE        # 指定暴露端口
CMD           # 指定这个容器启动的时候要运行的命令
ENTRYPOINT    # 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD       # 当构建一个被继承DockerFile这个时候就会运行ONBUILD 的指令，触发指令。
COPY          # 类似ADD，将我们的文件拷贝到镜像中
ENV           # 构建的时候设置环境变量
```

![img](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/955fa81e43433b623b0cab1eac39edc8.jpeg)

**实战：构建自己的CentOS**

Docker Hub中99%的镜像都是从`FROM scratch`开始的，然后配置需要的软件和配置来进行构建。

```shell
# 1.编写配置文件
FROM centos:7
MAINTAINER qinwei<qinweirz@qq.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "------Finished--------------"
CMD /bin/bash

# 2.通过上述文件构建镜像
## 命令：docker build -f dockerfile文件路径 -t 镜像名:(ta g) .
docker build -f dockerFileofCentOS -t mycentos:1.0 .

```

使用命令：`docker history 容器名称/容器ID`，可以查看容器的构建过程

![image-20240112162843125](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240112162843125.png)

> CMD 和 ENTRYPOINT 区别
>
> ```shell
> CMD           # 指定这个容器启动的时候要运行的命令，只有最后一个会生效，可被替代
> ENTRYPOINT    # 指定这个容器启动的时候要运行的命令，可以追加命令
> ```

测试CMD

```shell
[root@localhost dockerFile]# vim dockerfile-cmd-test 
-----
FROM centos:7
CMD ["ls","-a"]
-----
[root@localhost dockerFile]# docker build -f dockerfile-cmd-test -t cmdtest .
```

![image-20240116095946392](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116095946392.png)

想追加一个命令

![image-20240116100218001](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116100218001.png)

报错原因：

CMD情况下，-l 替换了 CMD["ls","-a"]命令，-l不是命令所以报错！

执行：`docker run 43232a86a7907f1 ls -al`正确

测试ENTRYPOINT

```shell
[root@localhost dockerFile]# vim dockerfile-entrypoint-test

[root@localhost dockerFile]# docker build -f dockerfile-entrypoint-test -t entrypoint .
[+] Building 0.0s (5/5) FINISHED                                         docker:default
 => [internal] load .dockerignore                                                  0.0s
 => => transferring context: 2B                                                    0.0s
 => [internal] load build definition from dockerfile-entrypoint-test               0.0s
 => => transferring dockerfile: 147B                                               0.0s
 => [internal] load metadata for docker.io/library/centos:latest                   0.0s
 => CACHED [1/1] FROM docker.io/library/centos                                     0.0s
 => exporting to image                                                             0.0s
 => => exporting layers                                                            0.0s
 => => writing image sha256:9f1a36acb55321d87fe7400634a6d558f473f6905d4caf1c6734e  0.0s
 => => naming to docker.io/library/entrypoint    
 
 [root@localhost dockerFile]# docker run 9f1a36acb5532
.
..
.dockerenv
bin
dev
etc
home
lib
lib64
lost+found
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
```

追加命令是直接在ENTRYPOINT命令之后

![image-20240116101824238](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116101824238.png)

**实战：构建自己的Tomcat镜像**

1、准备镜像文件 tomcat压缩包、JDK压缩包

2、编写dockerfile文件【该文件名称采用官方命名`Dockerfile`，原因：`build`时会自动寻找这个文件，不需要`-f`指定】

```shell
FROM centos:7
MAINTAINER qinwei<qinweirz@qq.com>

COPY readme.txt /usr/local/readme.txt
ADD jdk-8u391-linux-aarch64.tar.gz /usr/local
ADD apache-tomcat-9.0.85.tar.gz /usr/local

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_391
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

ENV CATALINA_HOME /usr/local/apache-tomcat-9.0.85
ENV CATALINA_BASH /usr/local/apache-tomcat-9.0.85
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apache-tomcat-9.0.85/bin/startup.sh && tail -F /usr/local/apache-tomcat-9.0.85/bin/logs/catalina.out
```

3、构建镜像

```shell
docker build -t mytomcat .
```

4、启动镜像

```shell
docker run -d -p 9090:8080 -v /home/qinwei/build/tomcat/test:/usr/local/apache-tomcat-9.0.85/webapps/test -v /home/qinwei/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-9.0.85/logs mytomcat
```

5、访问测试

6、发布项目（由于做了卷加载，我们直接在本地编写项目就可以发布）

tomcat文件夹下创建test目录（包含文件 WEB- INF/web.xml 和 index.jsp）

web.xml内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
xmlns="http://java.sun.com/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
</web-app>

```

index.jsp内容如下：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <form action="beandemo.jsp" method="post">
        用户名:<input type="text" name="username">
        密码:<input type="password" name="password">
    </form>
    <b>测试网页成功！！！</b>
</body>
</html>
```

访问时通过`http://ip地址:9090/test`

![image-20240116154727076](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116154727076.png)

### 7.4 发布镜像

#### 7.4.1 发布镜像到docker hub

1、在[Docker Hub](https://hub.docker.com/)网站注册账号

2、在自己的服务器上提交镜像

```shell
# 登录账号  docker login -u 用户名
docker login -u qwrz
# 为镜像添加tag
docker tag 85dc2bf3ea1d qwrz/diytomcat:1.0

# push镜像到dockerHub  /  docker push 作者名（作者名一定和dockerhub用户名保持一致）/镜像名:tag
docker push qwrz/diytomcat:1.0
```

3、发布成功后可在个人主页查看

![image-20240116161612751](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116161612751.png)

#### 7.4.2 发布镜像到阿里云仓库

--- 缺少阿里云账号---

## 8. Docker的全流程小结

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/008i3skNly1gsy8qkirb0j30zw0tiwh9.jpg" alt="docker command" style="zoom:50%;" />




## 9.Docker网络原理

### 9.1 理解Docker0

前置处理：清空所有环境

![image-20240116165034251](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240116165034251.png)

三个网络代表三种环境

> 问题：docker是如何处理容器网络访问的？

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117094226887.png" alt="image-20240117094226887" style="zoom:50%;" />

```shell
docker run -d -p 9090:8080 --name tomcat01 tomcat:7

# 查看容器内部的网络地址，发现容器启动的时候会得到一个eth0的ip地址，这个地址是由docker分配的
docker exec -it tomcat01 ip addr
---
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
136: eth0@if137: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
---

```

思考：Linux能不能ping通容器内部？

Linux可以ping通 docker 容器内部

原理：每启动一个docker容器，docker就会给docker容器分配一个IP，我们只要安装了docker，就会有一个网卡docker0「*这个网卡是桥接模式，使用evth-pair技术*」

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117102951805.png" alt="image-20240117102951805" style="zoom:50%;" />

发现容器的网卡都是一对一对的，`evth-pair`就是一堆的虚拟设备接口，他们都是成对出现的，一端连着协议，一端彼此相连，正是有了这种特性，`evth-pair`充当一个桥梁。 

结论：容器之间是可以互相ping通的。

![image-20240117110415707](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117110415707.png)

结论：tomcat01和tomcat02共用一个路由器docker0。

所有的容器在不指定网络的情况下，都是共用路由器docker0。docker会为每个容器分配一个默认可用的IP。

> 小结：Docker使用的是Linux的桥接模式，宿主机中是一个Docker容器的网桥docker0。

![image-20240117111351178](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117111351178.png)

Docker中所有的网络接口都是虚拟的，虚拟的转发效率高！！

只要容器删除，对应的网桥也会被删除。

### 9.2 --link

> 思考一个场景：我们编写了一个微服务，database url = ip:3306，在项目不重启的情况下，当数据库ip更换后，仍能够正常访问。
>
> 希望能够通过容器名称来实现容器之间的关联。

```shell
[root@localhost ~]# docker exec -it tomcattest01 ping tomcattest
ping: tomcattest: Name or service not known

# 通过--link可以解决tomcattest02连接tomcattest01的网络连接 
[root@localhost ~]# docker run -d -p 9092:8080 --name tomcattest02 --link tomcattest01 tomcat:7

[root@localhost ~]# docker exec -it tomcattest02 ping tomcattest01
PING tomcattest01 (172.17.0.3) 56(84) bytes of data.
64 bytes from tomcattest01 (172.17.0.3): icmp_seq=1 ttl=64 time=0.215 ms
64 bytes from tomcattest01 (172.17.0.3): icmp_seq=2 ttl=64 time=0.117 ms
64 bytes from tomcattest01 (172.17.0.3): icmp_seq=3 ttl=64 time=0.203 ms
```

通过查看tomcattest02的host文件，发现在host文件中配置了访问tomcattest01(172.17.0.3) 

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117142948095.png" alt="image-20240117142948095" style="zoom:50%;" />

本质探究：`--link`就是我们在host配置中增加了一个`172.17.0.3      tomcattest01 efb32c50a670`

**当前已经不在推荐使用此种方式，仅作了解即可。**

### 9.3 自定义网络

查看所有的Docker网络：`docker network ls`

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117144721961.png" alt="image-20240117144721961" style="zoom:50%;" />

**网络模式**

bridge：桥接模式（自定义网络采用bridge模式）

none：不配置网络

host：主机模式，和宿主机共享网络

container：容器网络连通（用得少，局限性很大）

**测试**

```shell
# 我们直接启动命令，默认就是 --net bridge，这个就是docker0[下面两个命令等价]
## docker0的特点：默认域名不能访问，--link可以打通连接 
docker run -d -P --name tomcat01 tomcat
docker run -d -P --name tomcat01 --net bridge tomcat

# 自定义网络
## --driver bridge
## --subnet 192.168.0.0/16   192.168.0.2～192.168.255.255
## --gateway 192.168.0.1
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

[root@localhost ~]# docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
ba9320597b8d   bridge    bridge    local
c4132eafaa93   host      host      local
fb808b9847b8   mynet     bridge    local
356c1878d15f   none      null      local
```

我们自己的网络就创建好了。

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117151911420.png" alt="image-20240117151911420" style="zoom:50%;" />

```shell
[root@localhost ~]# docker run -d -P --name tomcat-net-01 --net mynet tomcat:7
[root@localhost ~]# docker run -d -P --name tomcat-net-02 --net mynet tomcat:7
```

再次查看自己创建的网络，发现下面已经有了两个容器

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117152602710.png" alt="image-20240117152602710" style="zoom:50%;" />

在自定义网络中，通过容器名称可以互相ping通

![image-20240117152921402](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117152921402.png)

在自定义的网络中docker已经帮我们维护好了对应的关系，**因此在使用时推荐使用自定义网络。**

### 9.4 网络连通

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117162258659.png" alt="image-20240117162258659" style="zoom:50%;" />

```shell
# 1.创建tomcat01、tomcat02
docker run -d -P --name tomcat01 tomcat:7
docker run -d -P --name tomcat02 tomcat:7

# 2.创建mynet
docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet

# 3.创建tomcat-net-01、tomcat-net-02
docker run -d -P --name tomcat-net-01 --net mynet tomcat:7
docker run -d -P --name tomcat-net-02 --net mynet tomcat:7\

# 4.tomcat01连接mynet
docker network connect mynet tomcat01

# 测试：tomcat01 ping tomcat-net-01
[root@localhost ~]# docker exec -it tomcat01 ping tomcat-net-01
PING tomcat-net-01 (192.168.0.2) 56(84) bytes of data.
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=1 ttl=64 time=0.246 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=2 ttl=64 time=0.227 ms
64 bytes from tomcat-net-01.mynet (192.168.0.2): icmp_seq=3 ttl=64 time=0.134 ms
```

结论：假设要跨网络操作，就需要使用`docker network connect`连通。

### 9.5 实战：部署Redis集群

<img src="https://cdn.jsdelivr.net/gh/qw-null/BlogImages/image-20240117164052910.png" alt="image-20240117164052910" style="zoom:50%;" />

【图中r-m1、r-m2、r-m3为主机，r-s1、r-s2、r-s3为备用机】

```shell
# 创建网卡
docker network create redis --subnet 172.38.0.0/16

# 通过脚本创建六个redis配置
for port in $(seq 1 6);
do
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat << EOF >>/mydata/redis/node-${port}/conf/redis.conf
port 6379
bind 0.0.0.0
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 启动6个docker容器
for port in $(seq 1 6);
do
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} -v /mydata/redis/node-${port}/data:/data -v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf -d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf
done

# 进入到redis-1中
docker exec -it redis-1 /bin/bash

# 创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13
:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379 --cluster-replicas 1


```

### 9.6  SpringBoot微服务打包Docker镜像

1.构建Spring Boot项目

2.打包应用生成jar包

3.编写Dockerfile

```shell
FROM openjdk:8
# 或者：FROM java:8
LABEL authors="qinwei"

COPY *.jar /app.jar

CMD ["--server.port=8080"]

EXPOSE 8080

ENTRYPOINT ["java","-jar","/app.jar"]
```

4.上传jar包和Dockerfile到指定目录

5.在Dockerfile所在目录下构建镜像

```shell
docker build -t springbootdemo .
```

5.发布运行

```shell
docker run -d -p 9090:8080 --name webdemo springbootdemo
```



