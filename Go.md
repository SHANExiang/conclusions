
# Go编译运行
go 源代码首先要通过 go build 编译为可执行文件，在 linux 平台上为 ELF 格式的可执行文件，编译阶段会经过编译器、汇编器、链接器三个过程最终生成可执行文件。
1、编译器：.go 源码通过 go 编译器生成为 .s 的 plan9 汇编代码；
2、汇编器：通过 go 汇编器将编译器生成的 .s 汇编语言转换为机器代码，并写出最终的目标程序.o 文件；
3、链接器：汇编器生成的一个个 *.o 目标文件通过链接处理得到最终的可执行程序；

go 源码通过上述几个步骤生成可执行文件后，二进制文件在被操作系统加载起来运行时会经过如下几个阶段：
1、从磁盘上把可执行程序读入内存；
2、创建进程和主线程；
3、为主线程分配栈空间；
4、把由用户在命令行输入的参数拷贝到主线程的栈；
5、把主线程放入操作系统的运行队列等待被调度执起来运行；


### Go在windows环境下build生成linux程序
go env -w GOARCH=amd64
go env -w GOOS=linux

windows就是
go env -w GOOS=linux



## Go module
1. 查看开启情况，go env GO111MODULE；
2. 开启---go env -w GO111MODULE="on"；
3. 设置Go proxy---go env -w GOPROXY=http://goproxy.cn；
4. 查看GOMODECACHE；
5. 设置GOMODECACHE---go env -w GOMODCACHE=$GOPATH/pkg/mod；
6. GOPATH模式下 
    go get拉取外部依赖包会自动下载并安装到GOPATH下；
    没有版本控制的概念，无法知道当前更新的是哪个版本；
7. 进行Go module的初始化---go mod init <module-repo>；
8. 初始化项目时，会生成go.mod文件，描述当前项目的元信息；
9. 第一次拉取模块依赖后，生成go.sum，详细罗列当前项目依赖的模块版本；
10. go get过程分为三步，finding(发现)-->downloading(下载)-->extracting(提取)；
11. go mod init 项目路径 



## Go panic
1. 数组/索引索引越界；index out of range
2. 空指针调用；invalid memory address or nil pointer dereference
3. 过早关闭http响应体；指的是resp可能为nil；
4. 除以0；integer divide by zero；
5. 向已关闭的通道发消息；send on closed channel；
6. 重复关闭通道；close of closed channel；
7. 关闭未初始化通道；close of nil channel；
8. 未初始化map；assignment to entry in nil map；
9. 跨协程的panic处理，指的是一个协程panic，在另一个协程中recover；
10. sync计数为负值，指的是WaitGroup add方法传负值；negative WaitGroup counter；

## new 和 make 的区别？
make 
make(T, args) 返回的是初始化之后的 T 类型的值，这个新值并不是 T 类型的零值，也不是指针*T，是经过初始化之后的 T 的引用。
make 也是内建函数； 
slice := make([]int, 0, 100) 
hash := make(map[int]bool, 10) 
ch := make(chan int, 5) 
make 只能用于 slice,map,channel 三种类型, 并且只能是这三种对象  

new 
i := new(int) 
// 两者等价 
var v int 
i := &v 
new(T)为一个 T 类型新值分配空间并将此空间初始化为 T 的零值,返回的是新值
的地址,也就是 T 类型的指针*T,该指针指向 T 的新分配的零值； 
 
make 和 new 的区别 
1. new(T) 返回 T 的指针*T 并指向 T 的零值； 
2. make(T)返回的初始化的 T，只能用于 slice、map、channel，要获得一个显式的指针，使用 new 进行分配，或者显式地使用一个变量的地址； 
3. new 函数分配内存，make 函数初始化。 


## 什么是协程（Goroutine）与线程区别 
协程是用户态轻量级线程，它是线程调度的基本单位。通常在函数前加上 go 关键字就能实现并发。 
1. 调度方式不同： 
Go 语言的协程是由 Go 语言的运行时（runtime）进行调度的，而线程是由操作系统进行调度的。
Go 语言的协程调度器采用的是 M:N 调度模型，即将 M 个协程映射到 N 个操作系统线程上执行，这样可以更高效地利用 CPU 资源。 
2. 内存占用：
一个 Goroutine 会以一个很小的栈启动 2KB 或 4KB，当遇到栈空间不足时，栈会自动伸缩，因此可以轻易实现成千上万个 goroutine 同时启动。
独立的栈空间、 共享程序堆空间、调度由用户控制、协程是轻量级的线程。同一个程序中的所有goroutine 共享同一个地址空间。 
系统线程都会有一个固定大小的栈大小 2M，这个栈保存函数递归调用时的参数和局部变量，
固定大小出现两个问题，需要栈空间小时是浪费，需要栈空间较大时会溢出；
3. 切换代价不同： 
线程的切换需要保存和恢复线程的上下文，包括寄存器、栈指针等，这个过程需
要耗费一定的时间和资源。而协程的切换则只需要保存和恢复协程的上下文，这个过程非常快速，通常只需要几百纳秒。 
适用场景： 
由于协程可以快速切换并发执行的任务，因此比线程更适合处理 I/O 密集型的任务。 
线程则更适合处理 CPU 密集型任务。在 CPU 密集型任务下，Go 协程的优势要小很多，甚至可能更差。 


## 讲一下 Go 面向对象是如何实现的？ ok 
Go 实现面向对象的两个关键是 struct 和 interface。 
封装：对于同一个包，对象对包内的文件可见；对不同的包，需要将对象以大写开头才是可见的。 
继承：继承是编译时特征，在 struct 内加入所需要继承的类即可： 
type A struct{} 
type B struct{
    A 
}

组合
var _ Codec = (*GobCodec)(nil)

多态：多态是运行时特征，Go 多态通过 interface 来实现。类型和接口是松耦合
的，某个类型的实例可以赋给它所实现的任意接口类型的变量。 
Go 支持多重继承，就是在类型中嵌入所有必要的父类型。 


## map 的底层实现 
源码位于 src\runtime\map.go 中。 
go 的 map 和 C++map 不一样，底层实现是哈希表，包括两个部分：hmap 和 bucket。 
里面最重要的是 buckets（桶），buckets 是一个指针，最终它指向的是一个结构体： 
// A bucket for a Go map. 
type bmap struct { 
    tophash [bucketCnt]uint8 
} 
每个 bucket 固定包含 8 个 key 和 value(可以查看源码 bucketCnt=8)。
实现上面是一个固定的大小连续内存块，分成四部分：每个条目的状态，8 个 key 值，8 个 value 值，指向下个 bucket 的指针。 
创建哈希表使用的是 makemap 函数。map 的一个关键点在于，哈希函数的选择。 
在程序启动时，会检测 cpu 是否支持 aes，如果支持，则使用 aes hash，否则使用 memhash。这是在函数 alginit() 中完成，位于路径：src/runtime/alg.go 下。 
map 查找就是将 key 哈希后得到 64 位（64 位机）用最后 B 个比特位计算在哪个桶。
在 bucket 中，从前往后找到第一个空位。这样，在查找某个 key 时，先找到对应的桶，再去遍历 bucket 中的 key。 
关于 map 的查找和扩容可以参考 map 的用法到 map 底层实现分析。 


## 切片slice扩容策略
在 Go 语言中，切片的扩容是通过 append 函数实现的。
当我们向一个切片中添加元素时，如果当前切片的容量不足以存储新的元素，Go 会自动为该切片分配一块新的内存空间，
然后将原来的数据复制到新分配的内存空间中，并将新的元素添加进去。这个过程被称为切片的扩容。 
具体来说，当我们使用 append 函数向一个切片添加元素时，Go 会先判断当前切片的容量是否足够存储新元素。
如果足够，则直接将新元素添加到切片的末尾；如果不足，则需要进行扩容操作。 
在进行扩容操作时，Go 会先根据当前切片的长度和容量计算出新的容量。

具体的计算方式是，1.17 之前 
代码的扩容策略可以简述为以下三个规则： 
1.当期望容量>两倍的旧容量时，直接使用期望容量作为新切片的容量 
2.如果旧容量< 1024（注意这里单位是元素个数）,那么直接翻倍旧容量 
3.如果旧容量> 1024，那么会进入一个循环，每次增加 25%直到大于期望容量 
可以看到，原来的 go 对于切片扩容后的容量判断有一个明显的 magic number：1024，
在 1024 之前，增长的系数是 2，而 1024 之后则变为 1.25。关于为什么会这么设计，社区的相关讨论 1 给出了几点理由： 
1.如果只选择翻倍的扩容策略，那么对于较大的切片来说，现有的方法可以更好的节省内存。 
2.如果只选择每次系数为 1.25 的扩容策略，那么对于较小的切片来说扩容会很低效。 
3.之所以选择一个小于 2 的系数，在扩容时被释放的内存块会在下一次扩容时更容易被重新利用 

1.18 之后 
到了 Go1.18 时，又改成不和 1024 比较了，而是和 256 比较；
并且扩容的增量也有所变化，不再是每次扩容 1/4，而是（oldCap + 3 * 256）/4； 
然后，Go 会为新的容量分配一块内存空间，并将原来的数据复制到这个新的内存空间中，最后将新的元素添加到切片的末尾。 
需要注意的是，切片的扩容操作可能会导致原来的切片和新的切片共享同一块内存空间。
这种情况下，如果修改了原来的切片或新的切片中的数据，都会影响到另一个切片。
因此，在使用切片时，需要注意切片的扩容和共享内存的情况，避免引发不必要的错误。


 
## go 里面的 map 是并发安全的吗？如何并发安全 
Go 中的 map 是非并发安全的，多个 goroutine 对同一个 map 进行并发读写操作时可能会导致数据竞争和并发问题。
为了保证 map 的并发安全性，可以使用以下两种方式：
1. 使用 sync 包中的 Map 类型 
sync 包中的 Map 类型是一个并发安全的 map，可以用于多个 goroutine 对同一个 map进行并发读写操作。例如： 
```shell
var m sync.Map 
 
m.Store("key", "value") 
value, ok := m.Load("key") 
func main() { 
 
barVal := map[string]int{ 
"alpha": 34, "bravo": 56, "charlie": 23, 
"delta": 87, "echo": 56, "foxtrot": 12, 
"golf": 34, "hotel": 16, "indio": 87, 
"juliet": 65, "kili": 43, "lima": 98, 
} 
 
i := 0 
 
keys := make([]string, len(barVal)) 
 
for k, _ := range barVal{ 
keys[i] = k 
i++ 
} 
 
sort.Strings(keys) 
 
fmt.Println("sorted keys==", keys) 
 
for _, key := range keys{ 
fmt.Printf("key=%v, value=%v\n", key, barVal[key]) 
} 
} 
m.Delete("key") 

```
上面的代码使用 sync.Map 类型实现了一个并发安全的 map，可以在多个 goroutine 中对
其进行并发读写操作。 

2. 使用读写锁进行保护 
另一种方式是使用读写锁（sync.RWMutex）对 map 进行保护，实现并发安全。例如： 
var mu sync.RWMutex 
var m map[string]string 
 
func read(key string) string { 
    mu.RLock() 
    defer mu.RUnlock() 
    return m[key] 
} 
 
func write(key, value string) { 
    mu.Lock() 
    defer mu.Unlock() 
    m[key] = value 
} 
上面的代码使用读写锁对 map 进行保护，read 函数使用读锁进行保护，write 函数使用写
锁进行保护，可以在多个 goroutine 中对其进行并发读写操作。 
 
需要注意的是，使用读写锁进行保护时，需要根据实际情况选择读锁或写锁，以保证并发安
全和性能的平衡。同时，也可以使用 sync.Map 类型来简化代码，避免使用锁时出现的一些
问题，例如死锁等。 
 
解决 hash 碰撞的方法 
1、开放寻址法； 
这种方法的核心思想是依次探测和比较数组中的元素以判断目标键值对是否存
在于哈希表中。实现哈希表的底层数据接口是数组。 
哈希函数：index := hash(“key1”) % array.len 
当我们向当前哈希表写入新的数据时，如果发生了冲突，就会将键值对写入到下
一个索引不为空的位置，如果位置已经被占用了，则继续向下寻找，如果找到最
后还是被占用的话，则从头开始寻找位置。 
2、拉链法。 
实现哈希表的底层数据结构是链表数组。 
哈希函数：index := hash(“key1”) % array.len 
经过 hash 函数找到一个桶，然后就可以遍历当前桶的链表了，在遍历链表的过
程中会遇到以下两种情况： 
a、找到键相同的键值对 — 更新键对应的值； 
b、没有找到键相同的键值对 — 在链表的末尾追加新的键值对； 



## 链式调用
链式调用是一种简化代码的编程方式，能够使代码更简洁、易读。链式调用的原理也非常简单，某个对象调用某个方法后，
将该对象的引用/指针返回，即可以继续调用该对象的其他方法。通常来说，当某个对象需要一次调用多个方法来设置其属性时，
就非常适合改造为链式调用了。


## Hook机制
Hook，翻译为钩子，其主要思想是提前在可能增加功能的地方埋好(预设)一个钩子，当我们需要重新修改或者增加这个地方的逻辑的时候，
把扩展的类或者方法挂载到这个点即可。钩子的应用非常广泛，例如 Github 支持的 travis 持续集成服务，当有 git push 事件发生时，
会触发 travis 拉取新的代码进行构建。IDE 中钩子也非常常见，比如，当按下 Ctrl + s 后，自动格式化代码。
再比如前端常用的 hot reload 机制，前端代码发生变更时，自动编译打包，通知浏览器自动刷新页面，实现所写即所得。


## orm
对象关系映射（Object Relational Mapping，简称ORM）是通过使用描述对象和数据库之间映射的元数据，将面向对象语言程序中的对象自动持久化到关系数据库中。
数据库表迁移
新增字段  ALTER TABLE tableName ADD COLUMN col_name, col_type;
删除字段：
CREATE TABLE new_table AS SELECT col1, col2, ... from old_table   #从old_table中挑选需要保留的字段到 new_table 中。
DROP TABLE old_table                                              # 删除 old_table                    
ALTER TABLE new_table RENAME TO old_table;                        # 重命名 new_table 为 old_table

1. Session用于实现与数据的交互； 
    直接调用SQL语句进行原生交互RAW; 数据库表的增删；
2. Engine负责与数据库交互前的准备(连接和测试数据库)和交互后的收尾工作(关闭连接)；
3. orm框架往往需要兼容多个数据库，每一种数据库分别实现，实现最大程度的复用和解耦；Dialect接口,其它数据库实现它；
4. schema定义数据库表接口与对象映射,即对象object和表table的转换，给定一个任意对象，转换为关系型数据库中的表结构；
    表名-结构体/字段名和类型-成员变量和类型/额外约束条件(非空主键)-成员变量的Tag；
5. 使用反射(reflect)将数据库的记录转换为对应的结构体实例，实现查询(select)功能。
    Insert需要将已经存在的对象的每一个字段的值平铺开来，而 Find 则是需要根据平铺开的字段的值构造出对象；
6. 实现事务；
7. 钩子与结构体绑定，即每个结构体需要实现各自的钩子；
8. 数据库迁移；支持字段的新增和删除；
    ALTER TABLE table_name ADD COLUMN col_name, col_type;
    CREATE TABLE new_table AS SELECT col1, col2, ... from old_table DROP TABLE old_table ALTER TABLE new_table RENAME TO old_table;

# gorm

## gorm 加上 logger 
go get –u gorm.io/gorm/logger 
newLogger := logger.New( 
   log.New(os.Stdout, "\r\n", log.LstdFlags), 
   logger.Config{ 
      SlowThreshold: time.Second, //慢 SQL 阈值 
      LogLevel: logger.Info, //日志级别 
      Colorful: true, //彩色 
   }) 
 
DB, _ = gorm.Open(mysql.Open(viper.GetString("mysql.dsn")), &gorm.Config{Logger: newLogger}) 
访问接口，终端会有 sql 语句打印；

 
## gorm 里面如何批量插入某个字段 
在 GORM 中，可以使用 CreateInBatches 方法来批量插入数据。
如果要批量插入某个字段，可以先构造一个包含该字段的结构体切片，然后使用CreateInBatches 方法进行批量插入。 

下面是一个示例代码： 
type User struct { 
    ID   uint 
    Name string 
    Age  int 
    City string 
} 
 
// 构造包含 City 字段的结构体切片 
users := []User{ 
    {Name: "Alice", Age: 20, City: "New York"}, 
    {Name: "Bob", Age: 25, City: "Los Angeles"}, 
    {Name: "Charlie", Age: 30, City: "Chicago"}, 
} 
 
// 批量插入数据，只插入 City 字段 
result := db.CreateInBatches(users, len(users)).Select("City") 
 
if result.Error != nil { 
    // 处理错误 
} 
 
// 输出插入的记录数 
fmt.Printf("Inserted %d records\n", result.RowsAffected) 
在上面的代码中，我们先构造了一个包含 City 字段的结构体切片，然后使用 
CreateInBatches 方法进行批量插入。在调用 CreateInBatches 方法时，我们指定
了要插入的记录数和要选择的字段（即只插入 City 字段）。最后，我们可以通
过 result.RowsAffected 获取插入的记录数。 
 
## gorm 里面更新有几种方式？ 
1. 使用 Update 方法更新单条记录 
Update 方法可以用于更新单条记录，它接收一个结构体或 map 参数，表示要更 新的字段和值。例如： 
db.Model(&User{}).Where("id = ?", 1).Update("name", "Alice") 
上面的代码会将 id 为 1 的 User 记录的 name 字段更新为 "Alice"。 
2. 使用 Updates 方法更新单条记录 
Updates 方法可以用于更新单条记录，它接收一个结构体或 map 参数，表示要更新的多个字段和值。例如： 
db.Model(&User{}).Where("id = ?", 1).Updates(map[string]interface{}{"name": "Alice", "age": 20}) 
上面的代码会将 id 为 1 的 User 记录的 name 字段更新为 "Alice"，age 字段 更新为 20。 
3. 使用 UpdateColumn 方法更新单个字段 
UpdateColumn 方法可以用于更新单个字段，它接收两个参数，第一个参数表示要更新的字段名，第二个参数表示要更新的值。例如： 
db.Model(&User{}).Where("id = ?", 1).UpdateColumn("name", "Alice")
上面的代码会将 id 为 1 的 User 记录的 name 字段更新为 "Alice"。 
4. 使用 UpdateColumns 方法更新多个字段 
UpdateColumns 方法可以用于更新多个字段，它接收一个结构体或 map 参数，
表示要更新的多个字段和值。与 Updates 方法不同的是，UpdateColumns 方法只
更新指定的字段，不会更新其他字段。例如： 
db.Model(&User{}).Where("id = ?", 1).UpdateColumns(map[string]interface{}{"name": "Alice", "age": 20}) 
上面的代码会将 id 为 1 的 User 记录的 name 字段更新为 "Alice"，age 字段更新为 20。其他字段不会被更新。 
5. 使用 Save 方法更新记录 
Save 方法可以用于更新记录，它接收一个结构体参数，表示要更新的记录。如
果该记录的主键已经存在，则会更新该记录；否则会插入一条新记录。例如： 
user := User{ID: 1, Name: "Alice", Age: 20} 
db.Save(&user) 
上面的代码会将 id 为 1 的 User 记录的 name 字段更新为 "Alice"，age 字段
更新为 20。如果该记录不存在，则会插入一条新记录。 
需要注意的是，以上方法在执行时都会生成 SQL 语句并执行，可以通过 
db.Debug().Updates() 等方法查看生成的 SQL 语句。 


# Gin
框架提供的特性， 
1. 路由(Routing)：将请求映射到函数，支持动态路由。例如'/hello/:name； 
2. 模板(Templates)：使用内置模板引擎提供模板渲染机制； 
3. 工具集(Utilites)：提供对 cookies，headers 等处理机制； 
4. 插件(Plugin)：Bottle 本身功能有限，但提供了插件机制。可以选择安装到全局，也可以只针对某几个路由生效。

