

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
ip route add default via 10.50.114.157       # 增加默认路由
可以直接修改配置文件/etc/sysconfig/network-scripts/中的ifcfg-eth0；



# mysql
1. 数据库导出：mysqldump -u skyline -pa053k3fpWxlPIr6AmrtJtukBwt5Z8294Ui2u18Oe skyline > skyline.sql
2. 数据库导入：mysql -u skyline -pa0jiBfqyzMr5tmddHF95hI4hdonTQm4mNUnvaKjp skyline < skyline.sql
3. 修改表字段：alter table backups extra_attrs extra_attributes varchar(1000);
4. 表中增加字段：alter table blog_article add cover_image_url varchar(255) DEFAULT '' COMMENT '封面图片地址';
5. 修改表中值：update quota_usages set in_use=0 where id=5;
6. 插入数据：INSERT INTO users (name, email) VALUES ('John Doe', 'johndoe@example.com');
7. 



## 索引
1. explain查看执行计划时，key为PRIMARY表示使用了主键索引；key为null(type=ALL)表示使用了全表扫描；
    key为idx_name表示使用索引name,Exta为Using index；
    Exta为Using index condition表示使用了索引下推；
2. 执行器查询先调read_first_record，执行器查询是while循环，还会再查一次read_record，根据访问类型进行查询；
3. 在创建表时，InnoDB 存储引擎会根据不同的场景选择不同的列作为索引：
    如果有主键，默认会使用主键作为聚簇索引的索引键（key）；
    如果没有主键，就选择第一个不包含 NULL 值的唯一列作为聚簇索引的索引键（key）；
    在上面两个都没有的情况下，InnoDB 将自动生成一个隐式自增 id 列作为聚簇索引的索引键（key）；
4. 联合索引的多个索引字段的值是B+树的key；
5. 联合索引的最左匹配原则，在遇到范围查询（如 >、<）的时候，就会停止匹配，也就是范围查询的字段可以用到联合索引，
    但是在范围查询字段的后面的字段无法用到联合索引。注意，对于 >=、<=、BETWEEN、like 前缀匹配的范围查询，并不会停止匹配
6. 在联合索引的情况下，数据是按照索引第一列排序，第一列数据相同时才会按照第二列排序。
7. 索引下推优化（index condition pushdown)，可以在联合索引遍历过程中，对联合索引中包含的字段先做判断，直接过滤掉不满足条件的记录，减少回表次数。
8. 索引数据是存储在磁盘上的，通过索引查找某行数据的时候，就是先从磁盘读取索引到内存，再通过索引从磁盘中找到某行数据，然后读入到内存，
    也就是说查询过程中会发生多次磁盘 I/O，而磁盘 I/O 次数越多，所消耗的时间也就越大。
9. 要设计一个适合 MySQL 索引的数据结构，至少满足以下要求：
    1. 能在尽可能少的磁盘的 I/O 操作中完成查询工作；
    2. 要能高效地查询某一个记录，也要能高效地执行范围查找
10. 索引结构不会影响单表最大行数，2000W 也只是推荐值，超过了这个值可能会导致 B + 树层级更高，影响查询性能。
11. like关键字左或者左右模糊匹配无法走索引，有模糊匹配可以有索引；
12. select count(主键字段) from table; 如果表中有二级索引时，则查找二级索引树进行；如果无二级索引，则查找聚簇索引树；
13. 性能count(*)=count(1)>count(主键索引)>count(字段)；


