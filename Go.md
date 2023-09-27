
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
17. 



## Go编译运行
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
11. 


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




## Go设计模式
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
    直接调用SQL语句进行原生交互RAW; 
    数据库表的增删；
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

### gorm



### sqlite3
go get -u github.com/mattn/go-sqlite3


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


## RWMutex
它保护对内存的访问；你可以请求锁定进行读取，在这种情况下，你将被授予读取权限，除非锁定正在进行写入操作。
这意味着，只要没有别的东西占用写操作，任意数量的读取者就可以进行读取操作。
sync.RWMutex`类型有三个方法：
1. `func (rw *RWMutex) Lock()`: 获取写锁。如果有其他协程已经持有了写锁或读锁，那么当前协程会阻塞，直到其他协程释放锁。
2. `func (rw *RWMutex) Unlock()`: 释放写锁。如果当前协程没有持有写锁，该方法会引发 panic。
3. `func (rw *RWMutex) RLock()`: 获取读锁。如果有其他协程持有写锁，那么当前协程会阻塞，直到其他协程释放锁。
4. `func (rw *RWMutex) RUnlock()`: 释放读锁。如果当前协程没有持有读锁，该方法会引发 panic。
RWMutex 在读锁占用的情况下，会阻止写，但不阻止读RWMutex。 
在写锁占用情况下，会阻止任何其他Goroutine（无论读和写）进来，整个锁相当于由该 Goroutine独占。

## cond
Broadcast()   唤醒所有等待cond的goroutine
Signal()      唤醒一个等待cond的goroutine
Wait()会自动释放c.L锁，并挂起调用者的 goroutine。之后恢复执行，Wait()会在返回时对 c.L 加锁。 除非被 Signal 或者 Broadcast 唤醒，否则 Wait()不会返回


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
5. 在子context传入goroutine中后，应该在子goroutine中对该子context的Done channel进行监控，
    一旦该channel被关闭，应立即终止对当前请求的处理，并释放资源。

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


## GC
### 三色标记原理
首先把所有的对象都放到白色的集合中
 从根节点开始遍历对象，遍历到的白色对象从白色集合中放到灰色集合中
 遍历灰色集合中的对象，把灰色对象引用的白色集合的对象放入到灰色集合中，同时把遍历过的灰色集合中的对象放到黑色的集合中；
 循环步骤2，直到灰色集合中没有对象；
 步骤3结束后，白色集合中的对象就是不可达对象，也就是垃圾，进行回收；


### 写屏障
STW   Stop The Word
指的是垃圾回收器执行时会暂停所有运行的线程；
引入写屏障的原因：
有如下场景：对象A引用对象B和C，对象D引用对象E；在三色标记中扫描灰色集合中，扫描到了对象A，并标记到了A的应用B和C，这个时候开始扫描D的应用，
另一个goroutine修改了D对E的引用，将E让A引用，这样就导致E对象就扫描不到，而被误认为白色对象而被清理；

混合写屏障



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


## etcd

### 查看本地etcd服务
sc query etcd

### docker部署etcd服务
1. docker pull bitnami/etcd:latest；拉取镜像
2. docker network create app-tier --driver bridge；创建docker网络
3. 启动etcd服务实例；docker run -d --name etcd-server --network app-tier --publish 2379:2379 --publish 2380:2380 --env ALLOW_NONE_AUTHENTICATION=yes --env ETCD_ADVERTISE_CLIENT_URLS=http://10.50.7.108:2379 bitnami/etcd:latest
4. 查看版本；curl -L http://127.0.0.1:2379/version


## 缓存
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
1）方案一，使用互斥锁。请求发现缓存不存在后，去查询 DB 前，使用分布式
锁，保证有且只有一个线程去查询 DB ，并更新到缓存。
2）方案二，手
动过期。缓存上从不设置过期时间，功能上将过期时间存在 KEY 对应的 VALUE
里。流程如下：
 1、获取缓存。通过 VALUE 的过期时间，判断是否过期。如果未过期，则
直接返回；如果已过期，继续往下执行。
 2、通过一个后台的异步线程进行缓存的构建，也就是“手动”过期。通过
后台的异步线程，保证有且只有一个线程去查询 DB。
 3、同时，虽然 VALUE 已经过期，还是直接返回。通过这样的方式，保证
服务的可用性，虽然损失了一定的时效性。


### 缓存穿透
查询一个不存在的数据，因为不存在则不会写到缓存中，所以每次都会去请求 DB，如果瞬间流量过大，穿透到 DB，导致宕机。
解决方案：
方案一，缓存空对象。
当从 DB 查询数据为空，我们仍然将这个空结果进行缓存，具体的值需要使用
特殊的标识，能和真正缓存的数据区分开。另外，需要设置较短的过期时间，
一般建议不要超过5分钟。
方案二，BloomFilter布隆过滤器。
在缓存服务的基础上，构建BloomFilter数据结构，在BloomFilter中存储对应的
KEY是否存在，如果存在，说明该KEY对应的值不为空。


## redis
1. string类型内部实现；
    SDS--simple dynamic string,简单动态字符串； 
    可以保存字符串和二进制数据；
    用len属性记录字符串长度，所以获得字符串长度的时间复杂度是O(1)；
    拼接字符串之前会检查SDS空间是否满足，能够自动扩容，不会造成缓存区溢出；
2. Redis 单线程指的是「接收客户端请求->解析请求 ->进行数据读写等操作->发送数据给客户端」这个过程是由一个线程（主线程）来完成的，这也是我们常说 Redis 是单线程的原因。


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
所有数据读写在主服务器上，然后将最新的数据同步非从服务器，从服务器一般是只读；

### 哨兵模式
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


### redis过期删除策略
redis会将key带上过期时间存储到一个过期字典中；
策略：惰性删除+定期删除
惰性删除策略的做法是，不主动删除过期键，每次从数据库访问 key 时，都检测 key 是否过期，如果过期则删除该 key。
定期删除策略的做法是，每隔一段时间「随机」从数据库中取出一定数量的 key 进行检查，并删除其中的过期key。


### 保证数据库和缓存中的数据一致
采用方式先更新数据库，再删除缓存；等用户下次请求此key时，缓存没有即从数据库中取，再写入缓存中；
此方式会有问题：删除缓存会出现失败，导致数据库修改了，缓存中还是修改前的值；
解决：
1. 重试机制；将数据放入消息队列，如果应用删除缓存失败，即从消息队列中取数据，在此执行删除，重试超过一定次数即报错；如果删除缓存成功，则将数据从消息队列中移除；
2. 订阅Mysql binlog,再操作缓存；订阅 binlog 日志，拿到具体要操作的数据，然后再执行缓存删除




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

3. 节点配置，即redis.conf文件
port 6371
protected-mode no
daemonize no

################################ REDIS CLUSTER  ###############################

cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 15000
cluster-announce-bus-port 16371

4. 安装docker-compose,执行docker-compose up -d
5. 查看节点是否正常docker ps -a|grep redis
6. 设置密码 redis.conf中requirepass 123456;
7. 启动集群 redis-cli --cluster create 127.0.0.1:6371 127.0.0.1:6372 127.0.0.1:6373 127.0.0.1:6374 127.0.0.1:6375 127.0.0.1:6376 --cluster-replicas 1
8. redis-cli -c -p 6371



## jwt 
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




## defer
1. 多个defer语句，按先进后出的方式执行；
2. defer声明时，对应的参数会实时解析；
3. defer、return、返回值三者的执行逻辑-return最先执行，return负责将结果写入返回值中，defer执行收尾工作，最后函数携带当前返回值退出；
4. defer用于关闭文件和互斥锁；
5. panic后面的defer语句不被执行，panic语句前的defer语句会被执行；
6. 调用os.Exit(0)时defer语句不会被执行；

### defer的底层数据结构：
每个 defer 语句都对应一个_defer 实例，多个实例使用指针连接起来形成一个单连表，保存在 gotoutine 数据结构中，
每次插入_defer 实例，均插入到链表的头部，函数结束再一次从头部取出，从而形成后进先出的效果。


## rune
Go字符串底层是通过byte数组实现，中文字符在unicode下占2个字节，在utf-8编码下占3个字节，而golang默认编码正好是utf-8
byte 等同于int8，常用来处理ascii字符
rune 等同于int32,常用来处理unicode或utf-8字符



## 反射reflect
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




# 微服务


## 如何设计一个高并发系统
保证整体可用的同时，能够处理很高的并发用户请求，能够承受很大的流量冲击；
1. 横向扩展；采用分布式部署，部署多台服务器，将流量分开，分担流量；
2. 微服务拆分；按功能单一性，拆分成多个服务模块；
3. 数据库分库分表；
4. 池化技术；数据库连接池、http连接池、redis连接池；
5. 主从分离；实时性要求不高的读请求，都去读从库，写的请求或者实时性要求高的请求，才走主库；
6. 使用缓存；
7. CDN加速静态资源访问；
8. 消息队列，削峰；
9. ElasticSearch；
10. 降级熔断；
11. 限流；
12. 异步；
13. 扩容+切流量；




# Go packages
1. goroutine池包 github.com/panjf2000/ants
2. 单元测试包 github.com/smartystreets/goconvey
3. 权限访问控制库 github.com/casbin/casbin/v2
4. 错误处理 github.com/pkg/errors
5. 获取cpu利用率 github.com/shirou/gopsutil