## 前缀树
动态路由可以匹配某一类型路由而非一条固定的路由；例如/hello/:name，可以匹配/hello/geektutu、hello/jack等。
实现动态路由最常用的数据结构，被称为前缀树；

HTTP请求的路径恰好是由/分隔的多段构成的，因此，每一段可以作为前缀树的一个节点。
我们通过树结构查询，如果中间某一层的节点都不满足条件，那么就说明没有匹配到的路由，查询结束。

对于路由来说，最重要的当然是注册与匹配了。开发服务时，注册路由规则，映射handler；访问时，匹配路由规则，查找到对应的handler。
因此，Trie 树需要支持节点的插入与查询。插入功能很简单，递归查找每一层的节点，如果没有匹配到当前part的节点，则新建一个，
有一点需要注意，/p/:lang/doc只有在第三层节点，即doc节点，pattern才会设置为/p/:lang/doc。p和:lang节点的pattern属性皆为空。
因此，当匹配结束时，我们可以使用n.pattern == ""来判断路由规则是否匹配成功。
例如，/p/python虽能成功匹配到:lang，但:lang的pattern值为空，因此匹配失败。
查询功能，同样也是递归查询每一层的节点，退出规则是，匹配到了*，匹配失败，或者匹配到了第len(parts)层节点。


### 路由
#### 路由分组
defaultRouter := r.Group("/") 
{ 
   defaultRouter.GET("/", func(context *gin.Context) 
{ 
      context.String(http.StatusOK, "值%v", "您好 gin") 
   }) 
   defaultRouter.GET("/new", func(context 
*gin.Context) { 
      context.String(http.StatusOK, "值%v", "您好 gin") 
   }) 
}

#### gin定义路由多种方式 
在 Gin 中，路由是指将请求的 URL 映射到相应的处理函数上。Gin 提供了多种方式来定义路由，包括： 
1. GET 方法 
使用 GET 方法可以定义 GET 请求的路由，例如： 
router.GET("/hello", func(c *gin.Context) { 
    c.String(http.StatusOK, "Hello, world!") 
}) 
上面的代码定义了一个 GET 请求的路由，当请求的 URL 为 "/hello" 时，会调
用后面的处理函数返回 "Hello, world!"。 
2. POST 方法 
使用 POST 方法可以定义 POST 请求的路由，例如： 
router.POST("/users", func(c *gin.Context) { 
    // 处理 POST 请求 
}) 
上面的代码定义了一个 POST 请求的路由，当请求的 URL 为 "/users" 时，会
调用后面的处理函数处理 POST 请求。 
3. PUT 方法 
使用 PUT 方法可以定义 PUT 请求的路由，例如： 
router.PUT("/users/:id", func(c *gin.Context) { 
    // 处理 PUT 请求 
}) 
上面的代码定义了一个 PUT 请求的路由，当请求的 URL 为 "/users/:id" 时，
会调用后面的处理函数处理 PUT 请求。其中，":id" 表示一个参数，可以通过 
c.Param("id") 获取该参数的值。 
4. DELETE 方法 
使用 DELETE 方法可以定义 DELETE 请求的路由，例如： 
router.DELETE("/users/:id", func(c *gin.Context) { 
    // 处理 DELETE 请求 
}) 
上面的代码定义了一个 DELETE 请求的路由，当请求的 URL 为 "/users/:id" 
时，会调用后面的处理函数处理 DELETE 请求。其中，":id" 表示一个参数，可
以通过 c.Param("id") 获取该参数的值。 
5. PATCH 方法 
使用 PATCH 方法可以定义 PATCH 请求的路由，例如： 
router.PATCH("/users/:id", func(c *gin.Context) { 
    // 处理 PATCH 请求 
}) 
上面的代码定义了一个 PATCH 请求的路由，当请求的 URL 为 "/users/:id" 时，
会调用后面的处理函数处理 PATCH 请求。其中，":id" 表示一个参数，可以通
过 c.Param("id") 获取该参数的值。 
6. Any 方法 
使用 Any 方法可以定义任何请求方法的路由，例如： 
router.Any("/users/:id", func(c *gin.Context) { 
    // 处理任何请求方法 
}) 
上面的代码定义了一个任何请求方法的路由，当请求的 URL 为 "/users/:id" 时，
会调用后面的处理函数处理所有请求方法。其中，":id" 表示一个参数，可以通
过 c.Param("id") 获取该参数的值。 
7. Group 方法 
使用 Group 方法可以将多个路由分组，例如： 
users := router.Group("/users") 
{ 
    users.GET("/", func(c *gin.Context) { 
        // 处理 GET 请求 
    }) 
    users.POST("/", func(c *gin.Context) { 
        // 处理 POST 请求 
    }) 
    users.PUT("/:id", func(c *gin.Context) { 
        // 处理 PUT 请求 
    }) 
    users.DELETE("/:id", func(c *gin.Context) { 
        // 处理 DELETE 请求 
    }) 
} 
上面的代码将 "/users" 路由下的多个请求方法分组处理，可以更好地组织代码。 
8. Use 方法 
使用 Use 方法可以为路由添加中间件，例如： 
router.Use(authMiddleware) 
router.GET("/hello", func(c *gin.Context) { 
    c.String(http.StatusOK, "Hello, world!") 
}) 
上面的代码为 "/hello" 路由添加了一个 authMiddleware 中间件，可以在处理请
求前进行身份验证等操作。 
以上是 Gin 中定义路由的常用方式，可以根据实际需求选择适合的方式。 


#### 路由中间件 
路由中多个 HandlerFunc 参数，可以作为中间件； 
r.GET(“/”, InitMiddleWare, func(context *gin.Context){}) 
context.Next() ---调用该请求的剩余处理程序； 
context.Abort() ----终止该请求的剩余处理程序； 
r.use(middleWare1, middleWare2)  ---全局中间件 
r.Group()  ---可以在里面加路由分组中间件 


### swagger 使用 
在 Router 函数中加上 docs.SwaggerInfo.BasePath = ""
r.GET("/swagger/*any", ginSwagger.WrapHandler(swaggerFiles.Handler)) 
在对应 handler 上加上 swagger 注释，然后命令行执行 swag init，启动服务。 
浏览器访问http://localhost:8080/swagger/index.html 看到 swagger UI，
http://localhost:8080/swagger/doc.json 看到 json 样式； 


## GIN 怎么做参数校验 
go 采用 validator 作参数校验。 
它具有以下独特功能： 
1. 使用验证 tag 或自定义 validator 进行跨字段 Field 和跨结构体验证； 
2. 允许切片、数组和哈希表，多维字段的任何或所有级别进行校验； 
3. 能够对哈希表 key 和 value 进行验证； 
4. 通过在验证之前确定它的基础类型来处理类型接口； 
5. 别名验证标签，允许将多个验证映射到单个标签，以便更轻松地定义结构体上的验证； 
6. gin web 框架的默认验证器； 

## 你项目有优雅的启停吗？ 
所谓「优雅」启停就是在启动退出服务时要满足以下几个条件： 
1. 不可以关闭现有连接（进程） 
2. 新的进程启动并「接管」旧进程 
3. 连接要随时响应用户请求，不可以出现拒绝请求的情况 
4. 停止的时候，必须处理完既有连接，并且停止接收新的连接。 

为此我们必须引用信号来完成这些目的： 
启动： 
1. 监听 SIGHUP（在用户终端连接(正常或非正常)结束时发出）； 
2. 收到信号后将服务监听的文件描述符传递给新的子进程，此时新老进程同时接收请求； 

退出： 
1. 监听 SIGINT 和 SIGSTP 和 SIGQUIT 等。 
2. 父进程停止接收新请求，等待旧请求完成（或超时）； 
3. 父进程退出。 
 

### tips
jsonp 和 json 区别是，jsonp 可以传入回调函数，并且将参数传给回调函数；
context.XML(http.StatusOK, obj)  ---返回 xml 数据 



## 令牌桶
描述：有个固定大小的桶，系统会以恒定速率向桶中放入令牌，桶满则不放。请求到来时需要从桶中获取令牌才能执行，否则不能执行，要么阻塞，要么丢弃。

生产和消费令牌其实就是维护token数，token数是共享资源，可能会有多线程操作，需要加锁。

time/rate包的Limiter类型对限流器进行了定义：
limit：limit字段表示往桶里放Token的速率，它的类型是Limit，是int64的类型别名。设置limit时既可以用数字指定每秒向桶中放多少个Token，也可以指定向桶中放Token的时间间隔，其实指定了每秒放Token的个数后就能计算出放每个Token的时间间隔了。
burst: 令牌桶的大小。
tokens: 桶中的令牌。
last: 上次往桶中放 Token 的时间。
lastEvent：上次发生限速器事件的时间（通过或者限制都是限速器事件）

这个池子一开始容量为b，装满b个令牌，然后每秒往里面填充r个令牌。
由于令牌池中最多有b个令牌，所以一次最多只能允许b个事件发生，一个事件花费掉一个令牌。



# 并发编程
## goroutine
1. goroutine是由Go的运行时调度和管理的，在语言层面已经内置了调度和上下文切换的机制；
2. 操作系统线程有固定的栈内存（通常为2MB），一个goroutine的栈在其生命周期开始时只有很小的栈（2KB），goroutine的栈不是固定的，可以按需增加和缩小；
3. goroutine什么情况下会阻塞：
    a. 由于原子、互斥量或通道操作调用导致 Goroutine 阻塞;
    b. 


## GMP模型
G--->goroutine，拥有自己的栈空间，定时器，初始化时栈空间大小为2k，可随着需求增长；
M--->machine，操作系统线程，当一个 Goroutine 需要执行时，M 将会将其放入自己的执行队列中，并不断地从队列中取出 Goroutine 进行执行。
P--->processor，处理器，Go 运行时系统通常为每个可用的 CPU 核心分配一个 P。P 负责从全局的 Goroutine 队列中获取 Goroutine，并将其分配给可用的 M 执行。

M代表一个工作线程，在M上有一个P和G，P是绑定到M上的，G是通过P的调度获取的，在某一时刻，一个M上只有一个G（g0除外）。
在P上拥有一个G队列，里面是已经就绪的G，是可以被调度到线程栈上执行的协程，称为运行队列。
线程是运行goroutine的实体，调度器的功能是把可运行的goroutine分配到工作线程中。

全局队列（Global Queue）：存放等待运行的 G。
P 的本地队列：同全局队列类似，存放的也是等待运行的 G，存的数量有限，不超过 256 个。新建 G’时，G’优先加入到 P 的本地队列，
如果队列满了，则会把本地队列中一半的 G 移动到全局队列。
P 列表：所有的 P 都在程序启动时创建，并保存在数组中，最多有 GOMAXPROCS(可配置) 个。
M：线程想运行任务就得获取 P，从 P 的本地队列获取 G，P 队列为空时，M 也会尝试从全局队列拿一批 G 放到 P 的本地队列，
或从其他 P 的本地队列偷一半放到自己 P 的本地队列。M 运行 G，G 执行之后，M 会从 P 获取下一个 G，不断重复下去。


### 调度流程
1. go func()创建协程；
2. 放入到P的本地队列中；如果P的本地队列已满的话，放到全局队列中；
3. M从绑定的P的本地队列获取G执行；若M的P本地队列为空则从全局队列中取，当全局队列中也没有的话，从其它的MP组合中偷取G执行；这种从其它P偷的方式成为work stealing；
4. 调度；
5. 执行；若G的func()发送systemCall阻塞,创建一个新的M或从休眠M队列中取一个M,接管当前正在阻塞G的P；
    当G因channel或者Network I/O阻塞时，不会阻塞M，M会寻找其它runnable的G;当阻塞G恢复后重新进入runnable进入P队列等待执行；
6. 执行完后销毁G；
7. 返回给M；


## sync.RWMutex
它保护对内存的访问；你可以请求锁定进行读取，在这种情况下，你将被授予读取权限，除非锁定正在进行写入操作。
这意味着，只要没有别的东西占用写操作，任意数量的读取者就可以进行读取操作。
sync.RWMutex`类型有三个方法：
1. `func (rw *RWMutex) Lock()`: 获取写锁。如果有其他协程已经持有了写锁或读锁，那么当前协程会阻塞，直到其他协程释放锁。
2. `func (rw *RWMutex) Unlock()`: 释放写锁。如果当前协程没有持有写锁，该方法会引发 panic。
3. `func (rw *RWMutex) RLock()`: 获取读锁。如果有其他协程持有写锁，那么当前协程会阻塞，直到其他协程释放锁。
4. `func (rw *RWMutex) RUnlock()`: 释放读锁。如果当前协程没有持有读锁，该方法会引发 panic。
RWMutex 在读锁占用的情况下，会阻止写，但不阻止读RWMutex。 
在写锁占用情况下，会阻止任何其他Goroutine（无论读和写）进来，整个锁相当于由该 Goroutine独占。

### 实现原理
一个 writer goroutine 获得了内部的互斥锁，就会反转 readerCount 字段，把它从原来的正整数 readerCount(>=0)
修改为负数（readerCount - rwmutexMaxReaders），让这个字段保持两个含义（既保存了 reader 的数量，又表示当前有 writer）。
也就是说当readerCount为负数的时候表示当前writer goroutine持有写锁中，reader goroutine会进行阻塞。

当一个 writer 释放锁的时候，它会再次反转 readerCount 字段。可以肯定的是，因为当前锁由 writer 持有，
所以，readerCount 字段是反转过的，并且减去了 rwmutexMaxReaders 这个常数，变成了负数。
所以，这里的反转方法就是给它增加 rwmutexMaxReaders 这个常数值。

在正常模式下，锁的等待者会按照先进先出的顺序获取锁。但是刚被唤起的 Goroutine 与新创建的 Goroutine 竞争时，大概率会获取不到锁，在这种情况下，这个被唤醒的 Goroutine 会加入到等待队列的前面。 如果一个等待的 Goroutine 超过1ms 没有获取锁，那么它将会把锁转变为饥饿模式。
Go在1.9中引入优化，目的保证互斥锁的公平性。在饥饿模式中，互斥锁会直接交给等待队列最前面的 Goroutine。新的 Goroutine 在该状态下不能获取锁、也不会进入自旋状态，它们只会在队列的末尾等待。如果一个 Goroutine 获得了互斥锁并且它在队列的末尾或者它等待的时间少于 1ms，那么当前的互斥锁就会切换回正常模式。



## sync.Cond
Broadcast()   唤醒所有等待cond的goroutine
Signal()      唤醒一个等待cond的goroutine
Wait()会自动释放c.L锁，并挂起调用者的 goroutine。之后恢复执行，Wait()会在返回时对 c.L 加锁。 除非被 Signal 或者 Broadcast 唤醒，否则 Wait()不会返回
```Go
func main() {
    c := sync.NewCond(&sync.Mutex{})
    var ready int

    for i := 0; i < 10; i++ {
        go func(i int) {
            time.Sleep(time.Duration(rand.Int63n(10)) * time.Second)

            // 加锁更改等待条件
            c.L.Lock()
            ready++
            c.L.Unlock()

            log.Printf("运动员#%d 已准备就绪\n", i)
            // 广播唤醒所有的等待者
            c.Broadcast()
        }(i)
    }

    c.L.Lock()
    for ready != 10 {
        c.Wait()
        log.Println("裁判员被唤醒一次")
    }
    c.L.Unlock()

    //所有的运动员是否就绪
    log.Println("所有运动员都准备就绪。比赛开始，3，2，1, ......")
}
```


## WaitGroup
WaitGroup 主要维护了2 个计数器，一个是请求计数器 v，一个是等待计数器 w，二者组成一个64bit 的值，请求计数器占高32bit，等待计数器占低32bit。
每次 Add 执行，请求计数器 v 加 1，Done 方法执行，等待计数器减 1，v 为 0 时通过信号量唤醒Wait()


## channel
1. 从一个有缓冲的 channel 里读数据，当channel被关闭，依然能读出有效值。只有当返回的 ok 为 false 时，读出的数据才是无效的；
2. 不要从一个 receiver 侧关闭 channel，也不要在有多个 sender 时，关闭 channel；
3. 优雅地关闭channel--增加一个传递关闭信号的channel，receiver通过信号channel下达关闭数据channel指令。senders监听到关闭信号后，停止接收数据；
4. Channel 可能会引发 goroutine 泄漏。 泄漏的原因是 goroutine 操作 channel 后，处于发送或接收阻塞状态，而 channel 处于满或空的状态，一直得不到改变。
    同时，垃圾回收器也不会回收此类资源，进而导致 goroutine 会一直处于等待队列中，不见天日。
5. channel 的发送和接收操作本质上都是 “值的拷贝”；
6. 发生 panic 的情况有三种：向一个关闭的 channel 进行写操作；关闭一个 nil 的 channel；重复关闭一个 channel；
7. 给一个 nil channel 发送数据，造成永远阻塞
8. 从一个 nil channel 接收数据，造成永远阻塞
9. 给一个已经关闭的 channel发送数据，引起panic
10. 从一个已经关闭的 channel 接收数据，如果缓冲区中为空，则返回一个零值
11. 无缓冲的channel是同步的，而有缓冲的channel是非同步的
12. 无缓冲的channel发送操作会阻塞；
13. 使用锁的情景：1. 访问共享数据结构中的缓存信息；2. 保存应用程序上下文和状态信息数据。
14. 使用通道的情景：1. 与异步操作的结果进行交互; 2. 分发任务; 3. 传递数据所有权

### channel底层结构
用来保存goroutine之间传递数据的循环链表。=====> buf。
用来记录此循环链表当前发送或接收数据的下标值。=====> sendx和recvx。
用于保存向该chan发送和从该chan接收数据的goroutine的队列。=====> sendq 和 recvq
保证channel写入和读取数据时线程安全的锁。 =====> lock

### 向 channel 写数据的流程： 
如果等待接收队列 recvq 不为空，说明缓冲区中没有数据或者没有缓冲区，此时直接从 recvq 取出 G,并把数据写入，最后把该 G 唤醒，结束发送过程；
如果缓冲区中有空余位置，将数据写入缓冲区，结束发送过程；
如果缓冲区中没有空余位置，将待发送数据写入 G，将当前 G 加入 sendq，进入睡眠，等待被读 goroutine 唤醒；

### 向 channel 读数据的流程： 
如果等待发送队列 sendq 不为空，且没有缓冲区，直接从 sendq 中取出 G，把 G 中数据读出，最后把 G 唤醒，结束读取过程； 
如果等待发送队列 sendq 不为空，此时说明缓冲区已满，从缓冲区中首部读出数据，把 G 中数据写入缓冲区尾部，把 G 唤醒，结束读取过程；
如果缓冲区中有数据，则从缓冲区取出数据，结束读取过程；将当前 goroutine 加入 recvq，进入睡眠，等待被写 goroutine 唤醒；

使用场景： 消息传递、消息过滤，信号广播，事件订阅与广播，请求、响应转发，任务分发，结果汇总，并发控制，限流，同步与异步

### 对已经关闭的的 chan 进行读写，会怎么样？为什么？
1. 读已经关闭的 chan 能一直读到东西，但是读到的内容根据通道内关闭前是否有元素而不同。
    如果 chan 关闭前，buffer 内有元素还未读 , 会正确读到 chan 内的值，且返回的第二个 bool 值（是否读成功）为 true。
    如果 chan 关闭前，buffer 内有元素已经被读完，chan 内无值，接下来所有接收的值都会非阻塞直接成功，返回 channel 元素的零值，但是第二个 bool 值一直为 false。
2. 写已经关闭的 chan 会 panic


## select
1. 为Go提供多路复用机制，用于检测读写事件是否ready；
2. 超时处理；结合time.After函数进行超时处理；



## context
主要应用：1：上下文控制，2：多个 goroutine 之间的数据交互等，3：超时控制：到某个时间点超时，过多久超时。

Context通常被称为上下文，在go中，理解为goroutine的运行状态、现场，存在上下层goroutine context的传递，上层goroutine会把context传递给下层goroutine。
每个goroutine在运行前，都要事先知道程序当前的执行状态，通常将这些状态封装在一个 context变量，传递给要执行的goroutine中。
对于goroutine，他们的创建和调用关系总是像层层调用进行的，就像一个树状结构，而更靠顶部的context应该有办法主动关闭下属的goroutine的执行。
context.Background()函数的返回值就是一个根节点；
func WithCancel(parent Context) (ctx Context, cancel CancelFunc) {}
func WithDeadline(parent Context, d time.Time) (Context, CancelFunc) {}
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc) {}
func WithValue(parent Context, key, val interface{}) Context {}
以上WithXXX就是获得子节点；
子节点通过如下判断是否已经结束，并退出goroutine
select {
case <- ctx.Done():
    fmt.Println("do some clean work ...... ")
}

1. context包通过构建树形关系的context，来达到上一层goroutine对下一层goroutine的控制。
    对于处理一个request请求操作，需要通过goroutine来层层控制goroutine，以及传递一些变量来共享。
2. context变量的请求周期一般为一个请求的处理周期。即针对一个请求创建context对象；在请求处理结束后，撤销此ctx变量，释放资源。
3. 每创建一个goroutine，要不将原有context传递给子goroutine，要么创建一个子context传递给goroutine.
4. Context能灵活地存储不同类型、不同数目的值，并且使多个Goroutine安全地读写其中的值。
5. 当通过父 Context对象创建子Context时，可以同时获得子Context的撤销函数，这样父goroutine就获得了子goroutine的撤销权。

原则：
1. 不要把context放到一个结构体中，应该作为第一个参数显式地传入函数
2. 即使方法允许，也不要传入一个nil的context，如果不确定需要什么context的时候，传入一个context.TODO
3. 使用context的Value相关方法应该传递和请求相关的元数据，不要用它来传递一些可选参数
4. 同样的context可以传递到多个goroutine中，Context在多个goroutine中是安全的
5. 在子context传入goroutine中后，应该在子goroutine中对该子context的Done channel进行监控，一旦该channel被关闭，应立即终止对当前请求的处理，并释放资源。

### withCancel
WithCancel 返回带有新 Done 通道的父级副本。当调用返回的 cancel 函数或关闭父上下文的 Done 通道时，返回的 ctx 的 Done 通道将关闭。
取消此上下文会释放与其关联的资源，因此在此上下文中运行的操作完成后，代码应立即调用 cancel。

### withDeadline
WithDeadline 返回父上下文的副本，并将截止日期调整为不晚于 d。如果父级的截止日期已经早于 d，则 WithDeadline(parent, d) 在语义上等同于 parent。
当截止时间到期、调用返回的取消函数时或当父上下文的 Done 通道关闭时，返回的上下文的 Done 通道将关闭。
取消此上下文会释放与其关联的资源，因此在此上下文中运行的操作完成后，代码应立即调用取消。
其实就是设置过期时间；

### withTimeout
WithTimeout 返回 WithDeadline(parent, time.Now().Add(timeout))。
取消此上下文会释放与其关联的资源，因此在此上下文中运行的操作完成后，代码应立即调用取消。
其实就是设置超时时间；



### withValue
WithValue 返回父级的副本，其中与 key 关联的值为 val。
其中键必须是可比较的，并且不应是字符串类型或任何其他内置类型，以避免使用上下文的包之间发生冲突。 WithValue 的用户应该定义自己的键类型。
为了避免分配给 interface{}，上下文键通常具有具体的 struct{} 类型。或者，导出的上下文键变量的静态类型应该是指针或接口。


```shell
const ( 
 KEY = "trace_id" 
) 
 
