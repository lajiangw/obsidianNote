# Nginx 相关概念
## 什么是 Nginx ？
Nginx 是一个高性能的 HTTP 和反向代理的服务器，特点是占用内存少，并发能力强。

## 反向代理
正向代理：
我们在客户端（浏览器）设置一个代理，通过代理去访问我们想要访问的网络资源。

反向代理：
我们只需要将请求发送个反向代理服务器，由反向代理服务器去选择目标服务器获取访问资源。再返回给客户端。此时在客户端的视角，反向代理服务器和目标服务器就是一个整体，暴露的方向代理服务器，而不是目标服务器。
![](imgs/Pasted%20image%2020230624180925.png)
## 负载均衡
正常模式访问数据库无法解决并发数量大的问题。
单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器的情况改为将请求分发到多个服务器上，将负载分发到不同的服务器，也就是我们说的负载均衡。
![](imgs/Pasted%20image%2020230624193500.png)

## 动静分离
为了加快网站的解析速度，可以吧动态页面和静态页面由不同的服务器来解析，加快解析速度，降低原来单个服务器的压力。
![](imgs/Pasted%20image%2020230624194050.png)
## 安装 Nginx
可以使用宝塔一键部署

# Nginx 配置文件
配置文件位置在 `根目录下/conf/nginx.conf`

## Nginx 配置文件组成部分
由三个部分组成。
1 全局块
2 evens 块
3 http 块

## 全局块
从配置文件开始到 events 块之间的内容，主要设置一些会影响 nginx 服务器整体运行的配置指令。
例如：`worker_processes auto;` 是处理并发量的指令，值越大处理的并发数量也越大，也跟服务器的性能有关。

## Evens 块
Evens 块涉及的指令主要影响 Nginx 服务器与用户的网络连接。
例如：` worker_connections 51200;` 表示支持的最大连接数。

## HTTP 块
Nginx 服务器配置中最频繁的部分
HTTP 块又包括 `HTTP全局块` 和 `service块`

# Nginx 配置实例
## 反向代理
**通过 nginx 实现 www.123.com ，实现访问 tomcat 官网**
1. 在本地 windows 中 host 文件添加 ip 映射。

`192.168.200.130 www.123.com`
2. 在 nginx 配置反向代理。
![](imgs/Pasted%20image%2020230626090115.png)

3. 访问网址 www.123.com 测试。


**使用 nginx 作为反向代理，根据访问的路径不同跳转到不同的服务中去。Nginx 监听端口为 9001，访问 location: 9001/edu/跳转到 127.0.0.1:8080**
1 准备连个 tomcaot 服务器

2 配置 nginx
```
service{
lissten  9091；
service_name  192.168.200.130;

location ~/edu/{ # 正则表达式匹配
	proxy_pass http:127.0.0.1:8080
}

}
```

3  访问测试即可

**Location 指令说明：**
在 Nginx 中，以下是各种 Location 匹配规则的说明：

1. 精确匹配：
   ```
   location = /path {
       // 配置规则
   }
   ```
   只匹配路径为 "/path" 的请求。

2. 前缀匹配：
   ```
   location /prefix {
       // 配置规则
   }
   ```
   匹配以 "/prefix" 开头的所有路径。

3. 正则表达式匹配（区分大小写）：
   ```
   location ~ ^/pattern {
       // 配置规则
   }
   ```
   使用正则表达式进行匹配。这个示例将匹配以 "/pattern" 开头的路径。

4. 正则表达式匹配（不区分大小写）：
   ```
   location ~* ^/pattern {
       // 配置规则
   }
   ```
   与上述相同，但忽略路径的大小写。

5. 最长前缀匹配：
   ```
   location /path {
       // 配置规则
   }
   ```
   匹配任何以 "/path" 开头的路径，包括 "/path" 本身和以 "/path/" 开头的子路径。

