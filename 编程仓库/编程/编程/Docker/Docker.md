# Docker 简介
Docker 是基于 Go 语言实现的云开源项目。

Docker 的主要目标是：`Build, Ship and Run Any App, Anywhere`，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的 APP 及其运行环境能做到**一次镜像, 处处运行**。


# 传统虚拟机和容器
传统虚拟机（virtual machine）：
	传统虚拟机技术基于安装在主操作系统上的虚拟机管理系统（如VirtualBox、VMware等），创建虚拟机（虚拟出各种硬件），在虚拟机上安装从操作系统，在从操作系统中安装部署各种应用。
	缺点：资源占用多、冗余步骤多、启动慢  

Linux容器（Linux Container，简称LXC）：

Linux容器是与系统其他部分分隔开的一系列进程，从另一个镜像运行，并由该镜像提供支持进程所需的全部文件。容器提供的镜像包含了应用的所有依赖项，因而在从开发到测试再到生产的整个过程中，它都具有可移植性和一致性。

Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。有了容器，就可以将软件运行所需的所有资源打包到一个隔离的容器中。容器与虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和设置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一的运行。



![](https://cdn.nlark.com/yuque/0/2022/jpeg/12911942/1652093324480-073374a2-4ce3-4a56-932b-5a73329736a2.jpeg)

|   |   |   |
|---|---|---|
|特性|容器|虚拟机|
|启动|秒级|分钟级|
|大小|一般为Mb|一般为Gb|
|速度|接近原生|比较慢|
|系统支持数量|单机支持上千个容器|一般几十个|

 **Docker 一句话解析** ：解决了**运行环境和配置问题的软件容器**，方便做持续集成并有助于整体发布的容器虚拟化技术。

# Docker 运行速度快的原因
Docker 有比虚拟机更少的抽象层：

由于 Docker 不需要 Hypervisor（虚拟机）实现硬件资源虚拟化，运行在 Docker 容器上的程序直接使用的都是实际物理机的硬件资源，因此在 CPU、内存利用率上 docker 有明显优势。

Docker 利用的是宿主机的内核，而不需要加载操作系统 OS 内核：

当新建一个容器时，Docker 不需要和虚拟机一样重新加载一个操作系统内核。进而避免引寻、加载操作系统内核返回等比较耗时耗资源的过程。当新建一个虚拟机时，虚拟机软件需要加载 OS，返回新建过程是分钟级别的。而 Docker 由于直接利用宿主机的操作系统，则省略了返回过程，因此新建一个 docker 容器只需要几秒钟。

Docker 容器的本质就是一个进程。

# Docker 软件
安装
[docker官网](https://www.docker.com/)

Docker 并非一个通用的容器工具，它依赖于已经存在并运行的 Linux 内核环境。（在 Windows 上安装 Docker 时需要依赖 WLS，也即 Windows 下的 Linux 子系统）。

Docker 实质上是在已经运行的 Linux 下制造了一个隔离的文件环境，因此它执行的效率几乎等同于所部署的 Linux 主机。

Docker 的基本组成部分：
	- 镜像（image）
	- 容器（container）
	- 仓库（repository）

## Docker 镜像
Docker 镜像就是一个只读的模板。镜像可以用来创建 Docker 容器，一个镜像可以创建多个容器。

可以看作是 java 语言中的类模板

## Docker 容器
Docker 利用容器独立运行的一个或一组应用，应用程序或服务运行在容器里面，容器就类似于一个虚拟化的运行环境，容器是用镜像创建的运行实例。

可以看做是 java new 出来的类实例。再 docker 叫做容器实例。
## Docker 仓库
Docker 仓库是集中存放镜像文件的场所。
仓库分为公开仓库和私有仓库两种。
最大的公开仓库是 Docker 官方的 Docker Hub：[docker仓库](https://hub.docker.com/)
国内网络问题，一般使用阿里云，网易云等。

# Docker 架构
Docker 是一个 C/S（Client-Server） 结构的系统，后端是一个松耦合架构，众多模块各司其职。

Docker 守护进程运行在主机上，然后通过 Socket 连接从客户端访问，守护进程从容器接收命令并管理运行在主机上的容器。

![](imgs/Pasted%20image%2020230827191932.png)

Docker 运行的基本流程为：
1. 用户是使用Docker Client 与 Docker Daemon 建立通信，并发送请求给后者

2. Docker Daemon 作为 Docker 架构的主体部分，首先提供 Docker Server 的功能使其可以接收 Docker Client 的请求

3. Docker Engine 执行 Docker 内部的一系列工作，每一项工作都是以一个 Job 的形式存在

4. Job 的运行过程中，当需要容器镜像时，则从 Docker Registry 中下载镜像，并通过镜像管理驱动 Graph Driver 将下载镜像以 Graph 的形式存储

5. 当需要为 Docker 创建网络环境时，通过网络管理驱动 Network driver 创建并配置 Docker 容器网络环境

6. 当需要限制 Docker 容器运行资源或执行用户指令等操作时，则通过 Exec driver 来完成

7. Libcontainer 是一项独立的容器管理包，Network driver 以及 Exec driver 都是通过 Libcontainer 来实现具体对容器进行的操作
![](imgs/Pasted%20image%2020230827202010.png)

# Docker 安装
卸载旧版本
```
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

旧版本的 Docker 引擎包可能叫做：`docker`、`docker-engine`。

新版本的 Docker 引擎包叫做：`docker-ce`

**配置 yum 资料库**
安装 `yum-config-manager`：
```
# yum-util提供yum-config-manager功能 
sudo yum install -y yum-utils
```

配置 docker 的资源库地址：
这里使用阿里云作为资源库
```docker
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

安装 docker
安装最新版本
```
# docker-ce是Docker引擎，docker-ce-cli是客户端
sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

安装制定版本
```
# 查询版本列表
yum list docker-ce --showduplicates | sort -r

# 指定版本安装17.09.0.ce版
# sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io docker-compose-plugin
sudo yum install docker-ce-17.09.0.ce docker-ce-cli-17.09.0.ce containerd.io docker-compose-plugin
```

**启动 docker**
如果没有启动 Docker 引擎，那么执行 `docker version` 查看版本号时，只能看到 `Client: Docker Engine`（Docker 引擎客户端）的版本号。

```
# 新版本的Docker就是一个系统服务，可以直接使用启动系统服务方式启动
systemctl start docker

# 此时查看docker版本，可以看到Server: Docker Engine（Docker引擎）版本号
docker version
```

**卸载 docker**
```
关闭服务
systemctl stop docker
```

使用 yum 删除 doocker
```
sudo yum remove docker-ce docker-ce-cli containerd.io
```

删除镜像等文件
```
 sudo rm -rf /var/lib/docker
 sudo rm -rf /var/lib/containerd
```

运行 hello world 测试
```
docker run hello-world
```

## 配置 docker 下载加速
方式1：使用网易数帆、阿里云等容器镜像仓库进行下载。
例如，下载网易数帆镜像中的 mysql。（网易数帆的地址为 `hub.c.163.com`，网易数帆对 dockerhub 官方的镜像命名空间为 `library`）。
```
docker pull hub.c.163.com/library/mysql:latest
```

方式 2： 配置阿里云加速
登录阿里云，进入 `工作台` -> `容器镜像服务` -> `镜像工具` -> `镜像加速器`。

里面提供了一个加速器地址：`https://xxxxx.mirror.aliyuncs.com`，将该地址配置到 docker 中：
```
cd /etc/docker

# 初次进来时没有/etc/docker/daemon.json文件，直接创建该文件即可
vi /etc/docker/daemon.json
```

在 `daemon.json` 中写入以下内容：（即加速器地址）
```
{
  "registry-mirrors": ["https://xxxxx.mirror.aliyuncs.com"]  
}
```

然后刷新配置、重启docker即可：
```
# centos6 的命令
sudo chkconfig daemon-reload
sudo service docker restart

# centos7 的命令
sudo systemctl daemon-reload
sudo systemctl restart docker
```

# Docker 常用指令
## 启动类命令
Docker 是一种常用的容器化平台，用于构建、部署和运行应用程序。下面是一些 Docker 常用的启动命令：

1. **docker run**: 从镜像创建并启动一个新的容器。

   `````
   docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
   ```
   示例：
   ````
   docker run -it ubuntu:latest bash
   ````

   `````

1. **docker start**: 启动已经创建但停止的容器。

   ```
   docker start [OPTIONS] CONTAINER [CONTAINER...]
   ```

   示例：

   ```
   docker start mycontainer
   ```

1. **docker stop**: 停止正在运行的容器。

   ```
   docker stop [OPTIONS] CONTAINER [CONTAINER...]
   ```

   示例：

   ```
   docker stop mycontainer
   ```

1. **docker restart**: 重启容器。

   ```
   docker restart [OPTIONS] CONTAINER [CONTAINER...]
   ```

   示例：

   ```
   docker restart mycontainer
   ```

1. **docker pause**: 暂停容器中的所有进程。

   ```
   docker pause CONTAINER [CONTAINER...]
   ```

   示例：

   ```
   docker pause mycontainer
   ```

1. **docker unpause**: 恢复暂停的容器。

   ```
   docker unpause CONTAINER [CONTAINER...]
   ```

   示例：

   ```
   docker unpause mycontainer
   ```

1. **docker exec**: 在运行的容器中执行命令。

   ```
   docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
   ```

   示例：

   ```
   docker exec -it mycontainer bash
   ```

这些是 Docker 的一些常用启动命令，可以根据需要进行适当调整和组合使用。使用 `docker --help` 命令可以查看更多 Docker 命令的详细信息和选项。

## 镜像命令
以下是常用的 Docker 镜像命令及其参数的说明：

1. **docker images**: 列出本地主机上的所有镜像。

   - `OPTIONS`: 可选参数，用于进一步定制输出结果。
   - `REPOSITORY[:TAG]`: 指定要过滤的镜像仓库和标签。

   示例：

   ````
   docker images
   docker images -a           # 显示所有镜像，包括中间层镜像
   docker images myrepo/myimage   # 过滤指定仓库的镜像
   docker images myrepo/myimage:tag   # 过滤指定仓库和标签的镜像
   ```

   ````

1. **docker pull**: 从镜像仓库中拉取镜像到本地。

   - `OPTIONS`: 可选参数，用于配置拉取行为。
   - `NAME[:TAG|@DIGEST]`: 镜像的名称、标签或摘要。

   示例：

   ```
   docker pull ubuntu:latest
   ```

1. **docker push**: 将本地的镜像推送到镜像仓库。

   - `OPTIONS`: 可选参数，用于配置推送行为。
   - `NAME[:TAG]`: 镜像的名称和标签。

   示例：

   ```
   docker push myrepo/myimage:tag
   ```

1. **docker rmi**: 删除本地的一个或多个镜像。

   - `OPTIONS`: 可选参数，用于配置删除行为。
   - `IMAGE [IMAGE...]`: 要删除的镜像名称或 ID。

   示例：

   ```
   docker rmi myimage:tag
   ```

1. **docker build**: 根据 Dockerfile 构建镜像。

   - `OPTIONS`: 可选参数，用于配置构建行为。
   - `PATH | URL | -`: Dockerfile 的路径、URL 或者 `-`（从标准输入读取）。

   示例：

   ```
   docker build -t myimage:tag .
   ```

1. **docker tag**: 为镜像添加标签。

   - `SOURCE_IMAGE[:TAG]`: 要标记的源镜像名称和标签。
   - `TARGET_IMAGE[:TAG]`: 标记后的目标镜像名称和标签。

   示例：

   ```
   docker tag myimage:tag myrepo/myimage:tag
   ```

1. **docker history**: 显示镜像的历史记录。

   - `OPTIONS`: 可选参数，用于配置输出格式。
   - `IMAGE`: 要查看历史记录的镜像名称或 ID。

   示例：

   ```
   docker history myimage:tag
   ```

1. **docker inspect**: 获取有关镜像的详细信息。

   - `OPTIONS`: 可选参数，用于配置输出格式。
   - `NAME|ID`: 要检查的镜像名称或 ID。

   示例：

   ```
   docker inspect myimage:tag
   ```

这些命令和参数可以帮助你管理和操作 Docker 镜像。你可以根据需要使用适当的参数来定制命令行为。使用 `docker --help` 命令可以查看更多 Docker 命令的详细信息和选项。

## 容器命令
新建启动容器
```
docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

常用的参数：
- `--name`：为容器指定一个名称
- `-d`：后台运行容器并返回容器ID，也即启动守护式容器
- `-i`：以交互模式（interactive）运行容器，通常与`-t`同时使用
- `-t`：为容器重新分配一个伪输入终端（tty），通常与`-i`同时使用。也即启动交互式容器（前台有伪终端，等待交互）
- `-e`：为容器添加环境变量
- `-P`：随机端口映射。将容器内暴露的所有端口映射到宿主机随机端口
- `-p`：指定端口映射

`-p`指定端口映射的几种不同形式：
- `-p hostPort:containerPort`：端口映射，例如`-p 8080:80`
- `-p ip:hostPort:containerPort`：配置监听地址，例如 `-p 10.0.0.1:8080:80`
- `-p ip::containerPort`：随机分配端口，例如 `-p 10.0.0.1::80`
- `-p hostPort1:containerPort1 -p hostPort2:containerPort2`：指定多个端口映射，例如 `-p 8080:80 -p 8888:3306`

 **启动交互式容器**
 ```
 # -i 交互模式
# -t 分配一个伪输入终端tty
# ubuntu 镜像名称
# /bin/bash（或者bash） shell交互的接口
docker run -it ubuntu /bin/bash
```

退出交互模式：
```
exit
```

方式2：使用快捷键 `ctrl` + `P` + `Q`
方式1 退出后，容器会停止；
方式2 退出后容器依然正在运行。

**启动守护式容器**
大部分情况下，我们系统 docker 容器服务时在后台运行的，可以通过 `-d` 指定容器的后台运行模式：
```
docker run -d 容器名
```

注意事项：
	如果使用`docker run -d ubuntu`尝试启动守护式的ubuntu，会发现容器启动后就自动退出了。
	因为 Docker 容器如果在后台运行，就必须要有一个前台进程。容器运行的命令如果不是那些一直挂起的命令（例如 `top`、`tail`），就会自动退出。

**列出正在运行的容器**
```
docker ps [OPTIONS]
```

常用参数：
- `-a`：列出当前所有正在运行的容器+历史上运行过的容器
- `-l`：显示最近创建的容器
- `-n`：显示最近n个创建的容器
- `-q`：静默模式，只显示容器编号

**启动停止的容器**
```
docker start 容器ID或容器名
```

**重启容器**
```
docker restart 容器ID或容器名
```

**停止容器**
```
docker stop 容器ID或容器名
```
**强制停止容器**
```
docker kill 容器ID或容器名
```
**删除容器**
删除已经停止的容器：
```
docker rm 容器ID或容器名
```

强制删除正在运行的容器：
```
docker rm -f 容器ID或容器名
```

一次删除多个容器实例：
```
docker rm -f ${docker ps -a -q}

# 或者
docker ps -a -q | xargs docker rm
```

**查看容器日志**
```
docker logs 容器ID或容器名
```

**查看容器内运行的进程**
```
docker top 容器ID或容器名
```

**查看容器内部细节**
```
docker inspect 容器ID或容器名
```

**进入正在运行的容器**
进入正在运行的容器，并以命令行交互：
```
docker exec -it 容器ID bashShell
```

重新进入
```
docker attach 容器ID
```

`docker exec` 和 `docker attach` 区别：
- `attach`直接进入容器启动命令的终端，不会启动新的进程，用`exit`退出会导致容器的停止
- `exec`是在容器中打开新的终端，并且可以启动新的进程，用`exit`退出不会导致容器的停止
如果有多个终端，都对同一个容器执行了 `docker attach`，就会出现类似投屏显示的效果。一个终端中输入输出的内容，在其他终端上也会同步的显示。

**容器和宿主机文件拷贝**
容器内文件拷贝到宿主机：
```
docker cp 容器ID:容器内路径 目的主机路径
```

宿主机文件拷贝到容器中：
```
docker cp 主机路径 容器ID:容器内路径
```

**导入和导出容器**
`export`：导出容器的内容流作为一个 tar 归档文件（对应 `import` 命令）；
`import`：从 tar 包中的内容创建一个新的文件系统再导入为镜像（对应 `export` 命令）；

```
# 导出
# docker export 容器ID > tar文件名
docker export abc > aaa.tar

# 导入
# cat tar文件 | docker import - 自定义镜像用户/自定义镜像名:自定义镜像版本号
cat aaa.tar | docker import - test/mytest:1.0.1
```

**将容器生成新镜像**
其实可以理解就是扩张自己的容器，自己定制属于自己的容器，添加新功能或者其他，将当前修改好的镜像作为一个新的镜像让我们去使用。
**例如：默认自带的 centos 7 镜像没有 vim 指令，我们可以自己添加 vim 指令，并把添加好的作成一个新的镜像供我们使用**
`docker commit` 提交容器副本使之成为一个新的镜像。

docker 启动一个镜像容器后，可以在里面执行一些命令操作，然后使用 `docker commit` 将新的这个容器快照生成一个镜像。
```
docker commit -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[tag]
```

Docker 挂载主机目录，可能会出现报错：`cannot open directory .: Perission denied`。
解决方案：在命令中加入参数 `--privileged=true`。
CentOS7安全模块比之前系统版本加强，不安全的会先禁止，目录挂载的情况被默认为不安全的行为，在 SELinux 里面挂载目录被禁止掉了。如果要开启，一般使用 `--privileged=true`，扩大容器的权限解决挂载没有权限的问题。也即使用该参数，容器内的 root 才拥有真正的 root 权限，否则容器内的 root 只是外部的一个普通用户权限。 

**容器数据卷**
卷就是目录或文件，存在于一个或多个容器中，由 docker 挂载到容器，但不属于联合文件系统，因此能够绕过 UnionFS，提供一些用于持续存储或共享数据。

特性：卷设计的目的就是数据的持久化，完全独立于容器的生存周期，因此 Docker 不会在容器删除时删除其挂载的数据卷。
特点：
- 数据卷可以在容器之间共享或重用数据

- 卷中的更改可以直接实施生效

- 数据卷中的更改不会包含在镜像的更新中

- 数据卷的生命周期一直持续到没有容器使用它为止


运行一个带有容器卷存储功能的容器实例：
```
docker run -it --privileged=true -v 宿主机绝对路径目录:容器内目录[rw | ro] 镜像名
```
可以使用 `docker inspect` 查看容器绑定的数据卷。
权限：
- `rw`：读写
- `ro`：只读。如果宿主机写入内容，可以同步给容器内，容器内可以读取。

容器卷的继承：
**继承也会继承他的挂载路径和规则**
```
# 启动一个容器
docker run -it --privileged=true /tmp/test:/tmp/docker --name u1 ubuntu /bin/bash

# 使用 --volumes-from 继承 u1的容器卷映射配置
docker run -it --privileged=true --volumes-from u1 --name u2 ubuntu
```


## 总结
![](imgs/Pasted%20image%2020230829122927.png)

# Docker 镜像
镜像是一种轻量级、可执行的独立软件包，它包含运行某个软件所需的所有内容，我们把应用程序和配置依赖打包好行程一个可交付的运行环境（包括代码、运行时需要的库、环境变量和配置文件等），这个打包好的运行环境就是 image 镜像文件。 
##  Docker 镜像加载原理
### 联合文件系统
Docker 中的文件存储驱动叫做 storage driver。
Docker 最早支持的 stotage driver 是 AUFS，它实际上由一层一层的文件系统组成，这种层级的文件系统叫 UnionFS。

联合文件系统（UnionFS）：Union 文件系统，是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（unite serveral directories into a single virtual filesystem）。
Union文件系统是Docker镜像的基础。镜像可以通过分层来进行集成，基于基础镜像可以制作具体的应用镜像。
特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录。

后来出现的 docker 版本中，除了 AUFS，还支持 OverlayFS、Btrfs、Device Mapper、VFS、ZFS 等 storage driver。

## bootfs和rootfs
bootfs（boot file system）主要包含 bootloader 和 kernel，bootloader 主要是引导加载 kernel，Linux 刚启动时会加载 bootfs 文件系统。

在 Docker 镜像的最底层是引导文件系统 bootfs。这一层与我们典型的 Linux/Unix 系统是一样的，包含 boot 加载器和内核。当 boot 加载完成之后整个内核就都在内存中了，此时内存的使用权已经由 bootfs 转交给内核，此时系统也会卸载 bootfs。

rootfs（root file system），在 bootfs 之上，包含的就是典型 Linux 系统中的 `/dev`、`/proc`、`/bin`、`/etc` 等标准目录和文件。rootfs 就是各种不同的操作系统发行版，比如 Ubuntu、CentOS 等。 

docker 镜像底层层次：
![](imgs/Pasted%20image%2020230829142042.png)

对于一个精简的 OS，rootfs 可以很小，只需要包括最基本的命令、工具和程序库就可以了，因为底层直接使用 Host 的 Kernel，自己只需要提供 rootfs 就可以。所以，对于不同的 Linux 发行版，bootfs 基本是一致的，rootfs 会有差别，不同的发行版可以共用 bootfs。 

**这样做的目的就是快速加载，功能复用。**
## 镜像分层
  Docker 支持扩展现有镜像，创建新的镜像。新镜像是从 base 镜像一层一层叠加生成的。 
  ```
  # Version: 0.0.1
FROM debian  # 直接在debain base镜像上构建
MAINTAINER mylinux
RUN apt-get update && apt-get install -y emacs # 安装emacs
RUN apt-get install -y apache2 # 安装apache2
CMD ["/bin/bash"] # 容器启动时运行bash
```

**镜像分层的一个最大好处就是共享资源，方便复制迁移，方便复用。**

## 容器层
当容器启动时，一个新的**可写层**将被加载到镜像的顶部，这一层通常被称为 `容器层`，容器层之下的都叫 `镜像层`。 

所有对容器的改动，无论添加、删除、还是修改文件都只会发生在容器层中。 

只有容器层是可写的，容器层下面的所有镜像层都是只读的。 

如图：
![](imgs/Pasted%20image%2020230829142304.png)

# Registry 搭建 docker 私仓
Docker Registry 是官方提供的工具，用于构建私有镜像仓库。

Docker Registry 也是 Docker Hub 提供的一个镜像，可以直接拉取运行。

1. 拉取镜像
```
docker pull registry
```

2. 启动 Docker Registry
```
docker run -d -p 5000:5000 -v /app/myregistry/:/tmp/registry --privileged=true registry
```

3. 验证（查看私服中的所有镜像）
```
curl http://192.168.xxx.xxx:5000/v2/_catalog
```

## 向 Registry 私仓中上传镜像
修改 `/etc/docker/daemon.json`，添加 `insecure-registries` 允许 http：
```
{
    "registry-mirros": ["https://xxxx.mirror.aliyuncs.com"],
    "insecure-registries": ["192.168.xxx.xxx:5000"]
}
```

然后重启 docker：（新版本的 docker 会立即生效）
```
# centos6 的命令
sudo chkconfig daemon-reload
sudo service docker restart

# centos7 的命令
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 推送到私仓
1. 添加一个对应私仓地址的 tag
```
docker tag lee/myubuntu:1.0.1 192.168.xxx.xxx:5000/lee/myubuntu:1.0.1
```

2. push 到私仓
```
docker push 192.168.xxx.xxx:5000/lee/myubuntu:1.0.1
```

3. 查看私仓中镜像目录验证
```
curl http://192.168.xxx.xxx:5000/v2/_catalog
```

拉取验证：
```
docker pull 192.169.xxx.xxx:5000/lee/myubuntu:1.0.1
```

# Docker 安装 Mysql
正常在 Docker 中安装 mysql 会有两个坑，
- 一个是默认的字符编码会导致插入中文乱码。
- Docker 中的 mysql 需要持久化数据文件。否则容器关闭数据丢失。

在 `/app/mysql/conf` 下新建 `my.cnf`，通过容器卷同步给 mysql 实例，解决中文乱码问题：
```
[client]
default_character_set=utf8
[mysqld]
collation_server = utf8_general_ci
character_set_server = utf8
```

重启 mysql 容器，使得容器重新加载配置文件：
```
docker restart mysql
```

此时便解决了中文乱码（中文插入报错）问题。
而且因为启动时将容器做了容器卷映射，将 mysql 的配置（映射到 `/app/mysql/conf`）、数据（映射到 `/app/mysql/data`）、日志（映射到 `/app/mysql/log`）都映射到了宿主机实际目录，所以即使删除了容器，也不会产生太大影响。只需要再执行一下启动 Mysql 容器命令，容器卷还按相同位置进行映射，所有的数据便都可以正常恢复。

**使用挂载容器卷解决数据持久化问题**
```
docker run -d -p 3306:3306 \
           --privileged=true \
           -v /app/mysql/log:/var/log/mysql \
           -v /app/mysql/data:/var/lib/mysql \
           -v /app/mysql/conf:/etc/mysql/conf.d \
           -e MYSQL_ROOT_PASSWORD=root \
           --name mysql \
           mysql:5.7
```

## Mysql 主从复制安装
安装主服务器容器实例（端口号3307）：
1. 启动容器实例
```
docker run -it -p 3307:3306 \
           --name mysql-master \
           --privileged=true \
           -v /app/mysql-master/log:/var/log/mysql \
           -v /app/mysql-master/data:/var/lib/mysql \
           -v /app/mysql-master/conf:/etc/mysql \
           -e MYSQL_ROOT_PASSWORD=root \
           -d mysql:5.7 /bin/bash
```

2. 进入`/app/mysql-master/conf`，新建`my.cnf`配置文件：
```
[mysqld]
## 设置server_id, 同一个局域网中需要唯一
server_id=101
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能
log-bin=mall-mysql-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断
## 如：1062错误是指一些主键重复，1032错误是因为主从数据库数据不一致
slave_skip_errors=1062
```

3. 重启容器实例
```
docker restart mysql-master
```

4. 进入容器实例内
```
docker exec -it mysql-master /bin/bash
```

5. 登录 mysql，创建数据同步用户
```
-- 首先使用 mysql -uroot -p 登录mysql
-- 创建数据同步用户
create user 'slave'@'%' identified by '123456';
-- 授权
grant replication slave, replication client on *.* to 'slave'@'%';
flush privileges;
```

安装从服务器容器实例（端口号3308）：
1. 启动容器服务：
```
docker run -p 3308:3306 \
           --name mysql-slave \
           --privileged=true \
           -v /app/mysql-slave/log:/var/log/mysql \
           -v /app/mysql-slave/data:/var/lib/mysql \
           -v /app/mysql-slave/conf:/etc/mysql \
           -e MYSQL_ROOT_PASSWORD=root \
           -d mysql:5.7
```

2. 进入 `/app/mysql-slave/conf` 目录，创建 `my.cnf` 配置文件：
```
[mysqld]
## 设置server_id, 同一个局域网内需要唯一
server_id=102
## 指定不需要同步的数据库名称
binlog-ignore-db=mysql
## 开启二进制日志功能，以备slave作为其它数据库实例的Master时使用
log-bin=mall-mysql-slave1-bin
## 设置二进制日志使用内存大小（事务）
binlog_cache_size=1M
## 设置使用的二进制日志格式（mixed,statement,row）
binlog_format=mixed
## 二进制日志过期清理时间。默认值为0，表示不自动清理
expire_logs_days=7
## 跳过主从复制中遇到的所有错误或指定类型的错误，避免slave端复制中断
## 如：1062错误是指一些主键重复，1032是因为主从数据库数据不一致
slave_skip_errors=1062
## relay_log配置中继日志
relay_log=mall-mysql-relay-bin
## log_slave_updates表示slave将复制事件写进自己的二进制日志
log_slave_updates=1
## slave设置只读（具有super权限的用户除外）
read_only=1
```

3. 修改完配置需要重启 slave 容器实例
```
docker restart mysql-slave
```

在主数据库中查看主从同步状态：
1. 进入主数据库容器：
```
docker exec -it mysql-master /bin/bash
```

2. 进入 Mysql
```
mysql -uroot -p
```

3. 查看主从同步状态
```
show master status;
```

主要查看返回结果的文件名 `File`、当前位置 `Position`

进入从数据库容器，配置主从复制：
1. 进入从数据库容器：
```
docker exec -it mysql-slave /bin/bash
```

2. 登录数据库

3. 配置从数据库所属的主数据库：
```
-- 格式：
-- change master to master_host='宿主机ip',master_user='主数据库配置的主从复制用户名',master_password='主数据库配置的主从复制用户密码',master_port=宿主机主数据库端口,master_log_file='主数据库主从同步状态的文件名File',master_log_pos=主数据库主从同步状态的Position,master_connect_retry=连接失败重试时间间隔（秒）;

change master to master_host='192.168.xxx.xxx',master_user='slave',master_password='123456',master_port=3307,master_log_file='mall-mysql-bin.000001',master_log_pos=769,master_connect_retry=30;
```

4. 查看主从同步状态：
```
# \G 可以将横向的结果集表格转换成纵向展示。
# slave status的字段比较多，纵向展示比友好
show slave status \G;
```

除了展示刚刚配置的主数据库信息外，主要关注 `Slave_IO_Running`、`Slave_SQL_Running`。目前两个值应该都为 `No`，表示还没有开始。 
5. 开启主从同步：
```
start slave;
```

6. 再次查看主从同步状态，`Slave_IO_Running`、`Slave_SQL_Running` 都变为 `Yes`。

主从复制测试：
1. 在主数据库上新建库、使用库、新建表、插入数据
```
create database db01;
use db01;
create table t1 (id int, name varchar(20));
insert into t1 values (1, 'abc');
```

2. 在从数据库上使用库、查看记录
```
show databases;
use db01;
select * from t1;
```



# Docker 安装 redis
配置文件、数据文件都和容器卷进行映射。

1. 宿主机创建目录 `/app/redis`
2. 在 `/app/redis` 下创建文件 `redis.conf`，主要修改以下几项配置
```
# 开启密码验证（可选）
requirepass 123

# 允许redis外地连接，需要注释掉绑定的IP
# bind 127.0.0.1

# 关闭保护模式（可选）
protected-mode no

# 注释掉daemonize yes，或者配置成 daemonize no。因为该配置和 docker run中的 -d 参数冲突，会导致容器一直启动失败
daemonize no

# 开启redis数据持久化， （可选）
appendonly yes
```

即最后的配置文件为：
3. 启动 docker 容器：（因为要使用自定义的配置文件，所以需要指定容器运行的命令为 `redis-server 容器内配置文件路径`）
```
docker run -d -p 6379:6379 --name redis --privileged=true \
           -v /app/redis/redis.conf:/etc/redis/redis.conf \
           -v /app/redis/data:/data \
           redis:6.0.8 \
           redis-server /etc/redis/redis.conf
```

# 集群存储算法
## 分布式存储算法
分布式存储的常见算法：
	- 哈希取余算法分区
	- 一致性哈希算法分区
	- 哈希槽算法分区

## 哈希取余算法

算法描述：`hash(key) % N`（其中，`key` 是要存入 Redis 的键名，`N` 是 Redis 集群的机器台数）。用户每次读写操作，都是根据传入的键名经过哈希运算，对机器台数取余决定该键存储在哪台服务器上。 
优点：简单直接有效，只需要预估好数据规划好节点，就能保证一段时间的数据支撑。使用Hash算法让固定的一部分请求落到同一台服务器上，这样每台服务器固定处理一部分请求（并维护这些请求的信息），起到负载均衡+分而治之的作用。
缺点：原来规划好的节点，如果进行了扩容或者缩容，导致节点有变动，映射关系需要重新进行计算。在服务器个数固定不变时没问题，如果需要弹性扩容或者故障停机的情况下，原来取模公式中的 `N` 就会发生变化，此时经过取模运算的结果就会发生很大变化，导致根据公式获取的服务器变得不可控。 
**也就是当我们集群扩容或者某一天机器宕机之后，此时取余操作就不好使用了。如果正好数据落到了宕机的机器，就会造成事故。**

## 一致性哈希算法
算法背景：一致性哈希算法是**为了解决哈希取余算法中的分布式缓存数据变动和映射问题**。当服务器个数发生变化时，尽量减少影响到客户端与服务器的映射关系。

算法描述：
一致性哈希算法必然有个hash函数并按照算法产生Hash值，这个算法的所有可能哈希值会构成一个全量集，这个集合可以成为一个Hash区间`[0, 2^32 - 1]`，这是一个线性空间。但是在这个算法中，我们通过适当的逻辑控制将它首尾相连（`0 = 2^32`），这样让它逻辑上形成了一个环形空间。

它也是按照使用取模的方式。前面的哈希取余算法是对节点个数进行取模，而一致性哈希算法是对 `2^32` 取模。

简单来说，一致性 Hash 算法将整个哈希值空间组成一个虚拟的圆环。如假设某个哈希函数 `H` 的值空间为 `0` 到 `2^32 - 1`（即哈希值是一个32位无符号整形），整个哈希环如下图：整个空间按顺时针方向组织，圆环的正上方的点代表0，0点右侧的第一个点代表1，以此类推，2、3、4....... 直到 `2^32 - 1`，也就是说0点左侧的第一个点代表 `2^32 - 1`。0 和 `2^32 - 1` 在零点钟方向重合，我们把这个由 `2^32` 个点组成的圆环称为 Hash 环。
![](imgs/Pasted%20image%2020230830153334.png)

有了哈希环之后，还需要进行节点映射，将集群中各个 IP 节点映射到环上的某一个位置。 

将各个服务器使用 Hash 进行一个哈希，具体可以选择服务器的 IP 或主机名作为关键字进行哈希。这样每台机器就能确定其在哈希环上的位置。 

假如4个节点 NodeA、B、C、D，经过 IP 地址的哈希函数计算（`hash(ip)`），使用 IP 地址哈希值后在环空间的位置如下：
![](imgs/Pasted%20image%2020230830164452.png)

`key` 落到服务器的落键规则。当我们需要存储一个 `key` 键值对时，首先计算 `key` 的 `hash` 值（`hash(key)`），将这个 `key` 使用相同的函数 hash，计算出哈希值并确定此数据在环上的位置，从此位置沿环顺时针“行走”，第一台遇到的服务器就是其应该定位到的服务器，并将该键值对存储字该节点上。 

假如我们有 ObjectA、B、C、D 四个数据对象，经过哈希计算后，在环空间上的位置如下：根据一致性 hash 算法，数据 A 会被定位到 NodeA 上，B 被定位到 NodeB 上，C 被定位到 NodeC 上，D 被定位到 NodeD 上。
![](imgs/Pasted%20image%2020230830164628.png)

假设 NodeC 宕机，可以看到此时对象 A、B、D 不会受到影响，只有 C 对象被重新定位到 NodeD。 
一般的，在一致性 Hash 算法中，如果一台服务器不可用，则受影响的数据仅仅是此服务器到其环空间中前一台服务器（即沿着逆时针方向行走遇到的第一台服务器）之间的数据，其他不会受到影响。 
即：假设 NodeC 宕机，只会影响到 Hash 定位到 NodeB 到 NodeC 之间的数据，并且这些数据会被转移到 NodeD 进行存储。 
![](imgs/Pasted%20image%2020230830164717.png)

假如需要扩容增加一台节点 NodeX，NodeX 的 `hash(ip)` 位于 NodeB 和 NodeC 之间，那受到影响的就是 NodeB 到 NodeX 之间的数据。重新将 B 到 X 的数据录入到 X 节点上即可，不会导致 Hash 取余全部数据重新洗牌的后果。 
![](imgs/Pasted%20image%2020230830164736.png)

但是 Hash 环会存在数据倾斜问题。
一致性 Hash 算法在**服务节点太少时，容易因为节点分布不均匀而造成数据倾斜**（被缓存的对象都集中到某一台或某几台服务器）。
![](imgs/Pasted%20image%2020230830164759.png)

为了解决数据倾斜问题，一致性哈希算法引入了虚拟节点机制。

对每一个服务节点计算多个哈希，每个计算结果位置都放置一个此服务节点，称为虚拟节点。具体做法可以先确定每个物理节点关联的虚拟节点数量，然后在 IP 或主机名后面加上编号。

例如，可以对 NodeA 节点虚拟出 NodeA #1 、NodeA #2 、NodeA #3 ，对 NodeB 虚拟出 NodeB #1 、NodeB #2 、NodeB # 3的节点，形成六个虚拟节点。
![](imgs/Pasted%20image%2020230830164844.png)

优点：加入和删除节点时，只会影响哈希环中顺时针方向相邻节点，对其他节点无影响。

缺点：数据的分布和节点的位置有关，因为这些节点不是均匀分布在哈希环上的，所以在数据进行存储时达不到均匀部分的效果。

## 哈希槽分区
哈希槽分区是为了解决一致性哈希算法的数据倾斜问题。

哈希槽实质上就是一个数组，数组 `[0, 2^14 - 1]`形成的 hash slot空间。

目的是为了解决均匀分配的问题。在数据和节点之间又加入了一层，把这层称之为槽（slot），用于管理数据和节点之间的关系。就相当于节点上放的是槽，槽里面放的是数据。

![](imgs/Pasted%20image%2020230830170115.png)

槽解决的是粒度问题，相当于把粒度变大了，这样便于数据移动。

哈希解决的是映射问题，使用`key`的哈希值来计算所在的槽，便于数据分配。

一个集群只能有 16394个槽，编号 0 - 16383（`2^14 - 1`）。这些槽会分配给集群中所有的主节点，分配策略没有要求。可以指定哪些编号的槽分配给哪个主节点，集群会记录节点和槽的对应关系。

解决了节点和槽的关系后，接下来就需要对 `key` 求哈希值，然后对16384取余，根据余数决定 `key` 落到哪个槽里。
```
slot = CRC16(key) % 16384
```

以槽为单位移动数据，因为槽的数目是固定的，处理起来比较容易，这样数据移动问题就解决了。 

## Redis 集群存储策略
Redis 集群使用的就是哈希槽。Redis 集群有16384个哈希槽，每个 `key` 通过 `CRC16` 校验后对16384取模来决定放置在哪个槽，集群的每个节点负责一部分 hash 槽。 

哈希槽数量16384（`2^14`）的决定原因：
`CRC16` 算法产生的 hash 值有 16bit，该算法可以产生 `2^16` = 65536个值。但是为了心跳方便和数据传输最大化，槽的数量只能有 `2^14` 个。 

1. 如果槽位数量为65535个，那么发送心跳信息的消息头将达到 8k，发送的心跳包过于庞大。在消息头中最占空间的是 `myslots[CLUSTER_SLOTS/8]`。当槽位为65536时，这块的大小是 ： 
```
65536 ÷ 8 ÷ 1024 = 8Kb
```
每秒中 redis 节点需要发送一定数量的 ping 消息作为心跳，如果槽位为65536，那么这个 ping 消息头就会太大浪费带宽。 

2. redis 集群的主节点数量基本不可能超过1000个。集群节点越多，心跳包的消息体内携带的数据越多。如果节点超过1000个，也会导致网络拥堵。因此 redis 作者不建议 redis cluster 节点超过1000个。对于节点数在1000以内的 redis cluster 集群，16384个槽位足够了，没有必要扩展到65536个。 

3. 槽位越小，节点少的情况下压缩比越高，容易传输。Redis 主节点的配置信息中它锁负责的哈希槽是通过一张 bitmap 的形式来保存的，在传输过程中会对 bitmap 进行压缩，但是如果 bitmap 的填充率 `slots / N`（N 为节点数）很高的话，bitmap 的压缩率就很低。如果节点数很少，而哈希槽数很多的话，bitmap 的压缩率就很低。 

Redis 集群中内置了16384个哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。当需要在 Redis 集群中放置一个 `Key-Value` 时，redis 先对 `key` 使用 `CRC16` 算法算出一个结果，然后把结果对 16384 取余，这样每个 `key` 都会对应一个编号在0-16383之间的哈希槽，也就是映射到某个节点上。 
![](imgs/Pasted%20image%2020230830170345.png)

##  3主3从 Redis 集群
使用 docker 搭建3主3从的 Redis 集群，每台主机都对应一台从机。

启动6台 redis 容器
```
# 启动第1台节点
# --net host 使用宿主机的IP和端口，默认
# --cluster-enabled yes 开启redis集群
# --appendonly yes 开启redis持久化
# --port 6381 配置redis端口号
docker run -d --name redis-node-1 --net host --privileged=true -v /app/redis-cluster/share/redis-node-1:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6381

# 启动第2台节点
docker run -d --name redis-node-2 --net host --privileged=true -v /app/redis-cluster/share/redis-node-2:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6382

# 启动第3台节点
docker run -d --name redis-node-3 --net host --privileged=true -v /app/redis-cluster/share/redis-node-3:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6383

# 启动第4台节点
docker run -d --name redis-node-4 --net host --privileged=true -v /app/redis-cluster/share/redis-node-4:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6384

# 启动第5台节点
docker run -d --name redis-node-5 --net host --privileged=true -v /app/redis-cluster/share/redis-node-5:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6385

# 启动第6台节点
docker run -d --name redis-node-6 --net host --privileged=true -v /app/redis-cluster/share/redis-node-6:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6386
```

构建主从关系：
```
docker exec -it redis-node-1 /bin/bash
```

2. 构建主从关系：
```
# 宿主机IP:端口
redis-cli --cluster create 192.168.xxx.xxx:6381 192.168.xxx.xxx:6382 192.168.xxx.xxx:6383 192.168.xxx.xxx:6384 192.168.xxx.xxx:6385 192.168.xxx.xxx:6386 --cluster-replicas 1
```

3. redis 尝试自动进行主从节点分配

4. 因为我们的 docker 容器 IP 相同，所以会出现警告，可以直接忽略该警告
```
[WARNING] Some slaves are in the same host as their master
```

5. redis 自动分配结果完成后，需要输入 `Yes` 确认配置信息

6. 输入 `Yes` 确认后，redis 会向其他节点发送信息加入集群，并分配哈希槽
![](imgs/Pasted%20image%2020230830200225.png)

查看集群状态：
1. 进入容器节点1（或集群中其他节点）：
```
docker exec -it redis-node-1 /bin/bash
```

2. 使用 `redis-cli` 连接到 `6381` 节点：
```
redis-cli -p 6381
```

3. 使用 `redis` 的相关命令查看集群状态：
```
cluster info
```

其中，分配的哈希槽数量 `cluster_slots_assigned` 为16384，集群节点数量 `cluster_known_nodes` 为6

4. 查看集群节点信息
```
cluster nodes
```

## Redis 集群读写出错
当使用 `redis-cli` 连接 redis 集群时，需要添加 `-c` 参数，否则可能会出现读写出错。
1. 进入容器节点1
```
docker exec -it redis-node-1 /bin/bash
```

2. 使用 `redis-cli` 连接，不加 `-c` 参数时
```
redis-cli -p 6381
```

3. 此时向 redis 中添加键值对，可能会成功，也可能会失败
```
set k1 v1
```

报错：`k1` 经过计算得到的哈希槽为12706，但是当前连接的 redis-server 为 `6381`（即节点1），它的哈希槽为：`[0,5460]`（在创建构建主从关系时 redis 有提示，也可以通过 `cluster nodes` 查看），所以会因为存不进去而报错。 

执行 `set k2 v2` 可以成功，因为 `k2` 计算出的哈希槽在 `[0-5460]` 区间中。
4. 使用 `-c` 参数的 `redis-cli` 命令连接即可
```
redis-cli -p 6381 -c
```

5. 此时可以正常的插入所有数据
```
set k1 v1
```

会有提示信息，哈希槽为12706，重定向到 `6383`（即节点3，哈希槽 `[10923, 16383]`）：

## 集群信息检查
检查查看集群信息：

1. 进入容器节点1
```
docker exec -it redis-node-1 /bin/bash
```

2. 进行集群信息检查
```
# 输入任意一台主节点地址都可以进行集群检查
redis-cli --cluster check 192.168.xxx.xxx:6381
```

返回的检查结果：
```
当前集群中各个节点存储的key的数量
192.168.xxx.xxx:6381 (f451eb48...) -> 0 keys | 5461 slots | 1 slaves.
192.168.xxx.xxx:6383 (1fc935c1...) -> 1 keys | 5461 slots | 1 slaves.
192.168.xxx.xxx:6382 (05984211...) -> 0 keys | 5462 slots | 1 slaves.
[OK] 1 keys in 3 masters.  
0.00 keys per slot on average.

主从机器信息
>>> Performing Cluster Check (using node 192.168.xxx.xxx:6381)
M: f451eb48bbc0a7c31c7da022ffe80cc1696e0f37 192.168.xxx.xxx:6381
   slots:[0-5460] (5461 slots) master
   1 additional replica(s)
M: 1fc935c12b1d34a7df50aed643c195eb29bb3435 192.168.xxx.xxx:6383
   slots:[10923-16383] (5461 slots) master
   1 additional replica(s)
M: 05984211b8c38222a73abeff1d4e459c0fe1efbc 192.168.xxx.xxx:6382
   slots:[5461-10922] (5462 slots) master
   1 additional replica(s)
S: 0c0767e13a09ee48541738d4163592cd9842c143 192.168.xxx.xxx:6386
   slots: (0 slots) slave
   replicates 05984211b8c38222a73abeff1d4e459c0fe1efbc
S: f8d0de47114bf33438747acd713cce4e412ae721 192.168.xxx.xxx:6384
   slots: (0 slots) slave
   replicates 1fc935c12b1d34a7df50aed643c195eb29bb3435
S: de0b393c17e452d856f6de2b348e9ca4e5aa4002 192.168.xxx.xxx:6385
   slots: (0 slots) slave
   replicates f451eb48bbc0a7c31c7da022ffe80cc1696e0f37
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
```

## 主从扩容
假如因为业务量激增，需要向当前3主3从的集群中再加入1主1从两个节点。

1. 启动2台新的容器节点
```
# 启动第7台节点
docker run -d --name redis-node-7 --net host --privileged=true -v /app/redis-cluster/share/redis-node-7:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6387

# 启动第8台节点
docker run -d --name redis-node-8 --net host --privileged=true -v /app/redis-cluster/share/redis-node-8:/data redis:6.0.8 --cluster-enabled yes --appendonly yes --port 6388
```

2. 进入6387（节点7）容器内部
```
docker exec -it redis-node-7 /bin/bash
```

3. 将6387作为 master 加入集群
```
# redis-cli --cluster add-node 本节点地址 要加入的集群中的其中一个节点地址
redis-cli --cluster add-node 192.168.200.130:6387 192.168.200.130:6381
```

4. 检查当前集群状态
```
redis-cli --cluster check 192.168.200.130:6381
```

可以发现，6371节点已经作为 master 加入了集群，但是该节点没有被分配槽位。
5. 重新分配集群的槽位
```
redis-cli --cluster reshard 192.168.200.130:6381
```

redis 经过槽位检查后，会提示需要分配的槽位数量：  
例如，我们现在是4台master，我们想要给node7分配4096个槽位，这样每个节点都是4096个槽位。  
输入`4096`后，会让输入要接收这些哈希槽的节点ID，填入node7的节点ID即可。（就是节点信息中很长的一串十六进制串）。  
然后会提示，询问要从哪些节点中拨出一部分槽位凑足4096个分给Node7。一般选择 `all`，即将之前的3个主节点的槽位都均一些给Node7，这样可以使得每个节点的槽位数相等均衡。  
输入`all`之后，redis会列出一个计划，内容是自动从前面的3台master中拨出一部分槽位分给Node7的槽位，需要确认一下分配的计划。  
输入 `yes` 确认后，redis 便会自动重新洗牌，给 Node7分配槽位。 

重新分配完成后，可以进行集群信息检查，查看分配结果：、
```
redis-cli --cluster check 192.168.xxx.xxx:6381
```

可以发现重新洗牌后的槽位分配为：、
```
节点1：[1365-5460]（供4096个槽位），，，分配前为[0-5460]（共5461个槽位）
节点2：[6827-10922]（共4096个槽位），，，分配前为[5461-10922]（共5461个槽位）
节点3：[12288-16383]（共4096个槽位），，，分配前为[10923-16383]（共5462个槽位）

节点7：[0-1364],[5461-6826],[10923-12287]（共4096个槽位），从每个节点中匀出来了一部分给了节点7
```

因为可能有些槽位中已经存储了 `key`，完全的重新洗牌重新分配的成本过高，所以 redis 选择从前3个节点中匀出来一部分给节点7

为主节点6387分配从节点6388：
```
redis-cli --cluster add-node 192.168.xxx.xxx:6388 192.168.xxx.xxx:6381 --cluster-slave --cluster-master-id node7节点的十六进制编号字符串
```

## 主从缩容
假如业务高峰期过去，需要将4主4从重新缩容到3主3从。即从集群中移除 node8和 node7.

首先删除从节点6388：
1. 进入容器节点1
```
docker exec -it redis-node-1 /bin/bash
```

2. 检查容器状态，获取6388的节点编号
```
1582e58d95f12b0763fd6abf61668ce562eb6850
```

3. 将6388从集群中移除
```
redis-cli --cluster del-node 192.168.xxx.xxx:6388 6388节点编号
```

对 node7重新分配哈希槽：
1. 对集群重新分配哈希槽
```
redis-cli --cluster reshard 192.168.xxx.xxx:6381
```

2. redis 经过槽位检查后，会提示需要分配的槽位数量：
```
How many slots do you want to move (from 1 to 16384)?
```

如果我们想直接把 node7的4096个哈希槽全部分给某个节点，可以直接输入4096。  
输入`4096`后，会让输入要接收这些哈希槽的节点ID。假如我们想把这4096个槽都分给Node1，直接输入node1节点的编号即可。  
然后会提示，询问要从哪些节点中拨出一部分槽位凑足4096个分给Node1。这里我们输入node7的节点编号，回车后输入`done`。

node7上面没有了哈希槽，此时便可以将 node7从集群中移除。（如果 node7上面有哈希槽，直接从集群中移除会报错） 

```
redis-cli --cluster del-node 192.168.xxx.xxx:6387 node7节点编号
```

# Dockerfile
Dockerfile 是用来构建 Docker 镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。 

构建步骤：
1. 编写 Dockerfile 文件

2. `docker build`命令构建镜像

3. `docker run` 依据镜像运行容器实例

## 构建过程
Dockerfile编写：
- 每条保留字指令都必须为大写字母，且后面要跟随至少一个参数
- 指令按照从上到下顺序执行
- `#`表示注释
- 每条指令都会创建一个新的镜像层并对镜像进行提交

Docker引擎执行Docker的大致流程：
1. docker从基础镜像运行一个容器
2. 执行一条指令并对容器做出修改
3. 执行类似`docker commit`的操作提交一个新的镜像层
4. docker再基于刚提交的镜像运行一个新容器
5. 执行 Dockerfile 中的下一条指令，直到所有指令都执行完成

## Dockerfile 保留字
**FROM**
基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板。Dockerfile 第一条必须是 `FROM`
```
# FROM 镜像名
FROM hub.c.163.com/library/tomcat
```

**MAINTAINER**
镜像维护者的姓名和邮箱地址
```
# 非必须
MAINTAINER ZhangSan zs@163.com
```

**RUN**
容器构建时需要运行的命令。
有两种格式：
- shell 格式
```
# 等同于在终端操作的shell命令
# 格式：RUN <命令行命令>
RUN yum -y install vim
```

- exec 格式
```
# 格式：RUN ["可执行文件" , "参数1", "参数2"]
RUN ["./test.php", "dev", "offline"]  # 等价于 RUN ./test.php dev offline
```

`RUN` 是在 `docker build` 时运行

 **EXPOSE**
 当前容器对外暴露出的端口。
 ```
 # EXPOSE 要暴露的端口
# EXPOSE <port>[/<protocol] ....
EXPOSE 3306 33060
```

 WORKDIR
指定在创建容器后，终端默认登录进来的工作目录。
```
ENV CATALINA_HOME /usr/local/tomcat
WORKDIR $CATALINA_HOME
```

**USER**
指定该镜像以什么样的用户去执行，如果不指定，默认是 `root`。（一般不修改该配置）
```
# USER <user>[:<group>]
USER patrick
```

**ENV**
用来在构建镜像过程中设置环境变量
这个环境变量可以在后续的任何`RUN`指令或其他指令中使用
```
# 格式 ENV 环境变量名 环境变量值
# 或者 ENV 环境变量名=值
ENV MY_PATH /usr/mytest

# 使用环境变量
WORKDIR $MY_PATH
```

 **VOLUME**
容器数据卷，用于数据保存和持久化工作。类似于 `docker run` 的 `-v` 参数。
```
# VOLUME 挂载点
# 挂载点可以是一个路径，也可以是数组（数组中的每一项必须用双引号）
VOLUME /var/lib/mysql
```


**ADD**
将宿主机目录下（或远程文件）的文件拷贝进镜像，且会自动处理 URL 和解压 tar 压缩包。

**COPY**
类似 `ADD`，拷贝文件和目录到镜像中。
将从构建上下文目录中 `<源路径>` 的文件目录复制到新的一层镜像内的 `<目标路径>` 位置。
```
COPY src dest
COPY ["src", "dest"]
# <src源路径>：源文件或者源目录
# <dest目标路径>：容器内的指定路径，该路径不用事先建好。如果不存在会自动创建
```

**CMD**
指定容器启动后要干的事情。
有两种格式：
- shell 格式
```
# CMD <命令>
CMD echo "hello world"
```
- exec 格式
```
# CMD ["可执行文件", "参数1", "参数2" ...]
CMD ["catalina.sh", "run"]
```

- 参数列表格式
```
# CMD ["参数1", "参数2" ....]，与ENTRYPOINT指令配合使用
```

Dockerfile 中如果出现多个 `CMD` 指令，只有最后一个生效。`CMD` 会被 `docker run` 之后的参数替换。
例如，对于 tomcat 镜像，执行以下命令会有不同的效果：
```
# 因为tomcat的Dockerfile中指定了 CMD ["catalina.sh", "run"]
# 所以直接docker run 时，容器启动后会自动执行 catalina.sh run
docker run -it -p 8080:8080 tomcat

# 指定容器启动后执行 /bin/bash
# 此时指定的/bin/bash会覆盖掉Dockerfile中指定的 CMD ["catalina.sh", "run"]
docker run -it -p 8080:8080 tomcat /bin/bash
```

 **ENTRYPOINT**
 用来指定一个容器启动时要运行的命令。
 类似于 `CMD` 命令，但是 `ENTRYPOINT` 不会被 `docker run` 后面的命令覆盖，这些命令参数会被当做参数送给 `ENTRYPOINT` 指令指定的程序。 
`ENTRYPOINT` 可以和 `CMD` 一起用，一般是可变参数才会使用 `CMD`，这里的 `CMD` 等于是在给 `ENTRYPOINT` 传参。

当指定了 `ENTRYPOINT` 后，`CMD` 的含义就发生了变化，不再是直接运行期命令，而是将 `CMD` 的内容作为参数传递给 `ENTRYPOINT` 指令，它们两个组合会变成 `<ENTRYPOINT> "<CMD>"`。
```
FROM nginx

ENTRYPOINT ["nginx", "-c"]  # 定参
CMD ["/etc/nginx/nginx.conf"] # 变参
```

对于此 Dockerfile，构建成镜像 `nginx:test` 后，如果执行；
- `docker run nginx test`，则容器启动后，会执行 `nginx -c /etc/nginx/nginx.conf`

- `docker run nginx:test /app/nginx/new.conf`，则容器启动后，会执行 `nginx -c /app/nginx/new.conf`


##  构建镜像
创建名称为 `Dockerfile` 的文件，示例：
```
FROM ubuntu
MAINTAINER lee<lee@xxx.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN apt-get update
RUN apt-get install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "install ifconfig cmd into ubuntu success ....."
CMD /bin/bash
```

编写完成之后，将其构建成 docker 镜像。
```
# 注意：定义的TAG后面有个空格，空格后面有个点
# docker build -t 新镜像名字:TAG .
docker build -t ubuntu:1.0.1 .
```

## 虚悬镜像
虚悬镜像：仓库名、标签名都是 `<none>` 的镜像，称为 dangling images（虚悬镜像）。

在构建或者删除镜像时可能由于一些错误导致出现虚悬镜像。

```
# 构建时候没有镜像名、tag
docker build .
```

列出 docker 中的虚悬镜像：
```
docker image ls -f dangling=true
```

虚悬镜像一般是因为一些错误而出现的，没有存在价值，可以删除：
```
# 删除所有的虚悬镜像
docker image prune
```

# Docker 发布微服务
搭建一个简单的微服务 demo，并打包成 jar 文件，上传到 linux。

编写 dockerfile
```
FROM openjdk:8-oracle
MAINTAINER lee

# 在主机 /var/lib/docker目录下创建一个临时文件，并链接到容器的 /tmp
VOLUME /tmp

# 将jar包添加到容器中，并命名为 springboot_docker.jar
ADD dockerdemo2-1.0-SNAPSHOT.jar /springboot_docker.jar
# 运行jar包
RUN bash -c 'touch /springboot_docker.jar'
ENTRYPOINT ["java", "-jar", "/springboot_docker.jar"]

# SpringBoot项目配置的端口号为6001，需要将6001暴露出去
EXPOSE 6001
```

3. 构建镜像
```
docker build -t springboot_docker:1.0 .
```

4. 启动容器：
```
docker run -d -p 6001:6001 --name springboot springboot_docker:1.0
```