func NewRequestID() string { 
 return strings.Replace(uuid.New().String(), "-", "", -1) 
} 
 
func NewContextWithTraceID() context.Context { 
 ctx := context.WithValue(context.Background(), KEY,NewRequestID()) 
 return ctx 
} 
 
func PrintLog(ctx context.Context, message string)  { 
 fmt.Printf("%s|info|trace_id=%s|%s",time.Now().Format("2006-01-02 15:04:05") , 
GetContextValue(ctx, KEY), message) 
} 
 
func GetContextValue(ctx context.Context,k string)  string{ 
 v, ok := ctx.Value(k).(string) 
 if !ok{ 
  return "" 
 } 
 return v 
} 
 
func ProcessEnter(ctx context.Context) { 
 PrintLog(ctx, "Golang 梦工厂") 
} 
 
 
func main()  { 
 ProcessEnter(NewContextWithTraceID()) 
} 
 
超时控制： 
ProcessEnter(NewContextWithTraceID()) 
} 
func main()  { 
 HttpHandler() 
} 
 
func NewContextWithTimeout() (context.Context,context.CancelFunc) { 
 return context.WithTimeout(context.Background(), 3 * time.Second) 
} 
 
func HttpHandler()  { 
 ctx, cancel := NewContextWithTimeout() 
 defer cancel() 
 deal(ctx) 
} 
 
func deal(ctx context.Context)  { 
 for i:=0; i< 10; i++ { 
  time.Sleep(1*time.Second) 
  select { 
  case <- ctx.Done(): 
   fmt.Println(ctx.Err()) 
   return 
  default: 
   fmt.Printf("deal time is %d\n", i) 
  } 
 } 
} 
 
// deal time is 0 
// deal time is 1 
// context deadline exceeded 

```

## GC
### 三色标记原理
三色标记原理基于对象的可达性进行垃圾回收。它将对象分为三种不同的颜色：白色、灰色和黑色。
白色：表示对象尚未被扫描和标记。
灰色：表示对象已经被扫描，但其引用的对象尚未被扫描。灰色对象是有可能引用黑色对象的。
黑色：表示对象已经被扫描和标记，并且其引用的对象也已经被扫描和标记。

垃圾回收的过程如下：
1. 初始状态下，所有的对象都是白色。
2. 从根对象（如全局变量、栈中的对象等）开始，将根对象标记为灰色，并将其放入待处理的灰色对象队列中。
3. 循环执行以下步骤，直到灰色对象队列为空：
a. 从灰色对象队列中取出一个灰色对象。
b. 将该灰色对象标记为黑色。
c. 扫描该灰色对象引用的其他对象，如果引用的对象是白色，则将其标记为灰色，并放入灰色对象队列中。
4. 当灰色对象队列为空时，所有的可达对象都已经被标记为黑色。此时，白色对象即为不可达的垃圾对象。
5. 对于白色对象，可以进行相应的回收操作，如释放内存等。

### 写屏障
STW   Stop The Word
指的是垃圾回收器执行时会暂停所有运行的线程；
引入写屏障的原因：
有如下场景：对象A引用对象B和C，对象D引用对象E；在三色标记中扫描灰色集合中，扫描到了对象A，并标记到了A的应用B和C，这个时候开始扫描D的应用，
另一个goroutine修改了D对E的引用，将E让A引用，这样就导致E对象就扫描不到，而被误认为白色对象而被清理；

### 混合写屏障
强三色不变式：不存在黑色对象引用到白色对象的指针，即强制的不允许黑色对象引用白色对象；
弱三色不变式：所有被黑色对象引用的白色对象都处于灰色保护状态。


插入屏障：在A对象引用B对象的时候，B对象被标记为灰色。(将B挂在A下游，B必须被标记为灰色)
删除屏障：被删除的对象，如果自身为灰色或者白色，那么被标记为灰色。

混合写屏障：
1、GC开始将栈上的对象全部扫描并标记为黑色(之后不再进行第二次重复扫描，无需STW)，
2、GC期间，任何在栈上创建的新对象，均为黑色。
3、被删除的对象标记为灰色。
4、被添加的对象标记为灰色。


### 知道golang的内存逃逸吗？什么情况下会发生内存逃逸？
golang程序变量会携带有一组校验数据，用来证明它的整个生命周期是否在运行时完全可知。如果变量通过了这些校验，它就可以在栈上分配。否则就说它逃逸了，必须在堆上分配。

能引起变量逃逸到堆上的典型情况：
1、在方法内把局部变量指针返回 局部变量原本应该在栈中分配，在栈中回收。但是由于返回时被外部引用，因此其生命周期大于栈，则溢出。
2、发送指针或带有指针的值到 channel 中。 在编译时，是没有办法知道哪个 goroutine 会在 channel 上接收数据。所以编译器没法知道变量什么时候才会被释放。
3、在一个切片上存储指针或带指针的值。 一个典型的例子就是 []*string 。这会导致切片的内容逃逸。尽管其后面的数组可能是在栈上分配的，但其引用的值一定是在堆上。
4、slice 的背后数组被重新分配了，因为 append 时可能会超出其容量( cap )。 
    slice 初始化的地方在编译时是可以知道的，它最开始会在栈上分配。如果切片背后的存储要基于运行时的数据进行扩充，就会在堆上分配。
5、在 interface 类型上调用方法。 在 interface 类型上调用方法都是动态调度的 —— 方法的真正实现只能在运行时知道。
    想像一个 io.Reader 类型的变量 r , 调用 r.Read(b) 会使得 r 的值和切片b 的背后存储都逃逸掉，所以会在堆上分配。









# etcd
etcd 是一个高度一致的分布式键值存储，它提供了一种可靠的方式来存储需要由分布式系统或机器集群访问的数据。 
它可以优雅地处理网络分区期间的领导者选举，即使在领导者节点中也可以容忍机器故障。 
etcd 是用 Go 语言编写的，它具有出色的跨平台支持，小的二进制文件和强大的社区。
etcd 机器之间的通信通过 Raft 共识算法处理。 

etcd 是一个高可用的键值存储系统，用于共享配置和服务发现。下面是 etcd搭建和使用的简单步骤： 
1. 下载安装 etcd。 
2. 配置 etcd 集群，在生产环境中为了高可用性，往往需要将 etcd 部署成集群形式，避免单点故障。 
3. 配置 etcd TLS 加密通讯。etcd 支持通过 TLS 协议的加密通讯，TLS 通道可以用于加密伙伴间的内部集群通讯，也可以用于加密客户端请求。 
4. 编写客户端代码，使用 etcd 提供的 API 访问 etcd。etcd 提供多种语言的客户端库，可以根据需要选择使用。具体使用方法可以参考 etcd 的官方文档。 


### 查看本地etcd服务
sc query etcd

### docker部署etcd服务
1. docker pull bitnami/etcd:latest；拉取镜像
2. docker network create app-tier --driver bridge；创建docker网络
3. 启动etcd服务实例；docker run -d --name etcd-server --network app-tier --publish 2379:2379 --publish 2380:2380 --env ALLOW_NONE_AUTHENTICATION=yes --env ETCD_ADVERTISE_CLIENT_URLS=http://10.50.7.108:2379 bitnami/etcd:latest
4. 查看版本；curl -L http://127.0.0.1:2379/version


# 缓存
### 缓存实现高性能
场景：
电商某个商品的信息一天内不会变化，但是这个商品一次查询要耗费2s，1天只能被浏览100万次；
用户1查询数据1，第一次查询缓存没有，从数据库查询耗费800ms,将数据放入缓存，第二次查询只需查找缓存即可；后续数据修改时，更新数据库的同时更新缓存就ok；
假如10分钟数据没有变化，1000个人访问同一数据，就第一个人耗时800ms，后续999人只需耗时10ms就能查找到数据。

### 缓存实现高并发
电商里的商品，3/4放入缓存，1/4放入数据库；高峰期每秒4000个请求，其中3000走缓存，1000走数据库；
缓存是走内存，内存天然可以支撑4万/s个请求；数据库一般建议并发请求不要超过2000/s。

### 缓存雪崩
缓存在同一时刻全部失效，造成瞬时DB请求量大、压力骤增，引起雪崩。缓存雪崩通常因为缓存服务器宕机、缓存的 key 设置了相同的过期时间等引起。
解决方案：
1）缓存高可用：通过搭建缓存的高可用，避免缓存挂掉导致无法提供服务的
情况，从而降低出现缓存雪崩的情况。假设我们使用 Redis 作为缓存，则可以
使用 Redis Sentinel 或 Redis Cluster 实现高可用。
2）本地缓存：如果使用本地缓存时，即使分布式缓存挂了，也可以将 DB 查询到的结果缓存到本地，避免后续请求全部到达 DB 中。
如果我们使用 JVM ，则可以使用 Ehcache、Guava Cache 实现本地缓存的功能。

### 缓存击穿
一个存在的key，在缓存过期的一刻，同时有大量的请求，这些请求都会击穿到 DB ，造成瞬时DB请求量大、压力骤增。
解决：
有两种方案可以解决：
1）方案一，使用互斥锁。请求发现缓存不存在后，去查询DB前，使用分布式锁，保证有且只有一个线程去查询DB ，并更新到缓存。
2）方案二，手动过期。缓存上从不设置过期时间，功能上将过期时间存在KEY对应的VALUE里。流程如下：
 1、获取缓存。通过VALUE的过期时间，判断是否过期。如果未过期，则直接返回；如果已过期，继续往下执行。
 2、通过一个后台的异步线程进行缓存的构建，也就是“手动”过期。通过后台的异步线程，保证有且只有一个线程去查询DB。
 3、同时，虽然 VALUE 已经过期，还是直接返回。通过这样的方式，保证服务的可用性，虽然损失了一定的时效性。


### 缓存穿透
查询一个不存在的数据，因为不存在则不会写到缓存中，所以每次都会去请求 DB，如果瞬间流量过大，穿透到 DB，导致宕机。
解决方案：
方案一，缓存空对象。
当从 DB 查询数据为空，我们仍然将这个空结果进行缓存，具体的值需要使用特殊的标识，能和真正缓存的数据区分开。另外，需要设置较短的过期时间， 一般建议不要超过5分钟。
方案二，BloomFilter布隆过滤器。
在缓存服务的基础上，构建BloomFilter数据结构，在BloomFilter中存储对应的KEY是否存在，如果存在，说明该KEY对应的值不为空。

### 一致性hash
一致性哈希算法将 key 映射到 2^32 的空间中，将这个数字首尾相连，形成一个环。
1. 计算节点/机器(通常使用节点的名称、编号和 IP 地址)的哈希值，放置在环上。
2. 计算 key 的哈希值，放置在环上，顺时针寻找到的第一个节点，就是应选取的节点/机器。
3. 
数据倾斜问题
如果服务器的节点过少，容易引起 key 的倾斜。例如上面例子中的 peer2，peer4，peer6 分布在环的上半部分，下半部分是空的。那么映射到环下半部分的 key 都会被分配给 peer2，key 过度向 peer2 倾斜，缓存节点间负载不均。
为了解决这个问题，引入了虚拟节点的概念，一个真实节点对应多个虚拟节点。
假设 1 个真实节点对应 3 个虚拟节点，那么 peer1 对应的虚拟节点是 peer1-1、 peer1-2、 peer1-3（通常以添加编号的方式实现），其余节点也以相同的方式操作。
第一步，计算虚拟节点的 Hash 值，放置在环上。
第二步，计算 key 的 Hash 值，在环上顺时针寻找到应选取的虚拟节点，例如是 peer2-1，那么就对应真实节点 peer2。


# jwt 
JWT就是一种基于Token的轻量级认证模式，服务端认证通过后，会生成一个JSON对象，经过签名后得到一个Token（令牌）再发回给用户，
用户后续请求只需要带上这个Token，服务端解密之后就能获取该用户的相关信息了。

jwt三个部分组成,它是一个很长的字符串，中间用点（.）分隔成三个部分。
1. header(头部)，也是json对象，描述jwt的元数据，最后用Base64URL加密成字符串；
2. payload(负载)，用来存放实际需要传递的数据，最后用Base64URL加密成字符串；
3. signature(签名)，对前两部分的签名；


## json序列化与反序列化
1. json tag忽略某个字段；`json:"-"`；
2. json tag忽略空值字段；`json:"name,omitempty"`；
3. json tag忽略嵌套结构体空值；`json:"Person,omitempty"`+嵌套的结构体要是指针；
4. 




# defer
1. 多个defer语句，按先进后出的方式执行；
2. defer声明时，对应的参数会实时解析；
3. defer、return、返回值三者的执行逻辑-return最先执行，return负责将结果写入返回值中，defer执行收尾工作，最后函数携带当前返回值退出；
4. defer用于关闭文件和互斥锁；
5. panic后面的defer语句不被执行，panic语句前的defer语句会被执行；
6. 调用os.Exit(0)时defer语句不会被执行；

## defer的底层数据结构
每个 defer 语句都对应一个_defer 实例，多个实例使用指针连接起来形成一个单连表，保存在 goroutine 数据结构中，
每次插入_defer 实例，均插入到链表的头部，函数结束再一次从头部取出，从而形成后进先出的效果。


# rune
Go字符串底层是通过byte数组实现，中文字符在unicode下占2个字节，在utf-8编码下占3个字节，而golang默认编码正好是utf-8
byte 等同于int8，常用来处理ascii字符
rune 等同于int32,常用来处理unicode或utf-8字符


# errors包
1. errors.New创建一个表示特定错误的对象，接受一个字符串类型的参数，返回一个error类型的对象；
2. errors.Is用于判断给定的错误是否是目标错误类型或者基于目标错误类型包装过的错误，会递归检查错误链，直到找到目标错误类型或者到达错误链的末尾。如果找到目标错误类型，则返回true，否则返回false。


# io包
1. io.CopyN(io.Discard, conn, bytesToDiscard)           # 指的是将网络连接中bytesToDiscard大小的字节丢弃掉；
2. io.ReadAtLeast()                # 能够从数据源读取至少指定数量的字节数据到缓冲区中



# 反射reflect
反射是程序在运行时检查其变量和值并找到其类型的能力
1. 对比变量相等；reflect.DeepEqual(sm1, sm2)
2. 反射类型Type用于获取类型相关的信息（比如slice的长度，struct的成员，函数的参数个数）；反射类型Value用于获取和修改原始数据的值（修改slice和map中的元素，修改struct的成员变量）
3. reflect.TypeOf(o)得到反射类型，实际类型比如结构体order； 
4. reflect.ValueOf(o)得到反射类型，获取指针对应的反射值； 
5. Type通过New()转成Value,Value通过Type()转成Type；
6. reflect.TypeOf(o).Kind()     ---表示order的特定类型，结构体struct
7. reflect.TypeOf(o).Elem()/reflect.ValueOf(o)     ---指针类型转为非指针类型
8. reflect.TypeOf(o).NumField() ---获取成员变量的个数； 
    reflect.TypeOf(o).Field(i)   ---第i个成员变量； 
    field.Name              //变量名称
    field.Offset            //相对于结构体首地址的内存偏移量，string类型会占据16个字节 
    field.Anonymous         //是否为匿名成员 
    field.Type,             //数据类型，reflect.Type类型
    ld.IsExported(),        //包外是否可见（即是否以大写字母开头）
	field.Tag.Get("json"))  //获取成员变量后面``里面定义的tag
9. methodNum := reflect.TypeOf(o).NumMethod()   //成员方法的个数。接收者为指针的方法【不】包含在内获取struct成员方法的信息
    method := typeUser.Method(i)
10. reflect.TypeOf(funcName)    // 获取函数信息 
    typeFunc.NumIn()            //输入参数的个数
    typeFunc.NumOut()           //输出参数的个数
    typeFunc.In(i)              //第i个输入参数
	typeFunc.Out(i)             //第i个输出参数的类型
11. reflect.ValueOf(o).Addr()   //非指针类型转为指针类型
12. reflect.ValueOf(o).Interface().(int)   //通过Interface()函数把Value转为interface{}，再从interface{}强制类型转换，转为原始数据类型。或者在Value上直接调用Int()、String()等一步到位;
13. reflect.ValueOf(o).IsValid()   //空值判断
14. reflect.ValueOf(&o).Elem().FieldByName("name").SetString("dx")   //要想修改原始数据的值，给ValueOf传的必须是指针，而指针Value不能调用Set和FieldByName方法，所以得先通过Elem()转为非指针Value。
15. reflect.ValueOf(FuncName).Call(args)   //调用函数，args根据reflect.TypeOf(FuncName)的参数来进行赋值；
16. reflect.ValueOf(&user).MethodByName("Foo").Call([]reflect.Value{})   //调用方法;
17. value := reflect.New(reflect.TypeOf(User{}));value.Elem().FieldByName("Name").SetString("dx");value.Interface().(*User);  //创建struct
18. reflect.MakeSlice(reflect.TypeOf([]User));    //创建切片
19. reflect.MakeMapWithSize()     //创建map；
20. reflect.Indirect(reflect.ValueOf(&user)).Type()           //获取指针指向的对象的反射值。


# rpc
Remote Procedure Call，远程过程调用
grpc 使用了 Google 开源的 Protocol Buffers 作为数据格式，这种数据格式比 JSON 和 XML 更加高效，更加紧凑。 
grpc 的主要特点是高效、可靠、跨平台、易于扩展。它使用 HTTP/2 协议进行通信，可以在客户端和服务器之间建立长连接，从而提高通信的效率。
它还支持 TLS加密，可以保证通信的安全性。另外，grpc 还支持流式传输和双向流式传输，可
以在客户端和服务器之间进行大规模的数据交换。


在 Go 语言中使用 grpc 非常简单。
首先需要安装 grpc 和 protobuf 的 Go 语言库。
然后，定义一个 protobuf 文件，用于描述 RPC 接口和数据类型。
接下来，使用 protobuf 的编译器将 protobuf 文件编译成 Go 语言代码。
最后，使用 Go 语言代码 实现 RPC 服务端和客户端。 


## RPC 工作原理
远程过程调用协议，是一种通过网络从远程计算机程序上请求服务，而不需要了解底层网络技术的协议。
它假定某些传输协议的存在，如 TCP 或 UDP，以便为通信程序之间携带信息数据。
通过它可以使函数调用模式网络化。在 OSI 网络通信模型中，RPC 跨越了传输层和应用层。
RPC 使得开发包括网络分布式多程序在内的应用程序更加容易。 

运行时,一次客户机对服务器的 RPC 调用,其内部操作大致有如下十步： 
1. 调用客户端句柄；执行传送参数； 
2. 调用本地系统内核发送网络消息； 
3. 消息传送到远程主机； 
4. 服务器句柄得到消息并取得参数； 
5. 执行远程过程； 
6. 执行的过程将结果返回服务器句柄； 
7. 服务器句柄返回结果，调用远程系统内核； 
8. 消息传回本地主机； 
9. 客户句柄由内核接收消息； 
10. 客户接收句柄返回的数据。 

## rpc和http区别
rpc 是远程过程调用，就是本地去调用一个远程的函数，而 http 是通过url 和符合 restful 风格的数据包去发送和获取数据； 
rpc 的一般使用的编解码协议更加高效，比如 grpc 使用 protobuf 编解码。
而 http 的一般使用 json 进行编解码，数据相比 rpc 更加直观，但是数据包也更大，效率低下； 
rpc 一般用在服务内部的相互调用，而 http 则用于和用户交互； 
相似点： 都有类似的机制， 例如 grpc 的 metadata 机制和 http 的头机制作用相似，
而且 web 框架，和 rpc 框 架中都有拦截器的概念。grpc 使用的是 http2.0 协议。  


## grpc使用
1. 先使用protobuf定义服务;
2. 提前windows下载protoc到本地 https://github.com/protocolbuffers/protobuf/releases
下载protoc-gen-go到本地 https://github.com/protocolbuffers/protobuf-go/releases
git clone -b v1.30.0 https://github.com/grpc/grpc-go  
cd cmd/protoc-gen-go-grpc  
go install .
会在GOPATH/bin下生成protoc-gen-go-grpc.exe文件；
3. 到protobuf文件路径执行命令生成pd文件
protoc --go_out=. --go_opt=paths=source_relative --go-grpc_out=. --go-grpc_opt=paths=source_relative myprotobuf.proto


## grpc四种请求响应模式 
简单模式(Simple RPC)：客户端发起请求并等待服务端响应。
服务端流式（Server-side streaming RPC）：客户端发送请求到服务器，拿到一个流去读取返回的消息序列。 客户端读取返回的流，直到里面没有任何消息。
客户端流式（Client-side streaming RPC）：与服务端数据流模式相反，这次是客户端源源不断的向服务端发送数据流，而在发送结束后，由服务端返回一个响应。
双向流式（Bidirectional streaming RPC）：双方使用读写流去发送一个消息序列，两个流独立操作，双方可以同时发送和同时接收。


## tips
1. 不设置请求参数或者返回体，proto文件中import "google/protobuf/empty.proto"(下载放到同级目录)，google.protobuf.Empty；



# 微服务 
微服务是一种开发软件的架构和组织方法，其中软件由通过明确定义的 API进行通信的小型独立服务组成。
微服务架构使应用程序更易于扩展和更快地开发，从而加速创新并缩短新功能的上市时间。 

## 如何设计一个高并发系统
保证整体可用的同时，能够处理很高的并发用户请求，能够承受很大的流量冲击；
1. 横向扩展；采用分布式部署，部署多台服务器，将流量分开，分担流量；
2. 微服务拆分；按功能单一性，拆分成多个服务模块；根据情况，先拆分一轮，后面如果系统更复杂了，可以继续分拆。一个服务的代码不要太多，1万行左右，两三万撑死了吧
3. 数据库分库分表；数据库2000并发
4. 池化技术；数据库连接池、http连接池、redis连接池；
5. 主从分离；实时性要求不高的读请求，都去读从库，写的请求或者实时性要求高的请求，才走主库；
6. 使用缓存；redis几万并发；
7. CDN加速静态资源访问；
8. 消息队列，削峰；单机几万并发
9. ElasticSearch；es 是分布式的，可以随便扩容，分布式天然就可以支撑高并发，因为动不动就可以扩容加机器来扛更高的并发。那么一些比较简单的查询、统计类的操作，可以考虑用 es 来承载，还有一些全文搜索类的操作，也可以考虑用 es 来承载。
10. 降级熔断；
11. 限流；
12. 异步；
13. 扩容+切流量；


## 接口请求重试方式
1. 循环重试；直至接口重试成功或者达到最大重试次数
2. 递归重试；请求失败则继续调用，直到请求成功或达到最大重试次数。
3. 并发框架异步重试；请求接口转化成一个异步任务，将任务放入线程池中异步执行，并发地重试请求接口。可以在任务执行完成后，判断任务执行结果，如果失败则继续重试。
4. 消息队列重试；直接把消息投递到消息队列里，通过对消息的消费，来实现重试机制。

在请求重试的时候，我们也要注意一些关键点，以免因为重试，引发更多的问题：
1. 合理设置重试次数和重试间隔时间，避免频繁地发送请求，同时也不要设置过大的重试次数，以免影响系统的性能和响应时间。
2. 考虑接口幂等性：如果请求是写操作，而且下游的服务不保证请求的幂等性，那么在重试时需要谨慎处理，可以通过查询等幂等的方式进行重试
3. 在重试过程中，需要考虑并发的问题。如果多个线程同时进行重试，可能会导致请求重复发送或请求顺序混乱等问题。可以使用锁或者分布式锁来解决并发问题。
4. 在处理异常时，需要根据具体的异常类型来进行处理。有些异常是可以通过重试来解决的，例如网络超时、连接异常等；而有些异常则需要进行特殊的处理，例如数据库异常、文件读写异常等。
5. 在使用重试机制时，需要注意不要陷入死循环。如果请求一直失败，重试次数一直增加，可能会导致系统崩溃或者资源耗尽等问题。


## 服务发现是怎么做的？
主要有两种服务发现机制：客户端发现和服务端发现。 
客户端发现模式：当我们使用客户端发现的时候，客户端负责决定可用服务实例的网络地址并且在集群中对请求负载均衡, 客户端访问服务登记表，
也就是一个可用服务的数据库，然后客户端使用一种负载均衡算法选择一个可用的服务实例然后发起请求。
服务端发现模式：客户端通过负载均衡器向某个服务提出请求，负载均衡器查
询服务注册表，并将请求转发到可用的服务实例。如同客户端发现，服务实例在服务注册表中注册或注销。

中间件用过吗？
Middleware 是 Web 的重要组成部分，中间件（通常）是一小段代码，它们接受一个请求，对其进行处理，
每个中间件只处理一件事情，完成后将其传递给另一个中间件或最终处理程序，这样就做到了程序的解耦。 

持久化怎么做的？ 
所谓持久化就是将要保存的字符串写到硬盘等设备。 
 最简单的方式就是采用 ioutil 的 WriteFile()方法将字符串写到磁盘上，这种方法面临格式化方面的问题。 
 更好的做法是将数据按照固定协议进行组织再进行读写，比如 JSON，XML，Gob，csv 等。 
 如果要考虑高并发和高可用，必须把数据放入到数据库中，比如 MySQL，PostgreDB，MongoDB 等。 


## Go 中发生熔断怎么做的 
在 Go 中，熔断通常是由于一段服务的连续失败引起的，为避免故障扩散，需要使用熔断机制保护底层资源。
Go 语言提供了一些库和框架来实现熔断机制，例如：
Hystrix：Hystrix 是 Netflix 开源的一款熔断器组件，可以在 Go 程序中使用。
它通过统计系统的服务指标（如请求成功率、平均响应时间等）来观察是否需要进行熔断，一旦达到设定的阈值就会执行熔断操作。 
Gobreaker：Gobreaker 是一个 Go 实现的熔断器库，支持自定义熔断器状态存
储和熔断器策略等功能，可以用于保护底层服务或资源不被过度使用。 
Resilience4j：Resilience4j 是一个面向函数式编程的熔断器库，支持多种熔断
器策略和监控方式，可以帮助开发者实现高可用的服务治理。 
总之，Go 提供了多种熔断器库和框架，开发者可以根据具体情况选择并使用这些库来实现熔断机制，保障程序稳定性和可靠性。 


## 服务降级怎么搞
当 Go 服务发生压力剧增时，为了保证核心业务的正常高效运行，需要采取服务降级措施。
下面是一些具体做法：
针对不同服务和页面进行策略性地降级：通过对实际业务使用情况和流量的分析，针对不同服务和页面进行策略性地降级，从而释放服务器资源，提高服务可用性。 
对某些服务和页面采用简单的处理方式：对于一些重要但非核心的服务和页
面，可以采用一些简单的处理方式，如缓存数据、使用轻量级组件等，从而减轻服务器压力。 
通过技术手段实现服务降级：可以通过技术手段实现服务降级，例如使用
Hystrix 等熔断框架，当系统出现故障或超负荷时，自动切换到备份或降级服务，避免发生系统瘫痪。 
总之，针对不同的场景和业务需求，开发人员需要根据实际情况采取适当的服务降级措施，确保系统稳定运行。 


# go 里用过哪些设计模式? 

## 工厂方法模式 
问题描述：
假设你正在开发一款物流管理应用。最初版本只能处理卡车运输，因此大部分代码都在位于名为卡车的类中。 
一段时间后， 这款应用变得极受欢迎。你每天都能收到十几次来自海运公司的请求，希望应用能够支持海上物流功能。 
解决方案：我们抽象出一个物流工厂，然后分别实例化陆上，海上物流等工厂类，再生产对应产品。 
首先写一个表示通用物流的接口：
```Go
type iLogistics interface {  
    setName(string)
    getName() string 
}
```
之后就是陆上物流的工厂类： 
```Go
type RoadLogistics struct {
    name string 
}
func (r *RoadLogistics) setName(name string) {
    r.name = name 
}

