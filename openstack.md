
# nova
nova get-vnc-console <server> novnc      # 获取vnc地址
nova rescue <server> --password <password>
nova unrescue <server>
rescue用指定的image作为系统启动盘引导instance，而把instance原先的系统作为第二个磁盘挂载到系统上，
相当于把故障电脑磁盘拿出，插到另一台正在运行的电脑上，进而再进行一些拯救工作。
openstack server restore <server>    # 将软删除的虚机进行恢复

重装系统rebuild--->三个过程power_off/rebuild/power_on





## openstack CLI
openstack server create --host SCDA0052.uat.local --network ae468023-489f-49e1-856e-4aa3d5692aa7 --image mysql8028-uat-v0702 --flavor fc98f1ef-7c74-47db-a431-1cea0f2cc78a dx_vm1 --os-compute-api-version 2.74

openstack baremetal node list   # 查看裸金属的节点
openstack baremetal node console show <node>   # 查看裸金属控制台信息
nova interface-list dx_vm1     # 查看虚机的接口，包含port_id，ip


## 裸金属Ironic
所谓裸机，就是指没有配置操作系统的计算机。从裸机到应用还需要进行以下操作：
（1）硬盘RAID、分区和格式化；
（2）安装操作系统、驱动程序；
（3）安装应用程序。
Ironic实现的功能，就是可以很方便的对指定的一台或多台裸机，执行以上一系列的操作。例如部署大数据群集需要同时部署多台物理机，就可以使用Ironic来实现。
Ironic可以实现硬件基础设施资源的快速交付。

在OpenStack体系结构中，Ironic还是通过Nova来调用的，模拟Nova的一个虚拟化驱动（其它的虚拟化驱动还有KVM、VMware、Xen等），实现基于Ironic的虚拟化驱动。
预先配置好PXE、IPMI等服务，完成Ironic的相关配置之后，用户就可以使用Nova API来实现一个物理机实例的创建。Nova用于管理虚拟机的生命周期；
Ironic则是用于管理物理机的生命周期，它给Nova提供管理物理机的API接口。

裸金属管理服务的基本原理是：PXE服务器提供DHCP服务和TFTP服务，指示多台裸金属设备由PXE网卡启动并分配动态IP，
裸金属设备从PXE服务器中下载相关软件包，用于裸金属主机的系统安装。

裸金属虚机创建
nova   boot  ironic-test  --flavor 004008   --security-groups default --block-device source=image,device=sda,dest=volume,id=7a09baa5-897e-4217-8a3a-47e0ba978b9e,size=40,bootindex=0,bus=virtio,type=disk --nic net-id=9d893658-59c6-4ec6-a133-6dc6678705c1

socat网络工具


ompute节点日志： Instance xxx has allocations against this compute host but is not found in the database.
su -s /bin/sh -c "nova-manage cell_v2 discover_hosts --verbose" nova


### ironic部署三阶段
1. 入云阶段（Inspection）：裸金属服务器接入网络后，通过PXE启动加载启动镜像，将自身的主机信息汇报给裸金属管理节点，使其进入可调度状态。
Inspector阶段完成了OpenStack对接入的物理服务器硬件信息的收集，控制节点可以根据收集到的配置信息创建对应的“实例类型”，
便于根据用户申请资源要求调度指定物理服务器创建裸金属实例。
ironic-dnsmasq作为dhcp服务，第一阶段裸金属服务器安装操作系统时需要从其拿到IP；
在控制节点配置dhcp服务的网段IP；(192.168.130.0/24)
第一阶段也需要根据image安装系统，并且是通过ramdisk
baremetal node create
baremetal node manage
baremetal node inspect
2. 部署阶段（Provision）：租户向云平台申请裸金属资源，云平台启动被选中服务器，通过PXE启动加载启动镜像，之后部署系统镜像，完成裸金属服务器操作系统安装。
openstack创建vxlan内网192.168.140.0/24，创建路由关联子网192.168.140.0/24并且配置静态路（10.50.31.0/24--控制节点网段,192.168.140.254--交换机接口）；
之后通过sdn打通。
baremetal node provide
3. 运行阶段（Running）：服务器重启加载操作系统，通过DHCP获取IP地址，完成裸金属实例部署。
openstack server create


从PXE或者UEFI启动裸机需要在ironic-conductor节点配置TFTP服务器;

### 裸金属节点clean
openstack baremetal node clean <node>   --clean-steps '[{"interface": "deploy", "step": "erase_devices_metadata"}]'
1. provision state先从active变成deleting；
2. 解绑裸金属虚机的port；
3. provision state先从deleting变成cleaning；
4. 最终clean完毕的状态是available；




## 虚机filter
AggregateInstanceExtraSpecsFilter----在指定的HostAggregate中选定一个主机分配虚拟机
需要在HostAggregate打上flavor的对应标签，就是HostAggreagate和flavor要设置相同的metadata(key，value)，比如两个都增加ssd=true的key-value metadata，则表明只在存储为固态硬盘的主机分配虚拟机规格为flavor的虚拟机；
1. 创建主机聚合nova aggregate-create agg nova；
2. 把主机加入到主机聚合中nova aggregate-add-host agg compute1；
3. 定义元数据nova aggregate-set-metadata agg ssd=true；
4. 主机flavor加入元数据nova flavor-key m1.nano set ssd=true；


## 虚机创建
1. 虚机创建报错Port 0d1f4c34-2dc8-43e6-a769-9569cb68902f not usable for instance，由于虚机和port不属于同一个租户；


## 虚机重建
1. 原系统盘volume先detach（带attachment in-use）;
2. 数据盘detach;
3. 创建新系统盘volume
4. 原系统盘volume删除;
5. 新系统盘volume attach;
6. 原数据盘attach;
7. 虚机重建成功。




## GPU设备
1. 计算节点执行：lspci -nn | grep -i nvidia，获取到显卡的vendorid和product_id，并将其相应的写到nova的配置文件里面；
2. 在gpu计算节点的nova-compute和控制节点的nova-api，nova-schedule,nova-conductor配置文件下添加：
[pci]
passthrough_whitelist = { "vendor_id": "10de", "product_id": "2204" }
alias = { "vendor_id":"10de", "product_id":"2204","device_type":"type-PCI","name":"g3090" }
3. 创建flavor 
openstack flavor create 4c4g16g.3090 --ram 4096 --disk 16 --vcpus 4 --public --property "pci_passthrough:alias=g3090:1"
其中g3090与配置文件中的alias对应，后面跟的为该flavor对应的gpu数量



