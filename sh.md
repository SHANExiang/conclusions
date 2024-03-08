# 快捷键
1. 从后往前删除   ctrl+w；
2. 从前往后删除   ctrl+k；
3. 光标从前调到末尾  ctrl+e;  vim内部为删除光标所在行；


# curl
curl "http://localhost:9999/hello?name=geektutu"
curl "http://localhost:9999/login" -X POST -d 'username=geektutu&password=1234' -H ""
curl -e ssl_cacert.pem -k 'https://192.168.11.1:18002/controller/v2/tokens' --header 'Content-Type: application/json' --header 'Cookie:bspsession=deleted' -d '{"userName": "ops@huawei.com", "password": "Xsy@2023"}'
curl 33.33.33.232:30000 --connect-timeout 5    设置5s超时
curl -g -i --insecure -X PUT https://10.50.90.2:18002/restconf/data/huawei-ac-neutron:neutron-cfg/routers/router/e4bffe3a-24cb-4257-aab6-48cd95499aeb -H "Content-Type:application/json" -H "X-ACCESS-TOKEN:$token" -H "Accept:application/json" -d $data

# zip
zip -q -r html.zip /home/html    # 将/home/html/这个目录下所有文件和文件夹打包为当前目录下的 html.zip
unzip html.zip -d /home/         # 将html.zip解压到/home路径下；



# awk
-F     指定输入行的字段分隔符，以便将数据切分成不同的段
awk -F "\"" '{print $6}' 



# git
gitlab提交代码流程
1.gitlab新建分支 hotfix/master/etcdconf/chron--- bug用hotfix，功能代码用feature，master指的是要合并的分支，etcdconf--功能路径，chron用户；feature/master/backupdriver/dongxiang
2.本地拉取分支git fetch origin feature/master/backupdriver/dongxiang；
3.根据远端分支新建本地分支git checkout -b feature/backup origin/feature/master/backupdriver/dongxiang；
4.将本地另一个分支上的修改cherry-pick到新建分支git cherry-pick 5e882fedd834c4e2a4c8d41a69d565324f98c56a；
5.提交到远端分支git push origin HEAD:feature/master/backupdriver/dongxiang；

git恢复reset的代码
首先git reflog查看提交记录；
git rebase -i HEAD@{2}   进入vim直接:q！，解决冲突，提交，恢复之前reset掉的代码；

git远程分支已经删除，本地如何更新
直接执行git remote prune origin即可；

git查看代码行
git ls-files | xargs wc -l

gitignore加./idea

远端分支和master改动一致
本地checkout -b新分支，然后git pull，之后git rebase master，最后git push origin HEAD:sbx即可，不需要merge request；

# 网卡配置
ip addr add 10.50.114.157/32 dev eth0        # 增加网卡地址
ip addr del 10.50.114.157/32 dev eth0        # 删除网卡地址
ip route add default via 10.50.114.157 dev eth0       # 增加默认路由
ip addr show dev eht0                        # 查看网口的配置信息



1. 可以直接修改配置文件/etc/sysconfig/network-scripts/中的ifcfg-eth0；
如果一个网卡配置多个ip地址，则新增文件/etc/sysconfig/network-scripts/中的ifcfg-eth0:1；
2. BOOTPROTO设置为 dhcp 后，系统会在引导过程中自动向 DHCP 服务器发送请求，以获取 IP 地址和其他相关配置。
这样，你无需手动配置网络接口，系统会自动从 DHCP 服务器获取所需的网络配置信息，并将其应用于相应的网络接口。
手动配置网络接口，可以将 BOOTPROTO 设置为其他值，比如 static（静态IP地址）或 none（禁用IP配置）。
3. 


