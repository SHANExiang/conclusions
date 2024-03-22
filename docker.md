

# 云原生
Cloud Native，Cloud 表示应用程序位于云中，而不是传统的数据中心；Native 表示应用程序从设计之初即考虑到云的环境。 

云原生四要素：
微服务：单个应用程序由许多松散耦合且可独立部署的较小组件或服务组成。
这些服务通常有自己的堆栈，包括数据库和数据模型；通过 REST API，事件流和消息代理的组合相互通信；
它们是按业务能力组织的，分隔服务的线通常称为有界上下文。 

容器化：为微服务提供支持以及起到隔离的作用，k8s 就是容器的编排系统，它用于容器的管理和容器内部的负载均衡； 

DevOps：开发和运维的合体，还包括测试，为云原生提供可持续交付的能力； 

持续交付：又叫 CICD，实现在线不停机的更新，开发版本和稳定版本并存，需要标准的流程和工具来支撑，如 jenkins；  


# CLI
docker exec -i mariadb bash -c 'mysql -u cinder -pa0zB3ujjraymYtYNJaSNvVzvaPQCNxuJR0ttN1rC -Ne "use cinder;desc backup_metadata"'

构建镜像
docker build -t gin-blog-docker .
Docker image是由一系列Docker只读层创建出来的；Docker layer在Dockerfile配置文件中完成一条配置指令即表示一个Docker layer；

镜像导入导出
docker save -o nginx.tar nginx:latest或docker save > nginx.tar nginx:latest
docker load -i nginx.tar或docker load < nginx.tar
docker export -o nginx-test.tar nginx-test
docker import nginx-test.tar nginx:imp或cat nginx-test.tar | docker import - nginx:imp
export命令是从容器（container）中导出tar文件，而save命令则是从镜像（images）中导出

docker run -itd -u root -v /service/logs/dev/nonick/nonick-notifier-service:/service/logs/dev/nonick/nonick-notifier-service -v /etc/localtime:/etc/localtime -p 8081 notifier:0912 bash
docker启动的容器卷模式为ro，此时不能再文件夹内新建文件；除非在docker run时加上--privileged；


config.json中command值为sleep 600，容器可以启动；cinder-volume --config-file /etc/cinder/cinder.conf


# 容器时间和宿主机一致
将宿主机的时区文件映射到容器中  -v /etc/localtime:/etc/localtime


# 容器化技术在底层的运行原理
每个容器运行在它自己的命名空间中，但是，确实与其它运行中的容器共用相同的系统内核。
隔离的产生是由于系统内核清楚地知道命名空间及其中的进程，且这些进程调用系统api时，内核保证进程只能访问属于其命名空间中的资源。


# centos 安装 docker 
1.卸载旧版本 docker 和 docker-engine 
2.使用存储库安装 yum install yum-utils device-mapper-persistent-data lvm2 
3.yum-config-manager —add-repo https://download.docker.com/linux/centos/docker-
ce.repo  
4.yum install -y docker-ce  

 
# docker 容器与主机时间保持同步 
1.共享主机的 localtime 
创建容器指定启动参数，挂 载localtime文件到容器内 v /etc/localtime:/etc/localtime:ro  
2.复制主机的 localtime 
3.创建 dockerfile   
RUN 
/bin/cp 
/usr/share/zoneinfo/Asia/Shanghai 
/etc/localtime 
&& echo ‘Asia/Shanghai’ > /etc/timezone 

 
# docker 隔离原理 
docker 本质上是宿主机上的进程，通过 namespace 实现资源隔离，通过 cgroup 实现了资源限制，通过写时复制实现了高效文件操作。 

6 项资源进行隔离： 
主机名和域名； 
信号量，消息队列，共享内存； 
进程编号； 
网络设备，网络栈，端口等； 
挂载点即文件系统； 
用户和用户组； 

cgroups 
可以对任务使用资源总额进行限制； 
通过分配的 CPU 时间片数量以及磁盘 IO 带宽限制任务运行的优先级； 
统计系统资源使用量； 
对任务执行挂起恢复操作。 