## nova attach volume
1. _prep_block_device-->attach_block_devices;
2. 调用nova virt block_device的attach；
3. 通过cinderclient调用cinder volume api的attach；
4. 然后api通过amqp发到rabbitmq；
5. cinder volume manager调用driver进行attach。



## vnc console原理
1. 先通过/servers/{server_id}/remote-consoles创建一个console；
2. 然后拿着这个console通过浏览器访问；
3. 实际通信过程通过websocket---ws://10.50.1.251:6080/?token=<token>。


## 给实例注入一个密钥对并通过密钥对来访问实例
1. 创建秘钥对
$ openstack keypair create test > test.pem
$ chmod 600 test.pem
2. 启动实例
$ openstack server create --image cirros-0.3.5-x86_64 --flavor m1.small \
  --key-name test MyFirstServer
3. 使用ssh连接到实例
ip netns exec qdhcp-98f09f1e-64c4-4301-a897-5067ee6d544f ssh -i test.pem cirros@10.0.0.4


## nova_ssh免密ssh互访
在hypervisor节点之间进行resize或者迁移实例时，确保每个计算节点配置SSH key认证，以能使计算服务使用ssh进行disk移动；
主要流程就是生成公钥和私钥，将私钥拷贝到/var/lib/nova/.ssh/id_rsa，公钥拷贝到远端节点，切换nova账户即可不需密码登录到远端节点
https://docs.openstack.org/nova/pike/admin/ssh-configuration.html


## 冷迁移
1. 基于ssh；
2. 迁移过程：首先把实例先关机，然后重命名实例文件夹为instanceid_resize, ssh把实例文件给拷贝过去，数据库更改，
    在新计算节点上启动实例，此时可以revert实例，即复原迁移。确认后删除原实例文件，删除老节点qemu下xml文件并在新节点上新建；
3. 

## 热迁移
1. --block-migrate=False不需要共享存储，如NFS存储后端，也可以设置共享存储置成True；
2. 基于qemu+tcp；
3. 计算节点16509端口用于libvirtd的tcp连接监听；




# neutron


tracepath ip ---查看链路
ip route解释
default via 172.16.0.1 dev ens192 proto static metric 100 # 表示去任何地方，都发送给网卡ens192，并经过网关172.16.0.1发出；
172.16.0.0/16 dev ens192 proto kernel scope link src 172.16.1.11 metric 100 # 表示发往172.16.0.0/26网段的包都由网卡ens192发出，src表示ens192的网卡ip是172.16.1.11，metric表示路由距离，到达指定网络所需的中转数；
默认路由（Default route），是对IP数据包中的目的地址找不到存在的其他路由时，路由器所选择的路由。




## ml2
### 层次化端口绑定的流程:
1. 用户创建了一个虚拟机，并且将虚拟机创建在VxLAN A网络中。
2. Neutron需要创建一个VxLAN A的网络接口，请求被发送到了ML2。
3. Neutron ML2先调用到物理交换机对应的Mechanism driver进行端口绑定（port binding），将VxLAN A与网络接口进行绑定。
4. 因为底层还有VLAN，物理交换机的Mechanism Driver会再申请一个VLAN B，并告知Neutron ML2，当前网络接口还需要绑定到对应的VLAN上。
5. 之后物理交换机的Mechanism Driver会通过相应的API，告知物理交换机 VLAN B和VxLAN A的对应关系，这样物理交换机就有了VLAN To VxLAN ID MAP。
6. ML2因为知道了网络接口还需要绑定到对应的VLAN上，再调用OpenVSwitch的Mechanism Driver，将VLAN B与网络接口进行绑定。
7. 之后，OVS的Mechanism Driver会通过相应的API，告知位于计算节点的OpenVSwitch，对于这个网络接口的网络数据，打上VLAN B的Tag。

网络数据送到OpenVSwitch，OVS会打上VLAN B的Tag，带VLAN B Tag的网络数据送到物理交换机。 
物理交换机根据自己的VLAN To VxLAN ID MAP，将VLAN Tag去掉，再封装成相应的VxLAN数据。经过这样的处理，VxLAN的封装解封装被offload到了物理交换机。
ml2_port_bindings    # 绑定属性集
ml2_port_binding_levels   # 绑定层级
networksegments    # 每层的segment


### vpc互通
1. 同租户两个vpc通过vpc connection打通；
2. 

### ovs
1. 创建子网时会在br-int下建立一个tap口(tap-<dhcp port id前11位>)，会在dhcp命名空间中建立这个tap口；
2. 创建虚机时，会在br-int下建立一个qvoxxx，linux-bridge下会建立一个qbrxxx网桥，qbr网桥下有个qvbxxx接口和tapxxx，
    也就整个连接是br-int-->qvo-->qvb-->linux bridge qbr-->tap。         --xxx为port id的前11位；
3. 批量删除br-int上的port；for port in $(ovs-vsctl list-ports br-int); do ovs-vsctl del-port br-int $port; done
4. 查看网桥br-int的的虚机port；ovs-vsctl list-ports br-int；
5. 通过ovs-vsctl list qos,查看qos配置表,其中有queue配置;ovs-vsctl list queue，查看queue表，可以看到限速的速率；
6. tc qdisc show dev tap5a8435ab-22；


### ovs+vxlan网络流程
1. 同一host上同一子网内的虚机通信；只是经过br-int，不经过br-tun；
2. 不同host上同一子网内的虚机通信；
    vm1发出的packet经过br-int打上vlan tag,到达br-tun将vlan tag转成隧道id,从隧道发出，到达另外的计算节点；
    在另一个计算节点经过相反的顺序到达vm2；
3. 虚机访问外网
    vm1发出的packet经过br-int打上vlan tag,到达br-tun将vlan tag转成隧道id,从隧道发出，到达网络节点；
    经过网络节点的网卡达到与物理网卡相连的br-tun,将Tunnel ID转换成vlan id；
    达到br-int，再到达router，router的NAT表将fixed IP地址转换成floating IP地址，再被router到br-ex；
    从br-ex相连的物理网卡上出去外网。
4. 虚机发送DHCP请求
    1. 虚机的packet -> br-int -> br-tun -> vxlan Tunnel -> eth2 ------>eth2->br-tun->br-int->qDHCP
    2. qDHCP返回其fixed IP地址，原路返回；