## 事务
1. Read View 在 MVCC 里如何工作的？
    Read View 有四个重要的字段：
    m_ids ：指的是在创建 Read View 时，当前数据库中「活跃事务」的事务 id 列表，注意是一个列表，“活跃事务”指的就是，启动了但还没提交的事务。
    min_trx_id ：指的是在创建 Read View 时，当前数据库中「活跃事务」中事务 id 最小的事务，也就是 m_ids 的最小值。
    max_trx_id ：这个并不是 m_ids 的最大值，而是创建 Read View 时当前数据库中应该给下一个事务的 id 值，也就是全局事务中最大的事务 id 值 + 1；
    creator_trx_id ：指的是创建该 Read View 的事务的事务 id。
    对于使用 InnoDB 存储引擎的数据库表，它的聚簇索引记录中都包含下面两个隐藏列： 
    trx_id，当一个事务对某条聚簇索引记录进行改动时，就会把该事务的事务 id 记录在 trx_id 隐藏列里；
    roll_pointer，每次对某条聚簇索引记录进行改动时，都会把旧版本的记录写入到 undo 日志中，然后这个隐藏列是个指针，指向每一个旧版本记录，于是就可以通过它找到修改前的记录。
    一个事务去访问记录的时候，除了自己的更新记录总是可见之外，还有这几种情况：
    如果记录的 trx_id 值小于 Read View 中的 min_trx_id 值，表示这个版本的记录是在创建 Read View 前已经提交的事务生成的，所以该版本的记录对当前事务可见。
    如果记录的 trx_id 值大于等于 Read View 中的 max_trx_id 值，表示这个版本的记录是在创建 Read View 后才启动的事务生成的，所以该版本的记录对当前事务不可见。
    如果记录的 trx_id 值在 Read View 的 min_trx_id 和 max_trx_id 之间，需要判断 trx_id 是否在 m_ids 列表中：
    如果记录的 trx_id 在 m_ids 列表中，表示生成该版本记录的活跃事务依然活跃着（还没提交事务），所以该版本的记录对当前事务不可见。
    如果记录的 trx_id 不在 m_ids列表中，表示生成该版本记录的活跃事务已经被提交，所以该版本的记录对当前事务可见。
    这种通过「版本链」来控制并发事务访问同一个记录时的行为就叫 MVCC（多版本并发控制）。



## 数据库锁
1. 全局锁
    flush table with read lock,数据库处于只读状态
    unlock tables,释放全局锁
2. 表级锁
    表锁
    元数据锁(MDL)
        对一张表进行 CRUD 操作时，加的是 MDL 读锁；
        对一张表做结构变更操作的时候，加的是 MDL 写锁；
        MDL 是为了保证当用户对表执行 CRUD 操作时，防止其他线程对这个表结构做了变更。
    意向锁
        在使用 InnoDB 引擎的表里对某些记录加上「共享锁」之前，需要先在表级别加上一个「意向共享锁」；
        在使用 InnoDB 引擎的表里对某些纪录加上「独占锁」之前，需要先在表级别加上一个「意向独占锁」；
        意向锁的目的是为了快速判断表里是否有记录被加锁。
    AUTO-INC锁
        在插入数据时，会加一个表级别的 AUTO-INC 锁，然后为被 AUTO_INCREMENT 修饰的字段赋值递增的值，等插入语句执行完成后，才会把 AUTO-INC 锁释放掉。
3. 行级锁
    Record Lock，记录锁，也就是仅仅把一条记录锁上；
    Gap Lock，间隙锁，锁定一个范围，但是不包含记录本身；
    Next-Key Lock：Record Lock + Gap Lock 的组合，锁定一个范围，并且锁定记录本身。
        //对读取的记录加共享锁
        select ... lock in share mode;
        //对读取的记录加独占锁
        select ... for update;
        update table .... where id = 1;
        delete from table where id = 1;
4. mysql是怎么加锁的？
    加锁的对象是针对索引；
    用非唯一索引进行等值查询的时候：
        因为存在两个索引，一个是主键索引，一个是非唯一索引（二级索引），所以在加锁时，同时会对这两个索引都加锁，但是对主键索引加锁的时候，只有满足查询条件的记录才会对它们的主键索引加锁。
        针对非唯一索引等值查询时，查询的记录存不存在，加锁的规则也会不同：
        当查询的记录「存在」时，由于不是唯一索引，所以肯定存在索引值相同的记录，于是非唯一索引等值查询的过程是一个扫描的过程，直到扫描到第一个不符合条件的二级索引记录就停止扫描，然后在扫描的过程中，对扫描到的二级索引记录加的是 next-key 锁，而对于第一个不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。同时，在符合查询条件的记录的主键索引上加记录锁。
        当查询的记录「不存在」时，扫描到第一条不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。因为不存在满足查询条件的记录，所以不会对主键索引加锁。
    