func (r *RoadLogistics) getName() string {  
    return r.name
}
``` 
var roadInstance iLogistics = (*RoadLogistics)(nil)  
验证 RoadLogistics 是否实习了接口 iLogistics以及海上物流的工厂类，因为类似这里就省略了。
之后就是具体产品了，比如陆上的有汽车，火车和高铁。 
```Go
type Vehicle struct {
    RoadLogistics
} 
 
func NewVehicle() (v *Vehicle) { 
    v = new(Vehicle) 
    v.RoadLogistics = RoadLogistics{ 
        Name: "vehicle",
    }
    return v 
}
```
上面是汽车的例子，我们再写一个火车的例子： 
```Go
type Train struct {  
    RoadLogistics
}

func NewTrain() (t *Train) {  
    t = new(Train)
    t.RoadLogistics = RoadLogistics{
        Name: "train",
    }
    return t 
}
```
然后我们可以根据类别，直接初始化对应的类： 
```Go
func getLogistics(means string) (i iLogistics, err error){
    if means == "vehicle" {
        return NewVehicle(), nil
    } else if means == "train"{
        return NewTrain(), nil
    }
    return nil, errors.New("unknown means of transportation") 
}
```
客户端使用，直接调用 getLogistics 就好。 

## 抽象工厂模式 
抽象工厂模式是一种创建型设计模式，它能创建一系列相关的对象，而无需指定其具体类。 
❓问题描述：假设一下，如果你想要购买一组运动装备，比如一双鞋与一件衬衫这样由两种不同产品组合而成的套装。
相信你会想去购买同一品牌的商品，这样商品之间能够互相搭配起来。
解决方案：首先，抽象工厂模式为系列中的每件产品明确声明接口（例如 T 恤或者鞋子）。然后，确保所有产品变体都继承这些接口。 
下面是一个抽象工厂接口，它能产生工厂类
```Go
type iSportsFactory interface { 
    makeShoe() iShoe 
    makeShirt() iShirt 
} 
 
func getSportsFactory(brand string) (iSportsFactory, error) { 
    if brand == "adidas" { 
        return &adidas{}, nil 
    } 
 
    if brand == "nike" { 
        return &nike{}, nil 
    } 
 
    return nil, fmt.Errorf("Wrong brand type passed") 
}
``` 
现在有一个 adidas 的工厂： 
```Go
type adidas struct { 
} 
 
func (a *adidas) makeShoe() iShoe { 
    return &adidasShoe{ 
        shoe: shoe{ 
            logo: "adidas", 
            size: 14, 
        }, 
    } 
} 
 
func (a *adidas) makeShirt() iShirt { 
    return &adidasShirt{ 
        shirt: shirt{ 
            logo: "adidas", 
            size: 14, 
        }, 
    } 
} 
```
和 nike 的工厂（略）。 
然后是抽象的产品，它有尺寸和 logo 两个属性： 
```Go
type iShoe interface { 
    setLogo(logo string) 
    setSize(size int) 
    getLogo() string 
    getSize() int 
} 
 
type shoe struct { 
    logo string 
    size int 
} 
 
func (s *shoe) setLogo(logo string) { 
    s.logo = logo 
} 
 
func (s *shoe) getLogo() string { 
    return s.logo 
} 
 
func (s *shoe) setSize(size int) { 
    s.size = size 
} 
 
func (s *shoe) getSize() int { 
    return s.size 
} 
```
adidas 工厂必须能生产鞋子： 
type adidasShoe struct { 
    shoe 
} 
客户端代码： 
func main() { 
    adidasFactory, _ := getSportsFactory("adidas") 
    nikeFactory, _ := getSportsFactory("nike") 
 
    nikeShoe := nikeFactory.makeShoe() 
    nikeShirt := nikeFactory.makeShirt() 
 
    adidasShoe := adidasFactory.makeShoe() 
    adidasShirt := adidasFactory.makeShirt() 
} 
工厂方法模式和抽象工厂模式的区别在于：前者实例化具体的产品，后者实例化具体的工厂，每个工厂再实例化同样的产品系列。 

## 单例模式 
### 单例模式实现两种方式
一个只允许创建一个实例的类称为单例类，这种模式称为单例模式。
1. 懒汉式
```Go
package singleton

import "sync"

type Foo struct {
    Name         string
    Data         []byte
}

var (
    instance       *Foo
    lock           sync.Mutex
)

func NewInstance1() *Foo {
    if instance == nil {
        lock.Lock()
        if instance == nil {
            instance = &Foo{
                Name: "name",
                Data: []byte{},
            }
        }
        lock.Unlock()
    }
    return instance
}
```

2. 饿汉式
```Go
var singletonInstance *Foo = &Foo{Name: "name", Data: []byte{}}
```

3. 使用sync.Once库
```Go
var once sync.Once
var instance *Foo

func NewInstance2() *Foo {
    once.Do(func() {
        instance = &Foo{
            Name: "name",
            Data: []byte{},
        }
    })
    return instance
}
```

## 生成器模式 
❓问题描述：假设一下，我们需要在游戏里设计不同的虚拟房屋，每个房子有不同的门和窗户等属性。
现在有两种类型的房屋 normal 和 igloo（木制）。 
解决方案：我们需要考虑两个问题：1.如何确保全局唯一，2.如何保证并发控制。 
定义生成器接口： 
type iBuilder interface { 
    setWindowType() 
    setDoorType() 
    setNumFloor() 
    getHouse() house 
} 
 
func getBuilder(builderType string) iBuilder { 
    if builderType == "normal" { 
        return &normalBuilder{} 
    } 
 
    if builderType == "igloo" { 
        return &iglooBuilder{} 
    } 
    return nil 
} 
具体的房屋生成器（以 normal 为例子）： 
type normalBuilder struct { 
    windowType string 
    doorType   string 
    floor      int 
} 
 
func newNormalBuilder() *normalBuilder { 
    return &normalBuilder{} 
} 
 
func (b *normalBuilder) setWindowType() { 
    b.windowType = "Wooden Window" 
} 
 
func (b *normalBuilder) setDoorType() { 
    b.doorType = "Wooden Door" 
} 
 
func (b *normalBuilder) setNumFloor() { 
    b.floor = 2 
} 
 
func (b *normalBuilder) getHouse() house { 
    return house{ 
        doorType:   b.doorType, 
        windowType: b.windowType, 
        floor:      b.floor, 
    } 
} 
房屋（产品）： 
type house struct { 
    windowType string 
    doorType   string 
    floor      int 
} 
定义一个主管，他手下有可以修建所有类型房屋的工人。 
type director struct { 
    builder iBuilder 
} 
 
func newDirector(b iBuilder) *director { 
    return &director{ 
        builder: b, 
    } 
} 
 
func (d *director) setBuilder(b iBuilder) { 
    d.builder = b 
} 
 
func (d *director) buildHouse() house { 
    d.builder.setDoorType() 
    d.builder.setWindowType() 
    d.builder.setNumFloor() 
    return d.builder.getHouse() 
} 
客户端代码： 
func main() { 
    normalBuilder := getBuilder("normal") 
    iglooBuilder := getBuilder("igloo") 
 
    director := newDirector(normalBuilder) 
    normalHouse := director.buildHouse() 
 
    fmt.Printf("Normal House Door Type: %s\n", 
normalHouse.doorType) 
    fmt.Printf("Normal House Window Type: %s\n", 
normalHouse.windowType) 
    fmt.Printf("Normal House Num Floor: %d\n", 
normalHouse.floor) 
 
    director.setBuilder(iglooBuilder) 
    iglooHouse := director.buildHouse() 
 
    fmt.Printf("\nIgloo House Door Type: %s\n", 
iglooHouse.doorType) 
    fmt.Printf("Igloo House Window Type: %s\n", 
iglooHouse.windowType) 
    fmt.Printf("Igloo House Num Floor: %d\n", 
iglooHouse.floor) 
} 

## 原型模式 
原型模式使你能够复制已有对象，无需使代码依赖它们所属的类。 
❓问题描述：假设一下，你有一个原型，你想复制出一个一模一样的
复制品，但不巧的是，类的某些成员（比如登录模块）是私有的。 
解决方案：在原型类实现公共方法 clone()能够返回对象的复制。 
让我们尝试通过基于操作系统文件系统的示例来理解原型模式。 操作
系统的文件系统是递归的： 文件夹中包含文件和文件夹， 其中又包
含文件和文件夹， 以此类推。 
每个文件和文件夹都可用一个 inode 接口来表示。  inode 接口中同
样也有 clone 克隆功能。 
file 文件和 folder 文件夹结构体都实现了 print 打印和 clone 方
法， 因为它们都是 inode 类型。 同时， 注意 file 和 folder 中
的 clone 方法。 这两者的 clone 方法都会返回相应文件或文件夹的
副本。 同时在克隆过程中， 我们会在其名称后面添加 “_clone” 
字样。 
原型接口： 
type inode interface { 
    print(string) 
    clone() inode 
} 
文件原型： 
type file struct { 
    name string 
} 
 
func (f *file) print(indentation string) { 
    fmt.Println(indentation + f.name) 
} 
 
func (f *file) clone() inode { 
    return &file{name: f.name + "_clone"} 
} 
文件夹原型： 
type folder struct { 
    children []inode 
    name      string 
} 
 
func (f *folder) print(indentation string) { 
    fmt.Println(indentation + f.name) 
    for _, i := range f.children { 
        i.print(indentation + indentation) 
    } 
} 
 
func (f *folder) clone() inode { 
    cloneFolder := &folder{name: f.name + "_clone"} 
    var tempChildren []inode 
    for _, i := range f.children { 
        copy := i.clone() 
        tempChildren = append(tempChildren, copy) 
    } 
    cloneFolder.children = tempChildren 
    return cloneFolder 
} 
客户端代码： 
func main() { 
    file1 := &file{name: "File1"} 
    file2 := &file{name: "File2"} 
    file3 := &file{name: "File3"} 
 
    folder1 := &folder{ 
        children: []inode{file1}, 
        name:      "Folder1", 
    } 
 
    folder2 := &folder{ 
        children: []inode{folder1, file2, file3}, 
        name:      "Folder2", 
    } 
    fmt.Println("\nPrinting hierarchy for Folder2") 
    folder2.print("  ") 
 
    cloneFolder := folder2.clone() 
    fmt.Println("\nPrinting hierarchy for clone Folder") 
    cloneFolder.print("  ") 
} 
 
 
# go 的调试/分析工具用过哪些 
go 的自带工具链相当丰富，
 go cover : 测试代码覆盖率； 
 godoc: 用于生成 go 文档； 
 pprof：用于性能调优，针对 cpu，内存和并发； 
 race：用于竞争检测； 


# 进程被 kill，如何保证所有 goroutine 顺利退出 
goroutine 监听 SIGKILL 信号，一旦接收到 SIGKILL，则立刻退出。可采用select 方法。 
```Go
var wg = &sync.WaitGroup{} 
 
