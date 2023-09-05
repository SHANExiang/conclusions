# docker


config.json中command值为sleep 600，容器可以启动；cinder-volume --config-file /etc/cinder/cinder.conf

docker exec -i mariadb bash -c 'mysql -u cinder -pa0zB3ujjraymYtYNJaSNvVzvaPQCNxuJR0ttN1rC -Ne "use cinder;desc backup_metadata"'


1、 FROM
指定基础镜像（必须有的指令，并且必须是第一条指令）
2、 WORKDIR
格式为 
WORKDIR <工作目录路径>
使用 WORKDIR指令可以来指定工作目录（或者称为当前目录），以后各层的当前目录就被改为指定的目录，如果目录不存在，WORKDIR会帮你建立目录
3、COPY
格式：
COPY <源路径>... <目标路径>
COPY ["<源路径1>",... "<目标路径>"]

COPY指令将从构建上下文目录中 <源路径> 的文件/目录复制到新的一层的镜像内的 <目标路径> 位置
4、RUN
用于执行命令行命令
格式：
RUN <命令>
5、EXPOSE
格式为 
EXPOSE<端口1> [<端口2>...]

EXPOSE指令是声明运行时容器提供服务端口，这只是一个声明，在运行时并不会因为这个声明应用就会开启这个端口的服务
在 Dockerfile 中写入这样的声明有两个好处
帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射
运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE的端口

6、ENTRYPOINT

ENTRYPOINT的格式和 RUN
 指令格式一样，分为两种格式
exec
 格式：
<ENTRYPOINT> "<CMD>"
shell
 格式：
ENTRYPOINT [ "curl", "-s", "http://ip.cn" ]

ENTRYPOINT指令是指定容器启动程序及参数
二、构建镜像
docker build -t gin-blog-docker .

镜像导入导出
docker save -o nginx.tar nginx:latest或docker save > nginx.tar nginx:latest
docker load -i nginx.tar或docker load < nginx.tar
docker export -o nginx-test.tar nginx-test
docker import nginx-test.tar nginx:imp或cat nginx-test.tar | docker import - nginx:imp
export命令是从容器（container）中导出tar文件，而save命令则是从镜像（images）中导出


## docker-compose使用
docker-compose up -d
docker-compose stop # 停止所有容器
docker-compose down # 要删除所有正在运行的容器且停止服务,停止单个项目中所有容器，而不仅仅是当前目录中的文件所定义的容器
docker-compose restart    # 重启docker-compose文件中的容器


## 容器时间和宿主机一致
将宿主机的时区文件映射到容器中  -v /etc/localtime:/etc/localtime 