## l2 population原理
L2 population 的原理如下：
首先，创建一个 Overlay 网络（通常是 VXLAN），用于在不同计算节点之间传输虚拟机的二层数据帧。
当虚拟机发送一个二层数据帧到网络中，该数据帧会通过虚拟交换机（例如 Open vSwitch）被送往连接到虚拟机所在计算节点的虚拟交换机。
如果目的地是同一计算节点上的另一个虚拟机，那么数据帧将直接送达目标虚拟机。
如果目的地是其他计算节点上的虚拟机，源计算节点会复制该数据帧到 Overlay 网络，并将其发送到目标计算节点。
目标计算节点上的虚拟交换机接收到复制的数据帧后，将其转发给目标虚拟机。
这样，当虚拟机之间需要进行二层通信时，L2 population 可以在不同计算节点之间复制和处理数据帧，使得虚拟机可以透明地进行二层通信，就像它们在同一个物理网络上一样。


## iptables
iptables -t filter -I INPUT -s 192.168.1.2 -p tcp --dport 22 -j DROP ---拒绝192.168.1.2访问目标22端口
iptables -nL |grep 'policy' ----查看filter表各个链的默认策略
ip netns exec qrouter-1658595a-ee11-4a53-bd6f-34a49ce86b61 iptables -t nat -S   # 命名空间内查看nat规则
ip netns exec qrouter-1658595a-ee11-4a53-bd6f-34a49ce86b61 iptables-save  # 命名空间内将iptables规则打印
iptables -t nat -nvL
iptables-save
neutron l3-agent-list-hosting-router 1658595a-ee11-4a53-bd6f-34a49ce86b61    # 查看router关联的l3-agent所在的host

## router
neutron l3-agent-list-hosting-router <router_id>     # 查看router所在的namespace节点
namespace里中qg和qr没有ip时，通过neutron router-update --admin-state-up False，再者neutron router-update --admin-state-up True恢复；


### 在命名空间内抓包tcpdump -i qr-xxx/qg-xxx -n icmp
[root@SCDA0052 scadmin]# ip netns exec qrouter-3feea37e-5a67-4396-90af-56ab85698e4d tcpdump -i qg-564ac24f-e7 -n icmp
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on qg-564ac24f-e7, link-type EN10MB (Ethernet), capture size 262144 bytes
16:42:25.409226 IP 10.250.51.123 > 10.250.48.1: ICMP echo request, id 18494, seq 1, length 64
16:42:25.409879 IP 10.250.48.1 > 10.250.51.123: ICMP echo reply, id 18494, seq 1, length 64


### 缺少默认路由，导致外部ping不通外部网关，而命名空间是可以ping通的
切换一下router的主备，会让配置在备的router上刷过去。
ip link set ha-5dfc67f4-07 down
常用的办法是把主router的ha口down掉，等一段时间规则会切换到备节点的router上去，中间出口网络会有几秒的中断；
[root@SCDA0052 scadmin]# ip netns exec qrouter-3feea37e-5a67-4396-90af-56ab85698e4d ip r
default via 10.250.48.1 dev qg-564ac24f-e7 proto static 
10.250.33.0/24 dev qg-564ac24f-e7 proto static scope link 
10.250.48.0/22 dev qg-564ac24f-e7 proto kernel scope link src 10.250.51.123 
169.254.0.0/24 dev ha-5dfc67f4-07 proto kernel scope link src 169.254.0.90 
169.254.192.0/18 dev ha-5dfc67f4-07 proto kernel scope link src 169.254.192.133 
192.168.44.0/24 dev qr-e422ee4e-61 proto kernel scope link src 192.168.44.1
192.168.45.0/24 dev qr-bb32d29f-17 proto kernel scope link src 192.168.45.1

也可以加条静态路由解决openstack router set dx_test1 --route destination=0.0.0.0/0,gateway=10.250.48.1
或者openstack  router set <router> --disable + openstack  router set <router> --enable



## floatingip
1. 创建floatingip可以指定subnet，是指定的network下的subnet，内部逻辑是将传的subnet_id放到port中，去创建port；如果不传subnet_id返回时不带subnet_id；
2. 限速测试：服务端iperf3 -s -i 10 -p 5201--设置监控时间10s，端口为5201，防火墙端口要放行；客户端iperf3 -c x.x.x.x -p 5201 -t 5 -P 10 -R---指定-c测速服务器IPx.x.x.x，-p指定端口为5201，-t测速时间5s，-P指定发送连接数10，-R表示下载测速  
3. 限速可以限速floatingip出外网和端口转发；


### EIP
弹性公网IP（Elastic IP，简称EIP），提供独立的公网IP资源，包括公网IP地址与公网出口带宽服务。
可以与弹性云服务器、裸金属服务器、虚拟IP、弹性负载均衡、NAT网关等资源灵活地绑定及解绑。
一个弹性公网IP只能绑定一个云资源使用，且弹性公网IP和云资源必须在同一个区域，不支持跨区域使用弹性公网IP。
Openstack EIP网络流量监控,要求fixedIP与EIP分别统计---同一块网卡上，绑定内网与EIP的情况
使用Metering agent来监控l3流量
1. 配置
service_plugins追加metering；
编辑metering_agent.ini文件；
在每个l3-agent节点启动metering agent。neutron meter-label-rule-create dx_meter1 10.50.35.205/32 --direction ingress --tenant-id e4a97e45239340b090fcebde8edcbfe5
2. 使用
指定租户创建meter label  
neutron meter-label-create --tenant-id  e4a97e45239340b090fcebde8edcbfe5 dx_meter1
创建规则，10.50.35.205为floatingip，44.44.44.184为虚机ip
neutron meter-label-rule-create dx_meter1 44.44.44.184/32 --direction ingress --tenant-id e4a97e45239340b090fcebde8edcbfe5
neutron meter-label-rule-create dx_meter1 44.44.44.184/32 --direction egress --tenant-id e4a97e45239340b090fcebde8edcbfe5
iptables规则
[root@con01 dongxiang]# ip netns exec qrouter-0e9775f3-755a-4b10-9fa0-b65ca1601f89 iptables -t filter -nvxL neutron-meter-r-ecca93c6-8d1
Chain neutron-meter-r-ecca93c6-8d1 (1 references)
    pkts      bytes target     prot opt in     out     source               destination
      89    16839 neutron-meter-l-ecca93c6-8d1  all  --  qg-3a26535b-91 *       0.0.0.0/0            44.44.44.184
    1094    80015 neutron-meter-l-ecca93c6-8d1  all  --  *      qg-3a26535b-91  44.44.44.184         0.0.0.0/0



