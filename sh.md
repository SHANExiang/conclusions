

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



# mysql
1. 数据库导出：mysqldump -u skyline -pa053k3fpWxlPIr6AmrtJtukBwt5Z8294Ui2u18Oe skyline > skyline.sql
2. 数据库导入：mysql -u skyline -pa0jiBfqyzMr5tmddHF95hI4hdonTQm4mNUnvaKjp skyline < skyline.sql
3. 修改表字段：alter table backups extra_attrs extra_attributes varchar(1000);
4. 表中增加字段：alter table blog_article add cover_image_url varchar(255) DEFAULT '' COMMENT '封面图片地址';
5. 修改表中值：update quota_usages set in_use=0 where id=5;
6. 插入数据：INSERT INTO users (name, email) VALUES ('John Doe', 'johndoe@example.com');
7. docker启动mysql docker run --name mysql-container -e MYSQL_ROOT_PASSWORD=<root_password> -p 3306:3306 -d mysql:latest
8. 查看表的schema，show create table <table_name>;
9. 在与mysql交互终端执行select中文显示？，则mysql -u root -p123456 --default-character-set=utf8mb4 dbname；或者执行查询前SET NAMES utf8mb4;
10. 数据库和表的字符集设置为支持中文的字符集，如 utf8mb4。
11. .frm是表结构文件，存储数据库表的定义和结构信息；.ibd是真正存储数据的文件；
12. mysql基本操作单元是页，一页默认16K;
13. 页构成了区，一个区通常有64个连续的页，相邻的页构成一个双向链表，便于顺序访问，一个区的大小100*16K也就是1m；
14. 若干区构成了段，段中的区是随机分布的，创建一个表、索引的时候就会创建一个段结构来存储具体的数据信息，比如表段、索引段；
15. 表空间--是一个逻辑空间，一个表就是一个表空间，或者多个表共用一个表空间，可以看成一个管理结构；
16. 







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
    主要应用在全库做逻辑备份。