6. 命名位置匹配：
   ```
   location @name {
       // 配置规则
   }
   ```
   定义一个命名的位置块，可以在其他地方使用 `rewrite` 或 `error_page` 指令引用。

7. 匹配任意字符：
   ```
   location ~ .(gif|jpg)$ {
       // 配置规则
   }
   ```
   使用正则表达式匹配以 ". Gif" 或 ". Jpg" 结尾的路径。

8. 匹配任意路径：
   ```
   location / {
       // 配置规则
   }
   ```
   这是一个通用的匹配规则，将匹配所有请求，并被用作默认配置或其他特殊情况。

请注意，Nginx 的 Location 匹配按照配置文件中位置块的顺序进行匹配。因此，在编写配置时，请确保将最具体的规则放在前面，并根据需求进行适当排序，以避免意外匹配或冲突。

## 负载均衡
**访问一个 ip，均分到多台服务器中**
1 准备两个 tomcat 服务器，作为测试。

2 nginx 配置负载均衡
在 Nginx 中进行负载均衡配置可以通过使用 `upstream` 和 `proxy_pass` 指令来实现。下面是一个简单的负载均衡配置示例：

1. 配置后端服务器列表：
   在 `http` 块中使用 `upstream` 指令定义后端服务器列表，指定多个服务器及其权重。
   ```
   http {
       upstream backend {
           server 192.168.1.100 weight=2;
           server 192.168.1.101;
           server 192.168.1.102;
       }
   }
   ```

2. 配置负载均衡方式：
   在 `server` 块中使用 `location` 指令，并配合 `proxy_pass` 指令将请求转发给后端服务器。
   ```
   http {
       upstream backend {
           server 192.168.1.100 weight=2;
           server 192.168.1.101;
           server 192.168.1.102;
       }

       server {
           listen 80;

           location / {
               proxy_pass http://backend;
           }
       }
   }
   ```

上述配置中，`upstream` 指令定义了名为 `backend` 的后端服务器列表，其中包含三个服务器：192.168.1.100、192.168.1.101 和 192.168.1.102。第一个服务器具有权重 2，表示该服务器处理的请求数量是其他服务器的两倍。

然后，在 `server` 块中使用 `location /` 指令，并通过 `proxy_pass` 指令将请求代理到后端服务器组 `http://backend`。Nginx 将负载均衡地将请求分发给后端服务器，实现请求的平衡分流。

在实际应用中，您还可以根据需要配置其他负载均衡相关的参数，如负载均衡算法、健康检查、超时设置等。这样可以更好地控制和优化负载均衡行为。

### Nginx 负载均衡分配策略
在 Nginx 中，可以使用以下几种负载均衡分配策略：

1. 轮询（Round Robin）：
   这是默认的负载均衡策略。Nginx 按照请求的顺序将其分配给每个后端服务器，循环往复。适用于后端服务器性能相当的情况。

   示例配置：
   ```
   upstream backend {
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   ```

2. 权重（Weighted）：
   可以为不同的后端服务器设置权重，以使某些服务器处理更多的请求。可以根据服务器性能和负载能力进行权重分配。

   示例配置：
   ```
   upstream backend {
       server backend1.example.com weight=3;
       server backend2.example.com weight=5;
       server backend3.example.com;
   }
   ```

   在上述示例中，`backend2.example.com` 的权重是其他服务器的两倍。

3. IP 哈希（IP Hash）：
   根据客户端 IP 地址对后端服务器进行哈希，确保同一个客户端的所有请求都会被发送到同一台服务器。适用于需要会话保持的情况。

   示例配置：
   ```
   upstream backend {
       ip_hash;
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   ```

4. 最少连接（Least Connections）：
   动态选择连接数最少的后端服务器来分配请求，以平衡服务器的负载。适用于后端服务器性能不同的情况。

   示例配置：
   ```
   upstream backend {
       least_conn;
       server backend1.example.com;
       server backend2.example.com;
       server backend3.example.com;
   }
   ```