## fwaas v1
接入SDN后，fwaas v1即huawei的huawei_ac_fwaas service_plugin；
### 创建firewall
1. 首先判断router_ids字段中的是否有任何的router关联了其它的firewall，如何关联了即报错in-use error;
2. 调用firewall_db将数据进行存库,firewall状态为PENDING_CREATE；
3. 如果传参带router_ids，fw_id和router_id存入库firewall_router_associations中；
4. 下发到AC控制器上；


### 创建firewall policy
1. 首先写库；
2. 




## fwaas v2

### create_fw
1. firewall rule/firewall group/firewall policy均不支持批量创建； 
2. firewall group可以分别有一个出向和入向policy；
3. 创建firewall policy中可以添加多个rule(无序)；通过insert_rule接口(指定顺序)只能单次操作；
4. 调用此接口fwaas/firewall_groups?tenant_id=e4a97e45239340b090fcebde8edcbfe5时创建default fw或者创建firewall group，就是确保default firewall存在；
5. default firewall入向规则全拒绝，出向全允许；
6. 如果firewall group已经管理了内网网关端口，则根据此内网创建虚机时，虚机的port会加到firewall group管理的端口中，而内网网关端口被移除；
7. port已经被一个firewall管理，不允许另外的防火墙管理；
8. 不能创建fw name为default的fw，每个租户下只能有一个default fw；
9. 创建firewall group时，如果没有port，则firewall group状态是INACTIVE；如果有port，并且router_distributed为true时，状态为CREATED，否则为PENDING_CREATED；
10. firewall配额只能在fwaas_driver.ini中修改，默认firewall和firewall policy配额10,firewall rule配额100;
[quotas]
quota_firewall=10
quota_firewall_policy=10
quota_firewall_rule=100
11. 默认防火墙规则的配置可以在fwaas_driver.ini中修改；
[default_fwg_rules]
ingress_action=allow

### update_fw
1. 如果关联的port是内网网关端口，则最后firewall group会变成ACTIVE；
2. 如果更新时如果已有policy并且带port更新或者policy和port一起更新，则最后firewall group会变成ACTIVE；
3. 如果状态是PENDING，则不允许更新；
4. 支持更新的port为虚机port，router interface port，
5. 如果更新的port已经关联到别的firewall group了，则不支持更新；
6. update_precommit中如果port有变化或者policy有变化+更新的port不为空，则状态变为PENDING_UPDATE；
7. FWaaSL3AgentExtension中的update_firewall_group中，
8. 如果更新fw带policy且没有port,则fw先进入PENDING_UPDATE，最后会变成INACTIVE；
7. 关联port后qrouter命名空间内的增加了如下规则：
neutron-l3-agent-fwaas-defau - [0:0]
neutron-l3-agent-iv4a24248cf - [0:0]
neutron-l3-agent-ov4a24248cf - [0:0]
-A neutron-l3-agent-FORWARD -o qr-048c1996-4e -j neutron-l3-agent-iv4a24248cf
-A neutron-l3-agent-FORWARD -i qr-048c1996-4e -j neutron-l3-agent-ov4a24248cf
-A neutron-l3-agent-FORWARD -o qr-048c1996-4e -j neutron-l3-agent-fwaas-defau
-A neutron-l3-agent-FORWARD -i qr-048c1996-4e -j neutron-l3-agent-fwaas-defau
-A neutron-l3-agent-fwaas-defau -j neutron-l3-agent-dropped
-A neutron-l3-agent-iv4a24248cf -m state --state INVALID -j neutron-l3-agent-dropped
-A neutron-l3-agent-iv4a24248cf -m state --state RELATED,ESTABLISHED -j ACCEPT
-A neutron-l3-agent-iv4a24248cf -j neutron-l3-agent-dropped
-A neutron-l3-agent-ov4a24248cf -m state --state INVALID -j neutron-l3-agent-dropped
-A neutron-l3-agent-ov4a24248cf -m state --state RELATED,ESTABLISHED -j ACCEPT
-A neutron-l3-agent-ov4a24248cf -j neutron-l3-agent-accepted

-A neutron-l3-agent-FORWARD -o qr-048c1996-4e -j neutron-l3-agent-iv4a24248cf意思是从router namespace任何一个qr-048c1996-4e发出的流量都会应用chain neutron-l3-agent-iv4a24248cf；
-A neutron-l3-agent-iv4a24248cf -m state --state INVALID -j neutron-l3-agent-dropped意思是如果数据包的状态是INVALID，则DROP掉；
-A neutron-l3-agent-iv4a24248cf -m state --state RELATED,ESTABLISHED -j ACCEPT意思是如果数据包状态是RELATED,ESTABLISHED，则接收；



### delete_fw
1. 如果firewall_group是ACTIVE，则不允许删除；
2. 在管理状态置成down时，再remove rule，此时fw状态变成DOWN，fw就可以删除了；


### insert rule
1. insert rule/remove rule最后会触发FWaaSL3AgentExtension中的update_firewall_group，（如果本租户内有router port）从而导致fw进入ACTIVE状态;


### update policy
1. update policy会触发FWaaSL3AgentExtension中的update_firewall_group,（如果本租户内有router port）从而导致fw进入ACTIVE状态;

### update rule
1. update rule会触发FWaaSL3AgentExtension中的update_firewall_group,（如果本租户内有router port）从而导致fw进入ACTIVE状态;




## sg
1. 安全组是针对每个port做网络访问控制，防火墙是针对一个VPC网络，通常是在路由做策略；
因此security group在计算节点的tap设备上做，而firewall在网络节点的router上做。
2. 安全组定义的是允许通过的规则集合，规则的动作是ACCEPT；
3. 创建一条安全组，自动添加两条默认规则，egress ipv6和egress ipv4（禁止所有的流量访问，允许所有的流量出去）。
4. 只有当安全组被关联到port时才会真正创建对应的iptables规则。
5. 安全组规则支持批量创建；请求体 {"security_group_rules": [{}]}


Openstack中的安全组的实现有以下几种：
ovs + iptables + connection track
ovs + openflow + connection track
linuxbridge + iptables + connection track

此命令iptables -n --line-numbers -L neutron-linuxbri-ifa4e375e-b