Docker 通过多种技术实现了进程隔离和资源限制，主要原理如下：
1. 命名空间（Namespaces）：Docker 利用 Linux 内核的命名空间特性，为每个容器创建独立的命名空间，包括进程、网络、文件系统等，使容器内的进程看起来拥有自己独立的系统环境。
2. 控制组（Control Groups，cgroups）：Docker 使用 cgroups 对容器的资源使用进行限制和控制，如 CPU、内存、I/O 等，防止单个容器占用过多的系统资源。
3. 联合文件系统（Union File System）：Docker 使用联合文件系统，如 OverlayFS，将多个文件系统叠加挂载，形成分层的镜像。这种机制允许多个容器共享相同的文件系统层，节省磁盘空间并提高效率。
4. 网络隔离：Docker 为每个容器分配独立的网络栈和虚拟网卡，并通过网桥、veth pair 等技术实现容器间的网络通信和端口映射。
5. 安全限制：Docker 还利用 Linux 的安全特性，如 Capabilities、AppArmor、Seccomp 等，对容器进行安全限制，减少容器逃逸或攻击宿主机的风险。

通过以上技术的组合，Docker 实现了容器的隔离和资源控制，使得多个容器可以在同一台宿主机上运行，而不会相互干扰。每个容器都拥有自己的文件系统、网络、进程空间等，就像一个独立的轻量级虚拟机，但比传统虚拟机更加高效和便捷。

这种隔离机制不仅方便了应用的打包和部署，还提高了系统的安全性和稳定性。Docker 的隔离并不是完全绝对的，而是一种进程级别的隔离，通过 Linux 内核的特性来实现。了解 Docker 的隔离原理，有助于我们更好地使用和管理容器化应用。


# 网络模式 

bridge 模式：使用--network  bridge 指定，默认使用 docker0 
host 模式：使用--network host 指定 
none 模式：使用--network none 指定 
container 模式：使用--network container:NAME 或者容器 ID 指定 

## bridge  
Docker 服务默认会创建一个 docker0 网桥（其上有一个 docker0 内部接口），该桥接网络的名称为 docker0。
它在内核层连通了其他的物理或虚拟网卡，这就将所有容器和本地主机都放到同一个物理网络。
Docker 默认指定了 docker0 接口的 IP 地址和子网掩码，让主机和容器之间可以通过网桥相互通信。 

查看 bridge 网络的详细信息，并通过 grep 获取名称项 
1．Docker 使用 Linux 桥接，在宿主机虚拟一个 Docker 容器网桥(docker0)，Docker
启动一个容器时会根据 Docker 网桥的网段分配给容器一个 IP 地址，称为Container-IP，同时 Docker 网桥是每个容器的默认网关。
因为在同一宿主机内的容器都接入同一个网桥，这样容器之间就能够通过容器的 Container-IP 直接通信。 

2．docker run 的时候，没有指定 network 的话默认使用的网桥模式就是 bridge，使用的就是 docker0。
在宿主机 ifconfig,就可以看到 docker0 和自己 create 的network，eth0，eth1，eth2……代表网卡一，网卡二，网卡三……，
lo 代表 127.0.0.1，即 localhost，inet addr 用来表示网卡的 IP 地址

3．网桥 docker0 创建一对对等虚拟设备接口一个叫 veth，另一个叫 eth0，成对匹配。 
3.1 整个宿主机的网桥模式都是 docker0，类似一个交换机有一堆接口，每个接口
叫 veth，在本地主机和容器内分别创建一个虚拟接口，并让他们彼此联通（这样一对接口叫 veth pair）； 
3.2 每个容器实例内部也有一块网卡，每个接口叫 eth0； 
3.3 docker0 上面的每个 veth 匹配某个容器实例内部的 eth0，两两配对，一一匹配。 
 通过上述，将宿主机上的所有容器都连接到这个内部网络上，两个容器在同一个
网络下,会从这个网关下各自拿到分配的 ip，此时两个容器的网络是互通的。 

## host
直接使用宿主机的 IP 地址与外界进行通信，不再需要额外进行 NAT 转换。 
容器将不会获得一个独立的 Network Namespace， 而是和宿主机共用一个
Network Namespace。容器将不会虚拟出自己的网卡而是使用宿主机的 IP 和端口。 
docker 启动时指定--network=host 或-net=host，如果还指定了-p 映射端口，那
这个时候就会有此警告，并且通过-p 设置的参数将不会起到任何作用，端口号会以主机端口号为主，重复时则递增。 
解决: 
解决的办法就是使用 docker 的其他网络模式，例如--network=bridge，这样就可
以解决问题，或者直接无视。 