func main() {  
    wg.Add(1)
    go func() { 
        c1 := make(chan os.Signal, 1) 
        signal.Notify(c1, syscall.SIGINT, syscall.SIGTERM, syscall.SIGQUIT) 
        fmt.Printf("goroutine 1 receive a signal : %v\n\n", <-c1) 
        wg.Done() 
    }() 
    wg.Wait()  
    fmt.Printf("all groutine done!\n") 
}
```

# 1 亿条数据动态增长，取 top10，怎么实现
处理 1 亿条数据动态增长并取 TOP10 可以使用堆的方法。具体的做法如下： 
定义一个大根堆，初始化它的大小为 10，用于存放前 10 大的数据。
遍历数据，对于每个新来的数据，与堆顶元素进行比较，如果新数据大于堆顶元素，
则将新数据插入到堆中，并弹出堆顶元素，以保证堆中始终是前 10 大的数据。 
遍历完所有数据后，堆中剩余元素就是前 10 大的数据，按照从大到小的顺序输出即可。 
由于堆的大小固定为 10，因此算法的时间复杂度为 O(nlogk)，其中 k 为堆的大小，也就是 10。 
需要注意的是，如果数据量非常大，可能会涉及到内存限制问题，因此可以采用分布式计算、MapReduce 等方法来处理大规模数据。 




# channel 通道 
channel（通道）用于在 goroutine 之间传递数据和同步操作。
channel 可以阻塞发送方直到接收者准备好接收数据，并且可以阻塞接收方直到有可用的数据发送。
它是 Go 中实现并发的重要机制之一。 

## channel 死锁的场景
1. 当一个 channel 中没有数据，而直接读取时，会发生死锁： 
q := make(chan int,2) 
<-q 
解决方案是采用 select 语句，再 default 放默认处理方式： 
q := make(chan int,2) 
select{ 
   case val:=<-q: 
   default: 
         ...
}

2. 当 channel 数据满了，再尝试写数据会造成死锁； 
q := make(chan int,2) 
q<-1 
q<-2 
q<-3 
解决方法，采用 select 
func main() {
    q := make(chan int, 2)
    q <- 1
    q <- 2
    select {
    case q <- 3:
        fmt.Println("ok")
    default:
        fmt.Println("wrong")
    }
}

3. 向一个关闭的 channel 写数据。 
注意：一个已经关闭的 channel，只能读数据，不能写数据。 

## 对已经关闭的 chan 进行读写会怎么样？ 
1. 读已经关闭的 chan 能一直读到东西，但是读到的内容根据通道内关闭前是否有元素而不同。
如果 chan 关闭前，buffer 内有元素还未读,会正确读到 chan 内的值，且返回的第二个 bool 值（是否读成功）为 true。
如果 chan 关闭前，buffer 内有元素已经被读完，chan 内无值，接下来所有接收的值都会非阻塞直接成功，
返回 channel 元素的零值，但是第二个 bool 值一直为 false。 
2. 写已经关闭的 chan 会 panic。 


## channel 底层实现？是否线程安全。 
channel 底层实现在 src/runtime/chan.go 中 
channel 内部是一个循环链表。
内部包含 buf, sendx, recvx, lock ,recvq, sendq 几个部分； 
buf 是有缓冲的 channel 所特有的结构，用来存储缓存数据。是个循环链表； 
sendx 和 recvx 用于记录 buf 这个循环链表中的发送或者接收的 index； 
lock 是个互斥锁； 
recvq 和 sendq 分别是接收(<-channel)或者发送(channel <- xxx)的 goroutine 抽象出来的结构体(sudog)的队列。是个双向链表。 
channel 是线程安全的。 


## 无缓冲的 channel 和有缓冲的 channel 的区别？ 
无缓冲区 channel：发送的数据如果没有被接收方接收，那么发送方阻塞；如果一直接收不到发送方的数据，接收方阻塞； 
有缓冲的 channel：发送方在缓冲区满的时候阻塞，接收方不阻塞；接收方在缓冲区为空的时候阻塞，发送方不阻塞。


## Go 语言中可以使用 channel 和 mutex 实现阻塞队列 
```Go
package main

import ( 
    "fmt" 
    "sync" 
) 

type BlockingQueue struct {  
    queue     []interface{} 
    capacity  int 
    mutex     sync.Mutex 
    notEmpty *sync.Cond 
    notFull  *sync.Cond 
}

func NewBlockingQueue(n int) *BlockingQueue {  
    bq := &BlockingQueue{
        queue:    make([]interface{}, 0, n),
        capacity: n,
    }

    bq.notEmpty = sync.NewCond(&bq.mutex)  
    bq.notFull = sync.NewCond(&bq.mutex) 
    return bq 
}

func (bq *BlockingQueue) Push(item interface{}) {  
    bq.mutex.Lock() 
    defer bq.mutex.Unlock() 
    for len(bq.queue) == bq.capacity {
        bq.notFull.Wait()
    }
    bq.queue = append(bq.queue, item) 
    bq.notEmpty.Signal()
}
 
func (bq *BlockingQueue) Pop() interface{} { 
    bq.mutex.Lock()
    defer bq.mutex.Unlock() 
    for len(bq.queue) == 0 {
        bq.notEmpty.Wait() 
    }
    item := bq.queue[0] 
    bq.queue = bq.queue[1:] 
    bq.notFull.Signal() 
    return item 
}
 
func main() { 
    bq := NewBlockingQueue(3) 
    for i := 0; i < 5; i++ {
        go func(i int) {
             bq.Push(i)
             fmt.Printf("Push %d, queue size: %d\n", i, len(bq.queue)) 
        }(i) 
    }
    for i := 0; i < 5; i++ { 
        go func() {
            item := bq.Pop() 
            fmt.Printf("Pop %v, queue size: %d\n", item, len(bq.queue)) 
        }() 
    } 
}
```
在上述代码中，使用了一个 slice 来存放队列元素，同时通过 mutex 来保证线程安全，
当队列满时生产者需要等待，当队列空时消费者需要等待。
使用 sync.Cond实现等待通知机制，当队列满时，生产者调用 notFull.Wait()方法进行等待，当队列非满时，
生产者调用 notEmpty.Signal()方法通知消费者；
当队列为空时，消费者调用 notEmpty.Wait()方法进行等待，当队列非空时，消费者调用 notFull.Signal()方法通知生产者。 
该阻塞队列可以用于多线程环境下的任务分发、消息传递等场景。

## channel 为什么需要两个队列实现？
在 Go 中，每个 channel 都包含两个队列：发送队列和接收队列。
这是因为channel 的发送和接收操作是异步进行的，发送方和接收方可能会以不同的顺序执行。 
当发送方向一个未满的 channel 发送数据时，该数据会被添加到发送队列中。
如果接收方正在等待从 channel 接收数据，则该数据将从发送队列中移除并发送给接收方。
否则，该数据将一直留在发送队列中，直到有接收方准备好接收。 

类似地，当接收方从一个非空的 channel 接收数据时，该数据会从接收队列中取出并发送给接收方。
如果发送方正在等待向该 channel 发送数据，则该数据将直接从发送方传递给接收方，而不会先放入接收队列。 
因此，通过使用这两个队列，channel 可以有效地实现多个 goroutine 之间的同步和通信。


## go 通道和锁的使用场景？ 
在 Go 语言中，协程（Goroutine）之间的通信通常使用通道（Channel）来实现，
而不需要显式地加锁。这是因为通道本身就是一种并发安全的数据结构，它可以
保证在多个协程之间进行数据传递时不会出现竞态条件（Race Condition）。 
通道的实现方式是基于消息传递的模型，即一个协程向通道中发送消息，另一个
协程从通道中接收消息。在这个过程中，通道会自动进行同步和互斥操作，保证
每个消息只能被一个协程接收，从而避免了竞态条件的发生。 
通道适用于以下场景： 
1. 协程之间需要进行数据传递和同步操作。 
2. 多个协程需要共享数据，但是不需要进行复杂的同步和互斥操作。 
3. 协程之间的数据传递是单向的，即一个协程只负责发送数据，另一个协程只负责接收数据。 

而锁适用于以下场景： 
1. 多个协程需要共享数据，并且需要进行复杂的同步和互斥操作。 
2. 协程之间的数据传递是双向的，即一个协程既可以发送数据，也可以接收数据。 
3. 需要对共享数据进行读写操作，而通道只能进行单向的数据传递。 
总之，在 Go 语言中，通道是一种更加高效、安全和简单的并发编程方式，通常
优先考虑使用通道来进行协程之间的数据传递和同步操作。而锁则适用于一些复
杂的同步和互斥操作，或者需要进行读写操作的场景。 


# select
select 可以监听 channel 上的数据流动。
select 默认是阻塞的，只有当监听的 channel中有发送或接收可以进行时才会运行，当多个 channel 都准备好的时候，select 是随机的选择一个执行的。 

## 当 select 监控多个 chan 同时到达就绪态时，如何先执行某个任务？ 
可以在子 case 再加一个 for select 语句。 
func priority_select(ch1, ch2 <-chan string) {
    for {
        select {
        case val := <-ch1:
            fmt.Println(val)
        case val2 := <-ch2:
            priority:
                for {
                    select { 
                    case val1 := <-ch1: 
                        fmt.Println(val1) 
                    default: 
                        break priority 
                    } 
                } 
            fmt.Println(val2)
        }
    } 
} 
 
## select 的实现原理
select 源码位于 src\runtime\select.go
最重要的 scase 数据结构为： 
type scase struct {  
    c    *hchan         // chan
    elem unsafe.Pointer // data element 
}
scase.c 为当前 case 语句所操作的 channel 指针，这也说明了一个 case 语句只能操作一个 channel。 
scase.elem 表示缓冲区地址：
caseRecv ： scase.elem 表示读出 channel 的数据存放地址； 
caseSend ： scase.elem 表示将要写入 channel 的数据存放地址； 
select 的主要实现位于：select.go 函数：其主要功能如下： 
\1. 锁定 scase 语句中所有的 channel 
\2. 按照随机顺序检测 scase 中的 channel 是否 ready 
2.1 如果 case 可读，则读取 channel 中数据，解锁所有的 channel，然后返回(case 
index, true) 
2.2 如果 case 可写，则将数据写入 channel，解锁所有的 channel，然后返回(case 
index, false) 
2.3 所有 case 都未 ready，则解锁所有的 channel，然后返回（default index, false） 
\3. 所有 case 都未 ready，且没有 default 语句 
3.1 将当前协程加入到所有 channel 的等待队列 
3.2 当将协程转入阻塞，等待被唤醒 
\4. 唤醒后返回 channel 对应的 case index 
4.1 如果是读操作，解锁所有的 channel，然后返回(case index, true) 
4.2 如果是写操作，解锁所有的 channel，然后返回(case index, false) 

 

## goroutine 什么情况会发生内存泄漏？如何避免。
在 Go 中内存泄露分为暂时性内存泄露和永久性内存泄露。 
暂时性内存泄露 
1. 获取长字符串中的一段导致长字符串未释放 
2. 获取长 slice 中的一段导致长 slice 未释放 
3. 在长 slice 新建 slice 导致泄漏 
4. string 相比切片少了一个容量的 cap 字段，可以把 string 当成一个只读的切片
类型。获取长 string 或者切片中的一段内容，由于新生成的对象和老的 string 或
者切片共用一个内存空间，会导致老的 string 和切片资源暂时得不到释放，造成短暂的内存泄漏；

永久性内存泄露 
1. goroutine 永久阻塞而导致泄漏 
2. time.Ticker 未关闭导致泄漏 
3. 不正确使用 Finalizer（Go 版本的析构函数）导致泄漏 


## 如果若干个 goroutine，有一个 panic 会怎么做？
有一个 panic，那么剩余 goroutine 也会退出，程序退出。如果不想程序退出，那
么必须通过调用 recover() 方法来捕获 panic 并恢复将要崩掉的程序。  

# recover

## recover 的执行时机 
recover 必须在 defer 函数中运行。recover 捕获的是祖父级调用时的异常，直接调用时无效。 
func main() { 
    recover() 
    panic(1) 
} 
// panic: 1 
直接 defer 调用也是无效。 
func main() { 
    defer recover() 
    panic(1) 
} 
// panic: 1

defer 调用时多层嵌套依然无效。 
func main() {  
    defer func() {
        func() {
            recover()
            fmt.Println("recover...")
        }()
    }()
    panic(1) 
}
// recover... 
// panic: 1 

必须在 defer 函数中直接调用才有效。 
func main() {  
    defer func() {
        recover()
        fmt.Println("recover...")
    }()
    panic("") 
}
//recover... 


## 为什么有协程泄露(Goroutine Leak)？ 
协程泄漏是指协程创建之后没有得到释放。主要原因有： 
1. 缺少接收器，导致发送阻塞 
2. 缺少发送器，导致接收阻塞 
3. 死锁。多个协程由于竞争资源导致死锁。 
4．创建协程的没有回收。 


## Go 可以限制运行时操作系统线程的数量吗？ 常见的 goroutine 操作函数有哪些？ 
可以，使用 runtime.GOMAXPROCS(num int)可以设置线程数目。该值默认为 CPU
逻辑核数，如果设的太大，会引起频繁的线程切换，降低性能。 
runtime.Gosched()，用于让出 CPU 时间片，让出当前 goroutine 的执行权限，调
度器安排其它等待的任务运行，并在下次某个时候从该位置恢复执行。 
runtime.Goexit()，调用此函数会立即使当前的 goroutine 的运行终止（终止协程），
而其它的 goroutine 并不会受此影响。runtime.Goexit 在终止当前 goroutine 前会先
执行此 goroutine 的还未执行的 defer 语句。
请注意千万别在主函数调用runtime.Goexit，因为会引发 panic。 


## 如何控制协程数目 
The GOMAXPROCS variable limits the number of operating system threads 
that can execute user-level Go code simultaneously. There is no limit to the 
number of threads that can be blocked in system calls on behalf of Go code; 
those do not count against the GOMAXPROCS limit. 
从官方文档的解释可以看到，GOMAXPROCS 限制的是同时执行用户态 Go 代
码的操作系统线程的数量，但是对于被系统调用阻塞的线程数量是没有限制的。
GOMAXPROCS 的默认值等于 CPU 的逻辑核数，同一时间，一个核只能绑定
一个线程，然后运行被调度的协程。因此对于 CPU 密集型的任务，若该值过大，
例如设置为 CPU 逻辑核数的 2 倍，会增加线程切换的开销，降低性能。对于 
I/O 密集型应用，适当地调大该值，可以提高 I/O 吞吐率。 
另外对于协程，可以用带缓冲区的 channel 来控制，下面的例子是协程数为 1024的例子 
var wg sync.WaitGroup 
ch := make(chan struct{}, 1024) 
for i:=0; i<20000; i++{ 
    wg.Add(1) 
    ch<-struct{}{} 
    go func(){ 
        defer wg.Done() 
        <-ch 
    } 
} 
wg.Wait() 

此外还可以用协程池：其原理无外乎是将上述代码中通道和协程函数解耦，并封
装成单独的结构体。常见第三方协程池库，比如 tunny 等。 


## defer 可以捕获 goroutine 的子 goroutine 吗？ 
不可以。它们处于不同的调度器 P 中。对于子 goroutine，必须通过 recover() 机
制来进行恢复，然后结合日志进行打印（或者通过 channel 传递 error），下面是一个例子： 
// 心跳函数 
func Ping(ctx context.Context) error { 
    ... code ...
    go func() {
        defer func() {
            if r := recover(); r != nil {
                log.Errorc(ctx, "ping panic: %v, stack: %v", r, string(debug.Stack()))
            }
        }()
        ... code ...
    }() 
    ... code ...
    return nil
} 


## go 竞态条件了解吗？ 
所谓竞态竞争，就是当两个或以上的 goroutine 访问相同资源时候，对资源进行读/写。 
比如 var a int = 0，有两个协程分别对 a+=1，我们发现最后 a 不一定为 2.这就是竞态竞争。 
通常我们可以用 go run -race xx.go 来进行检测。
解决方法是，对临界区资源上锁，或者使用原子操作(atomics)，原子操作的开销小于上锁。 
 
## 有三个函数，分别打印"cat", "fish","dog"要求每一个函数都用一个 goroutine，按照顺序打印 100 次。 
此题目考察 channel，用三个无缓冲 channel，如果一个 channel 收到信号则通知下一个。 
```Go
package main 

import (  
    "fmt"
    "time" 
) 
 
var dog = make(chan struct{}) 
var cat = make(chan struct{}) 
var fish = make(chan struct{}) 
 
func Dog() {
    <-fish 
    fmt.Println("dog")
    dog <- struct{}{} 
}

func Cat() {
    <-dog
    fmt.Println("cat")
    cat <- struct{}{} 
}

func Fish() { 
    <-cat
    fmt.Println("fish")
    fish <- struct{}{} 
}

func main() {  
    for i := 0; i < 100; i++ {
        go Dog()
        go Cat()
        go Fish()
    }
    fish <- struct{}{} 
 
    time.Sleep(10 * time.Second) 
}
```

## 两个协程交替打印 10 个字母和数字 
思路：采用 channel 来协调 goroutine 之间顺序。 
主线程一般要 waitGroup 等待协程退出，这里简化了一下直接 sleep。 
```Go
package main 

import (  
    "fmt"
    "time" 
)

var word = make(chan struct{}, 1) 
var num = make(chan struct{}, 1) 
 
func printNums() {  
    for i := 0; i < 10; i++ {
        <-word
        fmt.Println(1)
        num <- struct{}{}
    } 
}

func printWords() {
    for i := 0; i < 10; i++ {
        <-num
        fmt.Println("a")
        word <- struct{}{}
    }
}