### ovs + openflow实现安全组
查看port的接口id  ovs-ofctl show br-int |grep 789dc709- -A 3
查看目的端口22并且接口id为502的安全组规则      ovs-ofctl dump-flows br-int |grep output:502|grep tp_dst=22

表示源地址为12.12.12.0/24报文送到接口502
[root@sdn-1 ~]# ovs-ofctl dump-flows br-int |grep 12.12.12.0
 cookie=0x151f0564881e5c71, duration=12.497s, table=82, n_packets=0, n_bytes=0, idle_age=14, priority=77,ct_state=+est-rel-rpl,tcp,reg5=0x1f6,nw_src=12.12.12.0/24,tp_dst=22 actions=output:502
 cookie=0x151f0564881e5c71, duration=12.497s, table=82, n_packets=0, n_bytes=0, idle_age=14, priority=77,ct_state=+new-est,tcp,reg5=0x1f6,nw_src=12.12.12.0/24,tp_dst=22 actions=ct(commit,zone=NXM_NX_REG6[0..15]),output:502,resubmit(,92)

表示目的地址33.33.33.0/24的报文，从接口502正常转发
[root@sdn-1 ~]# ovs-ofctl dump-flows br-int |grep 33.33.33.0
 cookie=0x151f0564881e5c71, duration=18.246s, table=72, n_packets=0, n_bytes=0, idle_age=19, priority=77,ct_state=+est-rel-rpl,tcp,reg5=0x1f6,nw_dst=33.33.33.0/24,tp_dst=22 actions=resubmit(,73)
 cookie=0x151f0564881e5c71, duration=18.246s, table=72, n_packets=0, n_bytes=0, idle_age=19, priority=77,ct_state=+new-est,tcp,reg5=0x1f6,nw_dst=33.33.33.0/24,tp_dst=22 actions=resubmit(,73)


## dhcp
虚机内部指定dhclient可向dhcp agent请求IP地址；

### Nova 虚机获取固定IP （Fixed IP）
（1）在创建虚机过程中，Neutron 随机生成 MAC 和 从配置数据中分配一个固定IP 地址，并保存到 Dnsmasq 的 hosts 文件中，让 Dnsmasq 做好准备；
（2）虚机在启动时向 Dnsmasq 获取 IP 地址。


## rbac policy
OpenStack RBAC（Role-Based Access Control）是一种基于角色的访问控制机制，用于管理OpenStack云平台中的资源和服务。
它允许管理员为用户和组分配不同的角色，以控制他们对云平台中不同资源和服务的访问权限。
OpenStack RBAC的核心是角色和权限。角色是一组权限的集合，而权限则是对特定资源或服务的访问控制。
OpenStack中的角色包括系统管理员、云管理员、项目管理员、用户等。每个角色都有不同的权限，例如创建、删除、修改、查看等。

openstack network rbac create --target-project 32016615de5d43bb88de99e7f2e26a1e --action access_as_shared \
--type security_group 5ba835b7-22b0-4be6-bdbe-e0722d1b5f24
target-project指的是要共享的项目id；action--access_as_shared表示可共享；type--资源类型，支持network,qos_policy,security_group,object_id--资源id；

1. 当创建network，指定字段shared为True，表示此network可以被其它每个project共享，等价于创建rbac policy指定参数--target-all-projects；
2. 当创建network，指定字段external为True，表示此network可以作为外部网络使用，等价于创建network,然后创建rbac policy指定参数action为access_as_external；


## lbaas
1. amphora是实现lb功能的云主机，负载均衡的载体，包含amphora-agent服务和实现底层lb功能的haproxy和keepalived；


### l7 policy



## quota
1. 获得租户的份额；首先初始化份额为默认值，然后根据Quota库中条目更新资源的份额；
2. 删除份额，即重装份额，就是删除Quota表中的条目，份额都变成默认值；



## octavia
### 概念
1. LB management network，打通Openstack management和Amphora;
2. VIP network，作为vip pool的网络，东侧接入Amphora有keepalived实现的VRRP协议支持具有高可用特性的虚拟IP地址；
3. Tenant network,业务主机所处的网络，东侧接入amphora由haproxy为业务云主机提供负载均衡数据流量分发服务。普通租户可见。
4. VIP network和Tenant network可以是同一网络，生产环境建议分开；
5. Amphora云主机，作为负载均衡器软件Haproxy和高可用支持Keepalived的运行载体，同时也运行着amphora-agent service对外提供REST API。

### 资源操作
1. 创建loadbalancer；
    会创建



# cinder
openstack volume create  dx_v1 --size 1
cinder service-list
cinder backup-create --container volumes_backup --display-name backuptoswift dx_v1
cinder backup-restore --volume-id cb0fe233-f9b6-4303-8a61-c31c863ef7ce dx_v1
cinder backup-delete
openstack volume backup list
openstack volume type list


volume-type
对于每个ceph后端，创建一个volume type，并将volume type关联配置文件中的volume_backend_name：
cinder type-create ceph
cinder type-key ceph set volume_backend_name=ceph
cinder type-create ceph-sas
cinder type-key ceph-sas set volume_backend_name=ceph-sas
然后执行cinder type-list可以看到配置的volume type

Cinder不仅支持本地volume的管理，还能把本地volume备份到远端存储系统中，比如备份到另一个Ceph集群或者Swift对象存储系统中。
创建volume分为4种类型：
1. raw: 创建空白卷。
2. create from snapshot: 基于快照创建volume。
3. create from volume: 相当于复制一个已存在的volume。
4. create from image: 基于Glance image创建一个volume。


## 创建volume
1. scheduler.create_volume-->error；
2. 根据快照创建快照，如果更换硬盘类型有如下条件：硬盘类型的加密不变；硬盘类型的volume_backend_name相同，如果volume_backend_name为空还是不能创建；


## 删除volume
volume.delete.end--->deleted,
1. 如果volume有snapshot，则不允许删除；
2. 删除虚机时，如果创建虚机带的BDM中delete_on_termination=true，则会删除系统盘；如果delete_on_termination不带或是false,则不删除系统盘；
3. 删除虚机nova传给cinder delete请求(cinder/volume/api.py)带cascade=true；



## 挂载volume
volume.attach.end--->in-use
1. 问题--比如说虚机挂载两个数据盘 vol1-->vdb，vol2-->vdc，把vol1卸载掉，重启云服务器，虚机fdisk -l显示的是vdb，和openstack目前存在的挂载情况vdc不一致；
2. 虚机挂载数据盘时，盘符按序vdb，vdc...进行自动分配；