## mysql数据存储
1. 每创建一个database（数据库）都会在/var/lib/mysql/目录里面创建一个以database为名的目录，然后保存表结构和表数据的文件都会存放在这个目录里； 
    db.opt，用来存储当前数据库的默认字符集和字符校验规则。 
    t_order.frm，t_order 的表结构会保存在这个文件。在 MySQL 中建立一张表都会生成一个.frm 文件，该文件是用来保存每个表的元数据信息的，主要包含表结构定义。
    t_order.ibd，t_order 的表数据会保存在这个文件。表数据既可以存在共享表空间文件（文件名：ibdata1）里，也可以存放在独占表空间文件（文件名：表名字.ibd）。
2. 表空间由段（segment）、区（extent）、页（page）、行（row）组成
    行：数据库表中的记录都是按行（row）进行存放的
    页：InnoDB 的数据是按「页」为单位来读写的，也就是说，当需要读一条记录的时候，并不是将这个行记录从磁盘读出来，而是以页为单位，将其整体读入内存。 
        默认每个页的大小为 16KB，也就是最多能保证 16KB 的连续存储空间。
        页是 InnoDB 存储引擎磁盘管理的最小单元，意味着数据库每次读写都是以 16KB 为单位的，一次最少从磁盘中读取 16K 的内容到内存中，一次最少把内存中的 16K 内容刷新到磁盘中。
    区：在表中数据量大的时候，为某个索引分配空间的时候就不再按照页为单位分配了，而是按照区（extent）为单位分配。
        每个区的大小为 1MB，对于 16KB 的页来说，连续的 64 个页会被划为一个区，这样就使得链表中相邻的页的物理位置也相邻，就能使用顺序 I/O 了。
    段：表空间是由各个段（segment）组成的，段是由多个区（extent）组成的。段一般分为数据段、索引段和回滚段等。
        索引段：存放 B + 树的非叶子节点的区的集合； 
        数据段：存放 B + 树的叶子节点的区的集合；
        回滚段：存放的是回滚数据的区的集合，MVCC 利用了回滚段实现了多版本查询数据。
3. varchar(n) 字段类型的 n 代表的是最多存储的字符数量，并不是字节大小；要算 varchar(n) 最大能允许存储的字节数，还要看数据库表的字符集，
    因为字符集代表着，1个字符要占用多少字节，比如 ascii 字符集， 1 个字符占用 1 字节，那么 varchar(100) 意味着最大能允许存储 100 字节的数据。
4. 存储字段类型为 varchar(n) 的数据时，其实分成了三个部分来存储：
    真实数据
    真实数据占用的字节数
    NULL 标识，如果不允许为NULL，这部分不需要
5. 一行数据的最大字节数 65535，其实是包含「变长字段长度列表」和 「NULL 值列表」所占用的字节数的。
6. 字段是允许为 NULL 的，所以会用 1 字节来表示「NULL 值列表」。
7. 条件一：如果变长字段允许存储的最大字节数小于等于 255 字节，就会用 1 字节表示「变长字段长度」； 
   条件二：如果变长字段允许存储的最大字节数大于 255 字节，就会用 2 字节表示「变长字段长度」；
8. 如果一个数据页存不了一条记录，InnoDB 存储引擎会自动将溢出的数据存放到「溢出页」中。
9. buffer pool，缓存池--连续的16k大小的页；读取数据从buffer pool中取；修改数据时，如果数据存在buffer pool中，则直接在内存中修改，然后合适的时机再将此页数据写入磁盘；


## mysql日志
1. undo log(回滚日志)
    Innodb存储引擎日志，用于事务回滚和MVCC；
2. redo log(重做日志)
    Innodb存储引擎日志，用于掉电等故障恢复；
    当有一条记录需要更新的时候，InnoDB 引擎就会先更新内存（同时标记为脏页），然后将本次对这个页的修改以 redo log 的形式记录下来，这个时候更新就算完成了。
    后续，InnoDB 引擎会在适当的时候，由后台线程将缓存在 Buffer Pool 的脏页刷新到磁盘里，这就是 WAL （Write-Ahead Logging）技术。
    WAL 技术指的是， MySQL 的写操作并不是立刻写到磁盘上，而是先写日志，然后在合适的时间再写到磁盘上。
    redo log 是物理日志，记录的是在某个数据页做了什么修改，比如对 XXX 表空间中的 YYY 数据页 ZZZ 偏移量的地方做了AAA 更新；
