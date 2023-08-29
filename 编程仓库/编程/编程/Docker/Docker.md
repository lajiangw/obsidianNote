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