这些是 Nginx 中常见的负载均衡分配策略。根据您的实际需求和后端服务器的特点，选择适合的策略进行配置。请记住，在配置更改后，重新加载或重启 Nginx 服务以使更改生效。

## Nginx 配置动静分离
动静分离不是物理的将动态资源和静态资源进行分离，而是将动态请求和静态请求进行分离。

1 设置动态资源和静态资源

2 在 nginx 进行配置
要在 Nginx 中配置动静分离，您可以根据以下步骤进行操作：

1. 配置静态资源的根目录：
   在 Nginx 配置文件中，指定您存放静态资源（如 CSS、JavaScript、图片等）的根目录。例如：
   ```
   server {
       ...
       root /path/to/static/files;
       ...
   }
   ```

2. 配置动态请求的代理转发：
   在 Nginx 配置文件中，使用 `location` 指令和 `proxy_pass` 指令将动态请求转发给后端应用程序处理。根据您的实际情况进行配置。以下是一个示例：
   ```
   server {
       ...
       location /dynamic/ {
           proxy_pass http://backend_server;
       }
       ...
   }
   ```

   在上面的示例中，以 `/dynamic/` 开头的 URL 路径会被转发到名为 `backend_server` 的后端服务器。

3. 配置缓存（可选）：
   如果您希望对某些静态资源进行缓存，以减轻后端服务器负担并提高响应速度，可以配置 Nginx 的缓存功能。例如，使用 `proxy_cache` 指令和相关选项进行配置。具体配置取决于您的需求和网站架构。

   注意：请确保设置适当的缓存策略，避免缓存过期或过量占用磁盘空间。

4. 重启 Nginx 服务：
   在完成配置后，保存并退出配置文件，然后使用适当的命令（如 `service nginx restart`）重新启动 Nginx 服务使更改生效。



## Nginx 高可用集群
搭建多台 Nginx 服务器，当其中一台宕机之后，请求任务交给另一台 Nginx，保证服务器依然可以用。

要配置 Nginx 高可用集群，可以采取以下步骤：

1. 安装和配置 Nginx 服务器：
   在每个服务器上安装和配置 Nginx。确保所有服务器的 Nginx 版本、配置文件和依赖项都是相同的。

2. 设置主从架构（Active-Standby）：
   选择一个服务器作为主服务器，其他服务器作为从服务器。主服务器接受并处理所有的请求，而从服务器处于备份状态，只有在主服务器故障时才会起作用。

3. 同步 Nginx 配置文件：
   将主服务器上的 Nginx 配置文件同步到从服务器上，以确保它们具有相同的配置。您可以使用工具如 rsync 或配置管理工具来自动化此过程。  

4. 配置健康检查：
   在主服务器和从服务器之间设置健康检查，以检测主服务器是否可用。如果主服务器发生故障，健康检查将自动切换流量到从服务器。

5. 配置主服务器和从服务器的通信：
   使用 Nginx 的 upstream 模块配置主服务器和从服务器之间的通信。在主服务器配置中定义从服务器，并使用相关的负载均衡策略。

6. 配置故障转移：
   当主服务器失效时，需要进行故障转移，将从服务器提升为新的主服务器。这可以通过使用监控工具、脚本或自动化工具来实现。

7. 配置 DNS 或负载均衡器：
   若要实现高可用集群的完整性，可以在 Nginx 服务器前面添加 DNS 解析或硬件负载均衡器，以平衡流量和提供冗余性。

# Nginx 原理解析
启动 Nginx ，有一个，master 多个 worker。Master 分配任务给到 worker。
利于热部署。
不需要加锁。
Io 复用。提高性能。

发送一个请求，占用了 `worker` 2 个或 4 个连接数。
一共有 4 个 worker，每个 worker 最大连接数是 1024，他能承受最大连接数是 · `1024 * 4/2` 或者  `1024 * 4/4`