## 卸载volume
volume.detach.end--->available
1. 卸载有条件判断，volume的status必须为in-use和attach_status必须为attached；


## os-force_detach
{
    "os-force_detach": {
        "attachment_id": "d8777f54-84cf-4809-a679-468ffed56cf1",
        "connector": {
            "initiator": "iqn.2012-07.org.fake:01"
        }
    }
}
使用场景
1. nova volume-attach时，出现cinder DB attaching，存储后端available,nova没有BDM；
2. nova volume-attach时，出现cinder DB attaching，存储后端attached,nova有BDM；
3. nova volume-detach时，出现cinder DB detaching，存储后端available,nova没有BDM；
4. nova volume-detach时，出现cinder DB detaching，存储后端attached,nova有BDM；



## 创建snapshot
snapshot.create.end--->available
1. 创建快照，volume状态必须是available，error_deleting不行；


## 删除snapshot
snapshot.delete.end--->deleted
1. snapshot状态必须是available和error,且不属于group的一部分才能删除；
2. 快照如果有根据其创建的硬盘，则此快照不允许删除；


## volume类型更改
volume.retype--->available/in-use

1. Retype needs volume to be in available or in-use state, not be part of an active migration or a consistency group, 
requested type has to be different that the one from the volume, and for in-use volumes front-end qos specs cannot change.

## 配置文件支持多个后端
cinder.conf
enabled_backends = lvmdriver-b21,lvmdriver-b22
storage_availability_zone=az1
[lvmdriver-b21]
iscsi_ip_address = 10.0.1.29
volume_group = cinder-volumes1
volume_driver = cinder.volume.drivers.lvm.LVMISCSIDriver volume_backend_name = lvmbackend  
[lvmdriver-b22]
volume_group = cinder-volumes2
volume_driver = cinder.volume.drivers.lvm.LVMISCSIDriver volume_backend_name = lvmbackend


Cinder为每一个backend运行一个cinder-volume服务

cinder backup支持将元数据序列化导出（export record)，这样即使数据库中的数据丢失了，也能从导出的元数据中快速恢复。





## cinder备份实现rdb和s3共存
S3 Simple Storage Service 简单存储服务
Amazon
通过 S3 存储和检索的资产被称为对象。对象存储在存储段（bucket）中。您可以用硬盘进行类比：对象就像是文件，存储段就像是文件夹（或目录）。与硬盘一样，对象和存储段也可以通过统一资源标识符（Uniform Resource Identifier，URI）查找。
S3 还提供了指定存储段和对象的所有者和权限的能力，就像对待硬件的文件和文件夹一样。

backup_driver = cinder.backup.drivers.s3.S3BackupDriver
backup_s3_block_size = 32768
backup_s3_enable_progress_timer = True
backup_s3_endpoint_url = http://10.50.114.112:6780
backup_s3_max_pool_connections = 10
backup_s3_md5_validation = True
backup_s3_object_size = 52428800
backup_s3_retry_max_attempts = 4
backup_s3_retry_mode = legacy
backup_s3_store_bucket = volumebackups
backup_s3_store_access_key = 8NAZFIL6MYEY5O6IRXZA
backup_s3_store_secret_key = wxiAE1ldYuSotbfxe8uk5i9tjnryIuO91Tvb4CoG
backup_s3_timeout = 60
backup_s3_verify_ssl = False



Backup 是将 volume 备份到别的地方（备份设备），将来可以通过 restore 操作恢复。

Backup VS Snapshot
Backup 与 snapshot 都可以保存 volume 的当前状态，以备以后恢复。但二者在用途和实现上还是有区别的，具体表现在：
Snapshot 依赖于源 volume，不能独立存在；而 backup 不依赖源 volume，即便源 volume 不存在了，也可以 restore。
Snapshot 与源 volume 通常存放在一起，都由同一个 volume provider 管理；而 backup 存放在独立的备份设备中，有自己的备份方案和实现，与 volume provider 没有关系。

创建备份：cinder-backup通过rpc请求cinder-volume服务提供需要备份的卷（get_backup_device）。如果需要备份的卷处于available状态，直接把该卷返回给
cinder_backup,如果需要备份的卷正在被使用，则先根据该卷创建一份快照或者克隆卷，返回快照或者克隆卷给cinder-backup。Cinder-backup收到备份卷后，把备份卷挂载到本机，将数据备份到后端存储

恢复备份：cinder-backup将需要进行数据恢复的卷挂载到本机，将数据从备份存储读出，恢复到卷上。
删除备份：cinder-backup直接调用backup driver中的接口进行删除



操作记录：
1. s3.py放到容器cinder_backup中的backup/drivers/路径下；
2. 容器中pip install boto3；
3. 升级oslo_utils到6.1.0；
4. 安装s3browser；
5. 进入容器docker exec -it ceph_rgw/bin/bash执行创建user；radosgw-admin user create --uid test_s3；
6. 查看user info获得key输入到s3 browser中 radosgw-admin user info --uid test_s3；
7. 之后再s3 browser新建bucket，ceph-rgw容器中可以看到bucket，radosgw-admin bucket stats；
8. 数据加字段backup_type，数据升级执行cinder-manage db sync；
9. 修改代码，重启cinder_api和cinder_backup，查看s3落地配置ceph-rgw容器中radosgw-admin bucket stats以及s3 browser上看bucket是否上传；查看rbd落地配置ceph-mon，rbd snap list volumes/volume-7bc60bb1-f640-4605-b5cb-28fdf2d4b35f/rbd snap ls volumes_ssd/volume-a96ff34e-a820-4f00-9b38-69508261b01e；


rbd snap list volumes/volume-uuid        # volume下有快照的情况
rbd snap list images/<image_uuid>        # volume下有快照的情况
rbd info volumes/volume-b45b18e6-9996-4a16-8504-5368881934f0    # 查看volume详情
rbd info  images/b45b18e6-9996-4a16-8504-5368881934f0    # 查看image详情
rbd ls -p volumes       # 查看所有volume


backup做得事：
创建volume的临时快照。
创建存放backup的container目录。
对临时快照数据进行压缩，并保存到 container目录。
创建并保存sha256（加密）文件和metadata文件。
删除临时快照。