func main() {
    num <- struct{}{}
    go printNums()
    go printWords()
    time.Sleep(time.Second * 1)
}
```


## 启动 2 个 groutine 2 秒后取消， 第一个协程 1 秒 执行完，第二个协程 3 秒执行完。 
思路：采用 ctx, _ := context.WithTimeout(context.Background(), time.Second*2)实现 2s 取消。
协程执行完后通过 channel 通知，是否超时。 
package main 
 
import ( 
 
"context" 
 
"fmt" 
 
"time" 
) 
 
func f1(in chan struct{}) { 
 
 
time.Sleep(1 * time.Second) 
 
in <- struct{}{} 
 
} 
 
func f2(in chan struct{}) { 
 
time.Sleep(3 * time.Second) 
 
in <- struct{}{} 
} 
 
func main() { 
 
ch1 := make(chan struct{}) 
 
ch2 := make(chan struct{}) 
 
ctx, _ := context.WithTimeout(context.Background(), 2*time.Second) 
 
 
go func() { 
 
 
go f1(ch1) 
 
 
select { 
 
 
case <-ctx.Done(): 
 
 
 
fmt.Println("f1 timeout") 
 
 
 
break 
 
 
case <-ch1: 
 
 
 
fmt.Println("f1 done") 
 
 
} 
 
}() 
 
 
go func() { 
 
 
go f2(ch2) 
 
 
select { 
 
 
case <-ctx.Done(): 
 
 
 
fmt.Println("f2 timeout") 
 
 
 
break 
 
 
case <-ch2: 
 
 
 
fmt.Println("f2 done") 
 
 
} 
 
}() 
 
time.Sleep(time.Second * 5) 
} 

 
Go 什么时候发生阻塞？阻塞时，调度器会怎么做。 
用于原子、互斥量或通道操作导致 goroutine 阻塞，调度器将把当前阻塞的
goroutine 从本地运行队列 LRQ 换出，并重新调度其它 goroutine； 
由于网络请求和 IO 导致的阻塞，Go 提供了网络轮询器（Netpoller）来处理，后
台用 epoll 等技术实现 IO 多路复用。 
其它回答： 
channel 阻塞：当 goroutine 读写 channel 发生阻塞时，会调用 gopark 函数，该 G
脱离当前的 M 和 P，调度器将新的 G 放入当前 M。 
系统调用：当某个 G 由于系统调用陷入内核态，该 P 就会脱离当前 M，此时 P
会更新自己的状态为 Psyscall，M 与 G 相互绑定，进行系统调用。结束以后，若
该 P 状态还是 Psyscall，则直接关联该 M 和 G，否则使用闲置的处理器处理该
G。 
系统监控：当某个 G 在 P 上运行的时间超过 10ms 时候，或者 P 处于 Psyscall 状
态过长等情况就会调用 retake 函数，触发新的调度。 
主动让出：由于是协作式调度，该 G 会主动让出当前的 P（通过 GoSched），更
新状态为 Grunnable，该 P 会调度队列中的 G 运行。 
更多关于 netpoller 的内容可以参看：https://strikefreedom.top/go-netpoll-io-
multiplexing-reactor 
❤Go 中 GMP 有哪些状态？ 
 
G 的状态： 
_Gidle：刚刚被分配并且还没有被初始化，值为 0，为创建 goroutine 后的默认值 
_Grunnable： 没有执行代码，没有栈的所有权，存储在运行队列中，可能在某个
P 的本地队列或全局队列中(如上图)。 
_Grunning： 正在执行代码的 goroutine，拥有栈的所有权(如上图)。 
_Gsyscall：正在执行系统调用，拥有栈的所有权，与 P 脱离，但是与某个 M 绑
定，会在调用结束后被分配到运行队列(如上图)。 
_Gwaiting：被阻塞的 goroutine，阻塞在某个 channel 的发送或者接收队列(如上
图)。 
_Gdead： 当前 goroutine 未被使用，没有执行代码，可能有分配的栈，分布在空
闲列表 gFree，可能是一个刚刚初始化的 goroutine，也可能是执行了 goexit 退出
的 goroutine(如上图)。 
_Gcopystac：栈正在被拷贝，没有执行代码，不在运行队列上，执行权在 
_Gscan ： GC 正在扫描栈空间，没有执行代码，可以与其他状态同时存在。 
P 的状态： 
_Pidle ：处理器没有运行用户代码或者调度器，被空闲队列或者改变其状态的结
构持有，运行队列为空 
_Prunning ：被线程 M 持有，并且正在执行用户代码或者调度器(如上图) 
_Psyscall：没有执行用户代码，当前线程陷入系统调用(如上图) 
_Pgcstop ：被线程 M 持有，当前处理器由于垃圾回收被停止 
_Pdead ：当前处理器已经不被使用 
M 的状态： 
自旋线程：处于运行状态但是没有可执行 goroutine 的线程，数量最多为
GOMAXPROC，若是数量大于 GOMAXPROC 就会进入休眠。 
非自旋线程：处于运行状态有可执行 goroutine 的线程。 
GMP 能不能去掉 P 层？会怎么样？ 
P 层的作用 
每个 P 有自己的本地队列，大幅度的减轻了对全局队列的直接依赖，所带来的
效果就是锁竞争的减少。而 GM 模型的性能开销大头就是锁竞争。 
每个 P 相对的平衡上，在 GMP 模型中也实现了 Work Stealing 算法，如果 P 
的本地队列为空，则会从全局队列或其他 P 的本地队列中窃取可运行的 G 来
运行，减少空转，提高了资源利用率。 
参考资料：https://juejin.cn/post/6968311281220583454 
如果有一个 G 一直占用资源怎么办？什么是 work 
stealing 算法？ 
如果有个 goroutine 一直占用资源，那么 GMP 模型会从正常模式转变为饥饿模
式（类似于 mutex），允许其它 goroutine 使用 work stealing 抢占（禁用自旋
锁）。 
work stealing 算法指，一个线程如果处于空闲状态，则帮其它正在忙的线程分
担压力，从全局队列取一个 G 任务来执行，可以极大提高执行效率。 
 
指针 
指针的作用 ok 
一个指针可以指向任意变量的地址，它所指向的地址在 32 位或 64 位机器上分别
固定占 4 或 8 个字节。指针的作用有： 
1. 获取变量的值 
import fmt 
 
func main(){ 
a := 1 
p := &a//取址& 
fmt.Printf("%d\n", *p);//取值* 
} 
2. 改变变量的值 
// 交换函数 
func swap(a, b *int) { 
    *a, *b = *b, *a 
} 
3. 用指针替代值传入函数，比如类的接收器就是这样的。 
type A struct{} 
 
func (a *A) fun(){} 
函数返回局部变量的指针是否安全？ 
这一点和 C++不同，在 Go 里面返回局部变量的指针是安全的。因为 Go 会进行
逃逸分析，如果发现局部变量的作用域超过该函数则会把指针分配到堆区，避免
内存泄漏。 
非接口的任意类型 T() 都能够调用 *T 的方法吗？
反过来呢？ 
一个 T 类型的值可以调用*T 类型声明的方法，当且仅当 T 是可寻址的。 
反之：*T 可以调用 T()的方法，因为指针可以解引用。 
 
反射 
实现使用字符串函数名，调用函数。 
思路：采用反射的 Call 方法实现。 
package main 
import ( 
 
"fmt" 
    "reflect" 
) 
 
type Animal struct{ 
     
} 
 
func (a *Animal) Eat(){ 
    fmt.Println("Eat") 
} 
 
func main(){ 
    a := Animal{} 
    reflect.ValueOf(&a).MethodByName("Eat").Call([]reflect.Value{}) 
} 
go 的 reflect 底层实现 
go reflect 源码位于 src\reflect\下面，作为一个库独立存在。反射是基于接口实现
的。 
Go 反射有三大法则： 
1. 反射从接口映射到反射对象； 
 
2. 反射从反射对象映射到接口值； 
 
3. 只有值可以修改(settable)，才可以修改反射对象。 
Go 反射基于上述三点实现。我们先从最核心的两个源文件入手 type.go 和
value.go. 
type 用于获取当前值的类型。value 用于获取当前的值。 
参考资料：The Laws of Reflection， 图解 go 反射实现原理 
 
接口 
2 个 interface 可以比较吗？ok 
Go 语言中，interface 的内部实现包含了 2 个字段，类型 T 和 值 V，interface 
可以使用 == 或 != 比较。2 个 interface 相等有以下 2 种情况： 
1. 两个 interface 均等于 nil（此时 V 和 T 都处于 unset 状态）； 
2. 类型 T 相同，且对应的值 V 相等。 
看下面的例子： 
type Stu struct { 
     Name string 
} 
 
type StuInt interface{} 
 
func main() { 
     var stu1, stu2 StuInt = &Stu{"Tom"}, &Stu{"Tom"} 
     var stu3, stu4 StuInt = Stu{"Tom"}, Stu{"Tom"} 
     fmt.Println(stu1 == stu2) // false 
     fmt.Println(stu3 == stu4) // true 
} 
stu1 和 stu2 对应的类型是 *Stu，值是 Stu 结构体的地址，两个地址不同，因
此结果为 false。 stu3 和 stu4 对应的类型是 Stu，值是 Stu 结构体，且各字段
相等，因此结果为 true。 
 
go 的 interface 怎么实现的？ 
go interface 源码在 runtime\iface.go 中。 
go 的接口由两种类型实现 iface 和 eface。iface 是包含方法的接口，而 eface 不包
含方法。 
iface 
对应的数据结构是（位于 src\runtime\runtime2.go）： 
type iface struct { 
 
tab  *itab 
 
data unsafe.Pointer 
} 
可以简单理解为，tab 表示接口的具体结构类型，而 data 是接口的值。 
itab： 
type itab struct { 
 
inter *interfacetype //此属性用于定位到具体 interface 
 
_type *_type //此属性用于定位到具体 interface 
 
hash  uint32 // copy of _type.hash. Used for type switches. 
 
_     [4]byte 
 
fun   [1]uintptr // variable sized. fun[0]==0 means _type does not implement 
inter. 
} 
属性 interfacetype 类似于_type，其作用就是 interface 的公共描述，类似的还有
maptype、arraytype、chantype…其都是各个结构的公共描述，可以理解为一种外
在的表现信息。interfaetype 和 type 唯一确定了接口类型，而 hash 用于查询和类
型判断。fun 表示方法集。 
eface 
与 iface 基本一致，但是用_type 直接表示类型，这样的话就无法使用方法。 
type eface struct { 
 
_type *_type 
 
data  unsafe.Pointer 
} 
这里篇幅有限，深入讨论可以看：深入研究 Go interface 底层实现 
 
2 个 nil 可能不相等吗？ ok 
可能不等。interface 在运行时绑定值，只有值为 nil 接口值才为 nil，但是与指针
的 nil 不相等。举个例子： 
var p *int = nil 
var i interface{} = nil 
if(p == i){ 
fmt.Println("Equal") 
} 
两者并不相同。总结：两个 nil 只有在类型相同时才相等。 
 
sync 
说说 atomic 底层怎么实现的. 
atomic 源码位于 sync\atomic。 
通过阅读源码可知，atomic 采用 CAS（CompareAndSwap）的方式实现的。所谓
CAS 就是使用了 CPU 中的原子性操作。在操作共享变量的时候，CAS 不需要对
其进行加锁，而是通过类似于乐观锁的方式进行检测，总是假设被操作的值未曾
改变（即与旧值相等），并一旦确认这个假设的真实性就立即进行值替换。本质
上是不断占用 CPU 资源来避免加锁的开销。 
❤mutex 有几种模式？ 
mutex 有两种模式：normal 和 starvation 
正常模式 
所有 goroutine 按照 FIFO 的顺序进行锁获取，被唤醒的 goroutine 和新请求锁的
goroutine 同时进行锁获取，通常新请求锁的 goroutine 更容易获取锁(持续占有
cpu)，被唤醒的 goroutine 则不容易获取到锁。公平性：否。 
饥饿模式 
所有尝试获取锁的 goroutine 进行等待排队，新请求锁的 goroutine 不会进行锁获
取(禁用自旋)，而是加入队列尾部等待获取锁。公平性：是。 
参考链接：Go Mutex 饥饿模式，GO 互斥锁（Mutex）原理 
 
GC 和内存管理 
你是否主动关闭过 http 连接，为啥要这样做？ok 
有关闭，不关闭会程序可能会消耗完 socket 描述符。有如下 2 种关闭方式： 
1. 直接设置请求变量的 Close 字段值为 true，每次请求结束后就会主动关闭连
接。设置 Header 请求头部选项 Connection: close，然后服务器返回的响应头部
也会有这个选项，此时 HTTP 标准库会主动断开连接 
// 主动关闭连接 
func main() { 
 req, err := http.NewRequest("GET", "http://golang.org", nil) 
 checkError(err) 
 
 req.Close = true 
 //req.Header.Add("Connection", "close") // 等效的关闭方式 
 
 resp, err := http.DefaultClient.Do(req) 
 if resp != nil { 
  defer resp.Body.Close() 
 } 
 checkError(err) 
 
 body, err := ioutil.ReadAll(resp.Body) 
 checkError(err) 
 
 fmt.Println(string(body)) 
} 
2. 你可以创建一个自定义配置的 HTTP transport 客户端，用来取消 HTTP 全局
的复用连接。 
func main() { 
 tr := http.Transport{DisableKeepAlives: true} 
 client := http.Client{Transport: &tr} 
 
 resp, err := client.Get("https://golang.google.cn/") 
 if resp != nil { 
  defer resp.Body.Close() 
 } 
 checkError(err) 
 
 fmt.Println(resp.StatusCode) // 200 
 
 body, err := ioutil.ReadAll(resp.Body) 
 checkError(err) 
 
 fmt.Println(len(string(body))) 
} 
 
Go GC 有几个阶段 
目前的 go GC 采用三色标记法和混合写屏障技术。 
Go GC 有四个阶段: 
1. STW，开启混合写屏障，扫描栈对象； 
2. 将所有对象加入白色集合，从根对象开始，将其放入灰色集合。每次从灰色集
合取出一个对象标记为黑色，然后遍历其子对象，标记为灰色，放入灰色集合； 
如此循环直到灰色集合为空。剩余的白色对象就是需要清理的对象。 
3. STW，关闭混合写屏障； 
4. 在后台进行 GC（并发）。 
 
1.标记阶段（Marking Phase）：标记阶段是垃圾收集器的第一个阶段，主要任务
是识别不再使用的对象并将其标记为“垃圾”。 
2.清扫阶段（Sweeping Phase）：清扫阶段是垃圾收集器的第二个阶段，主要任务
是回收被标记为“垃圾”的对象所占用的内存。 
3.整理阶段（Compacting Phase）：整理阶段是垃圾收集器的最后一个阶段，主要
任务是对已经回收的空间进行整理，以便在之后的内存分配中可以更容易地找到
连续的可用内存块。请注意，整理阶段只有在使用“压缩式垃圾回收”时才会发生。 

❤golang 的内存管理的原理清楚吗？简述 go 内存管理机制。 
golang 内存管理基本是参考 tcmalloc 来进行的。go 内存管理本质上是一个内存
池，只不过内部做了很多优化：自动伸缩内存池大小，合理的切割内存块。 
一些基本概念： 
页 Page：一块 8K 大小的内存空间。Go 向操作系统申请和释放内存都是以页
为单位的。 
span : 内存块，一个或多个连续的 page 组成一个 span。如果把 page 比喻成
工人， span 可看成是小队，工人被分成若干个队伍，不同的队伍干不同的
活。  
sizeclass : 空间规格，每个 span 都带有一个 sizeclass ，标记着该 span 中
的 page 应该如何使用。使用上面的比喻，就是 sizeclass 标志着 span 是一
个什么样的队伍。  
object : 对象，用来存储一个变量数据内存空间，一个 span 在初始化时，会被
切割成一堆等大的 object 。假设 object 的大小是 16B ， span 大小是 
8K ，那么就会把 span 中的 page 就会被初始化 8K/16B = 512 个 object 。所
谓内存分配，就是分配一个 object 出去。 
mheap 
一开始 go 从操作系统索取一大块内存作为内存池，并放在一个叫 mheap 的内存
池进行管理，mheap 将一整块内存切割为不同的区域，并将一部分内存切割为合
适的大小。 
mheap.spans ：用来存储 page 和 span 信息，比如一个 span 的起始地址是多少，
有几个 page，已使用了多大等等。 
mheap.bitmap 存储着各个 span 中对象的标记信息，比如对象是否可回收等等。 
mheap.arena_start : 将要分配给应用程序使用的空间。 
mcentral 
用途相同的 span 会以链表的形式组织在一起存放在 mcentral 中。这里用途用
sizeclass 来表示，就是该 span 存储哪种大小的对象。 
找到合适的 span 后，会从中取一个 object 返回给上层使用。 
mcache 
为了提高内存并发申请效率，加入缓存层 mcache。每一个 mcache 和处理器 P 对
应。Go 申请内存首先从 P 的 mcache 中分配，如果没有可用的 span 再从 mcentral
中获取。 
参考资料：Go 语言内存管理（二）：Go 内存管理 
 
05 ❤简述 Go 语言 GC(垃圾回收)的工作原理 ok 
垃圾回收机制是 Go 一大特(nan)色(dian)。Go1.3 采用标记清除法， Go1.5 采用
三色标记法，Go1.8 采用三色标记法+混合写屏障。 
*标记清除法* 
分为两个阶段：标记和清除 
标记阶段：从根对象出发寻找并标记所有存活的对象。 
清除阶段：遍历堆中的对象，回收未标记的对象，并加入空闲链表。 
缺点是需要暂停程序 STW。 
*三色标记法*： 
将对象标记为白色，灰色或黑色。 
白色：不确定对象（默认色）；黑色：存活对象。灰色：存活对象，子对象待处
理。 
标记开始时，先将所有对象加入白色集合（需要 STW）。首先将根对象标记为
灰色，然后将一个对象从灰色集合取出，遍历其子对象，放入灰色集合。同时将
取出的对象放入黑色集合，直到灰色集合为空。最后的白色集合对象就是需要清
理的对象。 
这种方法有一个缺陷，如果对象的引用被用户修改了，那么之前的标记就无效了。
因此 Go 采用了写屏障技术，当对象新增或者更新会将其着色为灰色。 
一次完整的 GC 分为四个阶段： 
1. 准备标记（需要 STW），开启写屏障。 
2. 开始标记 
3. 标记结束（STW），关闭写屏障 
4. 清理（并发） 
基于插入写屏障和删除写屏障在结束时需要 STW 来重新扫描栈，带来性能瓶颈。
混合写屏障分为以下四步： 
1. GC 开始时，将栈上的全部对象标记为黑色（不需要二次扫描，无需 STW）； 
2. GC 期间，任何栈上创建的新对象均为黑色 
3. 被删除引用的对象标记为灰色 
4. 被添加引用的对象标记为灰色 
总而言之就是确保黑色对象不能引用白色对象，这个改进直接使得 GC 时间从 
2s 降低到 2us。 
02 ❤如何知道一个对象是分配在栈上还是堆上？ 
Go 和 C++不同，Go 局部变量会进行逃逸分析。如果变量离开作用域后没有被引
用，则优先分配到栈上，否则分配到堆上。那么如何判断是否发生了逃逸呢？ 
go build -gcflags '-m -m -l' xxx.go. 
关于逃逸的可能情况：变量大小不确定，变量类型不确定，变量分配的内存超过
用户栈最大值，暴露给了外部指针。 
 
 

 


## rabbitMQ
rabbitmqctl list_queues  查看队列，并且显示消息数；
rabbitmqctl list_exchanges  查看交换机；
http://10.50.7.108:15672    浏览器登录页面，可以查看消息队列集群概况；


消息的持久化是指当消息从交换机发送到队列之后，被消费者消费之前，服务器突然宕机重启，消息仍然存在。
消息持久化的前提是队列持久化，假如队列不是持久化，那么消息的持久化毫无意义。

exchange交换机
auto_delete 设为true自动删除，设为0不自动删除，但是什么情况下可以自动删除呢，当没有队列和交换机绑定时，交换机会自动删除。
durable：设置是否持久化。durable设置为true为持久化，反之为非持久化。持久化可以将交换器存盘，在服务器重启的时候不会丢失相关信息。
internal：设置是否是内置的。设置为true，则表示是内置的交换器，客户端程序无法直接发送消息到这个交换器中，只能通过交换器路由到交换器这种方式。

queue队列
autoDelete：是否自动删除。true自动删除。自动删除的前提是至少有一个消费者连接到这个队列，之后所有与这个队列连接的客户端都断开时才会自动删除。
exclusive：是否是排他。true排他，排他队列只对首次创建它的连接可见，并在连接断开的时候自动删除。这里需要直以三点他是基于连接可见的，同一个连接（Connection）的不同信道（Channel）是可以同时访问同一个连接常见的排他队列；如果一个连接创建了一个排他队列，其他连接不允许创建同名的排他队列；排他队列即使是持久化的，以但连接关闭或客户端退出，该队列都会被删除。

routingKey：用来绑定交换器和队列的路由键。

### rabbitmq的三种模式
1. 单机模式
2. 普通集群模式
意思就是在多台机器上启动多个 RabbitMQ 实例，每台机器启动一个。
你创建的 queue，只会放在一个 RabbitMQ 实例上，但是每个实例都同步 queue 的元数据（元数据可以认为是 queue 的一些配置信息， 
通过元数据，可以找到 queue 所在实例）。你消费的时候，实际上如果连接到了另外一个实例，那么那个实例会从 queue 所在实例上拉取数据过来。
3. 镜像集群模式
在镜像集群模式下，你创建的 queue，无论是元数据还是 queue 里的消息都会存在于多个实例上，
就是说，每个 RabbitMQ 节点都有这个 queue 的一个完整镜像，包含 queue 的全部数据的意思。
然后每次你写消息到 queue 的时候，都会自动把消息同步到多个实例的 queue 上。

## rocketmq

新建producer时，如果多个producer group名称相同会报错producer group has been created, specify another one，解决就是每个producer group名称不要一致；

针对消息分类，您可以选择创建多个Topic，或者在同一个Topic下创建多个Tag。
发布消息时topic如果不存在就新建，存在直接使用；如果创建producer指定命名空间时，topic名称变为namespace%topic；


报错send message error: [REJECTREQUEST]system busy, start flow control for a while
调优：
#主从同步模式
brokerRole=ASYNC_MASTER
#消息发送队列等待时间，默认200
waitTimeMillsInSendQueue=400
#发送消息的最大线程数，默认1，因为服务器配置不同，具体多少需要压测后取最优值
sendMessageThreadPoolNums=32
#发送消息是否使用可重入锁（4.1版本以上才有）
useReentrantLockWhenPutMessage=true
#发送消息线程等待时间，默认200ms
waitTimeMillsInSendQueue=1000
#开启临时存储池
transientStorePoolEnable=true
#开启Slave读权限（分担master 压力）
slaveReadEnable=true
#关闭堆内存数据传输
transferMsgByHeap=false
#开启文件预热
warmMapedFileEnable=true




## lru
最近最少使用，least recently used
两个核心的数据结构：map(存储键和值的映射关系)+双向链表(所有值放到链表中)；







# flag
// 定义命令行参数
name := flag.String("name", "World", "Name to greet")
// 定义短参数
age := flag.Int("a", 0, "Age")
// 解析命令行参数
flag.Parse()



# regexp
// 定义正则表达式
r := regexp.MustCompile(`\d+`)
// 在字符串中查找匹配项
match := r.FindString("abc123def")


## Go基础
1. 单引号：表示byte类型或rune类型，对应 uint8和int32类型，默认是 rune 类型。
    byte用来强调数据是raw data，而不是数字；而rune用来表示Unicode的code point。
2. 双引号，才是字符串，实际上是字符数组。可以用索引号访问某字节，也可以用len()函数来获取字符串所占的字节长度。
3. 反引号，表示字符串字面量，但不支持任何转义序列。
4. Go没有Set类型，可以通过map实现，值可以用一个常量替代，比如不占任何内存的空结构体；
5. Go中的_
    1. 忽略返回值；
    2. 接口断言； type T struct{}  var _ X = T{}；判断 type T是否实现了X,用作类型断言，如果T没有实现接口X，则编译错误；
       或者var _ PeerPicker = (*HTTPPool)(nil)；判断HTTPPool是否实现PeerPicker
    3. 导包时，只初始化包，不使用包中的功能；
6. map，slice，chan 是引用拷贝；引用拷贝是浅拷贝,其余的，都是值拷贝；值拷贝是深拷贝
    深浅拷贝的本质区别：是否真正获取对象实体，而不是引用
    深拷贝： 拷贝的是数据本身，创造一个新的对象，并在内存中开辟一个新的内存地址，与原对象是完全独立的，不共享内存，
    修改新对象时不会影响原对象的值。释放内存时，也没有任何关联。
    如果切片传递后进行了扩容，函数内的修改不会影响到函数外的切片。
7. 从数组中截取切片
    1. 如果没有指定max,则max的值为截取对象的容量；
    2. 如果指定max,则max的值不能超过截取对象的容量；
    3. 利用数组创建切片，切片操作的是同一个数组；
8. map的key需要是可比的；map使用前一定要初始化；不是线程安全的；map循环是无序的；
9. map 中删除一个 key，它的内存会释放么？
    如果删除的元素是值类型，如int，float，bool，string以及数组和struct，map的内存不会自动释放
    如果删除的元素是引用类型，如指针，slice，map，chan等，map的内存会自动释放，但释放的内存是子元素应用类型的内存占用
    将map设置为nil后，内存被回收。
10. 可以对未初始化的map取值，但是取出来的东西是空；不能对未初始化的map赋值；nil map未初始化，空map长度为0；
11. 遍历map为啥不是有序的？
    1. map底层实现是哈希表，在进行插入数据时，会对key进行hash运算，这也就导致了数据不是按顺序存储的，和遍历的顺序也就会不一致；
    2. map 在扩容后，会发生 key 的搬迁，原来落在同一个 bucket 中的 key，搬迁后，有些 key 可能就到其他 bucket 了；
        而遍历的过程，就是按顺序遍历 bucket，同时按顺序遍历 bucket 中的 key。 
        搬迁后，key 的位置发生了重大的变化，有些 key 被搬走了，有些 key 则原地不动。这样，遍历 map 的结果就不可能按原来的顺序了。
    3. 在遍历 map 时，并不是固定地从 0 号 bucket 开始遍历，每次都是从一个随机值序号的 bucket 开始遍历，并且是从这个 bucket 的一个随机序号的 cell 开始遍历。
12. map三种并发安全的方式
    1. 读写锁；
    2. 分片加锁；将这个 map 分成 n 块，每个块之间的读写操作都互不干扰；
        type SafeMap2 struct {
            Maps               [N]map[string]string
            locks              [N]sync.RWMutex 
        }
    3. sync.Map
13. 数字类型
```Go
int8: represents 8 bit signed integers
size: 8 bits
range: -128 to 127