3. binlog(归档日志)
    server层日志，用于数据备份和主从复制；
    binlog 文件保存的是全量的日志，也就是保存了所有数据变更的情况，理论上只要记录在 binlog 上的数据，都可以恢复，所以如果不小心整个数据库的数据被删除了，得用 binlog 文件恢复数据。 
4. 两阶段提交
    事务的提交过程有两个阶段，就是将 redo log 的写入拆成了两个步骤：prepare 和 commit，中间再穿插写入binlog，具体如下：
    prepare 阶段：将 XID（内部 XA 事务的 ID） 写入到 redo log，同时将 redo log 对应的事务状态设置为 prepare，然后将 redo log 持久化到磁盘（innodb_flush_log_at_trx_commit = 1 的作用）；
    commit 阶段：把 XID 写入到 binlog，然后将 binlog 持久化到磁盘（sync_binlog = 1 的作用），接着调用引擎的提交事务接口，将 redo log 状态设置为 commit，此时该状态并不需要持久化到磁盘，只需要 write 到文件系统的 page cache 中就够了，因为只要 binlog 写磁盘成功，就算 redo log 的状态还是 prepare 也没有关系，一样会被认为事务已经执行成功；


## mysql主从复制过程
MySQL 集群的主从复制过程梳理成 3 个阶段：
1. 写入 Binlog：主库写 binlog 日志，提交事务，并更新本地存储数据。
2. 同步 Binlog：把 binlog 复制到所有从库上，每个从库把 binlog 写到暂存日志中。
3. 回放 Binlog：回放 binlog，并更新存储引擎中的数据。

具体详细过程如下：
1. MySQL 主库在收到客户端提交事务的请求之后，会先写入 binlog，再提交事务，更新存储引擎中的数据，事务提交完成后，返回给客户端“操作成功”的响应。
2. 从库会创建一个专门的 I/O 线程，连接主库的 log dump 线程，来接收主库的 binlog 日志，再把 binlog 信息写入 relay log 的中继日志里，再返回给主库“复制成功”的响应。
3. 从库会创建一个用于回放 binlog 的线程，去读 relay log 中继日志，然后回放 binlog 更新存储引擎中的数据，最终实现主从的数据一致性。



# 快捷键
1. 从后往前删除   ctrl+w；
2. 从前往后删除   ctrl+k；
3. 光标从前调到末尾  ctrl+e;  vim内部为删除光标所在行；


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





## tcp
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

### 查看tcp的连接状态
netstat -napt

### tcp连接
建立一个TCP连接是需要客户端和服务端达成三个信息的共识：
1. sockets;由ip和地址组成
2. 序列号；解决乱序问题；
3. 窗口大小；解决流量控制；


### 为什么需要三次握手？
#### 防止旧的重复连接初始化造成混乱；
#### 同步双方的初始化序列号；
#### 避免资源浪费；
两次握手会造成消息滞留情况下，服务端重复接受无用的连接请求 SYN 报文，而造成重复分配资源。


### 如果握手报文丢失了，会发生什么？
1. 如果第一次握手丢失了，客户端会触发超时重传SYN包；
2. 如果第二次握手丢失了，客户端会触发超时重传SYN包，服务端会触发超时重传SYN+ACK包；
3. 如果第三次握手丢失了，服务端会触发超时重传SYN+ACK包；


### 为什么需要四次挥手？
关闭连接时，客户端向服务端发送 FIN 时，仅仅表示客户端不再发送数据了但是还能接收数据。
服务端收到客户端的 FIN 报文时，先回一个 ACK 应答报文，而服务端可能还有数据需要处理和发送，等服务端不再发送数据时，才发送 FIN 报文给客户端来表示同意现在关闭连接。
从上面过程可知，服务端通常需要等待完成数据的发送和处理，所以服务端的 ACK 和 FIN 一般都会分开发送，因此是需要四次挥手。