# nginx
server {
    listen 80;
    server_name yourdomain.com;

    location /socket {
        proxy_pass http://backend.example.com/socket;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }

    location /api {
        proxy_pass http://backend.example.com/api;
        proxy_set_header Host $host;
    }
}
当客户端通过浏览器或其他方式发送请求到 yourdomain.com 这个域名时，Nginx 将监听 HTTP 请求的端口 80 ，
将带有/socket前缀的WebSocket请求转发到ws://backend.example.com/socket，
同时将带有/api前缀的HTTP请求转发到http://backend.example.com/api。

proxy_set_header Host $host; 将客户端请求中的 Host 头部信息传递给目标服务器。这是正常的 HTTP 头部信息传递，不涉及客户端 IP 地址。
proxy_set_header X-Real-IP $remote_addr;将客户端的真实 IP 地址作为 X-Real-IP 头部信息传递给目标服务器。这意味着目标服务器可以访问到客户端的真实 IP 地址。
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 将客户端的 IP 地址添加到 X-Forwarded-For 头部信息中，并传递给目标服务器。这是为了记录代理请求的前几个客户端的 IP 地址，通常包括客户端的真实 IP 地址。
proxy_read_timeout: 用于设置从后端服务器接收响应的超时时间。默认值60s
proxy_connect_timeout: 用于设置与后端服务器建立连接的超时时间。默认值60s
proxy_send_timeout: 用于设置向后端服务器发送请求的超时时间。默认值60s




# tcpdump
sudo tcpdump -n -t -S -i enp0s3  port 80
第一次握手，标志位Flags=S
IP 10.0.2.2.51323 > 10.0.2.15.80: Flags [S], seq 84689409, win 65535, options [mss 1460], length 0
第二次握手，标志位Flags=[S.]
IP 10.0.2.15.80 > 10.0.2.2.51323: Flags [S.], seq 1893430205, ack 84689410, win 64240, options [mss 1460], length 0
第三次握手，标志位Flags=[.]
IP 10.0.2.2.51323 > 10.0.2.15.80: Flags [.], ack 1893430206, win 65535, length 0
建立连接后，客户端发送http请求 
IP 10.0.2.2.51321 > 10.0.2.15.80: Flags [P.], seq 1:753, ack 1, win 65535, length 752: HTTP: GET / HTTP/1.1

tcpdump命令解析一下：
-i : 指定抓包的网卡是enp0s3
-n: 把域名转成IP显示
-t: 不显示时间
-S: 序列号使用绝对数值，不指定-S的话，序列号会使用相对的数值
port: 指定监听端口是80
host:指定监听的主机名


# 查看socket信息
netstat -napt或ss -ntlp
-n表示不显示名字，而是以数字方式显示ip和端口
-l只显示LISTEN状态的socket
-p表示显示进程信息
-t表示只显示tcp连接

ss -tunlp|grep 9696     查看端口打开情况

网络性能指标
带宽：链路的最大传输速率，b/s；
吞吐量：单位事件内成功传输的数据量；b/s(比特/s)或B/s(字节/s)



# 证书
CA 证书文件（ssl_cacert.pem）---即根证书；
客户端证书和私钥文件----cert.pem, key.pem


# 用户及用户组
添加用户---adduser dx
设置用户密码---passwd dx
添加用户组---groupadd docker
将用户加到用户组中---usermod -aG docker dx
激活对用户组的修改---newgrp docker


# kubernetes k8s


### CLI
kubectl get all -n <ns>
kubectl delete namespace --all
crictl ps -a    # 查看到运行的container
kubeadm reset -f ; ipvsadm --clear  ; rm -rf ~/.kube    # 初始化失败时进行重置
kubectl config view    # 查看当前的配置文件
kubectl config set-cluster <cluster-name> --server=<api-server-url>
kubectl config get-contexts            # 查看当前上下文的信息，包括集群名称

### kube-apiserver
1. 配置文件路径  /etc/kubernetes/kube-controller-manager.kubeconfig;
2. 重启systemctl restart kube-apiserver.service;
3. 设置日志输出到文件中        --logtostderr=false --log-dir=/var/log/kubernetes
4. k8s官方将apiversion分成了三个大类型，alpha、beta、stable。 
Alpha: 未经充分测试，可能存在bug，功能可能随时调整或删除。
Beta: 经过充分测试，功能细节可能会在未来进行修改。 
Stable: 稳定版本，将会得到持续支持
5. 查看k8s支持的资源  kubectl api-resources


### pod
kubectl run kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8000
1. 容器的集合，紧密相关的一组容器放到一个pod中，同一个pod中的容器共享ip地址和port空间；
2. 同一pod中的容器始终被一起调度；
3. 将pod的端口映射到节点的端口--->kubectl expose pod kubernetes-bootcamp --type="NodePort" --port 8000；
4. 查看服务；kubectl get services；
5. 删除pod--->kubectl delete pod <pod>；
6. 给命令起别名--->alias kubectl="minikube kubectl --"
7. 查看详细信息--->kubectl describe pod nginx；
8. restartPolicy: Never--->pod启动时，容器失败退出，容器不会重启，控制器会启动新的pod；
9. restartPolicy: OnFailure--->pod启动时，容器失败会重启；
10. kubectl logs；
11. kubectl exec <podName> <cmd>；
12. pod中容器间的通信；
    通过共享卷通信；
    进程间通信IPC；
    容器间的网络通信；通过localhost通信；需要为在pod中的接收连接的容器分配不通的端口；
13. pod之间的通信------IP；
14. 



### Deployment
kubectl create deployment kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8000
1. 创建deployment，会创建指定的pod;
2. 暴露端口--->kubectl expose deployment kubernetes-bootcamp --type=NodePort --port=8080；
3. 扩容副本--->kubectl scale deployment kubernetes-bootcamp --replicas=3；
4. 滚动更新--->升级镜像v2-->kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=docker.io/jocatalin/kubernetes-bootcamp:v2--->v1 pod被删除，然后启动v2 pod；
5. 回退--->kubectl rollout undo deployments/kubernetes-bootcamp；
6. kubectl rollout undo deployment httpd --to-revision=1;回退到revision记录为1的版本；
7. 根据yaml文件进行创建pod--->kubectl apply -f deployment-nginx.yaml；每次执行apply会保留当前的配置，保存为一个revision,这样就可以回滚到某个特定的revision；
    kubectl apply -f httpd.yaml --record；将当前命令记录到revision记录中；
8. 查看详细信息--->kubectl describe deployment nginx；
9. 删除deployment--->kubectl delete -f deployment-nginx.yaml或kubectl delete deployment deployment-nginx；
10. kubectl rollout history deployment httpd   查看revision历史记录；




### DaemonSet



### Job
工作类容器，一次性任务，比如批量处理程序，完成后容器退出；
1. kubectl get job；pod执行完毕后容器已经退出，pod状态变成Completed；
2. job并行--->设置参数parallelism，比如parallelism: 2会启动两个pod；
3. 定时job--->schedule指定什么时候运行job，比如*/1 * * * *含义是每一分钟启动一次，也就是每一分钟会启动一个pod指定任务；






### minikube安装
1. curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
2. sudo install minikube-linux-amd64 /usr/local/bin/minikube
3. 添加用户---adduser dx;设置用户密码---passwd dx
4. 添加用户组---groupadd docker;将用户加到用户组中---usermod -aG docker dx;激活对用户组的修改---newgrp docker
5. 切换dx用户执行minikube start --driver=docker；



### controller控制器
1. 通过controller管理pod;
2. controller种类：Deployment/ReplicaSet/DaemonSet/StatefuleSet/Job等；
3. 查看k8s master组件状态 kubectl get cs；
4. 


### service
定义了外界访问一组特定pod的方式，service有自己的IP和Port,为pod提供负载均衡；
kubernetes运行容器和访问容器，两项任务分别由controller和service执行；
1. Service的Cluster IP通过iptables映射到Pod IP；
2. iptables将访问Service的流量转发到后端Pod，而且使用类似轮询的负载均衡机制；Cluster的每个节点都配置了相同的iptables规则；
3. Service通过Cluster节点的静态端口对外提供服务；cluster外部可以通过<NodeIP>:<NodePort>访问Service；
4. nodePort是节点上监视的端口；
5. port是ClusterIp上监听的端口；
6. targetPort是Pod监听的端口；

### Namespace
如果有多个用户或项目组使用同一个Kubernetes Cluster，如何将他们创建的Controller、Pod等资源分开呢？
答案就是Namespace。Namespace可以将一个物理的Cluster逻辑上划分成多个虚拟Cluster，每个Cluster就是一个Namespace。
不同Namespace里的资源是完全隔离的。Kubernetes默认创建了两个Namespace。



### coredns
DNS服务器，每当有新的Service被创建，coredns会添加该Service的DNS记录，Cluster中的pod可以通过<Service_Name>.<Namespace_Name>访问Service；
1. nslookup查看Service的DNS信息；


### kubernetes健康检查机制
1. 每个容器启动时，都会执行一个进程，此进程由DockerFile的CMD或ENTRYPOINT指定。如果进程退出时返回码非零，则认为容器故障，k8s会根据restartPolicy进行重启容器；
2. Liveness探测让用户自定义判断容器是否健康的条件；告诉k8s什么时候通过重启容器实现自愈；探测失败后会重启容器；
3. Readiness探测；告诉k8s什么时候可以将容器加入到Service负载均衡池中，对外提供服务；探测失败后将容器设置为不可用，不接受Service转发的请求；


### kubernetes volume
1. 作用：持久化保存容器中的数据；
2. volume声明周期独立于容器，pod中的容器可能被销毁和重建，但volume会被保留；
3. 本质上，volume是一个目录，当volume被mount到pod，pod中的所有容器都可以访问这个volume；
4. volume支持多种后端backend类型，emptyDir、hostPath、GCE Persistent Disk、NFS、Ceph等；
5. emptyDir类型；就是host上的一个空目录；pod中的所有容器共享emptyDir，pod不存在了，emptyDir才不存在；
6. PersistentVolume（PV）外部存储系统中的一块存储空间，由管理员创建和维护；
7. PersistentVolumeClaim（PVC）对PV的申请；普通用户创建和维护；用户可以创建PVC，指明存储资源的容量和访问模式等信息，然后k8s会查找PV；
8. 当k8s有storage provider时，可以创建nfs、ceph、AWS EBS的PV，然后创建PVC，指定容量+访问模式+class，即将PVC绑定PV；
9. 回收PV---删除PVC来回收PV，kubectl delete pvc <pvcName>; kubectl patch pvc <pvcName> -p '{"metadata": "finalizers": null}'；pv状态变成Released-->数据清除完毕，最终变成Available；


### Secret
为Pod提供密码、Token、秘钥等敏感数据；
1. 通过--from-literal创建；kubectl create secret generic mysecret --from-literal=username=admin --from-literal=password=123456
2. 通过--from-file创建；
    echo admin > ./username
    echo 123456 > ./password 
    kubectl create secret generic mysecret2 --from-file=./username --from-file=./password
3. 通过--from-env-file创建；
    cat << EOF > env.txt
    > username=admin
    > password=123456
    > EOF
    kubectl create secret generic mysecret3 --from-env-file=env.txt 
4. 通过yaml配置文件创建；kubectl apply -f secret4.yaml；敏感数据必须是base64加密的；echo -n admin | base64；
5. 查看secret--->kubectl edit secret mysecret；通过base64反解码-->echo -n YWRtaW4= |base64 --decode；
6. 使用secret--->通过volume；将指定的路径下为每条敏感数据建立一个文件，文件名为key，文件内容为value；
7. 使用secret--->通过环境变量；


### ConfigMap
为Pod提供配置信息；
1. 通过--from-literal创建；kubectl create configmap myconfigmap --from-literal=config1=xxx --from-literal=config2=yyy
2. 通过--from-file创建；kubectl create configmap myconfigmap2 --from-file=./config1 --from-file=./config2
3. 通过--from-env-file创建；kubectl create configmap myconfigmap3 --from-env-file=env.txt 
4. 通过yaml配置文件创建；kubectl apply -f configmap4.yaml；


### Helm
应用打包工具；
1. chart；类似apt、yum；它包含一系列 k8s 资源配置文件的模板与参数，可供灵活配置
2. repo；chart的仓库，其中有很多chart可供选择；
3. release；一个chart部署后生成一个release；
4. helm repo add bitnami https://charts.bitnami.com/bitnami    添加helm chart repo;
5. helm repo list    查看已经支持的repo仓库列表；
6. helm search repo bitnami        查找可安装使用的chart列表；



### CNI
container networking interface



### network policy
1. 通过label选择pod,并指定其他pod或外界如果与这些pod进行通信；当为pod定义network policy时，只有policy允许的流量才能访问pod；
2. ingress:
  - from:
    - podSelector:
        matchLabels:
          access: "true"
  表示只有pod带label access=true的才能和建立network policy的pod进行通信；


### k8s部署
1. 格式化数据盘并挂载
sudo cat /etc/fstab |tail -n1
UUID=36158b9f-f0cb-46e0-9e8c-f9f463be06db /                       xfs     defaults        0 0
2. 生成ssh秘钥对；ssh-keygen -t rsa -b 2048；cat /root/.ssh/id_rsa.pub > /root/.pub；ssh免密设置；
sudo ssh-copy-id 33.33.33.232;
sudo ssh-copy-id 33.33.33.75
sudo ssh-copy-id 33.33.33.95
3. 安装集群
export release=3.1.0
sudo curl -C- -fLO --retry 3 https://github.com/easzlab/kubeasz/releases/download/${release}/ezdown
sudo chmod +x ezdown
sudo ./ezdown -D
sudo ./ezdown -S
4. 进去容器操作docker exec -it kubeasz bash
```shell
步骤一：创建集群：
ezctl new gpu_dev
步骤二：修改网络类型
修改网络类型暂为calico，因为底层是openstack，vxlan本环境冲突，不能使用flannel。 工程目录为：/etc/kubeasz/clusters/gpu_dev/
bash-5.1# grep -rw "calico" hosts 
// Network plugins supported: calico, flannel, kube-router, cilium, kube-ovn
CLUSTER_NETWORK="calico"
步骤三：添加集群的ip地址
添加master、worker节点ip地址，此处需要在hosts文本里添加即可
 
步骤四：修改数据目录
bash-5.1# grep -rw "/mnt" config.yml
ETCD_DATA_DIR: "/mnt/etcd"
DOCKER_STORAGE_DIR: "/mnt/docker"
KUBELET_ROOT_DIR: "/mnt/kubelet
修改数据目录，此处需要在config.yaml文本里添加即可
步骤五：部署
ezctl setup gpu_dev all
```

5. 安装dashboard
```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.0/aio/deploy/recommended.yaml
会在kubernetes-dashboard namespace中创建Deployment和Service；
kubectl --namespace=kubernetes-dashboard edit service kubernetes-dashboard
修改成NodePort模式；
#新建目录：
mkdir key && cd key

#生成证书
openssl genrsa -out dashboard.key 2048 

#我这里写的自己的node1节点，因为我是通过nodeport访问的；如果通过apiserver访问，可以写成自己的master节点ip
openssl req -new -out dashboard.csr -key dashboard.key -subj '/CN=10.13.1.3'
openssl x509 -req -in dashboard.csr -signkey dashboard.key -out dashboard.crt 

#删除原有的证书secret
kubectl delete secret kubernetes-dashboard-certs -n kubernetes-dashboard

#创建新的证书secret
kubectl create secret generic kubernetes-dashboard-certs --from-file=dashboard.key --from-file=dashboard.crt -n kubernetes-dashboard

#查看pod
kubectl get pod -n kubernetes-dashboard

#重启pod
kubectl delete pod kubernetes-dashboard-7b544877d5-2xqcr  -n kubernetes-dashboard

# 创建用户令牌
创建ServiceAccount-->绑定关系ClusterRoleBinding-->获取令牌
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

### helm
安装helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
    && chmod 700 get_helm.sh \
    && ./get_helm.sh

### k8s源码

#### kube-apiserver
对外提供api的方式与其它组件进行交互；



#### deepcopy-gen使用
deepcopy-gen -v 5 -h hack/boilerplate.go.txt --bounding-dirs . -i volcano.sh/apis/pkg/apis/scheduling/v1beta1 -O zz_generated.deepcopy
-v 5 指定输出内容的详细程度
-h boilerplate.txt指定所有生成的文件的头部声明内容
--bounding-dirs .指定生成目录为当前路径
-i github.com/lt90s/deepcopy-gen-demo/types指定此package需要进行代码生成
-O zz_generated.deepcopy指定生成的文件名称为zz_generated.deepcopy.go



#### client-gen
client-gen --clientset-name versioned -i volcano.sh/apis/pkg/apis/scheduling/v1beta1 --output-package clientset --go-header-file hack/boilerplate.go.txt -v 5
volcano.sh/apis增加资源client
拉代码到本地直接执行./hack/update-codegen.sh即可在本地生成client






# sar
怀疑CPU存在瓶颈，可用 sar -u 和 sar -q 等来查看
怀疑内存存在瓶颈，可用 sar -B、sar -r 和 sar -W 等来查看
怀疑I/O存在瓶颈，可用 sar -b、sar -u 和 sar -d 等来查看

sar -n DEV，显示网口的统计数据；
sar -n EDEV，显示关于网络错误的统计数据；
sar -n TCP，显示 TCP 的统计数据


# keepalived
/etc/keepalived/keepalived.conf中
vrrp_instance VI_1 {
    state BACKUP    # 主服务器为MASTER，备服务器为BACKUP
    interface eth0  # 替换为备份服务器上的网络接口名称
    virtual_router_id 51  # 虚拟路由器 ID，与主服务器配置相同
    priority 90  # 备份服务器的优先级较低
    advert_int 1  # 广告间隔，单位为秒
    authentication {
        auth_type PASS
        auth_pass your_password  # 与主服务器配置相同的密码
    }
    virtual_ipaddress {
        192.168.0.100  # 虚拟 IP 地址，与主服务器配置相同
    }
}


# 磁盘
fdisk---磁盘分区的工具
fdisk -l显示磁盘分区表
fdisk /dev/sda -l显示磁盘设备sda的详情


# 文件描述符
1. 每个文件描述符都会与一个打开的文件相对应；
2. 不同的文件描述符可能指向同一个文件；
3. 相同的文件可以被不同的进程打开，也可以在一个进程中被打开多次；



# ldap
### ldap搭建
https://blog.csdn.net/qq_37733540/article/details/123988481

### ldap使用
1. 服务为slapd；
2. ldapsearch检查内容
ldapsearch -x -D cn=Manager,dc=my-domain,dc=com -w admin -b "dc=my-domain,dc=com"
-x 启用认证
-D bind admin的dn
-w admin的密码
-b basedn, 查询的基础dn

ldapsearch -x -LLL -H ldap:/// -b dc=my-domain,dc=com
查看所有dn

3. 关键字列表
dn-----每个对象都有一个惟一的名称，如“uid= tom,ou=market,dc=example,dc=com”，在一个目录树中DN总是惟一的
dc-----域名的部分，其格式是将完整的域名分成几部分，如域名为example.com变成dc=example,dc=com
uid----用户id，如Tom；
ou-----组织单元
cn-----common name，公共名称；



# 内存
## 物理内存
平时所称的内存也叫随机访问存储器（ random-access memory ）也叫 RAM 。而 RAM 分为两类：
一类是静态 RAM（ SRAM ），这类 SRAM 用于 CPU 高速缓存 L1Cache，L2Cache，L3Cache。其特点是访问速度快，访问速度为 1 - 30 个时钟周期，但是容量小，造价高。
另一类则是动态 RAM ( DRAM )，这类 DRAM 用于我们常说的主存上，其特点的是访问速度慢（相对高速缓存），访问速度为 50 - 200 个时钟周期，但是容量大，造价便宜些（相对高速缓存）。



## 虚拟内存地址
64 位虚拟地址的格式为：全局页目录项（9位）+ 上层页目录项（9位）+ 中间页目录项（9位）+ 页表项（9位）+ 页内偏移（12位）。共 48 位组成的虚拟内存地址。
32 位虚拟地址的格式为：页目录项（10位）+ 页表项（10位） + 页内偏移（12位）。共 32 位组成的虚拟内存地址。

## 进程虚拟内存空间所包含的主要区域
1. 用于存放进程程序二进制文件中的机器指令的代码段
2. 用于存放程序二进制文件中定义的全局变量和静态变量的数据段和 BSS 段。
3. 用于在程序运行过程中动态申请内存的堆。
4. 用于存放动态链接库以及内存映射区域的文件映射与匿名映射区。
5. 用于存放函数调用过程中的局部变量和函数参数的栈。

## 内存分段
程序是由若干个逻辑分段组成的，如可由代码分段、数据分段、栈段、堆段组成。不同的段是有不同的属性的，所以就用分段（Segmentation）的形式把这些段分离出来。

出现内存碎片：
由于每个段的长度不固定，所以多个段未必能恰好使用所有的内存空间，会产生了多个不连续的小物理内存，导致新的程序无法被装载，所以会出现外部内存碎片的问题。

解决内存碎片：
在 Linux 系统里，也就是我们常看到的 Swap 空间，这块空间是从硬盘划分出来的，用于内存与硬盘的空间交换。
过程就是将某一段的内存写到硬盘上（Swap空间），然后再从硬盘读回到内存，在读回内存时会紧紧跟着被占用的区域，这样可以将碎片连续从而让别的程序转载这些碎片区域。


## 内存分页
分页是把整个虚拟和物理内存空间切成一段段固定尺寸的大小。这样一个连续并且尺寸固定的内存空间，我们叫页（Page）。在 Linux 下，每一页的大小为 4KB。

因为内存分页机制分配内存的最小单位是一页，即使程序不足一页大小，我们最少只能分配一个页，所以页内会出现内存浪费，所以针对内存分页机制会有内部内存碎片的现象。


## 多级页表
将页表（一级页表）分为 1024 个页表（二级页表），每个表（二级页表）中包含 1024 个「页表项」，形成二级分页。再吧二级分页推广到多级分页；
一级页表覆盖到了全部虚拟地址空间，二级页表在需要时创建。

## TLB
translation lookaside buffer，通常成为页表缓存、地址旁路缓存、快表等；
把最常访问的几个页表项存储到访问速度更快的硬件，于是计算机科学家们，就在 CPU 芯片中，加入了一个专门存放程序最常访问的页表项的 Cache，这个 Cache 就是 TLB。
在 CPU 芯片里面，封装了内存管理单元（Memory Management Unit）芯片，它用来完成地址转换和 TLB 的访问与交互。
有了 TLB 后，那么 CPU 在寻址时，会先查 TLB，如果没找到，才会继续查常规的页表。


## 段页式内存管理
内存分段和内存分页组合在同一个系统中使用。
段页式内存管理实现的方式：
1. 先将程序划分为多个有逻辑意义的段，也就是前面提到的分段机制；
2. 接着再把每个段划分为多个页，也就是对分段划分出来的连续空间，再划分固定大小的页；
这样，地址结构就由段号、段内页号和页内位移三部分组成。

## 内存分配的过程
应用程序通过 malloc 函数申请内存的时候，实际上申请的是虚拟内存，此时并不会分配物理内存。
当应用程序读写了这块虚拟内存，CPU 就会去访问这个虚拟内存， 这时会发现这个虚拟内存没有映射到物理内存， CPU 就会产生缺页中断，进程会从用户态切换到内核态，并将缺页中断交给内核的 Page Fault Handler （缺页中断函数）处理。
缺页中断处理函数会看是否有空闲的物理内存：
如果有，就直接分配物理内存，并建立虚拟内存与物理内存之间的映射关系。
如果没有空闲的物理内存，那么内核就会开始进行回收内存 (opens new window)的工作，如果回收内存工作结束后，空闲的物理内存仍然无法满足此次物理内存的申请，那么内核就会放最后的大招了触发 OOM （Out of Memory）机制。
32 位系统的内核空间占用 1G，位于最高处，剩下的 3G 是用户空间；
64 位系统的内核空间和用户空间都是 128T，分别占据整个内存空间的最高和最低处，剩下的中间部分是未定义的。

内存溢出(Out Of Memory，简称OOM)是指应用系统中存在无法回收的内存或使用的内存过多，最终使得程序运行要用到的内存大于能提供的最大内存。此时程序就运行不了，系统会提示内存溢出。

### 虚拟内存的作用
第一，虚拟内存可以使得进程对运行内存超过物理内存大小，因为程序运行符合局部性原理，CPU 访问内存会有很明显的重复访问的倾向性，对于那些没有被经常使用到的内存，我们可以把它换出到物理内存之外，比如硬盘上的 swap 区域。
第二，由于每个进程都有自己的页表，所以每个进程的虚拟内存空间就是相互独立的。进程也没有办法访问其他进程的页表，所以这些页表是私有的，这就解决了多进程之间地址冲突的问题。
第三，页表里的页表项中除了物理地址之外，还有一些标记属性的比特，比如控制一个页的读写权限，标记该页是否存在等。在内存访问方面，操作系统提供了更好的安全性。


## swap机制
当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间会被临时保存到磁盘，等到那些程序要运行时，再从磁盘中恢复保存的数据到内存中。

另外，当内存使用存在压力的时候，会开始触发内存回收行为，会把这些不常访问的内存先写到磁盘中，然后释放这些内存，给其他更需要的进程使用。再次访问这些内存时，重新从磁盘读入内存就可以了。

这种，将内存数据换出磁盘，又从磁盘中恢复数据到内存的过程，就是 Swap 机制负责的。

Swap 就是把一块磁盘空间或者本地文件，当成内存来使用，它包含换出和换入两个过程：

换出（Swap Out） ，是把进程暂时不用的内存数据存储到磁盘中，并释放这些数据占用的内存；
换入（Swap In），是在进程再次访问这些内存的时候，把它们从磁盘读到内存中来；

使用 Swap 机制优点是，应用程序实际可以使用的内存空间将远远超过系统的物理内存。由于硬盘空间的价格远比内存要低，因此这种方式无疑是经济实惠的。当然，频繁地读写硬盘，会显著降低操作系统的运行速率，这也是 Swap 的弊端。


Linux提供了两种方法启用Swap，分别是Swap分区和Swap文件；

## Linux操作系统的缓存
在应用程序读取文件的数据的时候，Linux操作系统会对读取的文件数据进行缓存，会缓存在文件系统中的Page Cache（页缓存）；
Page Cache 属于内存空间里的数据，由于内存访问比磁盘访问快很多，在下一次访问相同的数据就不需要通过磁盘 I/O 了，命中缓存就直接返回数据即可。
因此，Page Cache 起到了加速访问数据的作用。

### 预读机制
Linux 操作系统为基于 Page Cache 的读缓存机制提供预读机制
比如说，应用程序利用 read 系统调动读取 4KB 数据，实际上内核使用预读机制（ReadaHead） 机制完成了 16KB 数据的读取，也就是通过一次磁盘顺序读将多个 Page 数据装入 Page Cache。
这样下次读取 4KB 数据后面的数据的时候，就不用从磁盘读取了，直接在 Page Cache 即可命中数据。因此，预读机制带来的好处就是减少了 磁盘 I/O 次数，提高系统磁盘 I/O 吞吐量。


如果这些被提前加载进来的页，并没有被访问，相当于这个预读工作是白做了，这个就是预读失效。
如果使用传统的 LRU 算法，就会把「预读页」放到 LRU 链表头部，而当内存空间不够的时候，还需要把末尾的页淘汰掉。
如果这些「预读页」如果一直不会被访问到，就会出现一个很奇怪的问题，不会被访问的预读页却占用了 LRU 链表前排的位置，而末尾淘汰的页，可能是热点数据，这样就大大降低了缓存命中率 。

如何避免预读失效带来的影响？
实现两个LRU链表：活跃LRU链表（active_list）和非活跃LRU链表（inactive_list）；
active_list活跃内存页链表：存放的是最近被访问的内存页；
inactive_list非活跃内存页链表：存放是很少被访问的内存页；

预读页就只需要加入到 inactive list 区域的头部，当页被真正访问的时候，才将页插入 active list 的头部。如果预读的页一直没有被访问，就会从 inactive list 移除，这样就不会影响 active list 中的热点数据。


### 缓存污染
还是使用「只要数据被访问一次，就将数据加入到活跃 LRU 链表头部（或者 young 区域）」这种方式的话，那么还存在缓存污染的问题。
当我们在批量读取数据的时候，由于数据被访问了一次，这些大量数据都会被加入到「活跃 LRU 链表」里，然后之前缓存在活跃 LRU 链表（或者 young 区域）里的热点数据全部都被淘汰了，如果这些大量的数据在很长一段时间都不会被访问的话，那么整个活跃 LRU 链表（或者 young 区域）就被污染了。

只要我们提高进入到活跃 LRU 链表（或者 young 区域）的门槛，就能有效地保证活跃 LRU 链表（或者 young 区域）里的热点数据不会被轻易替换掉。
Linux 操作系统和 MySQL Innodb 存储引擎分别是这样提高门槛的：
Linux 操作系统：在内存页被访问第二次的时候，才将页从 inactive list 升级到 active list 里。
MySQL Innodb：在内存页被访问第二次的时候，并不会马上将该页从 old 区域升级到 young 区域，因为还要进行停留在 old 区域的时间判断：
如果第二次的访问时间与第一次访问的时间在 1 秒内（默认值），那么该页就不会被从 old 区域升级到 young 区域；
如果第二次的访问时间与第一次访问的时间超过 1 秒，那么该页就会从 old 区域升级到 young 区域；

### 程序局部性原理
时间局部性和空间局部性。时间局部性是指如果程序中的某条指令一旦执行，则不久之后该指令可能再次被执行；如果某块数据被访问，则不久之后该数据可能再次被访问。空间局部性是指一旦程序访问了某个存储单元，则不久之后，其附近的存储单元也将被访问。





# 进程间通信
## 管道
mkfifo myPipe    # 创建管道，在目录内生成一个文件，文件类型是p
echo test > myPipe    # 往管道内写数据,执行命令停在这里不动了，这是因为管道内的数据没有被读取；
cat < myPipe          # 读取管道内的数据，另一方面上面的echo也正常退出了。


# 格式化json：
cat test.json |python -m json.tool；


# pdcp
用于将文件或目录传输到多个远程主机上。
pdcp -w remote1,remote2,...remote10 local_file remote_directory/  将本地的local_file复制到每个指定的远程主机的remote_directory目录下。
pdcp的常用选项包括：
-w：指定要复制的远程主机列表，可以使用逗号分隔的主机名、IP地址或者主机列表文件。
-R：递归地复制整个目录。
-p：保留源文件的权限、所有者和时间戳等元数据。
-l：限制并行复制的最大进程数。


## pdsh
使用pdsh命令在多个远程主机上同时执行命令。需要在每个主机上安装pdsh包。
使用实例：pdsh -w host1,host2,host3 systemctl restart httpd



# 搭建本地yum源供其它主机rpm包安装
1. 本地路径/opt/src/slurm/下放rpm包；
2. 在上述路径下执行createrepo . 命令生成仓库索引；
3. 安装httpd包；
4. 新建文件/etc/httpd/conf.d/myrepo.conf，内容如下：
Alias /myrepo /opt/src/slurm
<Directory /opt/src/slurm>
    Options Indexes FollowSymLinks
    Require all granted
</Directory>
5. 为使用此yum源的主机增加文件/etc/yum.repos.d/slurm.repo
[slurm]
name=slurm
baseurl=http://9.9.33.117:80/myrepo
gpgcheck=0
enable=1



# slurm
开源的、具有容错性和高度可扩展的Linux集群超级计算系统资源管理和作业调度系统。
srun --mpi=list   # 列出已安装的mpi插件；
srun -w slurm[1-2] hostname  # 交互式作业提交，提交命令后，等待作业执行完成之后返回命令行窗口。

scontrol show jobs        # 查看MPI作业详细信息
scontrol show node              #查看所有节点详细信息
scontrol show node node-name    #查看指定节点详细信息
scontrol show node | grep CPU   #查看各节点cpu状态
scontrol show node node-name | grep CPU #查看指定节点cpu状态
scontrol token username=root lifespan=5000    # 为用户root生成token，token有效期5000s


sacct	                # 查看已完成作业

squeue                  # 查看job的运行状态
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
49     batch        myapp     root  R       0:12      1 slurm1
50     batch        myapp     root  R       0:04      1 slurm2

脚本SBATCH参数
-J,--job-name：指定作业名称
-N,--nodes：节点数量
-n,--ntasks：使用的CPU核数
--mem：指定每个节点上使用的物理内存
-t,--time：运行时间，超出时间限制的作业将被终止
-p,--partition：指定分区
--reservation：资源预留
-w,--nodelist：指定节点运行作业
-x,--exclude：分配给作业的节点中不要包含指定节点
--ntasks-per-node：指定每个节点使用几个CPU核心
--begin：指定作业开始时间
-D，--chdir：指定脚本/命令的工作目录


#添加账户，指定账户名称和所属集群名称，这里的账户可以理解成用户组的概念
sacctmgr add account name=<your_account> cluster=cluster

#添加用户，指定所属账户
sacctmgr add user name=my_user_name account=<your_account>

#添加俩qos，分别叫normal和long
sacctmgr add qos normal
sacctmgr add qos long

#修改qos
sacctmgr modify qos normal set MaxWall=3-00:00:00 MaxTRES="gres/gpu=4" MaxTRESPU="gres/gpu=4" MaxJobsPU=4 MaxSubmitJobsPU=4
sacctmgr modify qos long set MaxWall=7-00:00:00 MaxTRES="gres/gpu=8" MaxTRESPU="gres/gpu=8" MaxJobsPU=1 MaxSubmitJobsPU=1

#设置account可使用的qoslevel
sacctmgr modify account my_account_name set QosLevel=normal,long



## 场景
1. 2个计算节点，执行三个job，srun ./myapp -p 30000，2个job分别分发到2个计算节点上，第3个job等待；
srun: job 53 queued and waiting for resources
srun: job 53 has been allocated resources
squeue查看
53     batch    myapp     root PD       0:00      1 (Resources)

2. 设置任务在一个节点上运行，占用一个CPU，使用2G的运行内存，这样三个任务可以同时在此节点运行；
JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
69     batch calculate     root  R       0:11      1 slurm1
70     batch calculate     root  R       0:11      1 slurm1
71     batch calculate     root  R       0:08      1 slurm1
```shell
#SBATCH -N 1
#SBATCH --ntasks 1
#SBATCH --cpus-per-task=1
#SBATCH -t 1:00
#SBATCH -o /home/myapp.log
#SBATCH --ntasks-per-node 1
#SBATCH --mem 2000
```



## slurm安装
1. 安装jansson
wget http://www.digip.org/jansson/releases/jansson-2.13.1.tar.gz
tar -zxvf ~/jansson-2.13.1.tar.gz
cd ~/jansson-2.13.1
./configure --prefix=/user/local/jansson-2.13.1
cp jansson.pc /usr/lib/pkgconfig/jansson.pc

2. 编译安装libjwt
git clone --depth 1 --single-branch -b v1.12.0 https://github.com/benmcollins/libjwt.git libjwt
mv libjwt libjwt-1.12.0
cd libjwt-1.12.0
autoreconf --force --install
./configure --prefix=/usr/local/libjwt-1.12.0 && make j && make install
cd /usr/local && ln -s libjwt-1.12.0 libjwt

3. 编译安装slurm
下载源码slurm-20.11.9.tar.bz2，解压/home/slurm-20.11.9；
./configure --prefix=/usr/local/slurm-20.11.9 --sysconfdir=/etc/slurm/ --with-jwt=/usr/local/libjwt --enable-slurmrestd && make -j16 && make install 
配置文件路径 /etc/slurm/slurm.conf
```shell
# Cluster Name：集群名
 ClusterName=MyCluster # 集群名，任意英文和数字名字

# Control Machines：Slurmctld控制进程节点
 SlurmctldHost=slurm # 启动slurmctld进程的节点名，如这里的admin
# BackupController=   # 冗余备份节点，可空着
 SlurmctldParameters=enable_configless # 采用无配置模式

 # Slurm User：Slurm用户
 SlurmUser=slurm # slurmctld启动时采用的用户名

 # Slurm Port Numbers：Slurm服务通信端口
 SlurmctldPort=6817 # Slurmctld服务端口，设为6817，如不设置，默认为6817号端口
 SlurmdPort=6818    # Slurmd服务端口，设为6818，如不设置，默认为6818号端口

 # State Preservation：状态保持
 StateSaveLocation=/var/spool/slurmctld # 存储slurmctld服务状态的目录，如有备份控制节点，则需要所有SlurmctldHost节点都能共享读写该目录
 SlurmdSpoolDir=/var/spool/slurmd # Slurmd服务所需要的目录，为各节点各自私有目录，不得多个slurmd节点共享

 # auth type
 AuthAltTypes=auth/jwt
 AuthAltParameters=jwt_key=/var/spool/slurm/statesave/jwt_hs256.key

 ReturnToService=1 #设定当DOWN（失去响应）状态节点如何恢复服务，默认为0。
     # 0: 节点状态保持DOWN状态，只有当管理员明确使其恢复服务时才恢复
     # 1: 仅当由于无响应而将DOWN节点设置为DOWN状态时，才可以当有效配置注册后使DOWN节点恢复服务。如节点由于任何其它原因（内存不足、意外重启等）被设置为DOWN，其状态将不会自动更改。当节点的内存、GRES、CPU计数等等于或大于slurm.conf中配置的值时，该节点才注册为有效配置。
     # 2: 使用有效配置注册后，DOWN节点将可供使用。该节点可能因任何原因被设置为DOWN状态。当节点的内存、GRES、CPU计数等等于或大于slurm.conf 中配置的值，该节点才注册为有效配置。￼

 # Default MPI Type：默认MPI类型
 MPIDefault=pmi2
     # MPI-PMI2: 对支持PMI2的MPI实现
     # MPI-PMIx: Exascale PMI实现
     # None: 对于大多数其它MPI，建议设置

 # Process Tracking：进程追踪，定义用于确定特定的作业所对应的进程的算法，它使用信号、杀死和记账与作业步相关联的进程
 ProctrackType=proctrack/cgroup
     # Cgroup: 采用Linux cgroup来生成作业容器并追踪进程，需要设定/etc/slurm/cgroup.conf文件
     # Cray XC: 采用Cray XC专有进程追踪
     # LinuxProc: 采用父进程IP记录，进程可以脱离Slurm控制
     # Pgid: 采用Unix进程组ID(Process Group ID)，进程如改变了其进程组ID则可以脱离Slurm控制

 # Scheduling：调度
 # DefMemPerCPU=0 # 默认每颗CPU可用内存，以MB为单位，0为不限制。如果将单个处理器分配给作业（SelectType=select/cons_res 或 SelectType=select/cons_tres），通常会使用DefMemPerCPU
 # MaxMemPerCPU=0 # 最大每颗CPU可用内存，以MB为单位，0为不限制。如果将单个处理器分配给作业（SelectType=select/cons_res 或 SelectType=select/cons_tres），通常会使用MaxMemPerCPU
 # SchedulerTimeSlice=30 # 当GANG调度启用时的时间片长度，以秒为单位
 SchedulerType=sched/backfill # 要使用的调度程序的类型。注意，slurmctld守护程序必须重新启动才能使调度程序类型的更改生效（重新配置正在运行的守护程序对此参数无效）。如果需要，可以使用scontrol命令手动更改作业优先级。可接受的类型为：
     # sched/backfill # 用于回填调度模块以增加默认FIFO调度。如这样做不会延迟任何较高优先级作业的预期启动时间，则回填调度将启动较低优先级作业。回填调度的有效性取决于用户指定的作业时间限制，否则所有作业将具有相同的时间限制，并且回填是不可能的。注意上面SchedulerParameters选项的文档。这是默认配置
     # sched/builtin # 按优先级顺序启动作业的FIFO调度程序。如队列中的任何作业无法调度，则不会调度该队列中优先级较低的作业。对于作业的一个例外是由于队列限制（如时间限制）或关闭/耗尽节点而无法运行。在这种情况下，可以启动较低优先级的作业，而不会影响较高优先级的作业。
     # sched/hold # 如果 /etc/slurm.hold 文件存在，则暂停所有新提交的作业，否则使用内置的FIFO调度程序。

 # Resource Selection：资源选择，定义作业资源（节点）选择算法
 SelectType=select/cons_tres
     # select/cons_tres: 单个的CPU核、内存、GPU及其它可追踪资源作为可消费资源（消费及分配），建议设置
     # select/cons_res: 单个的CPU核和内存作为可消费资源
     # select/cray_aries: 对于Cray系统
     # select/linear: 基于主机的作为可消费资源，不管理单个CPU等的分配

 # SelectTypeParameters：资源选择类型参数，当SelectType=select/linear时仅支持CR_ONE_TASK_PER_CORE和CR_Memory；当SelectType=select/cons_res、SelectType=select/cray_aries和SelectType=select/cons_tres时，默认采用CR_Core_Memory
 SelectTypeParameters=CR_Core_Memory
     # CR_CPU: CPU核数作为可消费资源
     # CR_Socket: 整颗CPU作为可消费资源
     # CR_Core: CPU核作为可消费资源，默认
     # CR_Memory: 内存作为可消费资源，CR_Memory假定MaxShare大于等于1
     # CR_CPU_Memory: CPU和内存作为可消费资源
     # CR_Socket_Memory: 整颗CPU和内存作为可消费资源
     # CR_Core_Memory: CPU和和内存作为可消费资源

 # Task Launch：任务启动
 TaskPlugin=task/cgroup,task/affinity #设定任务启动插件。可被用于提供节点内的资源管理（如绑定任务到特定处理器），TaskPlugin值可为:
     # task/affinity: CPU亲和支持（man srun查看其中--cpu-bind、--mem-bind和-E选项）
     # task/cgroup: 强制采用Linux控制组cgroup分配资源（man group.conf查看帮助）
     # task/none: #无任务启动动作

 # Prolog and Epilog：前处理及后处理
 # Prolog/Epilog: 完整的绝对路径，在用户作业开始前(Prolog)或结束后(Epilog)在其每个运行节点上都采用root用户执行，可用于初始化某些参数、清理作业运行后的可删除文件等
 # Prolog=/opt/bin/prolog.sh # 作业开始运行前需要执行的文件，采用root用户执行
 # Epilog=/opt/bin/epilog.sh # 作业结束运行后需要执行的文件，采用root用户执行

 # SrunProlog/Epilog # 完整的绝对路径，在用户作业步开始前(SrunProlog)或结束后(Epilog)在其每个运行节点上都被srun执行，这些参数可以被srun的--prolog和--epilog选项覆盖
 # SrunProlog=/opt/bin/srunprolog.sh # 在srun作业开始运行前需要执行的文件，采用运行srun命令的用户执行
 # SrunEpilog=/opt/bin/srunepilog.sh # 在srun作业结束运行后需要执行的文件，采用运行srun命令的用户执行

 # TaskProlog/Epilog: 绝对路径，在用户任务开始前(Prolog)和结束后(Epilog)在其每个运行节点上都采用运行作业的用户身份执行
 # TaskProlog=/opt/bin/taskprolog.sh # 作业开始运行前需要执行的文件，采用运行作业的用户执行
 # TaskEpilog=/opt/bin/taskepilog.sh # 作业结束后需要执行的文件，采用运行作业的用户执行行

 # 顺序：
    # 1. pre_launch_priv()：TaskPlugin内部函数
    # 2. pre_launch()：TaskPlugin内部函数
    # 3. TaskProlog：slurm.conf中定义的系统范围每个任务
    # 4. User prolog：作业步指定的，采用srun命令的--task-prolog参数或SLURM_TASK_PROLOG环境变量指定
    # 5. Task：作业步任务中执行
    # 6. User epilog：作业步指定的，采用srun命令的--task-epilog参数或SLURM_TASK_EPILOG环境变量指定
    # 7. TaskEpilog：slurm.conf中定义的系统范围每个任务
    # 8. post_term()：TaskPlugin内部函数

 # Event Logging：事件记录
 # Slurmctld和slurmd守护进程可以配置为采用不同级别的详细度记录，从0（不记录）到7（极度详细）
 SlurmctldDebug=debug # 默认为info
 SlurmctldLogFile=/var/log/slurm/slurmctld.log # 如是空白，则记录到syslog
 SlurmdDebug=debug # 默认为info
 SlurmdLogFile=/var/log/slurm/slurmd.log # 如为空白，则记录到syslog，如名字中的有字符串"%h"，则"%h"将被替换为节点名
 #SlurmrestdDebug=debug
 #SlurmrestdLogFile=/var/log/slurm/slurmrestd.log

 # Job Completion Logging：作业完成记录
 JobCompType=jobcomp/mysql
 # 指定作业完成是采用的记录机制，默认为None，可为以下值之一:
    # None: 不记录作业完成信息
    # Elasticsearch: 将作业完成信息记录到Elasticsearch服务器
    # FileTxt: 将作业完成信息记录在一个纯文本文件中
    # Lua: 利用名为jobcomp.lua的文件记录作业完成信息
    # Script: 采用任意脚本对原始作业完成信息进行处理后记录
    # MySQL: 将完成状态写入MySQL或MariaDB数据库

 # JobCompLoc= # 设定记录作业完成信息的文本文件位置（若JobCompType=filetxt），或将要运行的脚本（若JobCompType=script），或Elasticsearch服务器的URL（若JobCompType=elasticsearch），或数据库名字（JobCompType为其它值时）

 # 设定数据库在哪里运行，且如何连接
 JobCompHost=localhost # 存储作业完成信息的数据库主机名
 # JobCompPort= # 存储作业完成信息的数据库服务器监听端口
 JobCompUser=slurm # 用于与存储作业完成信息数据库进行对话的用户名
 JobCompPass=SomePassWD # 用于与存储作业完成信息数据库进行对话的用户密码

 # Job Accounting Gather：作业记账收集
 JobAcctGatherType=jobacct_gather/linux # Slurm记录每个作业消耗的资源，JobAcctGatherType值可为以下之一：
    # jobacct_gather/none: 不对作业记账
    # jobacct_gather/cgroup: 收集Linux cgroup信息
    # jobacct_gather/linux: 收集Linux进程表信息，建议
 JobAcctGatherFrequency=30 # 设定轮寻间隔，以秒为单位。若为-，则禁止周期性抽样

 # Job Accounting Storage：作业记账存储
 AccountingStorageType=accounting_storage/slurmdbd # 与作业记账收集一起，Slurm可以采用不同风格存储可以以许多不同的方式存储会计信息，可为以下值之一：
     # accounting_storage/none: 不记录记账信息
     # accounting_storage/slurmdbd: 将作业记账信息写入Slurm DBD数据库
 # AccountingStorageLoc: 设定文件位置或数据库名，为完整绝对路径或为数据库的数据库名，当采用slurmdb时默认为slurm_acct_db

 # 设定记账数据库信息，及如何连接
 AccountingStorageHost=localhost # 记账数据库主机名
 # AccountingStoragePort= # 记账数据库服务监听端口
 # AccountingStorageUser=slurm # 记账数据库用户名
 # AccountingStoragePass=SomePassWD # 记账数据库用户密码。对于SlurmDBD，提供企业范围的身份验证，如采用于Munge守护进程，则这是应该用munge套接字socket名（/var/run/munge/global.socket.2）代替。默认不设置
 # AccountingStoreFlags= # 以逗号（,）分割的列表。选项是：
     # job_comment：在数据库中存储作业说明域
     # job_script：在数据库中存储脚本
     # job_env：存储批处理作业的环境变量
 # AccountingStorageTRES=gres/gpu # 设置GPU时需要
 # GresTypes=gpu # 设置GPU时需要

 # Process ID Logging：进程ID记录，定义记录守护进程的进程ID的位置
 SlurmctldPidFile=/var/run/slurmctld.pid # 存储slurmctld进程号PID的文件
 SlurmdPidFile=/var/run/slurmd.pid # 存储slurmd进程号PID的文件

 # Timers：定时器
 SlurmctldTimeout=120 # 设定备份控制器在主控制器等待多少秒后成为激活的控制器
 SlurmdTimeout=300 # Slurm控制器等待slurmd未响应请求多少秒后将该节点状态设置为DOWN
 InactiveLimit=0 # 潜伏期控制器等待srun命令响应多少秒后，将在考虑作业或作业步骤不活动并终止它之前。0表示无限长等待
 MinJobAge=300 # Slurm控制器在等待作业结束多少秒后清理其记录
 KillWait=30 # 在作业到达其时间限制前等待多少秒后在发送SIGKILLL信号之前发送TERM信号以优雅地终止
 WaitTime=0 # 在一个作业步的第一个任务结束后等待多少秒后结束所有其它任务，0表示无限长等待

 # Compute Machines：计算节点
 NodeName=slurm2 NodeAddr=192.168.32.227 CPUs=4 RealMemory=7800 Sockets=4 CoresPerSocket=1 ThreadsPerCore=1 State=UNKNOWN
 NodeName=slurm1 NodeAddr=192.168.32.210 CPUs=4 RealMemory=7800 Sockets=4 CoresPerSocket=1 ThreadsPerCore=1 State=UNKNOWN
 # NodeName=gnode[01-10] Gres=gpu:v100:2 CPUs=40 RealMemory=385560 Sockets=2 CoresPerSocket=20 ThreadsPerCore=1 State=UNKNOWN #GPU节点例子，主要为Gres=gpu:v100:2
     # NodeName=node[1-10] # 计算节点名，node[1-10]表示为从node1、node2连续编号到node10，其余类似
     # NodeAddr=192.168.1.[1-10] # 计算节点IP
     # CPUs=48 # 节点内CPU核数，如开着超线程，则按照2倍核数计算，其值为：Sockets*CoresPerSocket*ThreadsPerCore
     # RealMemory=192000 # 节点内作业可用内存数(MB)，一般不大于free -m的输出，当启用select/cons_res插件限制内存时使用
     # Sockets=2 # 节点内CPU颗数
     # CoresPerSocket=24 # 每颗CPU核数
     # ThreadsPerCore=1 # 每核逻辑线程数，如开了超线程，则为2
     # State=UNKNOWN # 状态，是否启用，State可以为以下之一：
         # CLOUD   # 在云上存在
         # DOWN    # 节点失效，不能分配给在作业
         # DRAIN   # 节点不能分配给作业
         # FAIL    # 节点即将失效，不能接受分配新作业
         # FAILING # 节点即将失效，但上面有作业未完成，不能接收新作业
         # FUTURE  # 节点为了将来使用，当Slurm守护进程启动时设置为不存在，可以之后采用scontrol命令简单地改变其状态，而不是需要重启slurmctld守护进程。当这些节点有效后，修改slurm.conf中它们的State。在它们被设置为有效前，采用Slurm看不到它们，也尝试与其联系。
               # 动态未来节点(Dynamic Future Nodes)：
                  # slurmd启动时如有-F[<feature>]参数，将关联到一个与slurmd -C命令显示配置(sockets、cores、threads)相同的配置的FUTURE节点。节点的NodeAddr和NodeHostname从slurmd守护进程自动获取，并且当被设置为FUTURE状态后自动清除。动态未来节点在重启时保持non-FUTURE状态。利用scontrol可以将其设置为FUTURE状态。
                  # 若NodeName与slurmd的HostName映射未通过DNS更新，动态未来节点不知道在之间如何进行通信，其原因在于NodeAddr和NodeHostName未在slurm.conf被定义，而且扇出通信(fanout communication)需要通过将TreeWidth设置为一个较高的数字（如65533）来使其无效。若做了DNS映射，则可以使用cloud_dns SlurmctldParameter。
          # UNKNOWN # 节点状态未被定义，但将在节点上启动slurmd进程后设置为BUSY或IDLE，该为默认值。

 PartitionName=batch Nodes=slurm[1-2] Default=YES MaxTime=INFINITE State=UP
     # PartitionName=batch # 队列分区名
     # Nodes=node[1-10] # 节点名
     # Default=Yes # 作为默认队列，运行作业不知明队列名时采用的队列
     # MaxTime=INFINITE # 作业最大运行时间，以分钟为单位，INFINITE表示为无限制
     # State=UP # 状态，是否启用
     # Gres=gpu:v100:2 # 设置节点有两块v100 GPU卡，需要在GPU节点 /etc/slum/gres.conf 文件中有类似下面配置：
         #AutoDetect=nvml
         #Name=gpu Type=v100 File=/dev/nvidia[0-1] #设置资源的名称Name是gpu，类型Type为v100，名称与类型可以任意取，但需要与其它方面配置对应，File=/dev/nvidia[0-1]指明了使用的GPU设备。
         #Name=mps Count=100
```
4. 配置slurm
```shell
SLURMPATH=/opt/slurm/21.08.6
echo "export PATH=\$PATH:$SLURMPATH/bin:$SLURMPATH/sbin" >> /etc/bash.bashrc
source /etc/bash.bashrc
```

5. slurm对jwt的支持
新建JWT key到控制器。该文件位于/var/spool/slurm/statesave/jwt_hs256.key
```shell
dd if=/dev/random of=/var/spool/slurm/statesave/jwt_hs256.key bs=32 count=1
chown slurm:slurm /var/spool/slurm/statesave/jwt_hs256.key
chmod 0600 /var/spool/slurm/statesave/jwt_hs256.key
chown slurm:slurm /var/spool/slurm/statesave
chmod 0755 /var/spool/slurm/statesave
```
slurmdbd进程仅仅运行在管理节点上，但修改了两个文件之后，还是应该把两个配置文件同步到计算节点上。
重启slurmctld和slurmdbd。

6. 关于slurmrestd服务
把Slurm为我们准备的slurmrestd.service拷贝至/etc/systemd/system目录下，该文件位于slurm安装包源文件目录下
cd ~/slurm-20.11.9/ && cp etc/slurmrestd.service /etc/systemd/system
systemctl daemon-reload && systemctl restart slurmrestd

7. 其余步骤参考 https://hmli.ustc.edu.cn/doc/linux/slurm-install/slurm-install.html#id1




# terraform

## terraform安装
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform


## 通过terraform编排openstack
1. 获得脚本到本地https://github.com/tf-openstack-modules/terraform-openstack-instances；
2. 增加provider.tf；
```shell
provider "openstack" {
  user_name = "admin"
  tenant_name = "admin"
  password  = "a0vhHyaRdAAkq2vH76rnMODTQFm6gnh8r0W7Opi6"
  auth_url  = "http://9.9.33.5:5000/v3"
  domain_name = "Default"
}
```
3. 设置变量
```shell
[root@openstack--1 terraform-openstack-instances-main]# cat 000-input-variables.tf
variable "key_pair_name" {
  type = string
  description = <<EOF
The name of the ssh key referenced in openstack
EOF
}

variable "image_id" {
  type = string
  default = "67b0efe9-2534-4b33-b9f8-6943e3ed2671"
  description = <<EOF
The image's id referenced in openstack
EOF
}

variable "name" {
  type = string
  default = "vm_test"
  description = <<EOF
Instance's name
EOF
}

variable "flavor_name" {
  type = string
  default = "4c4g16g"
  description = <<EOF
Instance's flavor name referenced in openstack
EOF
}

variable "public_ip_network" {
  type = string
  description = <<EOF
The name of the network who give floating IPs
EOF
  default = "ext_net"
}

variable "is_public" {
  type = bool
  description = <<EOF
Boolean who allow you to to make public or not your instance
EOF
  default = true
}

variable "ports" {
  type = list(object({
    name = string
    network_id = string
    subnet_id = string
    admin_state_up = optional(bool)
    security_group_ids = optional(list(string))
    ip_address = optional(string)
  }))
  description = <<EOF
The ports list, at least 1 port is required
EOF
  default = [{
    name = "port_test"
    network_id = "875de082-e7c6-429d-9ea7-8a31a66b18cc"
    subnet_id = "53ae6630-ce8e-434c-9a08-26bd590a53c9"
    admin_state_up = true
    security_group_ids = []
  }]
}

variable "block_device_volume_size" {
  type = number
  description = <<EOF
The volume size of block device
EOF
  default = 20
}

variable "block_device_delete_on_termination" {
  type = bool
  description = <<EOF
Delete block device when instance is shut down
EOF
  default = true
}

variable "server_groups" {
  type = list(string)
  description = <<EOF
List of server group id
EOF
  default = []
}
```

4. 设置资源参数
```shell
[root@openstack--1 terraform-openstack-instances-main]# cat 010-computes.tf
#
# Create instance
#
resource "openstack_compute_instance_v2" "instance" {
  name        = var.name
  flavor_name = var.flavor_name

  block_device {
    uuid                  = var.image_id
    source_type           = "image"
    volume_size           = var.block_device_volume_size
    boot_index            = 0
    destination_type      = "volume"
    delete_on_termination = var.block_device_delete_on_termination
  }

  key_pair = var.key_pair_name

  dynamic "scheduler_hints" {
    for_each = var.server_groups
    content {
      group = scheduler_hints.value
    }
  }

  dynamic "network" {
    for_each = openstack_networking_port_v2.port

    content {
      port = network.value["id"]
    }
  }
}

# Create network port

resource "openstack_networking_port_v2" "port" {
  count = length(var.ports)

  name = var.ports[count.index].name
  network_id = var.ports[count.index].network_id
  admin_state_up = var.ports[count.index].admin_state_up == null ? true : var.ports[count.index].admin_state_up
  security_group_ids = var.ports[count.index].security_group_ids == null ? [] : var.ports[count.index].security_group_ids
  fixed_ip {
    subnet_id = var.ports[count.index].subnet_id
    ip_address = var.ports[count.index].ip_address == null ? null : var.ports[count.index].ip_address
  }
}

# Create floating ip
resource "openstack_networking_floatingip_v2" "ip" {
  count = var.is_public ? 1 : 0

  pool = var.public_ip_network
}

# Attach floating ip to instance
resource "openstack_compute_floatingip_associate_v2" "ipa" {
  count = var.is_public ? 1: 0

  floating_ip = openstack_networking_floatingip_v2.ip[count.index].address
  instance_id = openstack_compute_instance_v2.instance.id
}
```

6. 设置输出形式
```shell
[root@openstack--1 terraform-openstack-instances-main]# cat 999-outputs.tf
output "instance" {
  value = openstack_compute_instance_v2.instance
  sensitive = true
}

output "ip" {
  value = openstack_networking_floatingip_v2.ip
}
```

7. terraform init;
8. terraform plan;
9. terraform apply；






# 概念
## kubeflow
它提供了一套工具和组件，用于构建、训练、部署和管理机器学习模型的端到端工作流程。

## rancher
Rancher提供了在生产环境中使用的管理Docker和Kubernetes的全栈化容器部署与管理平台。
也就是提供可视化web界面进行k8s部署；

## CRD
它代表自定义资源定义（Custom Resource Definition）。CRD 允许用户扩展 Kubernetes API，以添加自定义资源和自定义控制器。
Kubernetes 中的资源（Resource）是 API 对象的实例，例如 Pod、Service、Deployment 等。这些资源都有相应的 API 定义和控制器，用于管理它们的生命周期和状态。
CRD 允许用户定义自己的资源类型，这些资源类型可以扩展 Kubernetes 的功能，以满足特定的需求。用户可以创建自定义资源定义，定义自己的资源结构和行为，并编写自定义控制器来管理这些资源。

## HPC
高性能计算（High Performance Computing，缩写HPC）指利用聚集起来的计算能力来处理标准工作站无法完成的数据密集型的计算任务。

## tensorflow
TensorFlow是一个基于数据流编程的符号数学系统，被广泛应用于各类机器学习算法的编程实现。
PS-worker模型：Parameter Server执行模型相关业务，Work Server训练相关业务，推理计算、梯度计算等。

## NPU
神经处理单元（Neural Processing Unit）的缩写，它是一种专门设计用于进行人工神经网络计算的处理器。NPU 的设计旨在加速深度学习任务，包括图像识别、语音识别、自然语言处理等


# volcano
基础设施调度引擎，基于Kubernetes的容器批量计算平台，主要用于高性能计算场景。
作为一个通用批处理平台，Volcano与几乎所有的主流计算框架无缝对接，如Spark 、TensorFlow 、PyTorch 、 Flink 、Argo 、MindSpore、 PaddlePaddle等。

如何查看Volcano scheduler的配置
kubectl get configmap -n volcano-system
kubectl get configmap volcano-scheduler-configmap -n volcano-system -oyaml


## queue
Queue 是一个 PodGroup 队列，PodGroup 是一组强关联的 Pod 集合。而 VolcanoJob 则是一个 K8s Job 升级版，对应的下一级资源是 PodGroup。换言之，就好比 ReplicaSet 的下一级资源是 Pod 一样。
1. queue名称不能重复；
2. 名称限制：a lowercase RFC 1123 subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character (e.g. 'example.com', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)*'
3. Capability: 表示该queue中所有podgroup使用资源之和的上限，它是个硬约束；
4. Guarantee: 表示队列的预留资源；
5. Reclaimable: 表示该queue在资源使用量超过该queue所应得的资源份额时，是否允许其它queue回收该queue使用超额的资源，默认true；
6. Weight: 表示该queue在集群资源划分中所占的相对比重；
7. Status: podgroup统计信息与queue状态信息；



## podgroup
互相关联的一组pod集合。
1. 当queue设置Capability为2时，创建vcjob设置tasks.template.spec.containers.resources.requests大于2时，则不能调度；
podgroup和job都是PENDING状态，podgroup事件信息queue resource quota insufficient，vcjob事件信息是pod group is not ready；
2. minMember: 运行作业必须启动的最小总task数量；
3. minResources: 运行作业必须的最小资源；
4. minTaskMember: 运行作业所需的各类task的最小数量，其和等于minMember；
5. priorityClassName: 作业的优先级，用于调度抢占等行为中对作业进行排序，默认为0；
6. queue: 表示该podgroup所属的queue，必须提前创建好且状态为open；
7. status: 记录podgroup的状态；
8. conditions: 该podgroup的具体状态日志；
9. phase: 作业整体状态；
10. running: 处于running状态的pod数量；



## vcjob
1. 当vcjob删除时，跟着vcjob创建的podgroup一同删除了；
2. svc插件实现同一vcjob中各pod之间的通信；
3. sla插件，作业最长等待时间sla-waiting-time，表示作业停留在pending状态不被调度的最长等待时间；既可以配置在configmap中，作为sla插件的参数，对经由volcano调度的全部作业生效；也可以单独配置在作业的annotation中，只对改作业生效；
4. gpu共享。通过使用资源名volcano.sh/gpu-memory进行容器级的资源需求共享；多个pod共享一张GPU卡。
5. enqueue action
通过资源量等集群状态，预估作业无法调度时，阻止作业创建pod；Enqueue action是调度器配置必不可少的action。
6. proportion plugin
控制队列的可用资源，包括硬性指标：guarantee和capability以及weight
7. allocate action
过滤掉不符合要求的节点，对符合要求的节点进行打分排序，选出得分最高的节点，最终检查作业是否满足gang约束条件；
8. preempt action
检查作业是否处于有效状态；检查作业是否可以被抢占；Preempt用于同一个Queue中job之间的抢占，或同一Job下Task之间的抢占。
9. backfill action
处理待调度Pod列表中没有指明资源申请量的Pod调度，在对单个Pod执行调度动作的时候，遍历所有的节点，只要节点满足了Pod的调度请求，就将Pod调度到这个节点上。
10. reclaim action
选择要回收的任务；
11. binpack plugin
尽量把已有的节点填满（尽量不往空白节点分配）
12. predicate plugin
过滤掉不符合pod要求的节点；在AI场景下，它可以快速筛选出来需要GPU的进行集中调度；plugin cache可以对相同pod模板的task进行结果缓存，加速调度进程；
predicate.GPUSharingEnable:true
predicate.CacheEnable:true
predicate.proportionalEnable:true
predicate.resources:nvidia.com/gpu
predicate.resources:nvidia.com/gpu.cpu:8
predicate.resources:nvidia.com/gpu.memory:8
13. nodeorder plugin
通过用户配置的打分参数从各个维度为节点打分，从而找到最适合当前作业的节点；
14. priority plugin
优先级分配插件，priorityClassName
15. gang plugin
All or nothing,观察Job下的Pod已调度数量是否满足了最小运行数量，当Job的最小运行数量得到满足时，为Job下的所有Pod执行调度动作，否则，不执行。
16. drf plugin
Dominant Resource Fairness，volcano-scheduler观察每个Job请求的主导资源，并将其作为对集群资源使用的一种度量，根据Job的主导资源，计算Job的share值，在调度的过程中，具有较低share值的Job将具有更高的调度优先级。
17. Task-topology plugin
根据Job内task之间亲和性和反亲和性配置计算task优先级和Node优先级的算法。通过在Job内配置task之间的亲和性和反亲和性策略，并使用task-topology算法，可优先将具有亲和性配置的task调度到同一个节点上，将具有反亲和性配置的Pod调度到不同的节点上。
以TensorFlow计算为例，配置“ps”和“worker”之间的亲和性。可使“ps”和“worker”尽量调度到同一台节点上，从而提升“ps”和“worker”之间进行网络和数据交互的效率，进而提升计算效率。
ps”与“ps”之间的反亲和性。
18. sla plugin
Service Level agreement。用户向volcano提交job的时候，可能会给job增加特殊的约束，例如最长等待时间(JobWaitingTime)。这些约束条件可以视为用户与volcano之间的服务协议。SLA plugin可以为单个作业/整个集群接收或者发送SLA参数。
19. Numa-aware plugin
支持cpu资源的拓扑调度。
支持pod级别的拓扑协议。
20. action真正的调度过程--reclaim-->allocate-->backfill-->preempt
21. job插件
svc: 实现同一vc job内的各pod间通信；在所有pod中添加hostname和subdomain字段；
添加VC_<TASK_NAME>_NUM和VC_<TASK_NAME>_HOSTS环境变量；为job创建headless service,configmap和networkpolicy；
配置示例svc:["--publish-not-ready-address=false", "--disable-network-policy=false"]；
ssh: 实现同一vc job内的各container之间的免密ssh登录；
配置示例：ssh:["--ssh-key-file-path=/home/user/.ssh", "--ssh-private-key=xxx\n", "--ssh-public-key=xxx""]
env: 为vc job中的全部pod添加TASK_INDEX环境变量；
配置示例： env: []
pytorch: 为job中的所有container开放指定端口；开启svc插件；添加MASTER_ADDR，MASTER_PORT,MASTER_SIZE,RANK等pytorch相关环境变量；
配置示例：pytorch: ["--master=master", "--worker=worker", "--port=23456"]
tensorflow: 为job中的所有container开放指定端口；开启svc插件；
配置示例：pytorch: ["--ps=master", "--worker=worker", "--port=2222"]





# 调度GPU

## k8s中使用GPU
GPUs 只能设置在 limits 部分，这意味着：
不可以仅指定 requests 而不指定 limits
可以同时指定 limits 和 requests，不过这两个值必须相等
可以指定 GPU 的 limits 而不指定其 requests，K8S 将使用限制值作为默认的请求值；
每个容器可以请求一个或者多个GPU
limits和requests指定如下字段，系统会分配指定数量的显卡：
nvidia.com/gpu
amd.com/gpu


节点需要使用 NVIDIA 的 GPU 资源的话，需要先安装 k8s-device-plugin 这个插件，并且需要事先满足下面的条件：
Kubernetes 的节点必须预先安装了 NVIDIA 驱动
Kubernetes 的节点必须预先安装 nvidia-docker2.0
Docker 的默认运行时必须设置为 nvidia-container-runtime，而不是 runc
NVIDIA 驱动版本大于或者等于 384.81 版本


nvidia-smi -L         # 确认pod使用了GPU卡