int16: represents 16 bit signed integers
size: 16 bits
range: -32768 to 32767

int32: represents 32 bit signed integers
size: 32 bits
range: -2147483648 to 2147483647

int64: represents 64 bit signed integers
size: 64 bits
range: -9223372036854775808 to 9223372036854775807

int: represents 32 or 64 bit integers depending on the underlying platform. You should generally be using int to represent integers unless there is a need to use a specific sized integer.
size: 32 bits in 32 bit systems and 64 bit in 64 bit systems.
range: -2147483648 to 2147483647 in 32 bit systems and -9223372036854775808 to 9223372036854775807 in 64 bit systems

uint8: represents 8 bit unsigned integers
size: 8 bits
range: 0 to 255

uint16: represents 16 bit unsigned integers
size: 16 bits
range: 0 to 65535

uint32: represents 32 bit unsigned integers
size: 32 bits
range: 0 to 4294967295

uint64: represents 64 bit unsigned integers
size: 64 bits
range: 0 to 18446744073709551615

uint : represents 32 or 64 bit unsigned integers depending on the underlying platform.
size : 32 bits in 32 bit systems and 64 bits in 64 bit systems.
range : 0 to 4294967295 in 32 bit systems and 0 to 18446744073709551615 in 64 bit systems
```
14. byte 其实被 alias 到 uint8 上了;
15. golang语言中没有继承概念，只有组合，也没有虚方法，更没有重载。因此，不会覆写被组合的结构体的方法。
16. Go 程序会在两个地方为变量分配内存，一个是全局的堆上，另一个是函数调用栈；
Go 中声明一个函数内局部变量时，当编译器发现变量的作用域没有逃出函数范围时，就会在栈上分配内存，反之则分配在堆上，逃逸分析由编译器完成，作用于编译阶段。
17. strconv.Itoa(num)数字转字符串；strconv.Atoi(str)字符串转数字


= 和 := 的区别？ok 
=是赋值变量，:=是定义变量。 
❤ 如何高效地拼接字符串 ok 
拼接字符串的方式有：+ , fmt.Sprintf , strings.Builder, bytes.Buffer, strings.Join 
1、"+" 
使用+操作符进行拼接时，会对字符串进行遍历，计算并开辟一个新的空间来存
储原来的两个字符串。 
2、fmt.Sprintf 
由于采用了接口参数，必须要用反射获取值，因此有性能损耗。 
3、strings.Builder： 
用 WriteString()进行拼接，内部实现是指针+切片，同时 String()返回拼接后的字
符串，它是直接把[]byte 转换为 string，从而避免变量拷贝。 
4、bytes.Buffer 
bytes.Buffer 是一个一个缓冲 byte 类型的缓冲器，这个缓冲器里存放着都是 byte， 
bytes.buffer 底层也是一个[]byte 切片。 
5、strings.join 
strings.join 也是基于 strings.builder 来实现的，并且可以自定义分隔符，在 join 方
法内调用了 b.Grow(n)方法，这个是进行初步的容量分配，而前面计算的 n 的长
度就是我们要拼接的 slice 的长度，因为我们传入切片长度固定，所以提前进行
容量分配可以减少内存分配，很高效。 
性能比较： 
strings.Join ≈ strings.Builder > bytes.Buffer > "+" > fmt.Sprintf 
5 种拼接方法的实例代码 
func main(){ 
 
a := []string{"a", "b", "c"} 
 
//方式 1：+ 
 
ret := a[0] + a[1] + a[2] 
 
//方式 2：fmt.Sprintf 
 
ret := fmt.Sprintf("%s%s%s", a[0],a[1],a[2]) 
 
//方式 3：strings.Builder 
 
var sb strings.Builder 
 
sb.WriteString(a[0]) 
 
sb.WriteString(a[1]) 
 
sb.WriteString(a[2]) 
 
ret := sb.String() 
 
//方式 4：bytes.Buffer 
 
buf := new(bytes.Buffer) 
 
buf.Write(a[0]) 
 
buf.Write(a[1]) 
 
buf.Write(a[2]) 
 
ret := buf.String() 
 
//方式 5：strings.Join 
 
ret := strings.Join(a,"") 
} 
什么是 rune 类型 ok 
ASCII 码只需要 7 bit 就可以完整地表示，但只能表示英文字母在内的 128 个字
符，为了表示世界上大部分的文字系统，发明了 Unicode， 它是 ASCII 的超集，
包含世界上书写系统中存在的所有字符，并为每个代码分配一个标准编号（称为
Unicode CodePoint），在 Go 语言中称之为 rune，是 int32 类型的别名。 
Go 语言中，字符串的底层表示是 byte (8 bit) 序列，而非 rune (32 bit) 序列。 
golang 中 string 底层是通过 byte 数组实现的。中文字符在 unicode 下占 2 个字
节，在 utf-8 编码下占 3 个字节，而 golang 默认编码正好是 utf-8。 
func main() { 
 
var str = "Go 编程语言" 
 
for i := 0;i < len(str);i++ { 
 
 
fmt.Printf("%c ", str[i]) 
 
} 
 
fmt.Println() 
 
runes := []rune(str) 
 
for i := 0;i < len(runes);i++ { 
 
 
fmt.Printf("%c", runes[i]) 
 
} 
} 
打印： 
G o ç ¼ –  ç ¨ ‹  è ¯  - è ¨ 
Go 编程语言 
为什么会出现这种情况呢，原因就是 UTF-8 编码的中文它不是只占一个字节。因此
我们试图打印字符，假设每个代码点都是一个字节长，这是错误的。 
在 UTF-8 编码中，一个代码点可以占用 1 个以上的字节。 
rune 是 Go 中的内置类型，它是 int32 的别名。rune 代表 Go 中的 unicode 代码点。
代码点占用多少字节并不重要，可以用一个符文来表示。 
如何判断 map 中是否包含某个 key ？ok 
var sample map[int]int 
if _, ok := sample[10]; ok { 
 
} else { 
 
} 
Go 支持默认参数或可选参数吗？ok 
不支持。但是可以利用结构体参数，或者...传入参数切片数组。 
// 这个函数可以传入任意数量的整型参数 
func sum(nums ...int) { 
  total := 0 
  for _, num := range nums { 
      total += num 
  } 
  fmt.Println(total) 
} 
defer 的执行顺序 
defer 执行顺序和调用顺序相反，类似于栈后进先出(LIFO)。 
defer 在 return 之后执行，但在函数退出之前，defer 可以修改返回值。下面是一
个例子： 
func test() int { 
 
i := 0 
 
defer func() { 
 
 
fmt.Println("defer1") 
 
}() 
 
defer func() { 
 
 
i += 1 
 
 
fmt.Println("defer2") 
 
}() 
 
return i 
} 
 
func main() { 
 
fmt.Println("return", test()) 
} 
 
// defer2 
// defer1 
// return 0 
上面这个例子中，test 返回值并没有修改，这是由于 Go 的返回机制决定的，执
行 Return 语句后，Go 会创建一个临时变量保存返回值。如果是有名返回（也就
是指明返回值 func test() (i int)） 
func test() (i int) { 
 
i = 0 
 
defer func() { 
 
 
i += 1 
 
 
fmt.Println("defer2") 
 
}() 
 
return i 
} 
 
func main() { 
 
fmt.Println("return", test()) 
} 
// defer2 
// return 1 
这个例子中，返回值被修改了。对于有名返回值的函数，执行 return 语句时，并
不会再创建临时变量保存，因此，defer 语句修改了 i，即对返回值产生了影响。 
func main() { 
 
var a bool = true 
 
defer func(){ 
 
 
fmt.Println("1") 
 
}() 
 
if a == true { 
 
 
fmt.Println("2") 
 
 
return 
 
} 
 
defer func(){ 
 
 
fmt.Println("3") 
 
}() 
} 
// 输出 2 1 
// defer 关键字后面的函数或者方法想要执行必须先注册，return 之后的 defer 
是不能注册的， 也就不能执行后面的函数或方法 
 
如何交换 2 个变量的值？ok 
对于变量而言 a,b = b,a； 对于指针而言*a,*b = *b, *a 
Go 语言 tag 的用处？ 
tag 可以为结构体成员提供属性。常见的： 
1. json 序列化或反序列化时字段的名称 
2. db: sqlx 模块中对应的数据库字段名 
3. form: gin 框架中对应的前端的数据字段名 
4. binding: 搭配 form 使用, 默认如果没查找到结构体中的某个字段则不报错值
为空, binding 为 required 代表没找到返回错误给前端 
 
如何获取一个结构体的所有 tag？ 
利用反射： 
import reflect 
type Author struct { 
 
Name         int      `json:Name` 
 
Publications []string `json:Publication,omitempty` 
} 
 
func main() { 
 
t := reflect.TypeOf(Author{}) 
 
for i := 0; i < t.NumField(); i++ { 
 
 
name := t.Field(i).Name 
 
 
s, _ := t.FieldByName(name) 
 
 
fmt.Println(name, s.Tag) 
 
} 
} 
上述例子中，reflect.TypeOf 方法获取对象的类型，之后 NumField()获取结构体成
员的数量。 通过 Field(i)获取第 i 个成员的名字。 再通过其 Tag 方法获得标签。 
结构体打印时，%v 和 %+v 的区别 ok 
%v 输出结构体各成员的值； 
%+v 输出结构体各成员的名称和值； 
%#v 输出结构体名称和结构体各成员的名称和值 
 
Go 语言中如何表示枚举值(enums)？ 
在常量中用 iota 可以表示枚举。iota 从 0 开始。 
const ( 
 
B = 1 << (10 * iota) 
 
KiB  
 
MiB 
 
GiB 
 
TiB 
 
PiB 
 
EiB 
) 
空 struct{} 的用途 ok 
1、用 map 模拟一个 set，那么就要把值置为 struct{}，struct{}本身不占任何空间，
可以避免任何多余的内存分配。 
type Set map[string]struct{} 
 
func main() { 
 
set := make(Set) 
 
 
for _, item := range []string{"A", "A", "B", "C"} { 
 
 
set[item] = struct{}{} 
 
} 
 
fmt.Println(len(set)) // 3 
 
if _, ok := set["A"]; ok { 
 
 
fmt.Println("A exists") // A exists 
 
} 
} 
2、有时候给通道发送一个空结构体,channel<-struct{}{}，也是节省了空间。 
func main() { 
 
ch := make(chan struct{}, 1) 
 
go func() { 
 
 
<-ch 
 
 
// do something 
 
}() 
 
ch <- struct{}{} 
 
// ... 
} 
3、仅有方法的结构体 
type Lamp struct{} 
go 里面的 int 和 int32 是同一个概念吗？ok 
不是一个概念！千万不能混淆。go 语言中的 int 的大小是和操作系统位数相关的，
如果是 32 位操作系统，int 类型的大小就是 4 字节。如果是 64 位操作系统，int
类型的大小就是 8 个字节。除此之外 uint 也与操作系统有关。 
int8 占 1 个字节，int16 占 2 个字节，int32 占 4 个字节，int64 占 8 个字节。 
init() 函数是什么时候执行的？ok 
简答： 在 main 函数之前执行。 
详细：init()函数是 go 初始化的一部分，由 runtime 初始化每个导入的包，初始化
不是按照从上到下的导入顺序，而是按照解析的依赖关系，没有依赖的包最先初
始化。 
每个包首先初始化包作用域的常量和变量（常量优先于变量），然后执行包的 init()
函数。同一个包，甚至是同一个源文件可以有多个 init()函数。init()函数没有入参
和返回值，不能被其他函数调用，同一个包内多个 init()函数的执行顺序不作保
证。 
执行顺序：import –> const –> var –>init()–>main() 
一个文件可以有多个 init()函数！ 
init 函数非常特殊： 
• 
初始化不能采用初始化表达式初始化的变量； 
• 
程序运行前执行注册 
• 
实现 sync.Once 功能 
• 
不能被其它函数调用 
• 
init 函数没有入口参数和返回值： 
• 
每个包可以有多个 init 函数，每个源文件也可以有多个 init 函数。 
• 
同一个包的 init 执行顺序，golang 没有明确定义，编程时要注意程序不要依
赖这个执行顺序。 
• 
不同包的 init 函数按照包导入的依赖关系决定执行顺序。

 

uint 型变量值分别为 1，2，它们相减的结果是多少？ 
var a uint = 1 
var b uint = 2 
fmt.Println(a - b) // 结果会溢出，打印 18446744073709551615，如果是 32 位系统，结
果是 2^32-1，如果是 64 位系统，结果 2^64-1； 
fmt.Println(math.MinInt, uint(math.MaxInt * 2)) //-9223372036854775808 
18446744073709551614 
uint 范围：32 位系统 4 个字节--0~2^32-1；64 位系统 8 个字节—0~2^64-1。 
下面这句代码是什么作用，为什么要定义一个空
值？ 
type GobCodec struct{ 
conn io.ReadWriteCloser 
buf *bufio.Writer 
dec *gob.Decoder 
enc *gob.Encoder 
} 
 
type Codec interface { 
io.Closer 
ReadHeader(*Header) error 
ReadBody(interface{}) error 
Write(*Header, interface{}) error 
} 

var _ Codec = (*GobCodec)(nil) 
答：将 nil 转换为 GobCodec 类型，然后再转换为 Codec 接口，如果转换失败，说明 GobCodec 没有实现 Codec 接口的所有方法。 



fallthrough 
如果在执行完每个分支的代码后，还希望继续执行后续分支的代码，可以使
用 fallthrough 关键字来达到目的。 
 
 
nil 
nil 只能赋值给指针、chan、func、interface、map 或 slice 类型的变量，也可以
赋给 error； 
 
iota 
func main() { 
 
k := 6 
 
switch k { 
 
case 4: 
 
 
fmt.Println("was <= 4") 
 
 
fallthrough 
 
case 5: 
 
 
fmt.Println("was <= 5") 
 
 
fallthrough 
 
case 6: 
 
 
fmt.Println("was <= 6") 
 
 
fallthrough 
 
case 7: 
 
 
fmt.Println("was <= 7") 
 
 
fallthrough 
 
case 8: 
 
 
fmt.Println("was <= 8") 
 
 
fallthrough 
 
default: 
 
 
fmt.Println("default case") 
 
} 
} 
// was <= 6 
// was <= 7 
// was <= 8 
// default case 
1、 iota 只能在常量的表达式中使用； 
2、 每次 const 出现时，都会让 iota 初始化为 0.【自增长】； 
Go 中 Label 使用 
Go 语言中有 goto 这个功能，在处理多级嵌套时非常有用。 
Go 语言支持 label（标签）语法：分别是 break label 和 goto label 、continue label； 
break label 
break 一般用来跳出当前所在的循环，但是我们有业务场景，需要使用到跳出带
外层循环怎么办？break label 跳出循环不再执行 for 循环里的代码。 
func main() { 
LABEL1: 
 
for i := 0;i < 3;i++ { 
 
 
if i == 2 { 
 
 
 
break LABEL1 
 
 
} 
 
 
fmt.Println("i==", i) 
 
} 
} 
//i== 0 
//i== 1 
break 标签只能用于 for 循环，不能和 switch 使用，在其他语言里 switch 与 break
是搭档； 
goto label 
goto 可以无条件的跳转执行的位置，但是不能跨函数，需要配合标签使用。 
func main() { 
 
fmt.Println("start") 
 
goto LABEL1 
 
fmt.Println("goto ...")    //not run 
LABEL1: 
 
fmt.Println("label1...") 
 
goto LABEL1 
 
fmt.Println("end")  //not run 
} 
//start 
//label1 
//label1 
//...  一直循环 label1 
continue label 
continue 是继续循环下一个迭代，继续的是最外层循环。 
func main() { 
LABEL1: 
 
for i := 0;i < 3;i++{ 
 
 
for j := 0;j < 3;j++{ 
 
 
 
if j == 1{ 
 
 
 
 
continue LABEL1 
 
 
 
} 
 
 
 
fmt.Printf("i==%v, j==%v\n", i, j) 
 
 
} 
 
} 
} 
//i==0, j==0 
//i==1, j==0 
//i==2, j==0 
 
 
切片 
数组 
定义：arr := [...]int{1, 2, 3}或 arr := [3]int{1, 2, 3}     
如果数组中元素的个数小于或者等于 4 个，那么所有的变量会直接在栈上初始
化，如果数组元素大于 4 个，变量就会在静态存储区初始化然后拷贝到栈上； 
数组是固定产长度的，不能动态扩容，在编译期就确定大小。 
 
 
numbers = append(numbers, 2,3,4) ---往 numbers 中添加多个元素 
将一个切片 append 到另一个切片中  append(slice1, slice...)，返回新的切片 
copy(numbers1,numbers) ---拷贝 numbers 的内容到 numbers1 
delete() 函数用于删除集合的元素, 参数为 map 和其对应的 key 
当切片作为参数传递给函数时，函数内部所做的更改在函数外部也可见；而数组
不可见； 
切片初始化三种方式： 
1、使用下标获取数组或者切片的一部分；不会拷贝原数组或者原切片中的数据，
它只会创建一个指向原数组的切片结构体，所以修改新切片的数据也会修改原切
片。 
2、字面定义；sl := []int{1, 2, 3} --创建一个数组返回一个切片引用； 
3、make([]T, length, capacity)    必须要有 length 参数，len() ---长度 ，cap() ---
容量 
 
切片初始化加索引 
 
 
append 使用 
func main() { 
 
var a = []int{2: 3, 4, 0: 0} 
 
fmt.Println(a) 
} 
// 索引 0 值为 0，索引 2 值为 3，没有指定索引的元素会在前一个索引基础之上
加一 
切片在扩容时，容量的扩展规律是按容量的 2 倍数进行扩充。 
 
 
 
 
切片拷贝三种方式 
1、使用=操作符拷贝切片，这种就是浅拷贝 
2、使用[:]下标的方式复制切片，这种也是浅拷贝 
3、使用 Go 语言的内置函数 copy()进行切片拷贝，这种就是深拷贝 
深浅拷贝都是进行复制，区别在于复制出来的新对象与原来的对象在它们发生改
s := make([]int, 5)  # s = [0 0 0 0 0] 
r := append(s, 1, 2, 3) # r = [0 0 0 0 0 1 2 3] 
fmt.Println(r, len(r), cap(r)) # len(r)=8, cap(r)=10 
 
// 两个切片追加在一起 
s1 := []int{1, 2, 3} 
s2 := []int{4, 5} 
fmt.Println(append(s1, s2...))  # [1 2 3 4 5] 
 
a := [5]int{1, 2, 3, 4, 5} 
t := a[3:4:4] # 意思切取索引 3-->4(不包含 4)，第三个参数用来限制新
切片的容量，切片容量为第三个参数-第一个参数，如果第二个参数省略的话，
默认为切片的长度； 
fmt.Println(t[0]) # 4 
 
s1 := []int{1, 2, 3} 
 
s2 := s1[1:] 
 
s2[1] = 5 
 
fmt.Println(s1, "\n", s2) 
// golang 切片底层的数据结构是数组，当使用 s1[1:] 获得切片 s2，和 s1 共享
同一个底层数组，这会导致 s2[1] = 4 语句影响 s1 
变时，是否会相互影响，本质区别就是复制出来的对象与原对象是否会指向同一
个地址 
 
 
如何判断 2 个字符串切片（slice) 是相等的？ 
reflect.DeepEqual() ， 但反射非常影响性能。 
 
map 
map 基本用法 
x := map[string]string{"one": "1", "two": "2"} 
if v, ok := x["two"]; ok { 
   fmt.Println("ok==", ok, v) 
} 
x["two]返回----值，是否存在 
 
通过 make 函数定义 map 
make(map[string]int) 
mapCreated := make(map[string]float32)相当于：mapCreated := map[string]float32{}. 
 
 
如果只是 var 声明一个 map，此时不能添加新 key 到 map 中。 
如果 key 没有在 map 中，取值时去 type 的默认值； 
m := map[string]int{"1": 1, "2": 2} 
fmt.Println("value is", m["3"]) 
//value is 0 
 
delete(map, key)  --移除 map 中的元素，如果 key 不存在，该操作不会产生错误； 
len(map) ---可以获得 map 的长度 
map 和 slice 一样时引用类型 
不能通过==进行比较，==只能判断 map 是否是 nil； 
 
 
 
map 的排序 
// for-range 遍历 map 
for key, value := range map1 { 
} 
// 如果只想获取 key，可以这么使用： 
for key := range map1 { 
} 
map 默认是无序的，想为 map 排序，需要将 key（或者 value）拷贝到一个切片，
再对切片排序，然后可以使用切片的 for-range 方法打印出所有的 key 和 value。


# Go 有异常类型吗？ok 
有。Go 用 error 类型代替 try...catch 语句，这样可以节省资源。同时增加代码可读性： 
_, err := funcDemo() 
if err != nil { 
  fmt.Println(err) 
  return 
}
也可以用 errors.New()来定义自己的异常。errors.Error()会返回异常的字符串表示。
只要实现 error 接口就可以定义自己的异常， 
type errorString struct { 
    s string 
} 

func (e *errorString) Error() string { 
    return e.s 
} 

// 多一个函数当作构造函数 
func New(text string) error { 
    return &errorString{text} 
} 
 
 


## redis
1. string类型内部实现；
    SDS--simple dynamic string,简单动态字符串； 
    可以保存字符串和二进制数据；
    用len属性记录字符串长度，所以获得字符串长度的时间复杂度是O(1)；
    拼接字符串之前会检查SDS空间是否满足，能够自动扩容，不会造成缓存区溢出；
2. Redis 单线程指的是「接收客户端请求->解析请求 ->进行数据读写等操作->发送数据给客户端」这个过程是由一个线程（主线程）来完成的，这也是我们常说 Redis 是单线程的原因。


### 数据类型
1. strings         key value形式的字符串；
2. hashes          操作一个没有嵌套其它对象的对象的属性；
3. lists           有序列表，可以是粉丝列表、文章评论列表、分页查询；
4. sets            无序集合，分布式系统下，使用redis做全局去重、查看共同好友；
5. sorted set      排序的set；
6. 


### redis持久化
#### AOF日志
每执行一条写操作命令，就把该命令以追加的方式写入到一个文件中；
1. AOF重写机制，在AOF文件达到设定的阈值后，就会进行重写。
    在重写过程中，读取当前数据库中的所有键值对，然后将每一个键值对用一条命令记录到「新的 AOF 文件」，等到全部记录完后，就将新的 AOF 文件替换掉现有的 AOF 文件。
    老命令会用新命令覆盖；
2. 

#### RDB快照
将某一时刻的内存数据，以二进制的方式写入磁盘；


### redis主从复制
所有数据读写在主服务器上，然后将最新的数据同步到从服务器，从服务器一般是只读；
1. Redis 采用异步方式复制数据到 slave 节点，不过 Redis2.8 开始，slave node 会周期性地确认自己每次复制的数据量；
2. 一个 master node 是可以配置多个 slave node 的；
3. slave node 也可以连接其他的 slave node；
4. slave node 做复制的时候，不会 block master node 的正常工作；
5. slave node 在做复制的时候，也不会 block 对自己的查询操作，它会用旧的数据集来提供服务；但是复制完成的时候，需要删除旧数据集，加载新数据集，这个时候就会暂停对外服务了；
6. slave node 主要用来进行横向扩容，做读写分离，扩容的 slave node 可以提高读的吞吐量。

#### 主从复制核心原理
当启动一个 slave node 的时候，它会发送一个 PSYNC 命令给 master node。
如果这是 slave node 初次连接到 master node，那么会触发一次 full resynchronization 全量复制。
此时 master 会启动一个后台线程，开始生成一份 RDB 快照文件，同时还会将从客户端 client 新收到的所有写命令缓存在内存中。 
RDB 文件生成完毕后， master 会将这个 RDB 发送给 slave，slave 会先写入本地磁盘，然后再从本地磁盘加载到内存中，
接着 master 会将内存中缓存的写命令发送到 slave，slave 也会同步这些数据。slave node 如果跟 master node 有网络故障，断开了连接，会自动重连，
连接之后 master node 仅会复制给 slave 部分缺少的数据。



### 哨兵模式
哨兵是 Redis 集群架构中非常重要的一个组件，主要有以下功能：
集群监控：负责监控 Redis master 和 slave 进程是否正常工作。
消息通知：如果某个 Redis 实例有故障，那么哨兵负责发送消息作为报警通知给管理员。
故障转移：如果 master node 挂掉了，会自动转移到 slave node 上。
配置中心：如果故障转移发生了，通知 client 客户端新的 master 地址。

监控主从服务器；sentinel,实现主从节点故障转移；
它会监测主节点是否存活，如果发现主节点挂了，它就会选举一个从节点切换为主节点，并且把新主节点的相关信息通知给从节点和客户端。
哨兵在部署时不会部署一个节点，而是用多个节点不是成哨兵集群；避免单个哨兵因为自身网络状况不好，而误判主节点下线的情况。
当哨兵对主节点的下线赞同达到哨兵配置文件中的 quorum 配置项设定的值后，这时主节点就会被该哨兵标记为「客观下线」。
quorum 的值一般设置为哨兵个数的二分之一加1，例如 3 个哨兵就设置 2。

#### 成为候选者的条件：拿到半数的票数；大于等于quorum的值；

#### 故障转移过程
1. 选出主节点；
2. 将从节点指向新主节点；
3. 通知客户的主节点变更；
4. 将旧主节点变为从节点；


#### 导致数据丢失的两种情况
1. 异步复制导致的数据丢失。指的是在master-->slave复制时，有部分数据还没有复制到slave，master就宕机了，此时这部分数据丢失；
2. 脑裂导致的数据丢失。master突然脱离了正常的网络，跟slave连不上了，哨兵以为master宕机，然后开启选举新的master，client还没来得及切换master，继续向旧master写数据，而就master作为了新的slave而从新master复制数据，导致client向旧master写的这部分数据丢失；
解决：
min-slaves-to-write 1
min-slaves-max-lag 10
表示，要求至少有 1 个 slave，数据复制和同步的延迟不能超过 10 秒。
如果说一旦所有的 slave，数据复制和同步的延迟都超过了 10 秒钟，那么这个时候，master 就不会再接收任何请求了。减少数据丢失。


#### 哨兵的自动发现机制
每隔两秒钟，每个哨兵都会往自己监控的某个 master+slaves 对应的 __sentinel__:hello channel 里发送一个消息，内容是自己的 host、ip 和 runid 还有对这个 master 的监控配置。
每个哨兵也会去监听自己监控的每个 master+slaves 对应的 __sentinel__:hello channel，然后去感知到同样在监听这个 master+slaves 的其他哨兵的存在。


### redis过期删除策略
redis会将key带上过期时间存储到一个过期字典中；
策略：惰性删除+定期删除
惰性删除策略的做法是，不主动删除过期键，每次从数据库访问 key 时，都检测 key 是否过期，如果过期则删除该 key。
定期删除策略的做法是，每隔一段时间「随机」从数据库中取出一定数量的 key 进行检查，并删除其中的过期key。

### 过期key处理
slave不会过期key，只会等待master过期key。如果master过期了一个key，或者通过LRU淘汰了一个key，那么会模拟一条del命令发送给slave。

### 内存淘汰机制
定期删除漏掉很多过期的key，然后也没有及时去查，也就没有走惰性删除，这就导致大量过期的key堆积在内存中，导致redis内存耗尽。
此时就走内存淘汰机制。
allkeys-lru：当内存不足以容纳新写入数据时，在键空间中，移除最近最少使用的 key（这个是最常用的）。



### 保证数据库和缓存中的数据一致
采用方式先更新数据库，再删除缓存；等用户下次请求此key时，缓存没有即从数据库中取，再写入缓存中；
此方式会有问题：删除缓存会出现失败，导致数据库修改了，缓存中还是修改前的值；
解决：
1. 重试机制；将数据放入消息队列，如果应用删除缓存失败，即从消息队列中取数据，在此执行删除，重试超过一定次数即报错；如果删除缓存成功，则将数据从消息队列中移除；
2. 订阅Mysql binlog,再操作缓存；订阅 binlog 日志，拿到具体要操作的数据，然后再执行缓存删除


### redis-cluster
Redis cluster，10 台机器，5 台机器部署了 Redis 主实例，另外 5 台机器部署了 Redis 的从实例，每个主实例挂了一个从实例，5 个节点对外提供读写服务，每个节点的读写高峰 QPS 可能可以达到每秒 5 万，5 台机器最多是 25 万读写请求每秒。
机器是什么配置？32G 内存+ 8 核 CPU + 1T 磁盘，但是分配给 Redis 进程的是 10g 内存，一般线上生产环境，Redis 的内存尽量不要超过 10g，超过 10g 可能会有问题。
5 台机器对外提供读写，一共有 50g 内存。
因为每个主实例都挂了一个从实例，所以是高可用的，任何一个主实例宕机，都会自动故障迁移，Redis 从实例会自动变成主实例继续提供读写服务。
你往内存里写的是什么数据？每条数据的大小是多少？商品数据，每条数据是 10kb。100 条数据是 1mb，10 万条数据是 1g。常驻内存的是 200 万条商品数据，占用内存是 20g，仅仅不到总内存的 50%。目前高峰期每秒就是 3500 左右的请求量。



### docker-compose部署redis集群
1. 新建目录文件cluster-host/
├── docker-compose.yml
├── node1
│   ├── data
│   └── redis.conf
├── node2
│   ├── data
│   └── redis.conf
├── node3
│   ├── data
│   └── redis.conf
2. docker-compose.yml内容
```shell
version: "3"