## none
在 none 模式下，并不为 Docker 容器进行任何网络配置。  
也就是说，这个 Docker 容器没有网卡、IP、路由等信息，只有一个 lo 
需要我们自己为 Docker 容器添加网卡、配置 IP 等。 
禁用网络功能，只有 lo 标识(就是 127.0.0.1 表示本地回环) 

## container 
container⽹络模式  
新建的容器和已经存在的一个容器共享一个网络 ip 配置而不是和宿主机共享。
新创建的容器不会创建自己的网卡，配置自己的 IP，而是和一个指定的容器共享
IP、端口范围等。同样，两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。 




# DockerFile 解析
Dockerfile 是用来构建 Docker 镜像的文本文件，是由一条条构建镜像所需的指令和参数构成的脚本。

构建三步骤 
编写 Dockerfile 文件 
docker build 命令构建镜像 
docker run 依镜像运行容器实例 

Dockerfile 内容基础知识 
1：每条保留字指令都必须为大写字母且后面要跟随至少一个参数 
2：指令按照从上到下，顺序执行 
3：#表示注释 
4：每条指令都会创建一个新的镜像层并对镜像进行提交 
 
Docker 执行 Dockerfile 的大致流程 
（1）docker 从基础镜像运行一个容器 
（2）执行一条指令并对容器作出修改 
（3）执行类似 docker commit 的操作提交一个新的镜像层 
（4）docker 再基于刚提交的镜像运行一个新容器 
（5）执行 dockerfile 中的下一条指令直到所有指令都执行完成 
  
从应用软件的角度来看，Dockerfile、Docker 镜像与 Docker 容器分别代表软件的三个不同阶段， 
*  Dockerfile 是软件的原材料 
*  Docker 镜像是软件的交付品 
*  Docker 容器则可以认为是软件镜像的运行态，也即依照镜像运行的容器实例 
Dockerfile 面向开发，Docker 镜像成为交付标准，Docker 容器则涉及部署与运维，三者缺一不可，合力充当 Docker 体系的基石。 

1 Dockerfile，需要定义一个 Dockerfile，Dockerfile 定义了进
程需要的一切东西。Dockerfile 涉及的内容包括执行代码或
者是文件、环境变量、依赖包、运行时环境、动态链接库、
操作系统的发行版、服务进程和内核进程(当应用进程需要和
系统服务和内核进程打交道，这时需要考虑如何设计
namespace 的权限控制)等等; 
  
2 Docker 镜像，在用 Dockerfile 定义一个文件之后，docker 
build 时会产生一个 Docker 镜像，当运行 Docker 镜像时会
真正开始提供服务; 
  
3 Docker 容器，容器是直接提供服务的。 
  
  
## DockerFile 常用保留字指令 
### FROM 
基础镜像，当前新镜像是基于哪个镜像的，指定一个已经存在的镜像作为模板，第一条必须是 from 

### MAINTAINER 
镜像维护者的姓名和邮箱地址 

### RUN 
容器构建时需要运行的命令 
两种格式: 
1. shell 格式 
RUN yum -y install vim 
2. exec 格式
RUN 是在 docker build 时运行 

### EXPOSE
当前容器对外暴露出的端口 
格式为EXPOSE<端口1> [<端口2>...]

EXPOSE指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务
在 Dockerfile 中写入这样的声明有两个好处
帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射
运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE的端口

### WORKDIR 
指定在创建容器后，终端默认登陆的进来工作目录，一个落脚点 

### USER
指定该镜像以什么样的用户去执行，如果都不指定，默认是 root 

### ENV
用来在构建镜像过程中设置环境变量 
ENV MY_PATH /usr/mytest 
这个环境变量可以在后续的任何 RUN 指令中使用，这就如同在命令前面指定了环境变量前缀一样； 
也可以在其它指令中直接使用这些环境变量，
比如：WORKDIR $MY_PATH 

### ADD
将宿主机目录下的文件拷贝进镜像且会自动处理 URL 和解压 tar 压缩包 