全量备份：
调用librbd创建backup base image，与源volume大小一致，name的形式为 "volume-VOLUME_UUID.backup.base"
调用cinder-volume的rbd driver获取该image相关元数据（ 池子、用户、配置文件等）;
从源卷传输数据到该image;
源rbd image新建一个快照，name形式为backup.BACKUP_ID.snap.TIMESTRAMP
源rbd image上使用export-diff命令导出从刚开始创建时到上一步快照时的差异数据，其实就是现在整个rbd的数据，
然后通过import-diff将差量数据导入刚刚在备份集群上新创建的base image中。

增量备份：
判断是否有backup base image;
若没有，判断source image是否具有snap(from_snap)，有的话删除该snap;
若有，寻找满足"^backup.([a-z0-9-]+?).snap.(.+)$"的最近一次快照，若不存在则报错;
给source image创建一个新的snap，name形式为backup.BACKUP_ID.snap.TIMESTRAMP;
源rbd image上使用export-diff命令导出与最近的一次快照比较的差量数据，然后使用import-diff进行数据传输。


调此接口传入publicKey，返回的是publicKey加密后的数据，然后用本地的privateKey解密；


openstack的备份对接对象存储，主要的点就是怎么优化成支持多个对象存储集群，再怎么从备份转成对应的硬盘或镜像


## cinder qos限速
IOPS，每秒的输入输出量；单位事件内系统能处理的IO请求数量；

Cinder 支持 front-end 端和back-end 端设置QoS，front-end指的是在宿主机上设置虚机的qos，back-end指的是在存储设备上设置qos，ceph rbd不支持qos；
1.cinder qos-create，创建qos；cinder qos-create ceph-ssd-qos consumer=front-end read_bytes_sec=50000000 write_bytes_sec=50000000 read_iops_sec=400 write_iops_sec=400
2.cinder type-create ，创建存储类型；cinder type-create ceph-storage
3.将存储类型和后端存储绑定；cinder --os-username admin --os-tenant-name admin type-key ceph-storage set volume_backend_name=ceph
4.cinder qos-associate QOS_ID TYPE_ID将存储类型和qos绑定；
5.创建卷并将卷绑定到虚机上；

接口/v3/7e8babd4464e4c6da382a1a29d8da53a/qos-specs，uuid为project_id；

Cinder QoS限速是通过Ceph的存储池（pool）功能实现的。在Ceph中，可以为不同的存储池设置不同的QoS限制，包括带宽限制和IOPS限制等。
Cinder将这些限制应用于每个卷上，以确保符合用户的需求。
在设置QoS限制时，可以设置一个最大的带宽限制，以确保每个卷的带宽不会超过设定的限制。
然而，由于实际情况会受到网络状况和其他因素的影响，因此实际的带宽可能会略高于设置的限制。
read_iops_sec_max是指实例可以支持的最大读取每秒IOPS（Input/Output Operations Per Second），而read_iops_sec是指实例当前的读取每秒IOPS。
virsh dumpxml 3|grep sec

1. 如果创建虚机时选定的磁盘类型未关联qos，并且后续再关联qos,则限速不生效；
2. 硬盘的qos变更生效，系统盘重启，数据盘要重新挂载；


## 快照恢复成卷
ISCSI：基于TCP/IP的共享块设备的协议，通过它能够把本地的块设备共享给其它服务器；ISCSI服务端Target可以认为一个物理存储池，
包含多个backstores,backstore实际就是要共享出去的设备，backstore需要添加到指定的target中，target会把这些物理设备映射成逻辑设备，
并分配一个id，称为LUN(逻辑单元号)；
client端称为Initiator
tgtadm --lld iscsi --op show --mode target执行可以看到相关的target信息：
Target 9: iqn.2010-10.org.openstack:volume-556c537e-27c1-4d55-83a8-f0eb706e03ad
    System information:
        Driver: iscsi
        State: ready
    I_T nexus information:
        I_T nexus: 23
            Initiator: iqn.1994-05.com.redhat:2bebff03095 alias: dev-rds.vim1.local
            Connection: 0
                IP Address: 10.50.1.57
    LUN information:
        LUN: 0
            Type: controller
            SCSI ID: IET     00090000
            SCSI SN: beaf90
            Size: 0 MB, Block size: 1
            Online: Yes
            Removable media: No
            Prevent removal: No
            Readonly: No
            SWP: No
            Thin-provisioning: No
            Backing store type: null
            Backing store path: None
            Backing store flags:
        LUN: 1
            Type: disk
            SCSI ID: IET     00090001
            SCSI SN: beaf91
            Size: 10737 MB, Block size: 512
            Online: Yes
            Removable media: No
            Prevent removal: No
            Readonly: No
            SWP: No
            Thin-provisioning: No
            Backing store type: rdwr
            Backing store path: /dev/cinder-lvm/volume-556c537e-27c1-4d55-83a8-f0eb706e03ad
            Backing store flags:
    Account information:
        ZiEEns7qHiPRKV6a5UAT
    ACL information:
        ALL



lsblk --scsi                     --->查看本地块设备；
lvdisplay                        --->查看挂载的卷；
iscsiadm -m session              ---->查看已登录的target
快照恢复成卷，分为系统盘和数据盘
类型为rdb和lvm的数据盘可以正常恢复，流程：
1. 虚机添加数据卷；openstack server add volume;
2. 创建快照；openstack volume snapshot create --volume;
3. 虚机卸载数据卷；openstack server remove volume;
4. 调用恢复接口； 8776/v3/{project_id}/volumes/{volume_id}/action--->{"revert":{
        "snapshot_id": "d1bd3e66-eb27-4be4-a3e8-6b1e1a9e4648"
    }}
5. 挂载数据卷；openstack server add volume；


类型为rbd的系统盘可以正常恢复；lvm的存在问题，做下处理；
lvm类型的系统盘，操作流程：
1. 创建快照；
2. 手动将volume状态置成available；
3. 恢复快照-->内部逻辑是先根据快照生成一个临时卷，挂载临时卷，然后拷贝临时卷的数据到要恢复的卷中，再者卸载和删除临时卷，并且也detach要恢复的卷，最终调用create_export；
4. 将卷状态置成in-use。

sudo cinder-rootwrap /etc/cinder/rootwrap.conf lvchange -a n cinder-lvm/volume-c871cf96-f859-41a5-8687-dd6824ec5463

lvm--逻辑卷管理
docker exec -i -u root cinder_volume bash -c 'tgtadm --lld iscsi --op show --mode target |grep 556c537e-27c1-4d55-83a8-f0eb706e03ad'