services:
  node1:
    image: redis
    container_name: redis-cluster-node-1
    network_mode: "host"
    volumes:
      - "./node1/redis.conf:/etc/redis.conf"
      - "./node1/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always

  node2:
    image: redis
    container_name: redis-cluster-node-2
    network_mode: "host"
    volumes:
      - "./node2/redis.conf:/etc/redis.conf"
      - "./node2/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always

  node3:
    image: redis
    container_name: redis-cluster-node-3
    network_mode: "host"
    volumes:
      - "./node3/redis.conf:/etc/redis.conf"
      - "./node3/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always

  node4:
    image: redis
    container_name: redis-cluster-node-4
    network_mode: "host"
    volumes:
      - "./node4/redis.conf:/etc/redis.conf"
      - "./node4/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always

  node5:
    image: redis
    container_name: redis-cluster-node-5
    network_mode: "host"
    volumes:
      - "./node5/redis.conf:/etc/redis.conf"
      - "./node5/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always

  node6:
    image: redis
    container_name: redis-cluster-node-6
    network_mode: "host"
    volumes:
      - "./node6/redis.conf:/etc/redis.conf"
      - "./node6/data:/data"
    command: ["redis-server", "/etc/redis.conf"]
    restart: always
```
3. 节点配置，即redis.conf文件
```shell
port 6371
protected-mode no
daemonize no

################################ REDIS CLUSTER  ###############################

cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 15000
cluster-announce-bus-port 16371
```
4. 安装docker-compose,执行docker-compose up -d
5. 查看节点是否正常docker ps -a|grep redis
6. 设置密码 redis.conf中requirepass 123456;
7. 启动集群 redis-cli --cluster create 127.0.0.1:6371 127.0.0.1:6372 127.0.0.1:6373 127.0.0.1:6374 127.0.0.1:6375 127.0.0.1:6376 --cluster-replicas 1
8. redis-cli -c -p 6371


# Go packages
1. goroutine池包 github.com/panjf2000/ants
2. 单元测试包 github.com/smartystreets/goconvey
3. 权限访问控制库 github.com/casbin/casbin/v2
4. 错误处理 github.com/pkg/errors
5. 获取cpu利用率 github.com/shirou/gopsutil
6. 管理定时任务 github.com/robfig/cron/v3
7. 解码结构体 github.com/mitchellh/mapstructure
8. websocket库 nhooyr.io/websocket(gorilla/websocket 已停止维护)
9. 处理和操作通用类型的Go库 github.com/zclconf/go-cty/cty
10. 使用令牌桶算法实现限流器 golang.org/x/time/rate
11. init文件解析 go get -u github.com/go-ini/ini 
12. com 依赖包 go get -u github.com/unknwon/com 
13. gorm 依赖包 go get -u github.com/jinzhu/gorm 
14. mysql 驱动依赖包 go get -u github.com/go-sql-driver/mysql       gorm.io/driver/mysql 
15. validation 依赖包 go get -u github.com/astaxie/beego/validation    github.com/asaskevich/govalidator
16. gorm gorm.io/gorm
17. gin go get -u github.com/gin-gonic/gin 
18. viper文件读取工具 github.com/spf13/viper
19. swagger go get -u github.com/swaggo/swag/cmd/swag go get -u github.com/swaggo/gin-swagger go get -u github.com/swaggo/files
20. sqlite3 go get -u github.com/mattn/go-sqlite3