使用全局锁来备份数据时影响业务怎么办？
在可重复读隔离级别下mysqldump时加上参数-single-transaction，这样会创建read view,由于MVCC的支持业务依然可以进行更新操作。
2. 表级锁
a. 表锁---lock tables t_student read; lock tables t_student write; unlock tables;
b. 元数据锁(MDL)
不需要显示的使用 MDL，因为当我们对数据库表进行操作时，会自动给这个表加上 MDL：
对一张表进行 CRUD 操作时，加的是 MDL 读锁；
对一张表做结构变更操作的时候，加的是 MDL 写锁；
MDL 是为了保证当用户对表执行 CRUD 操作时，防止其他线程对这个表结构做了变更。
c. 意向锁
在使用 InnoDB 引擎的表里对某些记录加上「共享锁」之前，需要先在表级别加上一个「意向共享锁」； select ... lock in share mode(//先在表上加上意向共享锁，然后对读取的记录加共享锁);
在使用 InnoDB 引擎的表里对某些纪录加上「独占锁」之前，需要先在表级别加上一个「意向独占锁」； select ... for update(//先表上加上意向独占锁，然后对读取的记录加独占锁);
意向锁的目的是为了快速判断表里是否有记录被加锁。
3. AUTO-INC锁
在插入数据时，会加一个表级别的 AUTO-INC 锁，然后为被 AUTO_INCREMENT 修饰的字段赋值递增的值，等插入语句执行完成后，才会把 AUTO-INC 锁释放掉。
4. 行级锁
a. Record Lock，记录锁，也就是仅仅把一条记录锁上；
b. Gap Lock，间隙锁，锁定一个范围，但是不包含记录本身；
c. Next-Key Lock：Record Lock + Gap Lock 的组合，锁定一个范围，并且锁定记录本身。
//对读取的记录加共享锁
select ... lock in share mode;
//对读取的记录加独占锁
select ... for update;
update table .... where id = 1;
delete from table where id = 1; 

### mysql是怎么加锁的？
加锁的对象是针对索引；
用非唯一索引进行等值查询的时候：
因为存在两个索引，一个是主键索引，一个是非唯一索引（二级索引），所以在加锁时，同时会对这两个索引都加锁，但是对主键索引加锁的时候，只有满足查询条件的记录才会对它们的主键索引加锁。
针对非唯一索引等值查询时，查询的记录存不存在，加锁的规则也会不同：
当查询的记录「存在」时，由于不是唯一索引，所以肯定存在索引值相同的记录，于是非唯一索引等值查询的过程是一个扫描的过程，直到扫描到第一个不符合条件的二级索引记录就停止扫描，然后在扫描的过程中，对扫描到的二级索引记录加的是 next-key 锁，而对于第一个不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。同时，在符合查询条件的记录的主键索引上加记录锁。
当查询的记录「不存在」时，扫描到第一条不符合条件的二级索引记录，该二级索引的 next-key 锁会退化成间隙锁。因为不存在满足查询条件的记录，所以不会对主键索引加锁。
    
### MySQL 是怎么解决幻读的？
MySQL InnoDB 引擎的默认隔离级别虽然是「可重复读」，但是它很大程度上避免幻读现象（并不是完全解决了，详见这篇文章 (opens new window)），解决的方案有两种：
针对快照读（普通 select 语句），是通过 MVCC 方式解决了幻读，因为可重复读隔离级别下，事务执行过程中看到的数据，一直跟这个事务启动时看到的数据是一致的，即使中途有其他事务插入了一条数据，是查询不出来这条数据的，所以就很好了避免幻读问题。
针对当前读（select ... for update 等语句），是通过 next-key lock（记录锁+间隙锁）方式解决了幻读，因为当执行 select ... for update 语句的时候，会加上 next-key lock，如果有其他事务在 next-key lock 锁范围内插入了一条记录，那么这个插入语句就会被阻塞，无法成功插入，所以就很好了避免幻读问题。0

### 可重复读下发生幻读的场景？
1. 当事务 A 更新了一条事务 B 插入的记录，那么事务 A 前后两次查询的记录条目就不一样了，所以就发生幻读。
2. T1 时刻：事务 A 先执行「快照读语句」：select * from t_test where id > 100 得到了 3 条记录。 T2 时刻：事务 B 往插入一个 id= 200 的记录并提交； T3 时刻：事务 A 再执行「当前读语句」 select * from t_test where id > 100 for update 就会得到 4 条记录，此时也发生了幻读现象。



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
9. 

## mysql buffer pool
buffer pool，缓存池--连续的16k大小的页；读取数据从buffer pool中取；修改数据时，如果数据存在buffer pool中，则直接在内存中修改，然后合适的时机再将此页数据写入磁盘；

buffer pool除了缓存页和数据页，还包括undo页，插入缓存、自适应哈希索引、锁信息等；
默认大小是128M，可以通过innodb_buffer_pool_size参数调整缓冲池的大小。

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


### 主从同步延时问题
有两种场景：
1. 由于从库从主库拷贝日志以及串行执行 SQL 的特点，在高并发场景下，从库的数据一定会比主库慢一些，是有延时的。所以经常出现，刚写入主库的数据可能是读不到的，要过几十毫秒，甚至几百毫秒才能读取到。
2. 如果主库突然宕机，然后恰好数据还没同步到从库，那么有些数据可能在从库上是没有的，有些数据可能就丢失了。

解决方案：
1. 分库，将一个主库拆分为多个主库，每个主库的写并发就减少了几倍，此时主从延迟可以忽略不计。
2. 打开 MySQL 支持的并行复制，多个库并行复制。如果说某个库的写入并发就是特别高，单库写并发达到了 2000/s，并行复制还是没意义。
3. 重写代码，写代码的同学，要慎重，插入数据时立马查询可能查不到。

## 分库分表
中间件：
Cobar
TDDL
Atlas
Sharding-jdbc
Mycat

水平拆分：就是把一个表的数据给弄到多个库的多个表里去，但是每个库的表结构都一样，只不过每个库表放的数据是不同的，所有库表的数据加起来就是全部数据。
垂直拆分：就是把一个有很多字段的表给拆分成多个表，或者是多个库上去。

分库分表如何平滑过渡？
1. 停机迁移方案。写一个导数的一次性工具，在系统停掉下，将单库单表的数据哗哗哗读出来，写到分库分表里面去；
2. 双写迁移方案。除了对老库的增删改，都加上对新库的增删改；后台数据迁移工具读老库数据写新库，写的时候要根据 gmt_modified 这类字段判断这条数据最后修改的时间，除非是读出来的数据在新库里没有，或者是比新库的数据新才会写。

动态扩容步骤：
1. 设定好几台数据库服务器，每台服务器上几个库，每个库多少个表，推荐是 32 库 * 32 表，对于大部分公司来说，可能几年都够了。
2. 路由的规则，orderId 模 32 = 库，orderId / 32 模 32 = 表
3. 扩容的时候，申请增加更多的数据库服务器，装好 MySQL，呈倍数扩容，4 台服务器，扩到 8 台服务器，再到 16 台服务器。
4. 由 DBA 负责将原先数据库服务器的库，迁移到新的数据库服务器上去，库迁移是有一些便捷的工具的。
5. 我们这边就是修改一下配置，调整迁移的库所在数据库服务器的地址。
6. 重新发布系统，上线，原先的路由规则变都不用变，直接可以基于 n 倍的数据库服务器的资源，继续进行线上系统的提供服务。


分库分表id主键怎么处理？
使用雪花算法：
把一个64位的long型的id，1 个 bit 是不用的，用其中的 41 bits 作为毫秒数，用 10 bits 作为工作机器 id，12 bits 作为序列号。


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
proxy_read_timeout: 用于设置从后端服务器接收响应的超时时间。默认值60s
proxy_connect_timeout: 用于设置与后端服务器建立连接的超时时间。默认值60s
proxy_send_timeout: 用于设置向后端服务器发送请求的超时时间。默认值60s




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

### 查看socket信息
netstat -napt或ss -ntlp
-n表示不显示名字，而是以数字方式显示ip和端口
-l只显示LISTEN状态的socket
-p表示显示进程信息
-t表示只显示tcp连接



### tcp连接
建立一个TCP连接是需要客户端和服务端达成三个信息的共识：
1. sockets;由ip和地址组成
2. 序列号；解决乱序问题；
3. 窗口大小；解决流量控制；

#### tcp的三个特点
1. 面向连接
2. 可靠
3. 基于字节流
字节流可以理解为一个双向的通道里流淌的数据，这个数据其实就是我们常说的二进制数据，简单来说就是一大堆 01 串。



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
sysctl -w net.ipv4.tcp_keepalive_time=120   临时设置tcp超时时间
sysctl -a|grep net.ipv4.tcp_keepalive_time   查看tcp连接超时时间，超过这个时间默认会重试2次，间隔10s


### 网络性能指标
带宽：链路的最大传输速率，b/s；
吞吐量：单位事件内成功传输的数据量；b/s(比特/s)或B/s(字节/s)



### socket信息查看
netstat -nlp
ss -tunlp|grep 9696     查看端口打开情况



### TLS
一般情况下，不管 TLS 握手次数如何，都得先经过 TCP 三次握手后才能进行，
因为 HTTPS 都是基于 TCP 传输协议实现的，得先建立完可靠的 TCP 连接才能做 TLS 握手的事情。



## 证书
CA 证书文件（ssl_cacert.pem）---即根证书；
客户端证书和私钥文件----cert.pem, key.pem


## IP
ICMP--用于告知网络包传送过程中产生的错误以及各种控制信息。确认 IP 包是否成功送达目标地址、报告发送过程中 IP 包被废弃的原因和改善网络设置等。
ARP--用于根据 IP 地址查询相应的以太网 MAC 地址。   arp -a查看缓存内容
MTU--一个网络包的最大长度，以太网中的一般为1500字节；
MSS--除了IP和TCP头部后，一个网络包所能容纳的TCP数据的最大长度；

1. 因为路由器是基于 IP 设计的，俗称三层网络设备，路由器的各个端口都具有 MAC 地址和 IP 地址；
而交换机是基于以太网设计的，俗称二层网络设备，交换机的端口不具有 MAC 地址。
2. 


## IPv6
1. 128位,每16位一组，每组用：隔开；
2. 可自动配置，即使没有DHCP服务器也能实现自动分配IP地址；
3. IPv6 包头包首部长度采用固定的值 40 字节，去掉了包头校验和，简化了首部结构，减轻了路由器负荷，大大提高了传输的性能。
4. IPv6 有应对伪造 IP 地址的网络安全功能以及防止线路窃听的功能，大大提升了安全性。


## DHCP
DHCP 客户端进程监听的是 68 端口号，DHCP 服务端进程监听的是 67 端口号。
获取IP的过程：
1. 客户端发送dhcp发现报文，使用UDP广播报文（源地址0.0.0.0，源端口为68，目的地址为255.255.255.255，目的端口为67）；
2. dhcp服务端收到报文后用DHCP提供报文回应，该报文仍然使用 IP 广播地址 255.255.255.255，该报文信息携带服务器提供可租约的 IP 地址、子网掩码、默认网关、DNS 服务器以及 IP 地址租用期。
3. 客户端收到一个或多个服务器的 DHCP 提供报文后，从中选择一个服务器，并向选中的服务器发送 DHCP 请求报文（DHCP REQUEST进行响应，回显配置的参数。
4. 最后，服务端用 DHCP ACK 报文对 DHCP 请求报文进行响应，应答所要求的参数。


## ICMP
Internet Control Message Protocol，也就是互联网控制报文协议。
在 IP 通信中如果某个 IP 包因为某种原因未能达到目标地址，那么这个具体的原因将由 ICMP 负责通知。
1. 网络不可达Network Unreachable，路由表匹配不到接收方IP的网络号；
2. 主机不可达Host Unreachable，路由表没有该主机的信息，或者该主机没有连接到网络；
3. 协议不可达Protocol Unreachable，能找到对端的主机，但是但是对端主机禁止了TCP协议的访问；
4. 端口不可达Port Unreachable,对端主机没有进程监听此端口；
5. 超时消息ICMP Time Exceeded Message，IP 包中有一个字段叫做 TTL （Time To Live，生存周期），它的值随着每经过一次路由器就会减 1，直到减到 0 时该 IP 包会被丢弃。 此时，路由器将会发送一个 ICMP 超时消息给发送端主机，并通知该包已被丢弃。

traceroute <目的地址>    追踪去往目的地时沿途经过的路由；



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
4. docker启动的容器卷模式为ro，此时不能再文件夹内新建文件；除非在docker run时加上--privileged；
5. 

### 容器化技术在底层的运行原理
每个容器运行在它自己的命名空间中，但是，确实与其它运行中的容器共用相同的系统内核。
隔离的产生是由于系统内核清楚地知道命名空间及其中的进程，且这些进程调用系统api时，内核保证进程只能访问属于其命名空间中的资源。



## kubernetes k8s


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

#### Accept
客户端发送请求时，使用 Accept 字段声明自己可以接受哪些数据格式。
Accept: */*指的是客户端声明自己可以接受任何格式的数据。


#### Context-Encoding
Content-Encoding 字段说明数据的压缩方法。表示服务器返回的数据使用了什么压缩格式


### 错误码
504 错误码是网关超时错误，通常是nginx将请求代理到后端应用时，后端应用没有在规定的时间返回数据；代理服务器后面的后端服务器已经过载，它要处理的请求报文实在太多，忙不过来了。
502 网关错误
1. 后端服务端设置的超时时间过短,nginx转发后请求还没处理完成，就超时了
2. 看下后端服务端是否宕机或者有崩溃重启过 ps -ef|grep tomcat  如果是的话，可以分析崩溃堆栈日志找出具体原因（如果没有日志，看下是否可能是内存泄露导致进程占用内存越来越多，最后导致超过服务器的最大内存限制，触发OOM，或者是其他原因导致进程主动退出。）
1）通过监控排查服务端应用是否发生过崩溃重启（对服务端的cpu或者内存做过监控，可以看下CPU或内存的监控图是否出现过断崖式的突然下跌。如果有，十有八九百，就是你的服务端应用程序曾经崩溃过）
2）或者ps -o lstart {pid} 看下进程上次的启动时间是什么时候 ，这个时间如果跟你印象中的操作时间有差距，那说明进程可能是崩了之后被重新拉起了。
3. 如果进程没崩溃过，去排查下nginx的日志，看下是否将请求打到了某个不知名IP端口上。（看下nginx配置转发服务端，ip和端口是否请求的是正确地址）
https://juejin.cn/post/7155280646112280613
4. 检查网络连接和防火墙（nginx服务器上telnet后端服务器ip和端口看是否可以访问通）





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

sar -n DEV，显示网口的统计数据；
sar -n EDEV，显示关于网络错误的统计数据；
sar -n TCP，显示 TCP 的统计数据


## keepalived
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


## 磁盘
fdisk---磁盘分区的工具
fdisk -l显示磁盘分区表
fdisk /dev/sda -l显示磁盘设备sda的详情


## 文件描述符
1. 每个文件描述符都会与一个打开的文件相对应；
2. 不同的文件描述符可能指向同一个文件；
3. 相同的文件可以被不同的进程打开，也可以在一个进程中被打开多次；



## ldap
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



## 内存

### 内存分段
程序是由若干个逻辑分段组成的，如可由代码分段、数据分段、栈段、堆段组成。不同的段是有不同的属性的，所以就用分段（Segmentation）的形式把这些段分离出来。

出现内存碎片：
由于每个段的长度不固定，所以多个段未必能恰好使用所有的内存空间，会产生了多个不连续的小物理内存，导致新的程序无法被装载，所以会出现外部内存碎片的问题。

解决内存碎片：
在 Linux 系统里，也就是我们常看到的 Swap 空间，这块空间是从硬盘划分出来的，用于内存与硬盘的空间交换。
过程就是将某一段的内存写到硬盘上（Swap空间），然后再从硬盘读回到内存，在读回内存时会紧紧跟着被占用的区域，这样可以将碎片连续从而让别的程序转载这些碎片区域。


### 内存分页
分页是把整个虚拟和物理内存空间切成一段段固定尺寸的大小。这样一个连续并且尺寸固定的内存空间，我们叫页（Page）。在 Linux 下，每一页的大小为 4KB。

因为内存分页机制分配内存的最小单位是一页，即使程序不足一页大小，我们最少只能分配一个页，所以页内会出现内存浪费，所以针对内存分页机制会有内部内存碎片的现象。


### 多级页表
将页表（一级页表）分为 1024 个页表（二级页表），每个表（二级页表）中包含 1024 个「页表项」，形成二级分页。再吧二级分页推广到多级分页；
一级页表覆盖到了全部虚拟地址空间，二级页表在需要时创建。

### TLB
translation lookaside buffer，通常成为页表缓存、地址旁路缓存、快表等；
把最常访问的几个页表项存储到访问速度更快的硬件，于是计算机科学家们，就在 CPU 芯片中，加入了一个专门存放程序最常访问的页表项的 Cache，这个 Cache 就是 TLB。
在 CPU 芯片里面，封装了内存管理单元（Memory Management Unit）芯片，它用来完成地址转换和 TLB 的访问与交互。
有了 TLB 后，那么 CPU 在寻址时，会先查 TLB，如果没找到，才会继续查常规的页表。


### 虚拟内存
应用程序通过 malloc 函数申请内存的时候，实际上申请的是虚拟内存，此时并不会分配物理内存。
当应用程序读写了这块虚拟内存，CPU 就会去访问这个虚拟内存， 这时会发现这个虚拟内存没有映射到物理内存， CPU 就会产生缺页中断，进程会从用户态切换到内核态，并将缺页中断交给内核的 Page Fault Handler （缺页中断函数）处理。
缺页中断处理函数会看是否有空闲的物理内存：
如果有，就直接分配物理内存，并建立虚拟内存与物理内存之间的映射关系。
如果没有空闲的物理内存，那么内核就会开始进行回收内存 (opens new window)的工作，如果回收内存工作结束后，空闲的物理内存仍然无法满足此次物理内存的申请，那么内核就会放最后的大招了触发 OOM （Out of Memory）机制。
32 位系统的内核空间占用 1G，位于最高处，剩下的 3G 是用户空间；
64 位系统的内核空间和用户空间都是 128T，分别占据整个内存空间的最高和最低处，剩下的中间部分是未定义的。

内存溢出(Out Of Memory，简称OOM)是指应用系统中存在无法回收的内存或使用的内存过多，最终使得程序运行要用到的内存大于能提供的最大内存。此时程序就运行不了，系统会提示内存溢出。

### swap机制
当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间会被临时保存到磁盘，等到那些程序要运行时，再从磁盘中恢复保存的数据到内存中。

另外，当内存使用存在压力的时候，会开始触发内存回收行为，会把这些不常访问的内存先写到磁盘中，然后释放这些内存，给其他更需要的进程使用。再次访问这些内存时，重新从磁盘读入内存就可以了。

这种，将内存数据换出磁盘，又从磁盘中恢复数据到内存的过程，就是 Swap 机制负责的。

Swap 就是把一块磁盘空间或者本地文件，当成内存来使用，它包含换出和换入两个过程：

换出（Swap Out） ，是把进程暂时不用的内存数据存储到磁盘中，并释放这些数据占用的内存；
换入（Swap In），是在进程再次访问这些内存的时候，把它们从磁盘读到内存中来；

使用 Swap 机制优点是，应用程序实际可以使用的内存空间将远远超过系统的物理内存。由于硬盘空间的价格远比内存要低，因此这种方式无疑是经济实惠的。当然，频繁地读写硬盘，会显著降低操作系统的运行速率，这也是 Swap 的弊端。


Linux提供了两种方法启用Swap，分别是Swap分区和Swap文件；

### Linux操作系统的缓存
在应用程序读取文件的数据的时候，Linux操作系统会对读取的文件数据进行缓存，会缓存在文件系统中的Page Cache（页缓存）；
Page Cache 属于内存空间里的数据，由于内存访问比磁盘访问快很多，在下一次访问相同的数据就不需要通过磁盘 I/O 了，命中缓存就直接返回数据即可。
因此，Page Cache 起到了加速访问数据的作用。

#### 预读机制
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


#### 缓存污染
还是使用「只要数据被访问一次，就将数据加入到活跃 LRU 链表头部（或者 young 区域）」这种方式的话，那么还存在缓存污染的问题。
当我们在批量读取数据的时候，由于数据被访问了一次，这些大量数据都会被加入到「活跃 LRU 链表」里，然后之前缓存在活跃 LRU 链表（或者 young 区域）里的热点数据全部都被淘汰了，如果这些大量的数据在很长一段时间都不会被访问的话，那么整个活跃 LRU 链表（或者 young 区域）就被污染了。

只要我们提高进入到活跃 LRU 链表（或者 young 区域）的门槛，就能有效地保证活跃 LRU 链表（或者 young 区域）里的热点数据不会被轻易替换掉。
Linux 操作系统和 MySQL Innodb 存储引擎分别是这样提高门槛的：
Linux 操作系统：在内存页被访问第二次的时候，才将页从 inactive list 升级到 active list 里。
MySQL Innodb：在内存页被访问第二次的时候，并不会马上将该页从 old 区域升级到 young 区域，因为还要进行停留在 old 区域的时间判断：
如果第二次的访问时间与第一次访问的时间在 1 秒内（默认值），那么该页就不会被从 old 区域升级到 young 区域；
如果第二次的访问时间与第一次访问的时间超过 1 秒，那么该页就会从 old 区域升级到 young 区域；

#### 程序局部性原理
时间局部性和空间局部性。时间局部性是指如果程序中的某条指令一旦执行，则不久之后该指令可能再次被执行；如果某块数据被访问，则不久之后该数据可能再次被访问。空间局部性是指一旦程序访问了某个存储单元，则不久之后，其附近的存储单元也将被访问。





## 进程间通信
### 管道
mkfifo myPipe    # 创建管道，在目录内生成一个文件，文件类型是p
echo test > myPipe    # 往管道内写数据,执行命令停在这里不动了，这是因为管道内的数据没有被读取；
cat < myPipe          # 读取管道内的数据，另一方面上面的echo也正常退出了。


## shell
1. 格式化json：cat test.json |python -m json.tool；



## pdcp
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



## 搭建本地yum源供其它主机rpm包安装
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



## slurm
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



### 场景
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



### slurm安装
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




## terraform

### terraform安装
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform


### 通过terraform编排openstack
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






## 概念
### kubeflow
它提供了一套工具和组件，用于构建、训练、部署和管理机器学习模型的端到端工作流程。

### rancher
Rancher提供了在生产环境中使用的管理Docker和Kubernetes的全栈化容器部署与管理平台。
也就是提供可视化web界面进行k8s部署；

### CRD
它代表自定义资源定义（Custom Resource Definition）。CRD 允许用户扩展 Kubernetes API，以添加自定义资源和自定义控制器。
Kubernetes 中的资源（Resource）是 API 对象的实例，例如 Pod、Service、Deployment 等。这些资源都有相应的 API 定义和控制器，用于管理它们的生命周期和状态。
CRD 允许用户定义自己的资源类型，这些资源类型可以扩展 Kubernetes 的功能，以满足特定的需求。用户可以创建自定义资源定义，定义自己的资源结构和行为，并编写自定义控制器来管理这些资源。

### HPC
高性能计算（High Performance Computing，缩写HPC）指利用聚集起来的计算能力来处理标准工作站无法完成的数据密集型的计算任务。

### tensorflow
TensorFlow是一个基于数据流编程的符号数学系统，被广泛应用于各类机器学习算法的编程实现。
PS-worker模型：Parameter Server执行模型相关业务，Work Server训练相关业务，推理计算、梯度计算等。

### NPU
神经处理单元（Neural Processing Unit）的缩写，它是一种专门设计用于进行人工神经网络计算的处理器。NPU 的设计旨在加速深度学习任务，包括图像识别、语音识别、自然语言处理等


### volcano
基础设施调度引擎，基于Kubernetes的容器批量计算平台，主要用于高性能计算场景。
作为一个通用批处理平台，Volcano与几乎所有的主流计算框架无缝对接，如Spark 、TensorFlow 、PyTorch 、 Flink 、Argo 、MindSpore、 PaddlePaddle等。

如何查看Volcano scheduler的配置
kubectl get configmap -n volcano-system
kubectl get configmap volcano-scheduler-configmap -n volcano-system -oyaml


#### queue
Queue 是一个 PodGroup 队列，PodGroup 是一组强关联的 Pod 集合。而 VolcanoJob 则是一个 K8s Job 升级版，对应的下一级资源是 PodGroup。换言之，就好比 ReplicaSet 的下一级资源是 Pod 一样。
1. queue名称不能重复；
2. 名称限制：a lowercase RFC 1123 subdomain must consist of lower case alphanumeric characters, '-' or '.', and must start and end with an alphanumeric character (e.g. 'example.com', regex used for validation is '[a-z0-9]([-a-z0-9]*[a-z0-9])?(\.[a-z0-9]([-a-z0-9]*[a-z0-9])?)*'
3. Capability: 表示该queue中所有podgroup使用资源之和的上限，它是个硬约束；
4. Guarantee: 表示队列的预留资源；
5. Reclaimable: 表示该queue在资源使用量超过该queue所应得的资源份额时，是否允许其它queue回收该queue使用超额的资源，默认true；
6. Weight: 表示该queue在集群资源划分中所占的相对比重；
7. Status: podgroup统计信息与queue状态信息；



#### podgroup
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



#### vcjob
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





#### 调度GPU

##### k8s中使用GPU
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