### COPY
类似 ADD，拷贝文件和目录到镜像中。 将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置 
COPY src dest
COPY ["src", "dest"] 
<src 源路径>：源文件或者源目录 
<dest 目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。 

### VOLUME 
容器数据卷，用于数据保存和持久化工作 

### CMD
指定容器启动后的要干的事情
注意:
Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效，CMD 会被 docker run 之后的参数替换 
 
它和前面 RUN 命令的区别 
CMD 是在 docker run 时运行。 
RUN 是在 docker build 时运行。 

### ENTRYPOINT 
也是用来指定一个容器启动时要运行的命令 
类似于 CMD 指令，但是 ENTRYPOINT 不会被 docker run 后面的命令覆盖， 而
且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序
ENTRYPOINT 可以和 CMD 一起用，一般是变参才会使用 CMD ，这里的 CMD等于是在给 ENTRYPOINT 传参。 
当指定了 ENTRYPOINT 后，CMD 的含义就发生了变化，不再是直接运行其命
令而是将 CMD 的内容作为参数传递给 ENTRYPOINT 指令。

ENTRYPOINT的格式和 RUN 指令格式一样，分为两种格式
exec 格式：
<ENTRYPOINT> "<CMD>"
shell 格式：
ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]


# Docker-compose 容器编排  
Compose 是 Docker 公司推出的一个工具软件，可以管理多个 Docker 容器组成
一个应用。你需要定义一个 YAML 格式的配置文件 docker-compose.yml，写好多
个容器之间的调用关系。然后，只要一个命令，就能同时启动/关闭这些容器。 
负责实现对 Docker 容器集群的快速编排。 
 
为什么需要 docker-compose 
docker 建议我们每一个容器中只运行一个服务,因为 docker 容器本身占用资源极
少，所以最好是将每个服务单独的分割开来但是这样我们又面临了一个问题？ 
如果我需要同时部署好多个服务,难道要每个服务单独写 Dockerfile 然后在构建
镜像,构建容器,这样累都累死了,所以 docker 官方给我们提供了 docker-compose 多服务部署的工具 

例如要实现一个 Web 微服务项目，除了 Web 服务容器本身，往往还需要再加上
后端的数据库 mysql 服务容器，redis 服务器，注册中心 eureka，甚至还包括负载均衡容器等等。。。。。。 
Compose 允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）
来定义一组相关联的应用容器为一个项目（project）。 
可以很容易地用一个配置文件定义一个多容器的应用，然后使用一条指令安装这
个应用的所有依赖，完成构建。Docker-Compose 解决了容器与容器之间如何管理编排的问题。  


## Compose 核心概念 
一文件 docker-compose.yml 
两要素  
服务（service）：一个个应用容器实例，比如订单微服务、库存微服务、mysql容器、nginx 容器或者 redis 容器 
工程（project）：由一组关联的应用容器组成的一个完整业务单元，在 docker-compose.yml 文件中定义。 

## Compose 使用的三个步骤
1. 编写 Dockerfile 定义各个微服务应用并构建出对应的镜像文件 
2. 使用 docker-compose.yml 定义一个完整业务单元，安排好整体应用中的各个容器服务。 
3. 最后，执行 docker-compose up 命令 来启动并运行整个应用程序，完成一键部署上线 

## Compose 常用命令 
docker-compose -h                           # 查看帮助 
docker-compose up                           # 启动所有 docker-compose 服务 
docker-compose up -d                        # 启动所有 docker-compose 服务并后台运行 
docker-compose down                         # 停止并删除容器、网络、卷、镜像。 
docker-compose exec  yml 里面的服务 id                 # 进入容器实例内部  docker-
compose exec docker-compose.yml 文件中写的服务 id /bin/bash 
docker-compose ps                      # 展示当前 docker-compose 编排过的运行的所有容器 
docker-compose top                     # 展示当前 docker-compose 编排过的容器进程 

docker-compose logs  yml 里面的服务 id     # 查看容器输出日志 
docker-compose config     # 检查配置 
docker-compose config -q  # 检查配置，有问题才有输出 
docker-compose restart   # 重启服务 
docker-compose start     # 启动服务 
docker-compose stop      # 停止服务 