### 如果挥手报文丢失了， 会发生什么？
1. 第一次挥手丢失了
客户端迟迟收不到服务端的ACK报文，触发超时重传机制；重传 FIN 报文，重发次数由 tcp_orphan_retries 参数控制。
当客户端重传 FIN 报文的次数超过 tcp_orphan_retries 后，就不再发送 FIN 报文，则会在等待一段时间（时间为上一次超时时间的 2 倍），
如果还是没能收到第二次挥手，那么直接进入到 close 状态。
2. 第二次挥手丢失了
ACK 报文是不会重传的，所以如果服务端的第二次挥手丢失了，客户端就会触发超时重传机制，重传 FIN 报文，直到收到服务端的第二次挥手，或者达到最大的重传次数。
3. 第三次挥手丢失了
服务端处于 CLOSE_WAIT 状态时，调用了 close 函数，内核就会发出 FIN 报文，同时连接进入 LAST_ACK 状态，等待客户端返回 ACK 来确认连接关闭。
如果迟迟收不到这个 ACK，服务端就会重发 FIN 报文，重发次数仍然由 tcp_orphan_retries 参数控制，这与客户端重发 FIN 报文的重传次数控制方式是一样的。
客户端和服务端都会断开连接；
4. 第四次挥手丢失了
客户端进入了TIME_WAIT状态，持续2MSL后才会进入关闭状态；
服务端会重发FIN报文；超过了tcp_orphan_retries次数，等待一段事件服务端即关闭；


### 2MSL
MSL---Maximum Segment Lifetime，报文最大生存时间；
2MSL时长 这其实是相当于至少允许报文丢失一次。比如，若 ACK 在一个 MSL 内丢失，这样被动方重发的 FIN 会在第 2 个 MSL 内到达，TIME_WAIT 状态的连接可以应对。

### TCP超时重传
就是在发送数据时，设定一个定时器，当超过指定的时间后，没有收到对方的 ACK 确认应答报文，就会重发该数据，也就是我们常说的超时重传。
两种情况进行超时重传：
1. 数据包丢失；
2. 确认应答包丢失；

### TCP快速重传
快速重传的工作方式是当收到三个相同的 ACK 报文时，会在定时器过期之前，重传丢失的报文段。
第一份 Seq1 先送到了，于是就 Ack 回 2；
结果 Seq2 因为某些原因没收到，Seq3 到达了，于是还是 Ack 回 2；
后面的 Seq4 和 Seq5 都到了，但还是 Ack 回 2，因为 Seq2 还是没有收到；
发送端收到了三个 Ack = 2 的确认，知道了 Seq2 还没有收到，就会在定时器过期之前，重传丢失的 Seq2。
最后，收到了 Seq2，此时因为 Seq3，Seq4，Seq5 都收到了，于是 Ack 回 6 。

### TCP SACK方法
selective acknowledgment,选择性确认
TCP 头部「选项」字段里加一个 SACK 的东西，它可以将已收到的数据的信息发送给「发送方」，
这样发送方就可以知道哪些数据收到了，哪些数据没收到，知道了这些信息，就可以只重传丢失的数据。

### TCP duplicate SACK
主要使用了 SACK 来告诉「发送方」有哪些数据被重复接收了。
「接收方」发给「发送方」的两个 ACK 确认应答都丢失了，所以发送方超时后，重传第一个数据包（3000 ~ 3499）
于是「接收方」发现数据是重复收到的，于是回了一个 SACK = 3000~3500，告诉「发送方」 3000~3500 的数据早已被接收了，因为 ACK 都到了 4000 了，已经意味着 4000 之前的所有数据都已收到，所以这个 SACK 就代表着 D-SACK。
这样「发送方」就知道了，数据没有丢，是「接收方」的 ACK 确认报文丢了。


### 滑动窗口
窗口大小就是指无需等待确认应答，而可以继续发送数据的最大值。
TCP 头里有一个字段叫 Window，也就是窗口大小。
这个字段是接收端告诉发送端自己还有多少缓冲区可以接收数据。于是发送端就可以根据这个接收端的处理能力来发送数据，而不会导致接收端处理不过来。
所以，通常窗口的大小是由接收方的窗口大小来决定的。
发送方窗口：
已发送但未收到ACK确认的数据+未发送但总大小在接收方处理范围内；
窗口左边是已发送且已收到ACK确认的数据；
窗口右边是未发送但查过接收方处理方位的数据；