## lvm卷的备份
ceph作为后端时，cinder_volume使用非RBD作为backend时，仅支持全量备份。
create_backup
1. 如果volume状态是in-use,则根据源卷创建一个临时卷，如果volume状态是available，则临时卷就是源卷；
2. 根据临时卷向ISCSI服务端创建export，并连接到device；
3. 之后调back service进行back，首先创建一个backup image，然后每次从源卷读入chunk_size（即backup_ceph_chunk_size，默认是128MB）大小的数据，
写入到backup image，直到把整个源卷都复制完；
4. 再者终止连接，remove export；
5. 删除临时卷；


## ceph osd
查看ceph集群---->ceph osd status




# glance

1. 创建虚拟机时并没有拷贝镜像，也不需要下载镜像，而是一个简单clone操作，因此创建虚拟机基本可以在秒级完成。
2. 如果镜像中还有虚拟机依赖，则不能删除该镜像，换句话说，删除镜像之前，必须删除基于该镜像创建的所有虚拟机。

## CLI
openstack image create --file /tmp/linux_cirros-0.3.4-x86_64-disk.raw --disk-format raw --container-format bare --public dx_cirros
cinder upload-to-image <volume_id> <image_name> --force --visibility private


## 创建镜像
1. 创建空镜像，不指定--file,状态变成queued；
2. 通过导入方式创建镜像，即指定导入方法import-method；
3. 查看rbd存储的镜像文件   ceph_mon容器中执行rbd ls -p images；rbd -p images info <image_id>

## tips
1. disk_format属性只能对queued状态的image才能替换；
2. 设置镜像裸机使用，openstack image set <image> --property usage_type=ironic；云主机--usage_type=common


## 四种镜像导入方法
glance-direct，将镜像文件直接上传到后端存储系统；--file
web-download，从官方网站或者其他可靠的镜像站点下载所需的镜像文件,之后创建镜像，--uri
copy-image
glance-download


## 支持s3后端配置文件说明
* s3_store_host
s3服务地址(e.g. s3-region.amazonaws.com, http://s3-region.amazonaws.com, https://s3-region.amazonaws.com or my-object-storage.com, http://my-object-storage.com, https://my-object-storage.com)

* s3_store_access_key
s3存储服务的访问密钥，string类型，s3 token access key.

* s3_store_secret_key
s3存储服务的安全密钥,string类型，S3 query token secret key.

* s3_store_bucket
string类型，存储glance数据的S3 bucket，如果s3_store_create_bucket_on_put被设置成True,即使bucket不存在也会自动创建bucket；

* s3_store_create_bucket_on_put
boolean类型，在上传数据时，如果bucket不存在，则新创建;

* s3_store_bucket_url_format
指定存储桶的URL格式,string类型，三种取值auto,path,virtual
path模式指的是bucket名称放在域名之外，当bucket名称包含.时，使用path模式；'https://s3.amazonaws.com/bucket/example.img' or 'https://my-object-storage.com/bucket/example.img'.
virtual模式指的bucket放入域名内部，'https://bucket.s3.amazonaws.com/example.img' or 'https://bucket.my-object-storage.com/example.img'.

* s3_store_large_object_size
单位MB，单个镜像能够支持的最大大小，即开启分段上传的容量大小限制；

* s3_store_large_object_chunk_size
指定分段上传期间上传的每个分段的大小。默认值为5MB,每个部分的最大限制为5GB，最大镜像划分不超过10000块；
将大型对象上传到 S3 时，建议使用分段上传，而不是在单个请求中上传整个文件。这有助于确保高效上传大文件，并在上传过程中发生错误时可以恢复。

* s3_store_thread_pools
执行多段上传时线程池大小；

* s3_store_region_name 
s3存储区域名称



## 镜像共享
1. 在sdn_test用户下，通过volume upload_to_image方式创建的镜像，默认私有的，并且不是受保护的，即visibility=private，unprotected；
2. 当更新image protected时，则不允许删除，本用户以及admin用户均不能删除；
3. 设置image community指的是此image可供所有其它用户共享，但是貌似不支持设置；
4. 在设置image的member时，image的visibility属性必须是shared；
5. 在sdn_test用户下创建image member让admin共享此image，此时image member状态是pending，admin用户调用接口put image member状态为accepted后，则admin用户创建虚机可使用此image；
6. upload_to_image不带force=True参数，volume状态必须是available,支持force upload(不管volume是否挂载在虚机上)需cinder-api配置enable_force_upload；
7. upload_to_image会在ceph image池中新建块存储；rbd info images/d61320f9-e786-46dd-a6fc-f91af39b0523；
8. upload_to_image接口调用返回image状态为queued，ceph中新建完后状态变成active；根据此image创建的虚机内部存储和原volume一致；
9. upload_to_image后，原volume所挂载的虚机可以删除，同样volume也可以删除；
10. upload_to_image时，原volume先是uploading，然后in-use；



# 普罗米修斯(Prometheus)
一款开源的监控和警报系统，使用节点监控模块(node_exporter)；



# 部署
## kolla-ansible
1. --tags neutron                                                      ---部署单独的组件；
2. --skip-tags neutron nova                                            ---部署时跳过某些组件
3. kolla-ansible post-deploy                                           ---生成admin-openrc.sh
4. kolla-ansible -i /etc/ansible/hosts/ --tags nova reconfigure        ---修改所有节点的配置/etc/kolla/config/nova.conf
5. kolla-ansible -i /etc/ansible/hosts/ deploy
6. kolla-ansible -i /etc/ansible/hosts/ destroy --yes-i-really-really-mean-it


# notification机制
from neutron_lib import rpc as n_rpc
notifier = n_rpc.get_notifier('network')
notifier.info(context, "firewall_group.update_status",
             {"firewall_group": {"id": fwg_id, "status": to_update,
             "error_msg": error_msg}})



# Python

## eventlet
1. 使用线程池
    pool = eventlet.GreenPool(<threads_num>)
    pool.spawn(<func_name>, <args>)
    pool.waitall()
    






===================================
Support force detach volume to nova
===================================

Proposed change
===============

1. Add a nova-manage command: nova-manage db volume-force-detach
    which will be placed in nova/cmd/manage.py:DbCommands

Note: Nova doesn't currently allow you to detach root volumes, so none of this
applies to root disk volumes.


Alternatives
------------

Add a new extension api "reset volume attach state" which defaults to admin
only. It will clean up the records about the attachment and reset the
attachment state of the volume in nova side. Then call force detach api in
cinder to make consistent state between nova and cinder.