### tcp的keepalive和http的Keep-Alive
HTTP 的 Keep-Alive 也叫 HTTP 长连接，该功能是由「应用程序」实现的，可以使得用同一个 TCP 连接来发送和接收多个 HTTP 请求/应答， 减少了 HTTP 短连接带来的多次 TCP 连接建立和释放的开销。
TCP 的 Keepalive 也叫 TCP 保活机制，该功能是由「内核」实现的，当客户端和服务端长达一定时间没有进行数据交互时， 内核为了确保该连接是否还有效，就会发送探测报文，来检测对方是否还在线，然后来决定是否要关闭该连接。


### 网络性能指标
带宽：链路的最大传输速率，b/s；
吞吐量：单位事件内成功传输的数据量；b/s(比特/s)或B/s(字节/s)



### socket信息查看
netstat -nlp
ss -ltnp


### TLS
一般情况下，不管 TLS 握手次数如何，都得先经过 TCP 三次握手后才能进行，
因为 HTTPS 都是基于 TCP 传输协议实现的，得先建立完可靠的 TCP 连接才能做 TLS 握手的事情。



## 证书
CA 证书文件（ssl_cacert.pem）---即根证书；
客户端证书和私钥文件----cert.pem, key.pem



## 用户及用户组
添加用户---adduser dx
设置用户密码---passwd dx
添加用户组---groupadd docker
将用户加到用户组中---usermod -aG docker dx
激活对用户组的修改---newgrp docker



## docker
1. Dockerfile中COPY将本地文件拷贝到容器中；ADD本地自动解压文档文件到容器中；
2. Docker image是由一系列Docker只读层创建出来的；Docker layer在Dockerfile配置文件中完成一条配置指令即表示一个Docker layer；
3. docker run -itd -u root -v /service/logs/dev/nonick/nonick-notifier-service:/service/logs/dev/nonick/nonick-notifier-service -v /etc/localtime:/etc/localtime -p 8081 notifier:0912 bash


### 容器化技术在底层的运行原理
每个容器运行在它自己的命名空间中，但是，确实与其它运行中的容器共用相同的系统内核。
隔离的产生是由于系统内核清楚地知道命名空间及其中的进程，且这些进程调用系统api时，内核保证进程只能访问属于其命名空间中的资源。



## kubernetes

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
修改网络类型暂为calico，因为底层是openstack，vxlan本环境冲突，不能使用flannel。 工程目录为：/etc/kubeasz/cluster/gpu_dev/
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
ezctl setup gpu_uat all
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



### k8s源码

#### kube-apiserver
对外提供api的方式与其它组件进行交互；







## http

### http常见字段
#### host
客户端发送请求时，用来指定服务器的域名。

#### context-length
服务器在返回数据时，会有 Content-Length 字段，表明本次回应的数据长度。

#### connection
Connection 字段最常用于客户端要求服务器使用「HTTP 长连接」机制，以便其他请求复用。
Connection: Keep-Alive

#### Context-Type
用于服务器回应时，告诉客户端，本次数据是什么格式；
客户端请求的时候，可以使用 Accept 字段声明自己可以接受哪些数据格式。Accept: */*

#### Context-Encoding
Content-Encoding 字段说明数据的压缩方法。表示服务器返回的数据使用了什么压缩格式


#### 错误码
504 错误码是网关超时错误，通常是nginx将请求代理到后端应用时，后端应用没有在规定的时间返回数据；



## websocket
http/1.1 101 Switching Protocols\r\n     # 101状态码，表示协议转换
Connection: Upgrade    # 浏览器想升级协议
Upgrade: WebSocket     # 想升级成WebSocket协议
Sec-WebSocket-Key: T2a6wZlAwhgQNqruZ2YUyg==\r\n         # 随机生成的base64码

websocket只有在建立连接时才用到http,升级完后就和http没有关系了。



## sar
怀疑CPU存在瓶颈，可用 sar -u 和 sar -q 等来查看
怀疑内存存在瓶颈，可用 sar -B、sar -r 和 sar -W 等来查看
怀疑I/O存在瓶颈，可用 sar -b、sar -u 和 sar -d 等来查看
