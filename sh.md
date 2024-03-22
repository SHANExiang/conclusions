
<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->


## 内存

### 内核态和用户态区别？内核态的底层操作有什么？为什么要分两个不同的态？
内核态和用户态是操作系统中的两种运行模式。它们的主要区别在于权限和可执行的操作：

内核态（Kernel Mode）：在内核态下，CPU可以执行所有的指令和访问所有的硬件资源。这种模式下的操作具有更高的权限，主要用于操作系统内核的运行。

用户态（User Mode）：在用户态下，CPU只能执行部分指令集，无法直接访问硬件资源。这种模式下的操作权限较低，主要用于运行用户程序。

内核态的底层操作主要包括：内存管理、进程管理、设备驱动程序控制、系统调用等。这些操作涉及到操作系统的核心功能，需要较高的权限来执行。

分为内核态和用户态的原因主要有以下几点：

安全性：通过对权限的划分，用户程序无法直接访问硬件资源，从而避免了恶意程序对系统资源的破坏。

稳定性：用户态程序出现问题时，不会影响到整个系统，避免了程序故障导致系统崩溃的风险。

隔离性：内核态和用户态的划分使得操作系统内核与用户程序之间有了明确的边界，有利于系统的模块化和维护。

内核态和用户态的划分有助于保证操作系统的安全性、稳定性和易维护性。


### 物理内存
平时所称的内存也叫随机访问存储器（ random-access memory ）也叫 RAM 。而 RAM 分为两类：

一类是静态 RAM（ SRAM ），这类 SRAM 用于 CPU 高速缓存 L1Cache，L2Cache，L3Cache。其特点是访问速度快，访问速度为 1 - 30 个时钟周期，但是容量小，造价高。

另一类则是动态 RAM ( DRAM )，这类 DRAM 用于我们常说的主存上，其特点的是访问速度慢（相对高速缓存），访问速度为 50 - 200 个时钟周期，但是容量大，造价便宜些（相对高速缓存）。



### 虚拟内存地址
64 位虚拟地址的格式为：全局页目录项（9位）+ 上层页目录项（9位）+ 中间页目录项（9位）+ 页表项（9位）+ 页内偏移（12位）。共 48 位组成的虚拟内存地址。

32 位虚拟地址的格式为：页目录项（10位）+ 页表项（10位） + 页内偏移（12位）。共 32 位组成的虚拟内存地址。



### 进程虚拟内存空间所包含的主要区域
1. 用于存放进程程序二进制文件中的机器指令的代码段
2. 用于存放程序二进制文件中定义的全局变量和静态变量的数据段和 BSS 段。
3. 用于在程序运行过程中动态申请内存的堆。
4. 用于存放动态链接库以及内存映射区域的文件映射与匿名映射区。
5. 用于存放函数调用过程中的局部变量和函数参数的栈。


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
将页表（一级页表）分为 1024 个页表（二级页表），每个表（二级页表）中包含 1024 个「页表项」，形成二级分页。再把二级分页推广到多级分页；
一级页表覆盖到了全部虚拟地址空间，二级页表在需要时创建。

当一个进程访问某个虚拟地址时,CPU 会首先在页目录中查找对应的页表,然后在页表中查找对应的页,最后访问这个页所对应的物理内存。
这种层级结构使得内存管理更加高效灵活。操作系统可以只将需要的页表加载到内存中,而不需要将整个页表都加载进来。


### TLB
translation lookaside buffer，通常称为页表缓存、地址旁路缓存、快表等；
把最常访问的几个页表项存储到访问速度更快的硬件，于是计算机科学家们，就在 CPU 芯片中，加入了一个专门存放程序最常访问的页表项的 Cache，这个 Cache 就是 TLB。
在 CPU 芯片里面，封装了内存管理单元（Memory Management Unit）芯片，它用来完成地址转换和 TLB 的访问与交互。
有了 TLB 后，那么 CPU 在寻址时，会先查 TLB，如果没找到，才会继续查常规的页表。


### 段页式内存管理
内存分段和内存分页组合在同一个系统中使用。

段页式内存管理实现的方式：
1. 先将程序划分为多个有逻辑意义的段，也就是前面提到的分段机制；
2. 接着再把每个段划分为多个页，也就是对分段划分出来的连续空间，再划分固定大小的页；
这样，地址结构就由段号、段内页号和页内位移三部分组成。


### 内存分配的过程
应用程序通过 malloc 函数申请内存的时候，实际上申请的是虚拟内存，此时并不会分配物理内存。
当应用程序读写了这块虚拟内存，CPU 就会去访问这个虚拟内存，这时会发现这个虚拟内存没有映射到物理内存，
CPU 就会产生缺页中断，进程会从用户态切换到内核态，并将缺页中断交给内核的 Page Fault Handler （缺页中断函数）处理。

缺页中断处理函数会看是否有空闲的物理内存：
如果有，就直接分配物理内存，并建立虚拟内存与物理内存之间的映射关系。
如果没有空闲的物理内存，那么内核就会开始进行回收内存 (opens new window)的工作，
如果回收内存工作结束后，空闲的物理内存仍然无法满足此次物理内存的申请，那么内核就会放最后的大招了触发 OOM （Out of Memory）机制。

32 位系统的内核空间占用 1G，位于最高处，剩下的 3G 是用户空间；
64 位系统的内核空间和用户空间都是 128T，分别占据整个内存空间的最高和最低处，剩下的中间部分是未定义的。

内存溢出(Out Of Memory，简称OOM)是指应用系统中存在无法回收的内存或使用的内存过多，最终使得程序运行要用到的内存大于能提供的最大内存。
此时程序就运行不了，系统会提示内存溢出。



### 虚拟内存的作用
第一，虚拟内存可以使得进程对运行内存超过物理内存大小，因为程序运行符合局部性原理，CPU 访问内存会有很明显的重复访问的倾向性，
对于那些没有被经常使用到的内存，我们可以把它换出到物理内存之外，比如硬盘上的 swap 区域。

第二，由于每个进程都有自己的页表，所以每个进程的虚拟内存空间就是相互独立的。
进程也没有办法访问其他进程的页表，所以这些页表是私有的，这就解决了多进程之间地址冲突的问题。

第三，页表里的页表项中除了物理地址之外，还有一些标记属性的比特，比如控制一个页的读写权限，标记该页是否存在等。
在内存访问方面，操作系统提供了更好的安全性。


### swap机制
当系统的物理内存不够用的时候，就需要将物理内存中的一部分空间释放出来，以供当前运行的程序使用。
那些被释放的空间可能来自一些很长时间没有什么操作的程序，这些被释放的空间会被临时保存到磁盘，等到那些程序要运行时，再从磁盘中恢复保存的数据到内存中。

另外，当内存使用存在压力的时候，会开始触发内存回收行为，会把这些不常访问的内存先写到磁盘中，然后释放这些内存，给其他更需要的进程使用。
再次访问这些内存时，重新从磁盘读入内存就可以了。

这种，将内存数据换出磁盘，又从磁盘中恢复数据到内存的过程，就是 Swap 机制负责的。

Swap 就是把一块磁盘空间或者本地文件，当成内存来使用，它包含换出和换入两个过程：

换出（Swap Out），是把进程暂时不用的内存数据存储到磁盘中，并释放这些数据占用的内存；
换入（Swap In），是在进程再次访问这些内存的时候，把它们从磁盘读到内存中来；

使用 Swap 机制优点是，应用程序实际可以使用的内存空间将远远超过系统的物理内存。由于硬盘空间的价格远比内存要低，因此这种方式无疑是经济实惠的。
当然，频繁地读写硬盘，会显著降低操作系统的运行速率，这也是 Swap 的弊端。

Linux提供了两种方法启用Swap，分别是Swap分区和Swap文件；


### 系统内存紧张的时候，就会进行回收内存的工作，那具体哪些内存是可以被回收的呢？

主要有两类内存可以被回收，而且它们的回收方式也不同。

文件页（File-backed Page）：内核缓存的磁盘数据（Buffer）和内核缓存的文件数据（Cache）都叫作文件页。大部分文件页，都可以直接释放内存，以后有需要时，再从磁盘重新读取就可以了。而那些被应用程序修改过，并且暂时还没写入磁盘的数据（也就是脏页），就得先写入磁盘，然后才能进行内存释放。所以，回收干净页的方式是直接释放内存，回收脏页的方式是先写回磁盘后再释放内存。

匿名页（Anonymous Page）：这部分内存没有实际载体，不像文件缓存有硬盘文件这样一个载体，比如堆、栈数据等。这部分内存很可能还要再次被访问，所以不能直接释放内存，它们回收的方式是通过 Linux 的 Swap 机制，Swap 会把不常访问的内存先写到磁盘中，然后释放这些内存，给其他更需要的进程使用。再次访问这些内存时，重新从磁盘读入内存就可以了。



### Linux操作系统的缓存
在应用程序读取文件的数据的时候，Linux操作系统会对读取的文件数据进行缓存，会缓存在文件系统中的Page Cache（页缓存）；

Page Cache 属于内存空间里的数据，由于内存访问比磁盘访问快很多，在下一次访问相同的数据就不需要通过磁盘 I/O 了，命中缓存就直接返回数据即可。
因此，Page Cache 起到了加速访问数据的作用。


#### 预读机制
Linux 操作系统为基于 Page Cache 的读缓存机制提供预读机制。

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
时间局部性和空间局部性。时间局部性是指如果程序中的某条指令一旦执行，则不久之后该指令可能再次被执行；如果某块数据被访问，则不久之后该数据可能再次被访问。
空间局部性是指一旦程序访问了某个存储单元，则不久之后，其附近的存储单元也将被访问。


### 查询正在运行的进程的状态和资源占用情况
在 Linux 系统中，有多种方法可以查询正在运行的进程的状态和资源占用情况。下面是一些常用的命令和工具：

1. ps 命令
ps aux 可以显示所有进程的详细信息，包括进程 ID（PID）、CPU 和内存占用率、进程状态等。
ps -ef 可以显示进程的父进程 ID（PPID）和命令行参数等信息。

2. top 命令
top 命令可以实时显示系统中运行的进程及其资源占用情况。
它提供了交互式界面，可以根据 CPU 使用率、内存使用量等对进程进行排序。
通过 top 命令，可以快速定位占用资源较多的进程。
top -Hp <pid>
H参数表示要显示线程级别的信息，p则表示指定的pid，也就是进程id。

3. htop 命令
htop 是一个增强版的 top 命令，提供了更友好的用户界面和更多的功能。
它可以显示进程的树形结构、CPU 和内存使用情况的图形化表示等。

4. pmap 命令
pmap 命令可以显示指定进程的内存映射情况。
通过 pmap -x <PID> 可以查看进程的内存映射详情，包括映射的地址范围、权限、占用的物理内存大小等。

5. /proc 文件系统
Linux 的 /proc 文件系统提供了进程的详细信息。
对于每个进程，都有一个对应的 /proc/PID 目录，其中包含了进程的各种信息文件。
例如，/proc/PID/status 文件包含进程的状态信息，/proc/PID/maps 文件包含进程的内存映射信息等。



### 查看内存使用情况
要通过命令查看分配的内存的使用情况，可以使用以下几个常用的命令：
1. free命令
   ```
   free -h
   ```
   该命令以人类可读的格式显示内存的使用情况。输出结果包括总内存、已用内存、可用内存、缓存和交换空间的大小。

2. top命令
   ```
   top
   ```
   top命令可以实时显示系统的资源使用情况，包括内存使用情况。在top命令的输出中，查看"KiB Mem"行可以看到总内存和已用内存的大小。

3. vmstat命令
   ```
   vmstat
   ```
   vmstat命令可以显示虚拟内存的统计信息，包括内存的使用情况。在输出结果中，查看"free"列可以看到可用内存的大小。

4. ps命令
   ```
   ps aux --sort=-%mem | head
   ```
   该命令可以显示当前系统中内存占用最高的进程。--sort=-%mem参数表示按内存使用量降序排序，head命令用于显示前面几行的输出。

5. cat /proc/meminfo命令
   ```
   cat /proc/meminfo
   ```
   该命令可以显示详细的内存信息，包括总内存、可用内存、缓存、交换空间等的大小和使用情况。

以上命令可以帮助你查看分配的内存的使用情况。你可以根据需要选择适合的命令来获取所需的信息。



### 进程的内存管理
关于进程的内存管理，Linux 系统采用了虚拟内存管理机制：

每个进程都有自己独立的虚拟地址空间，通过页表将虚拟地址映射到物理内存。
当进程需要分配内存时，操作系统会在物理内存中找到空闲的页框，并将其映射到进程的虚拟地址空间中。
如果物理内存不足，操作系统会将一些不常用的页面换出到交换空间（如交换分区或交换文件）中，以腾出内存空间。
当进程访问已经换出的页面时，会触发缺页异常，操作系统会将页面从交换空间换入到物理内存中。
Linux 还使用了写时复制（Copy-on-Write）技术，多个进程可以共享相同的物理内存页面，直到其中一个进程需要修改页面时，才会复制一份副本。

通过以上机制，Linux 系统可以高效地管理进程的内存使用，并提供了各种工具和接口供用户查询和监控进程的状态和资源占用情况。



## 进程间通信
a) 管道/匿名管道(Pipes)：管道是半双工的，数据只能向一个方向流动；
双方通信时，需要建立起两个管道；一个进程向管道中写的内容被管道另一端的进程读出。
写入的内容每次都添加在管道缓冲区的末尾，并且每次都是从缓冲区的头部读出数据；
用于具有亲缘关系的父子进程间或者兄弟进程之间的通信。 

mkfifo myPipe    ## 创建管道，在目录内生成一个文件，文件类型是p
echo test > myPipe    ## 往管道内写数据,执行命令停在这里不动了，这是因为管道内的数据没有被读取；
cat < myPipe          ## 读取管道内的数据，另一方面上面的echo也正常退出了。

b) 有名管道(Names Pipes): 匿名管道由于没有名字，只能用于亲缘关系的进程间通信。
为了克服这个缺点，提出了有名管道。有名管道严格遵循先进先出(first in first out)。
有名管道以磁盘文件的方式存在，可以实现本机任意两个进程通信。 

c) 消息队列(Message Queuing)：消息队列是消息的链表，具有特定的格式，存放在内存中并由消息队列标识符标识。
管道和消息队列的通信数据都是先进先出的原则。
与管道（无名管道：只存在于内存中的文件；命名管道：存在于实际的磁盘介质或者文件系统）不同的是消息队列存放在内核中，
只有在内核重启(即，操作系统重启)或者显示地删除一个消息队列时，该消息队列才会被真正的删除。
消息队列可以实现消息的随机查询，消息不一定要以先进先出的次序读取，也可以按消息的类型读取，比 FIFO 更有优势。
消息队列克服了信号承载信息量少，管道只能承载无格式字节流以及缓冲区大小受限等缺点。 

d) 信号(Signal)：信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生；
（对于异常情况下的工作模式，就需要用「信号」的方式来通知进程，信号事件的来源主要有硬件来源（如键盘 Cltr+C ）和软件来源（如 kill 命令）。
比如，Ctrl+C 产生 SIGINT 信号，表示终止该进程，Ctrl+Z 产生 SIGSTP，表示停止该进程，但还未结束） 

e) 信号量(Semaphores)：信号量是一个计数器，用于多进程对共享数据的访问，信号量的意图在于进程间同步。
这种通信方式主要用于解决与同步相关的问题并避免竞争条件。
（信号量其实是一个整型的计数器，主要用于实现进程间的互斥与同步，而不是用于缓存进程间通信的数据。） 

f) 共享内存(Shared memory)：使得多个进程可以访问同一块内存空间，不同进程可以及时看到对方进程中对共享内存中数据的更新。
这种方式需要依靠某种同步操作，如互斥锁和信号量等。
可以说这是最有用的进程间通信方式。（共享内存的机制，就是拿出一块虚拟地址空间来，映射到相同的物理内存中） 

h) 套接字(Sockets): 此方法主要用于在客户端和服务器之间通过网络进行通信。
套接字是支持 TCP/IP 的网络通信的基本操作单元，可以看做是不同主机之间的
进程进行双向通信的端点，简单的说就是通信的两方的一种约定，用套接字中的相关函数来完成通信过程。 
int socket(int domain, int type, int protocal)

优缺点：
管道：速度慢，容量有限； 
Socket：任何进程间都能通讯，但速度慢； 
消息队列：容量受到系统限制，且要注意第一次读的时候，要考虑上一次没有读完数据的问题； 
信号量：不能传递复杂消息，只能用来同步； 
共享内存区：能够很容易控制容量，速度快，但要保持同步，比如一个进程在写的时候，另一个进程要注意读写的问题，相当于线程中的线程安全，当然，共享内存区同样可以用作线程间通讯，不过没这个必要，线程间本来就已经共享了同一进程内的一块内存。 


### 操作系统中的锁机制能起到锁lock 这个功能的一些操作系统特性有哪些？

在操作系统中，有多种锁机制可以实现对共享资源的互斥访问和同步控制。以下是一些常见的锁机制及其使用场景：

1. 自旋锁（Spinlock）：
自旋锁是一种简单且低开销的锁机制，适用于短时间的临界区保护。
当一个线程尝试获取已经被其他线程持有的自旋锁时，它会一直循环等待（自旋），直到锁被释放。
自旋锁适用于锁的持有时间非常短且线程间竞争不激烈的情况，例如内核中的一些简单操作。
自旋锁的优点是低延迟，缺点是在高竞争情况下会浪费 CPU 资源。

2. 信号量（Semaphore）：
信号量是一种计数器式的同步机制，用于控制对共享资源的访问。
信号量的值表示可用资源的数量，线程可以通过 P（等待）和 V（释放）操作来获取和释放资源。
当信号量的值为零时，尝试获取资源的线程会被阻塞，直到有其他线程释放资源。
信号量适用于控制有限的共享资源，如限制同时访问某个资源的线程数量。
信号量还可以用于线程间的同步和通信，例如生产者-消费者模型中的缓冲区同步。

3. 互斥量（Mutex）：
互斥量是一种用于实现互斥访问的锁机制，确保同一时刻只有一个线程可以进入临界区。
线程通过 lock 和 unlock 操作来获取和释放互斥量，如果互斥量已经被其他线程持有，尝试获取的线程会被阻塞。
互斥量适用于保护共享数据结构的完整性，防止多个线程同时修改共享数据而导致的竞态条件。
互斥量的特点是互斥性，即同一时刻只能有一个线程持有互斥量。

4. 条件变量（Condition Variable）：
条件变量是一种用于线程间通信和同步的机制，常与互斥量一起使用。
线程可以在条件变量上等待（wait）特定条件的满足，而其他线程可以在条件满足时通知（signal）等待的线程。
条件变量适用于线程间的协调和等待，例如在生产者-消费者模型中，消费者线程在缓冲区为空时等待，生产者线程在生产数据后通知消费者线程。
条件变量的特点是可以让线程在特定条件未满足时进入休眠状态，避免了忙等待的资源浪费。



## 线程间通信有哪些
在Linux系统中，线程间通信的方式包括：

互斥锁（Mutex）：线程可以使用互斥锁来保护共享资源，确保同时只有一个线程可以访问该资源。

条件变量：线程可以使用条件变量来等待特定条件的发生，以实现线程间的协调和通知。

信号量：线程可以使用信号量来控制对共享资源的访问，实现线程间的同步和互斥。

读写锁：允许多个线程同时读取共享资源，但只允许一个线程写入共享资源。


## 了解过死锁吗？怎么避免
产生死锁的四个必要条件是：互斥条件、持有并等待条件、不可剥夺条件、环路等待条件。那么避免死锁问题就只需要破环其中一个条件就可以，最常见的并且可行的就是使用资源有序分配法，来破环环路等待条件。

那什么是资源有序分配法呢？

线程 A 和 线程 B 获取资源的顺序要一样，当线程 A 是先尝试获取资源 A，然后尝试获取资源 B 的时候，线程 B 同样也是先尝试获取资源 A，然后尝试获取资源 B。也就是说，线程 A 和 线程 B 总是以相同的顺序申请自己想要的资源。

我们使用资源有序分配法的方式来修改前面发生死锁的代码，我们可以不改动线程 A 的代码。

我们先要清楚线程 A 获取资源的顺序，它是先获取互斥锁 A，然后获取互斥锁 B。

所以我们只需将线程 B 改成以相同顺序的获取资源，就可以打破死锁了。


## 讲讲IO多路复用的实现原理，select、pool、epoll的区别是什么？
I/O 的多路复用，可以只在一个进程里处理多个文件的 I/O，Linux 下有三种提供 I/O 多路复用的 API，分别是：select、poll、epoll。

select 实现多路复用的方式是，将已连接的 Socket 都放到一个文件描述符集合，然后调用 select 函数将文件描述符集合拷贝到内核里，让内核来检查是否有网络事件产生，检查的方式很粗暴，就是通过遍历文件描述符集合的方式，当检查到有事件产生后，将此 Socket 标记为可读或可写， 接着再把整个文件描述符集合拷贝回用户态里，然后用户态还需要再通过遍历的方法找到可读或可写的 Socket，然后再对其处理。

所以，对于 select 这种方式，需要进行 2 次「遍历」文件描述符集合，一次是在内核态里，一个次是在用户态里 ，而且还会发生 2 次「拷贝」文件描述符集合，先从用户空间传入内核空间，由内核修改后，再传出到用户空间中。

select 使用固定长度的 BitsMap，表示文件描述符集合，而且所支持的文件描述符的个数是有限制的，在 Linux 系统中，由内核中的 FD_SETSIZE 限制， 默认最大值为 1024，只能监听 0~1023 的文件描述符。

poll 不再用 BitsMap 来存储所关注的文件描述符，取而代之用动态数组，以链表形式来组织，突破了 select 的文件描述符个数限制，当然还会受到系统文件描述符限制。

但是 poll 和 select 并没有太大的本质区别，都是使用「线性结构」存储进程关注的 Socket 集合，因此都需要遍历文件描述符集合来找到可读或可写的 Socket，时间复杂度为 O(n)，而且也需要在用户态与内核态之间拷贝文件描述符集合，这种方式随着并发数上来，性能的损耗会呈指数级增长。

epoll 通过两个方面解决了 select/poll 的问题。
1. epoll 在内核里使用「红黑树」来关注进程所有待检测的 Socket，红黑树是个高效的数据结构，增删改一般时间复杂度是 O(logn)，通过对这棵黑红树的管理，不需要像 select/poll 在每次操作时都传入整个 Socket 集合，减少了内核和用户空间大量的数据拷贝和内存分配。
2. epoll 使用事件驱动的机制，内核里维护了一个「链表」来记录就绪事件，只将有事件发生的 Socket 集合传递给应用程序，不需要像 select/poll 那样轮询扫描整个集合（包含有和无事件的 Socket ），大大提高了检测的效率。

epoll 具体工作流程：
1. 通过epoll_create创建epoll对象，此时epoll对象的内核结构包含就绪链表和红黑树，就绪队列是用于保存所有读写事件到来的socket。红黑树用于保存所有待检测的socket。
2. 通过 epoll_crt 将待检测的socket，加入到红黑树中，并注册一个事件回调函数，当有事件到来的之后，会调用这个回调函数，进而通知到 epoll 对象。
3. 调用 epoll_wait 等待事件的发生，当内核检测到事件发生后，调用该socket注册的回调函数，执行回调函数就能找到socket对应的epoll对象，然后会将事件加入到epoll对象的绪队列中，最后将就绪队列返回给应用层。


IO多路复用是一种同时监控多个IO事件的机制,可以提高系统的并发处理能力。其基本原理是:
1. 首先将要监听的IO事件注册到内核中的一个事件表。
2. 然后调用一个函数如select或epoll_wait,阻塞等待这些事件发生。
3. 当有事件发生时,该函数返回,通知应用程序轮询处理这些就绪的IO事件。
4. 处理完后,继续调用该函数阻塞监听。

这样,一个线程就可以同时监控多个IO,而不是阻塞在单个IO上,提高了效率。

select和epoll是Linux中两种IO多路复用的实现方式,区别在于:
1. 监听事件的存储方式
- select使用线性表存储监听事件,有最大数量限制,通常是1024
- epoll使用红黑树存储,理论上没有最大数量限制

2. 事件通知方式
- select通过轮询整个监听列表来找到就绪事件,时间复杂度O(n)
- epoll通过回调函数直接得到就绪事件,时间复杂度O(1)

3. 内核与用户空间的数据拷贝
- select需要将整个监听列表在内核和用户空间之间来回拷贝
- epoll通过mmap共享内存,避免了不必要的数据拷贝

因此,epoll相比select,具有更好的性能和可扩展性,是目前使用更广泛的IO多路复用技术。但select实现更简单,在监听事件数量不多时,性能差异并不明显。


## nginx
反向代理和负载均衡

Nginx 可以作为反向代理服务器，将客户端请求转发到后端服务器。
通过配置反向代理规则，Nginx 可以根据请求的 URL、路径、头部等信息将请求分发到不同的后端服务器。
Nginx 支持多种负载均衡算法，如轮询、最少连接、IP 哈希等，可以根据后端服务器的性能和负载情况进行请求分发。
反向代理和负载均衡可以提高系统的可伸缩性、可用性和性能。
```shell
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

```
当客户端通过浏览器或其他方式发送请求到 yourdomain.com 这个域名时，Nginx 将监听 HTTP 请求的端口 80，将带有/socket前缀的WebSocket请求转发到ws://backend.example.com/socket，
同时将带有/api前缀的HTTP请求转发到http://backend.example.com/api。

proxy_set_header Host $host; 将客户端请求中的 Host 头部信息传递给目标服务器。这是正常的 HTTP 头部信息传递，不涉及客户端 IP 地址。

proxy_set_header X-Real-IP $remote_addr;将客户端的真实 IP 地址作为 X-Real-IP 头部信息传递给目标服务器。这意味着目标服务器可以访问到客户端的真实 IP 地址。

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 将客户端的 IP 地址添加到 X-Forwarded-For 头部信息中，并传递给目标服务器。这是为了记录代理请求的前几个客户端的 IP 地址，通常包括客户端的真实 IP 地址。

proxy_read_timeout: 用于设置从后端服务器接收响应的超时时间。默认值60s

proxy_connect_timeout: 用于设置与后端服务器建立连接的超时时间。默认值60s

proxy_send_timeout: 用于设置向后端服务器发送请求的超时时间。默认值60s


#### Nginx有哪些负载均衡算法？
Nginx支持的负载均衡算法包括：
1. 轮询：按照顺序依次将请求分配给后端服务器。这种算法最简单，但是也无法处理某个节点变慢或者客户端操作有连续性的情况。
2. IP哈希：根据客户端IP地址的哈希值来确定分配请求的后端服务器。
适用于需要保持同一客户端的请求始终发送到同一台后端服务器的场景，如会话保持。
3. URL哈希：按访问的URL的哈希结果来分配请求，使每个URL定向到一台后端服务器，可以进一步提高后端缓存服务器的效率。
4. 最短响应时间：按照后端服务器的响应时间来分配请求，响应时间短的优先分配。
适用于后端服务器性能不均的场景，能够将请求发送到响应时间快的服务器，实现负载均衡。
5. 加权轮询：按照权重分配请求给后端服务器，权重越高的服务器获得更多的请求。
适用于后端服务器性能不同的场景，可以根据服务器权重分配请求，提高高性能服务器的利用率。



## 快捷键
1. 从后往前删除   ctrl+w；
2. 从前往后删除   ctrl+k；
3. 光标从前调到末尾  ctrl+e;  vim内部为删除光标所在行；


## curl
curl "http://localhost:9999/hello?name=geektutu"

curl "http://localhost:9999/login" -X POST -d 'username=geektutu&password=1234' -H ""

curl -e ssl_cacert.pem -k 'https://192.168.11.1:18002/controller/v2/tokens' --header 'Content-Type: application/json' --header 'Cookie:bspsession=deleted' -d '{"userName": "ops@huawei.com", "password": "Xsy@2023"}'

curl 33.33.33.232:30000 --connect-timeout 5    设置5s超时

curl -g -i --insecure -X PUT https://10.50.90.2:18002/restconf/data/huawei-ac-neutron:neutron-cfg/routers/router/e4bffe3a-24cb-4257-aab6-48cd95499aeb -H "Content-Type:application/json" -H "X-ACCESS-TOKEN:$token" -H "Accept:application/json" -d $data



## df –h
列出文件系统的磁盘使用状况。


## free 命令,它是用来查看系统内存的命令 
free -h #查看内存使用情况,并且以合适的单位显示大小
Mem内存的使用情况
Swap交换空间的使用情况

total系统总的可用物理内存大小
used已使用的物理内存大小
free还有多少物理内存可用
shared被共享使用的物理内存大小
buff/cache被buff和cache使用的物理内存大小
available还可以被使用的物理内存大小
available = free + buff + cache


## 查看网络服务和端口 - netstat / ss。 
[root ~]# netstat -nap | grep nginx 


## pstree –aup 以树状图的方式展现进程之间的派生关系 
-a：显示每个程序的完整指令，包含路径，参数或是常驻服务的标示；  
-c：不使用精简标示法；  
-G：使用 VT100 终端机的列绘图字符；  
-h：列出树状图时，特别标明现在执行的程序；  
-H<程序识别码>：此参数的效果和指定”-h”参数类似，但特别标明指定的程序；  
-l：采用长列格式显示树状图；  
-n：用程序识别码排序。预设是以程序名称来排序；  
-p：显示程序识别码；  
-u：显示用户名称；  
 
## uptime 
查看系统的负载情况。 


## mount 
将 /dev/hda1 挂在 /mnt 之下 


## ldd 
list dynamic dependencies，列出动态库依赖关系 

ldd 本身不是一个程序，而仅是一个 shell 脚本：ldd 可以列出一个程序所需要得动态链接库（so） 

ldd 命令通常使用"-v"或"--verbose"选项来显示所依赖的动态连接库的尽可能的详细信息。 

即可得到/bin/ls 命令的相关共享库文件列表： 
root@xxhui:/home/hui# ldd /bin/ls 
linux-vdso.so.1 (0x00007ffeeffc3000) 
libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 
(0x00007f8e631c7000) 
libacl.so.1 => /lib/x86_64-linux-gnu/libacl.so.1 
(0x00007f8e62fbe000) 
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 
(0x00007f8e62c19000) 
libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 
(0x00007f8e629a9000) 
libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 
(0x00007f8e627a5000) 
/lib64/ld-linux-x86-64.so.2 (0x00005599a18e8000) 
libattr.so.1 => /lib/x86_64-linux-gnu/libattr.so.1 
(0x00007f8e6259f000) 
libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 
(0x00007f8e62382000) 
注意： 在 ldd 命令打印的结果中，“=>”左边的表示该程序需要连
接的共享库之 so 名称，右边表示由 Linux 的共享库系统找到的对应
的共享库在文件系统中的具体位置。默认情况下， /etc/ld.so.conf 
文件中包含有默认的共享库搜索路径。 


## ss 
ss 是 Socket Statistics 的缩写。ss 命令可以用来获取 socket 统计信息

-h, –help 帮助  
-V, –version 显示版本号  
-t, –tcp 显示 TCP 协议的 sockets  
-u, –udp 显示 UDP 协议的 sockets  
-x, –unix 显示 unix domain sockets，与 -f 选项相同  
-n, –numeric 不解析服务的名称，如 “22” 端口不会显示成 “ssh”  
-l, –listening 只显示处于监听状态的端口  
-p, –processes 显示监听端口的进程(Ubuntu 上需要 sudo)  
-a, –all 对 TCP 协议来说，既包含监听的端口，也包含建立的连接  
-r, –resolve 把 IP 解释为域名，把端口号解释为协议名称 
 

## chmod 
修改/test 下的 aaa.txt 的权限为文件所有者有全部权限，文件所有者所在的组有读写权限，其他用户只有读的权限。 
chmod u=rwx,g=rw,o=r aaa.txt 或者 chmod 764 aaa.txt 
 

## shell 单引号和双引号 
在单引号中所有的特殊符号，如$和反引号都没有特殊含义。在双引号
中，除了"$",""和反引号，其他的字符没有特殊含义。 


## shell 数组 
array=(1 2 3 4 5); 
获取数组长度 length=${#array[@]}    # 或者length2=${#array[*]}

输出数组第三个元素 echo ${array[2]} #输出：3

unset array[1]# 删除下标为 1 的元素也就是删除第二个元素

for i in ${array[@]};do echo $i ;done # 遍历数组，输出： 1 3 4 5

unset array; # 删除数组中的所有元素

for i in ${array[@]};do echo $i ;done # 遍历数组，数组元素为空，没有任何输出内容 
 

 
## VIM 使用
删除：在命令模式下可以用 dd 来删除整行；可以在 dd 前加数字来指定删除的行数；
可以用 d$ 来实现删除从光标处删到行尾的操作，也可以通过 d0 来实现从光标处删到行首的操作；如果想删除一个单词，
可以使用 dw ；如果要删除全文，可以在输入 :%d （其中 : 用来从命令模式进入末行模式）。 
撤销和恢复：在命令模式下输入 u 可以撤销之前的操作；通过 Ctrl+r 可以恢复被撤销的操作。

对内容进行排序：在命令模式下输入 %!sort 。 

查找操作需要输入 / 进入末行模式并提供正则表达式来匹配与之对应的内容，例如： 
/doc.*\. ，输入 n 来向前搜索，也可以输入 N 来向后搜索。

在输入 : 进入末行模式后可以对 vim 进行设定。 

设置 Tab 键的空格数： set ts=4 

设置显示/不显示行号： set nu /  set nonu 

设置启用/关闭高亮语法： syntax on /  syntax off 

设置显示标尺（光标所在的行和列）：  set ruler 

设置启用/关闭搜索结果高亮： set hls /  set nohls 

比较多个文件。[root ~]# vim -d foo.txt bar.txt
 

vim 非编辑模式

yy：复制光标当前行 

p：粘贴 

dd:删除光标当前行 

$:光标跳到当前行的行尾 

^:光标跳到当前行的行首 

:s/原字符串/新字符串/:替换光标当前行内容 

:%s/原字符串/新字符串/g:全文替换 #g 表示 global，i 表示 ignore 忽略大小写 /要查找的内容:从光标当前行向后查找内容 

/d #在文件中查找 d 字母 

?要查找的内容：从光标当前位置向前查找内容 

?d #查找文件中的 d 字母 

CTRL+F:向下翻 1 页 

CTRL+B:向上翻 1 页 

:set nu：显示文件的行号 

:set nonu: 去掉行号显示 

u:撤消 
:set ff :显示文件的格式 #unix 表示在 unix 上的文件 dos 表示文件是 windows 上的文件

:w ：表示保存文件

:q :表示退出 vim 命令

:wq:保存并退出

:w!:强制保存

:q!:强制退出但不保存

:wq!:强制保存并退出

i:表示进入编辑模式，并且光标在当前行

o：表示进入编辑模式，并且光标出现的当前行的下一行(新行) 
 
 
 
## tips
获取登录信息 - w / who / last/ lastb。 
查看命令的说明和位置 - whatis / which / whereis。 
查看帮助文档 - man / info / help / apropos。 
查看系统和主机名 - uname / hostname。 
时间和日期 - date / cal。 
重启和关机 - reboot / shutdown。 
查看文件内容 - cat / tac / head / tail / more / less / rev / od。 
文件重命名 - rename。 
查找文件和查找内容 - find / grep。 
创建链接和查看链接 - ln / readlink。 
将标准输入转成命令行参数 - xargs。 
显示文件或目录 - basename / dirname。 
sort - 对内容排序 
uniq - 去掉相邻重复内容 
tr - 替换指定内容为新内容 
cut/paste - 剪切/黏贴内容 
split - 拆分文件 
file - 判断文件类型 
wc - 统计文件行数、单词数、字节数 
wc -l linux 常用命令.txt #-l 表示 line 行数 计算文件的行数 
wc -w linux 常用命令.txt #-w 表示 word 单词个数 计算文件的单词个数 
iconv - 编码转换 
输出重定向和错误重定向 - > / >> / 2>。 
 

## 多重定向 - tee。 
下面的命令除了在终端显示命令 ls 的结果之外，还会追加输出到 ls.txt 文件中 
ls | tee -a ls.txt 
 
## 别名 alias 
alias ll='ls -l' 

alias frm='rm -rf' 

## 磁盘分区表操作 - fdisk。 

## 磁盘分区工具 - parted。 

## 格式化文件系统 - mkfs。 

## 文件系统检查 - fsck。 

## 转换或拷贝文件 - dd 
 
# 创建/激活/关闭交换分区 - mkswap / swapon / swapoff。 
 
## 文件同步工具 - rsync 
说明：使用 rsync 可以实现文件的自动同步，这个对于文件服务器来说相当重要。 


## 用一条命令强制终止正在运行的 Redis 进程。 
ps -ef | grep redis | grep -v grep | awk '{print $2}' | xargs kill -9 
 


## echo 命令 
echo #输出命令，可以输入变量，字符串的值 
echo Hello World #打印 Hello World 
echo $PATH #打印环境变量 PATH 的值,其中$是取变量值的符号，用法：$变量名 或者 
${变量名} 
echo -n #打印内容但不换行 
echo -n Hello World 

 
## >和>>命令 
和>>:输出符号，将内容输出到文件中，>表示覆盖(会删除原文件内容) >>表示追加

echo Hello World > 1.txt #将 Hello World 输出到当前目录下的 1.txt 文件
如果当前目录下没有 1.txt 文件会创建一个新文件， 
如果当前目录下有 1.txt，则会删除原文件内容，写入 Hello World

echo 1234 >> 1.txt #将 1234 追加到当前目录下的 1.txt 中，如果文件不存在会创建新文件通过>和>>都可以创建文件
 

## export
/etc/profile #linux 上的系统环境变量配置文件 
source /etc/profile #将系统环境变量生效 

export 导入全局变量(环境变量) 
export 变量名=变量值 
export 变量名 

变量的赋值: 
变量名=变量值 


环境变量修改 
通过 export 命令可以修改指定的环境变量。不过，这种方式修改环境
变量仅仅对当前 shell 终端生效，关闭 shell 终端就会失效。修改完
成之后，立即生效。
export CLASSPATH=./JAVA_HOME/lib;$JAVA_HOME/jre/lib 

通过 vim 命令修改环境变量配置文件。这种方式修改环境变量永久有效。vim ~/.bash_profile 
如果修改的是系统级别环境变量则对所有用户生效，如果修改的是用
户级别环境变量则仅对当前用户生效。修改完成之后，需要 source 
命令让其生效或者关闭 shell 终端重新登录。source /etc/profile
 

## <<EOF 
<<EOF … EOF:将<<EOF 和 EOF 之间的多行内容传给前面的命令, 
其中 EOF 可以是任意字符串,但约定都使用 EOF 
 
 
## cut 
-f 参数,指定列

-d 参数指定列和列之间的分隔符,默认的分隔符是\t(行向制表符)

cut -f 1 1.txt #取 1.txt 文件中的第 1 列内容(列分隔符默认为\t)

cut -f 2 1.txt #取 1.txt 文件中的第 2 列内容

cut -f 1 -d ',' 3.txt #取 3.txt 文件中的第 1 列(列分隔符为,)

cut -f 2 -d ',' 3.txt #取 3.txt 第 2 列 

 
## sudo 命令 
sudo 命令,它在非 root 用户下,去调用一些 root 用户的命令,或者修改一些文件 

sudo 命令是需要配置的,sudo 的配置文件是/etc/sudoers 
用户配置 sudo 权限 
vim /etc/sudoers
Allow root to run any commands anywhere 
root ALL=(ALL) ALL 

bow 用户设置 sudo 命令权限 
bow ALL=(ALL) ALL 

[root@bow ~]# su - bow 
上一次登录：四 3 月 26 07:30:53 CST 2020pts/0 上 
[bow@bow ~]$ sudo vim /etc/profile 
 
 
## ifconfig 
ifconfig 命令属于 net-tools 软件包,使用前需要安装 net-tools 
net-tools 的安装: 
yum -y install net-tools 
ifconfig 查看 ip 地址 
 
## netstat 
netstat 命令也属于 net-tools 软件包 
netstat -tulp | grep 1521 #查看 oracle 监听器程序是否正常启动 
 
## rpm 
rpm -ivh .rpm 文件的路径 #表示安装软件包 
rpm -qa #查看已安装的软件 
rpm -qa | grep mysql #查看已经安装的 mysql 软件包 
rpm -e --nodeps 安装包名 #卸载软件包 -e 表示卸载 --nodeps 表示不理会的依赖关系 
 


## zip/unzip/tar/gzip
zip -q -r html.zip /home/html    ## 将/home/html/这个目录下所有文件和文件夹打包为当前目录下的 html.zip

unzip html.zip -d /home/         ## 将html.zip解压到/home路径下；

zip 2.zip 2.txt #将 2.txt 压缩到 2.zip 中

zip data.zip data #只会压缩文件夹,不会压缩文件夹下的内容

zip da.zip da/* #压缩文件夹和文件夹内的文件(压缩文件夹和它的下一级文件)

zip -r data.zip date #-r 表示递归地将文件夹及它的子目录文件全部压缩

unzip 2.zip #将 2.zip 压缩包解压到当前目录下

unzip -l 压缩文件名 #不解压文件,查看压缩包内的文件

unzip -l da.zip #查看 da.zip 压缩文件中包含的文件

unzip da.zip -d 目标目录 #将压缩文件解压到指定目录

unzip da.zip -d tm/ #将压缩文件 da.zip 解压到 tm 目录下

tar cvf 压缩文件名 要压缩的文件或目录

tar cvf 2.tar 2.txt #将 2.txt 压缩为 2.tar 包

tar cvf data.tar data #将 data 目录夸张到 data.tar 包中

tar xvf 2.tar #将 2.tar 解压到当前目录

tar xvf 2.tar -C a/ #将 2.tar 解压到 a 目录

tar xvf data.tar #解压 data.tar 到当前目录

tar zcvf 压缩文件名 要压缩的文件

tar zcvf tm.tar.gz tm #将当前目录下的 tm 目录压缩为 tm.tar.gz

tar zxvf 压缩文件名

tar zxvf tm.tar.gz #将 tm.tar.gz 解压到当前目录

gzip 命令,将文件压缩为.gz 包(可以用来压缩.tar 文件)

gzip 要压缩的文件

gzip 2.txt #将 2.txt 压缩为 2.txt.gz

gzip data.tar #将 data.tar 压缩为 data.tar.gz 




## awk
模式匹配和处理语言。可以从文本中提取出指定的列、用正则表达式从文本中取出我们想要的内容、显示指定的行以及进行统计和运算。

-F     指定输入行的字段分隔符，以便将数据切分成不同的段
awk -F "\"" '{print $6}' 

假设有一个名为 fruit2.txt 的文件，内容如下所示。 
[root ~]# cat fruit2.txt 
1 banana 120 
2 grape 500 
3 apple 1230 
4 watermelon 80 
5 orange 400 
 
### 显示文件的第 3 行。 
[root ~]# awk 'NR==3' fruit2.txt 
3 apple 1230 
 
### 显示文件的第 2 列。 
[root ~]# awk '{print $2}' fruit2.txt 
banana 
grape 
apple 
watermelon 
orange 
 
### 显示文件的最后一列。 
[root ~]# awk '{print $NF}' fruit2.txt 
120 
500 
1230 
80 
400 
 
### 输出末尾数字大于等于 300 的行。 
[root ~]# awk '{if($3 >= 300) {print $0}}' fruit2.txt 
2 grape 500 
3 apple 1230 
5 orange 400


## sed
字符流编辑器--sed，sed 是操作、过滤和转换文本内容的工具。 

假设有一个名为 fruit.txt 的文件，内容如下所示。 
[root ~]# cat -n fruit.txt 
1 banana 
2 grape 
3 apple 
4 watermelon 
5 orange 

### 第 2 行后面添加一个 pitaya
[root ~]# sed '2a pitaya' fruit.txt 
banana 
grape 
pitaya 
apple 
watermelon 
orange 

### 在第 2 行前面插入一个 waxberry
[root ~]# sed '2i waxberry' fruit.txt 
banana 
waxberry 
grape 
apple 
watermelon 
orange 

### 删除第 3 行 
[root ~]# sed '3d' fruit.txt 
banana 
grape 
watermelon 
orange 

### 删除第 2 行到第 4 行。 
[root ~]# sed '2,4d' fruit.txt 
banana 
orange 

### 将文本中的字符 a 替换为@。 
[root ~]# sed 's#a#@#' fruit.txt 
b@nana 
gr@pe 
@pple 
w@termelon 
or@nge 

### 将文本中的字符 a 替换为@，使用全局模式 
[root ~]# sed 's#a#@#g' fruit.txt 
b@n@n@ 
gr@pe 
@pple 
w@termelon 
or@nge 

### sed 可以查找日志文件特定的一段, 根据时间的一个范围查询，可以按照行号和时间范围查询 
按照行号
sed -n '5,10p' filename 这样你就可以只查看文件的第 5 行到第 10 行。 
按照时间段 
sed -n '/2014-12-17 16:17:20/,/2014-12-17 16:17:36/p' test.log 

## find
find 通常用来再特定的目录下搜索符合条件的文件，也可以用来搜索特定用户属主的文件。

find /home -name test.txt    查找/home路径下文件名为test.txt的文件

find /home -type d -name test    查找/home路径下文件夹为test的文件


## grep 
grep 命令是一种强大的文本搜索工具，grep 搜索内容串可以是正则表达式，允许对文本文件进行模式查找。如果找到匹配模式，grep 打印包含模式的所有行。 



## git
gitlab提交代码流程
1. gitlab新建分支 hotfix/master/etcdconf/chron--- bug用hotfix，功能代码用feature，master指的是要合并的分支，etcdconf--功能路径，chron用户；feature/master/backupdriver/dongxiang
2. 本地拉取分支git fetch origin feature/master/backupdriver/dongxiang；
3. 根据远端分支新建本地分支git checkout -b feature/backup origin/feature/master/backupdriver/dongxiang；
4. 将本地另一个分支上的修改cherry-pick到新建分支git cherry-pick 5e882fedd834c4e2a4c8d41a69d565324f98c56a；
5. 提交到远端分支git push origin HEAD:feature/master/backupdriver/dongxiang；

### git恢复reset的代码
首先git reflog查看提交记录；
git rebase -i HEAD@{2}   进入vim直接:q！，解决冲突，提交，恢复之前reset掉的代码；

### git远程分支已经删除，本地如何更新
直接执行git remote prune origin即可；

### git查看代码行
git ls-files | xargs wc -l

gitignore加./idea

### 远端分支和master改动一致
本地checkout -b新分支，然后git pull，之后git rebase master，最后git push origin HEAD:sbx即可，不需要merge request；


### 配置 user.name 以及 user.email 
git 使用你的用户名将提交与身份相关联。 git config 命令可用来更改你的 git 配置，包括你的用户名。 
 
----git config --global user.name dongxiang 

----git config --global user.email dong.xiangxiang@zte.com.cn

git config --list                  # 列出 Git 可以在该处找到的所有的设置


### Git 终端的配置，生成公钥文件 
1. ssh-keygen -t rsa -C (这里是你的邮箱地址) # 之后一路回车即可； 
2. 执行完后，在指定路径生成公钥文件； 
linux 路径：~/.ssh 
windows 路径：c 盘>用户>自己的用户名>.ssh 
3. 将公钥 id_rsa.pub 文本内容拷贝到远端 ssh keys 中。

### git 修改上次提交 
修改后执行 git add .； 
然后 git commit --amend，把文件和上次提交合并（--amend 可以保持 change_Id 和上次
一样，如果被删掉的话，这条命令会生成新的 chang_id,此时如果想合并到上次的修改
中，必须复制上次的 Change_Id 作为本次的 Change_id）; 
最后 git push origin HEAD:refs/for/master 
 
git 本地只有在 add+commit 之后才能出现 master 分支； 
 
 
### git 添加 github 远程仓库 
----- git remote add -m master origin git@github.com:SHANExiang/blog.git

### git 本地 master 分支与远程 master 分支关联 
当在本地新建一个已经存在代码的本地仓库时，想将这个仓库与远端的仓库关联，即 
----- git branch --set-upstream-to=origin/master master 

Branch 'master' set up to track remote branch 'master' from 'origin'. 
----- git push origin master 报错 
To github.com:SHANExiang/blog.git 
! [rejected] master -> master (non-fast-forward) 
error: failed to push some refs to 'git@github.com:SHANExiang/blog.git' 
----- git pull 
fatal: refusing to merge unrelated historie 
----- git pull --allow-unrelated-histories 
Merge made by the 'recursive' strategy. 
README.md | 1 + 
1 file changed, 1 insertion(+) 
create mode 100644 README.md 
可以发现将两者合并起来了； 
----- git push origin master 
 
 
### git 版本回退 
本地修改但未 add/commit
git checkout .      # 撤销对所有已修改但未提交的文件的修改，但不包括新增的文件

git checkout [filename]     # 撤销对指定文件的修改，[filename]为文件名


已 add/commit 但未 push 
git revert commitID # 其实，git revert 可以用来撤销任意一次的修改，不一定要是最近一次 
或 git reset --hard commitID/git reset --hard HEAD^  
HEAD 表示当前版本，几个^表示倒数第几个版本，倒数第 100 个版本可以用HEAD~100）； 
参数--hard：强制将暂存区和工作区都同步到指定的版本 
git reset 和 git revert 的区别是：reset 是用来回滚的，将 HEAD 的指针指向了想要回滚的
版本，作为最新的版本，而后面的版本也都没有了；而 revert 只是用来撤销某一次更改，
对之后的更改并没有影响，然后再用 git push -f 提交到远程仓库。 
 

已 push 
首先查询这个文件的 log 
其次查找到这个文件的上次 commit id xxx，并对其进行 reset 操作 
再撤销对此文件的修改 
最后 amend 一下，再 push 上去 

$ git log <fileName> 
$ git reset <commit-id> <fileName> 
$ git checkout <fileName> 
$ git commit --amend 
$ git push origin <remoteBranch>


### 查看操作记录 
----- git reflog


### 工作区和暂存区 
git 管理的问题的修改，它只会提交暂存区的修改来创建版本； 
 
撤销工作区的修改----- git checkout -- 文件名 

作了修改，但还没 git add，撤销到上一次提交：git checkout -f -- filename；git checkout -f -- . 

把暂存区的修改撤销掉，重新放回工作区----- git reset HEAD file 
作了修改，并且已经 git add，但还没 git commit： 
先将暂存区的修改撤销：git reset HEAD filename/git reset HEAD；此时修改只存在于工
作区，变为了 "unstaged changes"； 
再利用上面的 checkout 命令从工作区撤销修改 

### 对比工作区和某个版本中文件的不同 
----- git diff HEAD -- file 
+表示工作区中的内容，-表示版本中的内容；

### 对比两个版本间文件的不同 
----- git diff HEAD HEAD^ -- file 
+表示 HEAD 版本文件内容，-表示 HEAD^版本文件的内容


### 删除一个文件 
----- git rm file 

### 分支操作 
----- git branch # 查看当前分支 

----- git branch 分支名 # 创建分支

----- git checkout -b dev # 创建分支 dev，并切换到它上面（也就是将 head 指向当前分支）

----- git checkout 分支名 # 切换分支

----- git merge 分支名 # 合并某分支到当前分支

----- git branch -d 分支名 # 删除分支

----- git log --graph --pretty=oneline   # 看到分支的合并情况 
 

### fast-forward    快速合并--分支管理策略 
1. checkout 一个分支 dev 上做了 commit，之后 checkout master 分支，对同一地方做了修
改，之后再 master 上执行 merge 操作，由于默认执行的是快速合并，这这个快速合并只
能试图把各自的修改合并起来，但是会有冲突，手动解决冲突后， 就是一次新的提交； 
2. 通常，合并分支时，如果可能，git 会用 fast-forward 模式，但是有些快速合并不能成
功而且合并时没有冲突，这个时候会合并之后并做一次新的提交； 
3. 禁用 fast-forward，----- git merge --no-ff -m "禁用 fast-forward 模式" dev  由于本次合
并要创建一个新的 commit,所以加上-m 参数； 

### 工作现场保存 git stash 
----- git stash # 暂时保存工作现场

----- git stash pop # 回到工作现场

git stash drop 命令用于删除隐藏的项目。默认情况下，它将删除最后添加的存储项，如果提供参数的话，它还可以删除特定项。

下面举个例子。 
如果要从隐藏项目列表中删除特定的存储项目，可以使用以下命令： 
git stash list：它将显示隐藏项目列表，如： 
stash@{0}: WIP on master: 049d078 added the index file stash@{1}: WIP on master: c264051 
Revert “added file_size” stash@{2}: WIP on master: 21d80a5 added number to log 
如果要删除名为 stash@{0} 的项目，请使用命令 git stash drop stash@{0}。 

### git 标签操作 
首先切换到需要打标签的分支上，然后使用 git tag v1.0 就可以在当前 commit 打上 v1.0 的标签 
git tag v1.0 commitID 对特定 commit 打标签 
打标签时加上 message：git tag -a <tagname> -m "message" 
git tag 查看所有标签 

git show [tagname] 查看标签详细信息

git push origin <tagname>可以推送一个本地标签到远程仓库

git push origin --tags 可以推送全部未推送过的本地标签

git tag -d <tagname>可以删除一个本地标签

git push origin :refs/tags/<tagname>可以删除一个远程标签（先从本地删除）


### git pull 和 git fetch 有什么区别？ 
git pull 命令从中央存储库中提取特定分支的新更改或提交，并更新本地存储库中的目标分支。

git fetch 也用于相同的目的，但它的工作方式略有不同。当你执行 git fetch 时，它会从
所需的分支中提取所有新提交，并将其存储在本地存储库中的新分支中。如果要在目标
分支中反映这些更改，必须在 git fetch 之后执行 git merge 。只有在对目标分支和获取
的分支进行合并后才会更新目标分支。为了方便起见，请记住以下等式： 
git pull = git fetch + git merge

### 如何找到特定提交中已更改的文件列表？ 
要获取特定提交中已更改的列表文件，请使用以下命令：git diff-tree -r {hash}

给定提交哈希，这将列出在该提交中更改或添加的所有文件。 -r 标志使命令列出单个文件，而不是仅将它们折叠到根目录名称中。

你还可以包括下面提到的内容，虽然它是可选的，但有助于给面试官留下深刻印象。输
出还将包含一些额外信息，可以通过包含两个标志把它们轻松的屏蔽掉： 
git diff-tree –no-commit-id –name-only -r {hash} 
这里 -no-commit-id 将禁止提交哈希值出现在输出中，而 -name-only 只会打印文件名
而不是它们的路径。 

### git merge 和 git rebase 之间有什么区别？ 
简单的说，git merge 和 git rebase 都是合并分支的命令。  
git merge branch 会把 branch 分支的差异内容 pull 到本地，然后与本地分支的内容一并
形成一个 committer 对象提交到主分支上，合并后的分支与主分支一致；
git rebase branch 会把 branch 分支优先合并到主分支，然后把本地分支的 commit 放到主
分支后面，合并后的分支就好像从合并后主分支又拉了一个分支一样，本地分支本身不会保留提交历史。 





## 网卡配置
ip addr add 10.50.114.157/32 dev eth0        ## 增加网卡地址

ip addr del 10.50.114.157/32 dev eth0        ## 删除网卡地址

ip route add default via 10.50.114.157 dev eth0       ## 增加默认路由

ip addr show dev eht0                        ## 查看网口的配置信息


1. 可以直接修改配置文件/etc/sysconfig/network-scripts/中的ifcfg-eth0；
如果一个网卡配置多个ip地址，则新增文件/etc/sysconfig/network-scripts/中的ifcfg-eth0:1；

2. BOOTPROTO设置为 dhcp 后，系统会在引导过程中自动向 DHCP 服务器发送请求，以获取 IP 地址和其他相关配置。
这样，你无需手动配置网络接口，系统会自动从 DHCP 服务器获取所需的网络配置信息，并将其应用于相应的网络接口。
手动配置网络接口，可以将 BOOTPROTO 设置为其他值，比如 static（静态IP地址）或 none（禁用IP配置）。
 




## tcpdump
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


## 查看socket信息
netstat -napt或ss -ntlp
-n表示不显示名字，而是以数字方式显示ip和端口
-l只显示LISTEN状态的socket
-p表示显示进程信息
-t表示只显示tcp连接

ss -tunlp|grep 9696     查看端口打开情况

网络性能指标
带宽：链路的最大传输速率，b/s；
吞吐量：单位事件内成功传输的数据量；b/s(比特/s)或B/s(字节/s)



## 证书
CA 证书文件（ssl_cacert.pem）---即根证书；
客户端证书和私钥文件----cert.pem, key.pem


## 用户及用户组
添加用户---adduser dx

设置用户密码---passwd dx

添加用户组---groupadd docker

将用户加到用户组中---usermod -aG docker dx

激活对用户组的修改---newgrp docker

创建和删除用户 - useradd / userdel 

创建和删除用户组 - groupadd / groupdel。 

说明：用户组主要是为了方便对一个组里面所有用户的管理。 
-d - 创建用户时为用户指定用户主目录 
-g - 创建用户时指定用户所属的用户组 


修改密码 - passwd
```shell
[root ~]# passwd hellokitty 
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully. 
```
 
查看和修改密码有效期 - chage。 
设置 hellokitty 用户 100 天后必须修改密码，过期前 15 天通知该用户，过期后 15 天禁用该用户。 
chage -M 100 -W 15 -I 15 hellokitty 

编辑 sudoers 文件 - visudo。 

显示用户与用户组的信息 - id。 

给其他用户发消息 -write / wall。 

查看/设置是否接收其他用户发送的消息 - mesg。 

chown - 改变文件所有者。 
[root ~]# chown hellokitty readme.txt 



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
    state BACKUP    ## 主服务器为MASTER，备服务器为BACKUP
    interface eth0  ## 替换为备份服务器上的网络接口名称
    virtual_router_id 51  ## 虚拟路由器 ID，与主服务器配置相同
    priority 90  ## 备份服务器的优先级较低
    advert_int 1  ## 广告间隔，单位为秒
    authentication {
        auth_type PASS
        auth_pass your_password  ## 与主服务器配置相同的密码
    }
    virtual_ipaddress {
        192.168.0.100  ## 虚拟 IP 地址，与主服务器配置相同
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


 
## 虚拟环境 virtualenv 
virtualenv----虚拟环境，就是可以在一个主机上，自定义出多套的 python 环境，
多套环境中使用不同的 python 解析器，环境变量设置，第三方依赖包，执行不
同的测试命令，最重要的是各个环境之间互不影响，相互隔离； 
virtualenv 使用 
1. pip install virtualenv； 
2. cd 到存放虚拟环境的目录，执行 virtualenv ENV----在当前目录下创建名为 ENV
的虚拟环境（如果第三方包 virtualenv 安装在 python3 下面，此时创建的虚拟环
境就是基于 python3 的）; virtualenv -p /usr/local/bin/python2.7 ENV2 参数 -p 指
定 python 版本创建虚拟环境 virtualenv --system-site-packages ENV 参数 --
system-site-packages 指定创建虚拟环境时继承系统三方库 
3. source bin/activate 激活虚拟环境 pip list 查看当前虚拟环境下所安装的第三方
库 deactivate 退出虚拟环境 
4. 删除的话，直接删除虚拟环境所在目录即可。 
 

## iptables 使用 
iptables –I INPUT –s 192.168.11.12 –p –tcp m tcp –j DROP 
iptables –D INPUT –s 192.168.11.12 –p –tcp m tcp –j DROP 
iptables –nL |grep 192.168.11.12 

linux 下的后台进程管理利器 supervisor 
每次文件修改后再 linux 执行 service supervisord restart 


## 用过 ping 命令么？简单介绍一下。TTL 是什么意思？ 
ping : 查看与某台机器的连接情况。TTL：生存时间。数据报被路由器丢弃之前允许通过的网段数量。 


## 怎么判断一个主机是不是开放某个端口？
telnet IP 地址 端口 
telnet 127.0.0.1 3389 




## 格式化json
cat test.json |python -m json.tool；


## pdcp
用于将文件或目录传输到多个远程主机上。
pdcp -w remote1,remote2,...remote10 local_file remote_directory/  将本地的local_file复制到每个指定的远程主机的remote_directory目录下。
pdcp的常用选项包括：
-w：指定要复制的远程主机列表，可以使用逗号分隔的主机名、IP地址或者主机列表文件。
-R：递归地复制整个目录。
-p：保留源文件的权限、所有者和时间戳等元数据。
-l：限制并行复制的最大进程数。


#### pdsh
使用pdsh命令在多个远程主机上同时执行命令。需要在每个主机上安装pdsh包。
使用实例：pdsh -w host1,host2,host3 systemctl restart httpd



## k8s部署
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
bash-5.1## grep -rw "calico" hosts 
// Network plugins supported: calico, flannel, kube-router, cilium, kube-ovn
CLUSTER_NETWORK="calico"
步骤三：添加集群的ip地址
添加master、worker节点ip地址，此处需要在hosts文本里添加即可
 
步骤四：修改数据目录
bash-5.1## grep -rw "/mnt" config.yml
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
##新建目录：
mkdir key && cd key

##生成证书
openssl genrsa -out dashboard.key 2048 

##我这里写的自己的node1节点，因为我是通过nodeport访问的；如果通过apiserver访问，可以写成自己的master节点ip
openssl req -new -out dashboard.csr -key dashboard.key -subj '/CN=10.13.1.3'
openssl x509 -req -in dashboard.csr -signkey dashboard.key -out dashboard.crt 

##删除原有的证书secret
kubectl delete secret kubernetes-dashboard-certs -n kubernetes-dashboard

##创建新的证书secret
kubectl create secret generic kubernetes-dashboard-certs --from-file=dashboard.key --from-file=dashboard.crt -n kubernetes-dashboard

##查看pod
kubectl get pod -n kubernetes-dashboard

##重启pod
kubectl delete pod kubernetes-dashboard-7b544877d5-2xqcr  -n kubernetes-dashboard

## 创建用户令牌
创建ServiceAccount-->绑定关系ClusterRoleBinding-->获取令牌
kubectl -n kubernetes-dashboard describe secret $(kubectl -n kubernetes-dashboard get secret | grep admin-user | awk '{print $1}')
```

## helm
安装helm
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 \
    && chmod 700 get_helm.sh \
    && ./get_helm.sh


## deepcopy-gen使用
deepcopy-gen -v 5 -h hack/boilerplate.go.txt --bounding-dirs . -i volcano.sh/apis/pkg/apis/scheduling/v1beta1 -O zz_generated.deepcopy
-v 5 指定输出内容的详细程度
-h boilerplate.txt指定所有生成的文件的头部声明内容
--bounding-dirs .指定生成目录为当前路径
-i github.com/lt90s/deepcopy-gen-demo/types指定此package需要进行代码生成
-O zz_generated.deepcopy指定生成的文件名称为zz_generated.deepcopy.go



## client-gen
client-gen --clientset-name versioned -i volcano.sh/apis/pkg/apis/scheduling/v1beta1 --output-package clientset --go-header-file hack/boilerplate.go.txt -v 5
volcano.sh/apis增加资源client
拉代码到本地直接执行./hack/update-codegen.sh即可在本地生成client







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



 
## 安装 Python 3.6 
[root ~]# yum install gcc 
[root ~]# wget https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tgz 
[root ~]# gunzip Python-3.6.5.tgz 
[root ~]# tar -xvf Python-3.6.5.tar 
[root ~]# cd Python-3.6.5 
[root ~]# ./configure --prefix=/usr/local/python36 --enable-optimizations 
[root ~]# yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel 
readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel 
[root ~]# make && make install 
... 
[root ~]# ln -s /usr/local/python36/bin/python3.6 /usr/bin/python3 
[root ~]# python3 --version 
Python 3.6.5 
[root ~]# python3 -m pip install -U pip 
[root ~]# pip3 --version 
说明：上面在安装好 Python 之后还需要注册 PATH 环境变量，将 Python 安装路
径下 bin 文件夹的绝对路径注册到 PATH 环境变量中。注册环境变量可以修改用
户主目录下的.bash_profile 或者/etc 目录下的 profile 文件，二者的区别在于前者
相当于是用户环境变量，而后者相当于是系统环境变量 
Linux 下的大多数服务都被设置为守护进程（驻留在系统后台运行，但不会因为
服务还在运行而导致 Linux 无法停止运行），所以我们安装的服务通常名字后面
都有一个字母 d ，它是英文单词 daemon 的缩写，例如：防火墙服务叫 firewalld，
我们之前安装的 MySQL 服务叫 mysqld，Apache 服务器叫 httpd 等。在安装好服
务之后，可以使用 systemctl 命令或 service 命令来完成对服务的启动、停止等
操作 
 
 
 
### centos7 下编译安装 python3 
1. 必须解决编译所需的基础开发环境 
yum install gcc patch libffi-devel python-devel zlib-devel bzip2-devel openssl-devel ncurses-
devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel -y 
2. 下载 python3 的编代码包,解压缩 
wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tar.xz 
xz -d Python-3.7.4.tar.xz 
tar -xf Python-3.7.4.tar 
3. 进入解压缩生成的源码文件夹 
cd Python-3.7.4 
4. 执行编译三部曲的命令 
第一曲：找到一个[配置的可执行文件，configure]，执行它，且指定软件安装位置 
./configure --prefix=/opt/python374/ 
第二曲：在上一步，会生成一个 makefile，编译安装，在 linux 下必须用 gcc 工具去编
译，使用的命令时 
make&&make 
第三曲：这一步是执行安装，会生成一个/opt/python374 文件夹，可用的解释器都在这里了 
make install 
5. 配置环境变量，便于快捷使用 python3 
1).先获取当前的 PATH 变量，然后把 python3 的 bin 目录加进去 
echo $PATH 
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin 

2).永久修改 PATH 的值 
-直接修改/etc/profile ，系统全局的配置文件，每个用户在登陆系统的时候，都会加载这
个文件 
vim /etc/profile 
-写入新的 PATH 变量 
PATH="/opt/python367/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/root/bin"

3).重新登陆，或者手动读取这个/etc/profile 
source /etc/profile # 让这个文件中的变量生效 
 
 
 
## Linux 下安装 openJDK 
1. 卸载旧版本 
rpm -qa|grep gcj 
如果有 gcj 文件，yum remove 即可； 
2. 安装 OpenJDK，只是 1.8 以上 
注意：安装 java-1.8.0-openjdk 后只有 jre，必须继续安装 yum install java-1.8.0-openjdk-
devel 
安装完后的路径为：/usr/lib/jvm 
yum install java-1.8.0-openjdk 
yum install java-1.8.0-openjdk-devel 
3. 配置环境变量 
vi /etc/profile 
export JAVA_HOME=/usr/lib/jvm/java-1.8.0 
export JRE_HOME=$JAVA_HOME/jre 
export 
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib 
export PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin 
 
 


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
srun --mpi=list   ## 列出已安装的mpi插件；
srun -w slurm[1-2] hostname  ## 交互式作业提交，提交命令后，等待作业执行完成之后返回命令行窗口。

scontrol show jobs        ## 查看MPI作业详细信息
scontrol show node              ##查看所有节点详细信息
scontrol show node node-name    ##查看指定节点详细信息
scontrol show node | grep CPU   ##查看各节点cpu状态
scontrol show node node-name | grep CPU ##查看指定节点cpu状态
scontrol token username=root lifespan=5000    ## 为用户root生成token，token有效期5000s


sacct	                ## 查看已完成作业

squeue                  ## 查看job的运行状态
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


##添加账户，指定账户名称和所属集群名称，这里的账户可以理解成用户组的概念
sacctmgr add account name=<your_account> cluster=cluster

##添加用户，指定所属账户
sacctmgr add user name=my_user_name account=<your_account>

##添加俩qos，分别叫normal和long
sacctmgr add qos normal
sacctmgr add qos long

##修改qos
sacctmgr modify qos normal set MaxWall=3-00:00:00 MaxTRES="gres/gpu=4" MaxTRESPU="gres/gpu=4" MaxJobsPU=4 MaxSubmitJobsPU=4
sacctmgr modify qos long set MaxWall=7-00:00:00 MaxTRES="gres/gpu=8" MaxTRESPU="gres/gpu=8" MaxJobsPU=1 MaxSubmitJobsPU=1

##设置account可使用的qoslevel
sacctmgr modify account my_account_name set QosLevel=normal,long



#### 场景
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
##SBATCH -N 1
##SBATCH --ntasks 1
##SBATCH --cpus-per-task=1
##SBATCH -t 1:00
##SBATCH -o /home/myapp.log
##SBATCH --ntasks-per-node 1
##SBATCH --mem 2000
```



#### slurm安装
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
## Cluster Name：集群名
 ClusterName=MyCluster ## 集群名，任意英文和数字名字

## Control Machines：Slurmctld控制进程节点
 SlurmctldHost=slurm ## 启动slurmctld进程的节点名，如这里的admin
## BackupController=   ## 冗余备份节点，可空着
 SlurmctldParameters=enable_configless ## 采用无配置模式

 ## Slurm User：Slurm用户
 SlurmUser=slurm ## slurmctld启动时采用的用户名

 ## Slurm Port Numbers：Slurm服务通信端口
 SlurmctldPort=6817 ## Slurmctld服务端口，设为6817，如不设置，默认为6817号端口
 SlurmdPort=6818    ## Slurmd服务端口，设为6818，如不设置，默认为6818号端口

 ## State Preservation：状态保持
 StateSaveLocation=/var/spool/slurmctld ## 存储slurmctld服务状态的目录，如有备份控制节点，则需要所有SlurmctldHost节点都能共享读写该目录
 SlurmdSpoolDir=/var/spool/slurmd ## Slurmd服务所需要的目录，为各节点各自私有目录，不得多个slurmd节点共享

 ## auth type
 AuthAltTypes=auth/jwt
 AuthAltParameters=jwt_key=/var/spool/slurm/statesave/jwt_hs256.key

 ReturnToService=1 ##设定当DOWN（失去响应）状态节点如何恢复服务，默认为0。
     ## 0: 节点状态保持DOWN状态，只有当管理员明确使其恢复服务时才恢复
     ## 1: 仅当由于无响应而将DOWN节点设置为DOWN状态时，才可以当有效配置注册后使DOWN节点恢复服务。如节点由于任何其它原因（内存不足、意外重启等）被设置为DOWN，其状态将不会自动更改。当节点的内存、GRES、CPU计数等等于或大于slurm.conf中配置的值时，该节点才注册为有效配置。
     ## 2: 使用有效配置注册后，DOWN节点将可供使用。该节点可能因任何原因被设置为DOWN状态。当节点的内存、GRES、CPU计数等等于或大于slurm.conf 中配置的值，该节点才注册为有效配置。￼

 ## Default MPI Type：默认MPI类型
 MPIDefault=pmi2
     ## MPI-PMI2: 对支持PMI2的MPI实现
     ## MPI-PMIx: Exascale PMI实现
     ## None: 对于大多数其它MPI，建议设置

 ## Process Tracking：进程追踪，定义用于确定特定的作业所对应的进程的算法，它使用信号、杀死和记账与作业步相关联的进程
 ProctrackType=proctrack/cgroup
     ## Cgroup: 采用Linux cgroup来生成作业容器并追踪进程，需要设定/etc/slurm/cgroup.conf文件
     ## Cray XC: 采用Cray XC专有进程追踪
     ## LinuxProc: 采用父进程IP记录，进程可以脱离Slurm控制
     ## Pgid: 采用Unix进程组ID(Process Group ID)，进程如改变了其进程组ID则可以脱离Slurm控制

 ## Scheduling：调度
 ## DefMemPerCPU=0 ## 默认每颗CPU可用内存，以MB为单位，0为不限制。如果将单个处理器分配给作业（SelectType=select/cons_res 或 SelectType=select/cons_tres），通常会使用DefMemPerCPU
 ## MaxMemPerCPU=0 ## 最大每颗CPU可用内存，以MB为单位，0为不限制。如果将单个处理器分配给作业（SelectType=select/cons_res 或 SelectType=select/cons_tres），通常会使用MaxMemPerCPU
 ## SchedulerTimeSlice=30 ## 当GANG调度启用时的时间片长度，以秒为单位
 SchedulerType=sched/backfill ## 要使用的调度程序的类型。注意，slurmctld守护程序必须重新启动才能使调度程序类型的更改生效（重新配置正在运行的守护程序对此参数无效）。如果需要，可以使用scontrol命令手动更改作业优先级。可接受的类型为：
     ## sched/backfill ## 用于回填调度模块以增加默认FIFO调度。如这样做不会延迟任何较高优先级作业的预期启动时间，则回填调度将启动较低优先级作业。回填调度的有效性取决于用户指定的作业时间限制，否则所有作业将具有相同的时间限制，并且回填是不可能的。注意上面SchedulerParameters选项的文档。这是默认配置
     ## sched/builtin ## 按优先级顺序启动作业的FIFO调度程序。如队列中的任何作业无法调度，则不会调度该队列中优先级较低的作业。对于作业的一个例外是由于队列限制（如时间限制）或关闭/耗尽节点而无法运行。在这种情况下，可以启动较低优先级的作业，而不会影响较高优先级的作业。
     ## sched/hold ## 如果 /etc/slurm.hold 文件存在，则暂停所有新提交的作业，否则使用内置的FIFO调度程序。

 ## Resource Selection：资源选择，定义作业资源（节点）选择算法
 SelectType=select/cons_tres
     ## select/cons_tres: 单个的CPU核、内存、GPU及其它可追踪资源作为可消费资源（消费及分配），建议设置
     ## select/cons_res: 单个的CPU核和内存作为可消费资源
     ## select/cray_aries: 对于Cray系统
     ## select/linear: 基于主机的作为可消费资源，不管理单个CPU等的分配

 ## SelectTypeParameters：资源选择类型参数，当SelectType=select/linear时仅支持CR_ONE_TASK_PER_CORE和CR_Memory；当SelectType=select/cons_res、SelectType=select/cray_aries和SelectType=select/cons_tres时，默认采用CR_Core_Memory
 SelectTypeParameters=CR_Core_Memory
     ## CR_CPU: CPU核数作为可消费资源
     ## CR_Socket: 整颗CPU作为可消费资源
     ## CR_Core: CPU核作为可消费资源，默认
     ## CR_Memory: 内存作为可消费资源，CR_Memory假定MaxShare大于等于1
     ## CR_CPU_Memory: CPU和内存作为可消费资源
     ## CR_Socket_Memory: 整颗CPU和内存作为可消费资源
     ## CR_Core_Memory: CPU和和内存作为可消费资源

 ## Task Launch：任务启动
 TaskPlugin=task/cgroup,task/affinity ##设定任务启动插件。可被用于提供节点内的资源管理（如绑定任务到特定处理器），TaskPlugin值可为:
     ## task/affinity: CPU亲和支持（man srun查看其中--cpu-bind、--mem-bind和-E选项）
     ## task/cgroup: 强制采用Linux控制组cgroup分配资源（man group.conf查看帮助）
     ## task/none: ##无任务启动动作

 ## Prolog and Epilog：前处理及后处理
 ## Prolog/Epilog: 完整的绝对路径，在用户作业开始前(Prolog)或结束后(Epilog)在其每个运行节点上都采用root用户执行，可用于初始化某些参数、清理作业运行后的可删除文件等
 ## Prolog=/opt/bin/prolog.sh ## 作业开始运行前需要执行的文件，采用root用户执行
 ## Epilog=/opt/bin/epilog.sh ## 作业结束运行后需要执行的文件，采用root用户执行

 ## SrunProlog/Epilog ## 完整的绝对路径，在用户作业步开始前(SrunProlog)或结束后(Epilog)在其每个运行节点上都被srun执行，这些参数可以被srun的--prolog和--epilog选项覆盖
 ## SrunProlog=/opt/bin/srunprolog.sh ## 在srun作业开始运行前需要执行的文件，采用运行srun命令的用户执行
 ## SrunEpilog=/opt/bin/srunepilog.sh ## 在srun作业结束运行后需要执行的文件，采用运行srun命令的用户执行

 ## TaskProlog/Epilog: 绝对路径，在用户任务开始前(Prolog)和结束后(Epilog)在其每个运行节点上都采用运行作业的用户身份执行
 ## TaskProlog=/opt/bin/taskprolog.sh ## 作业开始运行前需要执行的文件，采用运行作业的用户执行
 ## TaskEpilog=/opt/bin/taskepilog.sh ## 作业结束后需要执行的文件，采用运行作业的用户执行行

 ## 顺序：
    ## 1. pre_launch_priv()：TaskPlugin内部函数
    ## 2. pre_launch()：TaskPlugin内部函数
    ## 3. TaskProlog：slurm.conf中定义的系统范围每个任务
    ## 4. User prolog：作业步指定的，采用srun命令的--task-prolog参数或SLURM_TASK_PROLOG环境变量指定
    ## 5. Task：作业步任务中执行
    ## 6. User epilog：作业步指定的，采用srun命令的--task-epilog参数或SLURM_TASK_EPILOG环境变量指定
    ## 7. TaskEpilog：slurm.conf中定义的系统范围每个任务
    ## 8. post_term()：TaskPlugin内部函数

 ## Event Logging：事件记录
 ## Slurmctld和slurmd守护进程可以配置为采用不同级别的详细度记录，从0（不记录）到7（极度详细）
 SlurmctldDebug=debug ## 默认为info
 SlurmctldLogFile=/var/log/slurm/slurmctld.log ## 如是空白，则记录到syslog
 SlurmdDebug=debug ## 默认为info
 SlurmdLogFile=/var/log/slurm/slurmd.log ## 如为空白，则记录到syslog，如名字中的有字符串"%h"，则"%h"将被替换为节点名
 ##SlurmrestdDebug=debug
 ##SlurmrestdLogFile=/var/log/slurm/slurmrestd.log

 ## Job Completion Logging：作业完成记录
 JobCompType=jobcomp/mysql
 ## 指定作业完成是采用的记录机制，默认为None，可为以下值之一:
    ## None: 不记录作业完成信息
    ## Elasticsearch: 将作业完成信息记录到Elasticsearch服务器
    ## FileTxt: 将作业完成信息记录在一个纯文本文件中
    ## Lua: 利用名为jobcomp.lua的文件记录作业完成信息
    ## Script: 采用任意脚本对原始作业完成信息进行处理后记录
    ## MySQL: 将完成状态写入MySQL或MariaDB数据库

 ## JobCompLoc= ## 设定记录作业完成信息的文本文件位置（若JobCompType=filetxt），或将要运行的脚本（若JobCompType=script），或Elasticsearch服务器的URL（若JobCompType=elasticsearch），或数据库名字（JobCompType为其它值时）

 ## 设定数据库在哪里运行，且如何连接
 JobCompHost=localhost ## 存储作业完成信息的数据库主机名
 ## JobCompPort= ## 存储作业完成信息的数据库服务器监听端口
 JobCompUser=slurm ## 用于与存储作业完成信息数据库进行对话的用户名
 JobCompPass=SomePassWD ## 用于与存储作业完成信息数据库进行对话的用户密码

 ## Job Accounting Gather：作业记账收集
 JobAcctGatherType=jobacct_gather/linux ## Slurm记录每个作业消耗的资源，JobAcctGatherType值可为以下之一：
    ## jobacct_gather/none: 不对作业记账
    ## jobacct_gather/cgroup: 收集Linux cgroup信息
    ## jobacct_gather/linux: 收集Linux进程表信息，建议
 JobAcctGatherFrequency=30 ## 设定轮寻间隔，以秒为单位。若为-，则禁止周期性抽样

 ## Job Accounting Storage：作业记账存储
 AccountingStorageType=accounting_storage/slurmdbd ## 与作业记账收集一起，Slurm可以采用不同风格存储可以以许多不同的方式存储会计信息，可为以下值之一：
     ## accounting_storage/none: 不记录记账信息
     ## accounting_storage/slurmdbd: 将作业记账信息写入Slurm DBD数据库
 ## AccountingStorageLoc: 设定文件位置或数据库名，为完整绝对路径或为数据库的数据库名，当采用slurmdb时默认为slurm_acct_db

 ## 设定记账数据库信息，及如何连接
 AccountingStorageHost=localhost ## 记账数据库主机名
 ## AccountingStoragePort= ## 记账数据库服务监听端口
 ## AccountingStorageUser=slurm ## 记账数据库用户名
 ## AccountingStoragePass=SomePassWD ## 记账数据库用户密码。对于SlurmDBD，提供企业范围的身份验证，如采用于Munge守护进程，则这是应该用munge套接字socket名（/var/run/munge/global.socket.2）代替。默认不设置
 ## AccountingStoreFlags= ## 以逗号（,）分割的列表。选项是：
     ## job_comment：在数据库中存储作业说明域
     ## job_script：在数据库中存储脚本
     ## job_env：存储批处理作业的环境变量
 ## AccountingStorageTRES=gres/gpu ## 设置GPU时需要
 ## GresTypes=gpu ## 设置GPU时需要

 ## Process ID Logging：进程ID记录，定义记录守护进程的进程ID的位置
 SlurmctldPidFile=/var/run/slurmctld.pid ## 存储slurmctld进程号PID的文件
 SlurmdPidFile=/var/run/slurmd.pid ## 存储slurmd进程号PID的文件

 ## Timers：定时器
 SlurmctldTimeout=120 ## 设定备份控制器在主控制器等待多少秒后成为激活的控制器
 SlurmdTimeout=300 ## Slurm控制器等待slurmd未响应请求多少秒后将该节点状态设置为DOWN
 InactiveLimit=0 ## 潜伏期控制器等待srun命令响应多少秒后，将在考虑作业或作业步骤不活动并终止它之前。0表示无限长等待
 MinJobAge=300 ## Slurm控制器在等待作业结束多少秒后清理其记录
 KillWait=30 ## 在作业到达其时间限制前等待多少秒后在发送SIGKILLL信号之前发送TERM信号以优雅地终止
 WaitTime=0 ## 在一个作业步的第一个任务结束后等待多少秒后结束所有其它任务，0表示无限长等待

 ## Compute Machines：计算节点
 NodeName=slurm2 NodeAddr=192.168.32.227 CPUs=4 RealMemory=7800 Sockets=4 CoresPerSocket=1 ThreadsPerCore=1 State=UNKNOWN
 NodeName=slurm1 NodeAddr=192.168.32.210 CPUs=4 RealMemory=7800 Sockets=4 CoresPerSocket=1 ThreadsPerCore=1 State=UNKNOWN
 ## NodeName=gnode[01-10] Gres=gpu:v100:2 CPUs=40 RealMemory=385560 Sockets=2 CoresPerSocket=20 ThreadsPerCore=1 State=UNKNOWN ##GPU节点例子，主要为Gres=gpu:v100:2
     ## NodeName=node[1-10] ## 计算节点名，node[1-10]表示为从node1、node2连续编号到node10，其余类似
     ## NodeAddr=192.168.1.[1-10] ## 计算节点IP
     ## CPUs=48 ## 节点内CPU核数，如开着超线程，则按照2倍核数计算，其值为：Sockets*CoresPerSocket*ThreadsPerCore
     ## RealMemory=192000 ## 节点内作业可用内存数(MB)，一般不大于free -m的输出，当启用select/cons_res插件限制内存时使用
     ## Sockets=2 ## 节点内CPU颗数
     ## CoresPerSocket=24 ## 每颗CPU核数
     ## ThreadsPerCore=1 ## 每核逻辑线程数，如开了超线程，则为2
     ## State=UNKNOWN ## 状态，是否启用，State可以为以下之一：
         ## CLOUD   ## 在云上存在
         ## DOWN    ## 节点失效，不能分配给在作业
         ## DRAIN   ## 节点不能分配给作业
         ## FAIL    ## 节点即将失效，不能接受分配新作业
         ## FAILING ## 节点即将失效，但上面有作业未完成，不能接收新作业
         ## FUTURE  ## 节点为了将来使用，当Slurm守护进程启动时设置为不存在，可以之后采用scontrol命令简单地改变其状态，而不是需要重启slurmctld守护进程。当这些节点有效后，修改slurm.conf中它们的State。在它们被设置为有效前，采用Slurm看不到它们，也尝试与其联系。
               ## 动态未来节点(Dynamic Future Nodes)：
                  ## slurmd启动时如有-F[<feature>]参数，将关联到一个与slurmd -C命令显示配置(sockets、cores、threads)相同的配置的FUTURE节点。节点的NodeAddr和NodeHostname从slurmd守护进程自动获取，并且当被设置为FUTURE状态后自动清除。动态未来节点在重启时保持non-FUTURE状态。利用scontrol可以将其设置为FUTURE状态。
                  ## 若NodeName与slurmd的HostName映射未通过DNS更新，动态未来节点不知道在之间如何进行通信，其原因在于NodeAddr和NodeHostName未在slurm.conf被定义，而且扇出通信(fanout communication)需要通过将TreeWidth设置为一个较高的数字（如65533）来使其无效。若做了DNS映射，则可以使用cloud_dns SlurmctldParameter。
          ## UNKNOWN ## 节点状态未被定义，但将在节点上启动slurmd进程后设置为BUSY或IDLE，该为默认值。

 PartitionName=batch Nodes=slurm[1-2] Default=YES MaxTime=INFINITE State=UP
     ## PartitionName=batch ## 队列分区名
     ## Nodes=node[1-10] ## 节点名
     ## Default=Yes ## 作为默认队列，运行作业不知明队列名时采用的队列
     ## MaxTime=INFINITE ## 作业最大运行时间，以分钟为单位，INFINITE表示为无限制
     ## State=UP ## 状态，是否启用
     ## Gres=gpu:v100:2 ## 设置节点有两块v100 GPU卡，需要在GPU节点 /etc/slum/gres.conf 文件中有类似下面配置：
         ##AutoDetect=nvml
         ##Name=gpu Type=v100 File=/dev/nvidia[0-1] ##设置资源的名称Name是gpu，类型Type为v100，名称与类型可以任意取，但需要与其它方面配置对应，File=/dev/nvidia[0-1]指明了使用的GPU设备。
         ##Name=mps Count=100
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

7. 其余步骤参考 https://hmli.ustc.edu.cn/doc/linux/slurm-install/slurm-install.html##id1




## terraform

#### terraform安装
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/RHEL/hashicorp.repo
sudo yum -y install terraform


#### 通过terraform编排openstack
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
[root@openstack--1 terraform-openstack-instances-main]## cat 000-input-variables.tf
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
[root@openstack--1 terraform-openstack-instances-main]## cat 010-computes.tf
##
## Create instance
##
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

## Create network port

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

## Create floating ip
resource "openstack_networking_floatingip_v2" "ip" {
  count = var.is_public ? 1 : 0

  pool = var.public_ip_network
}

## Attach floating ip to instance
resource "openstack_compute_floatingip_associate_v2" "ipa" {
  count = var.is_public ? 1: 0

  floating_ip = openstack_networking_floatingip_v2.ip[count.index].address
  instance_id = openstack_compute_instance_v2.instance.id
}
```

6. 设置输出形式
```shell
[root@openstack--1 terraform-openstack-instances-main]## cat 999-outputs.tf
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
#### kubeflow
它提供了一套工具和组件，用于构建、训练、部署和管理机器学习模型的端到端工作流程。

#### rancher
Rancher提供了在生产环境中使用的管理Docker和Kubernetes的全栈化容器部署与管理平台。
也就是提供可视化web界面进行k8s部署；

#### CRD
它代表自定义资源定义（Custom Resource Definition）。CRD 允许用户扩展 Kubernetes API，以添加自定义资源和自定义控制器。
Kubernetes 中的资源（Resource）是 API 对象的实例，例如 Pod、Service、Deployment 等。这些资源都有相应的 API 定义和控制器，用于管理它们的生命周期和状态。
CRD 允许用户定义自己的资源类型，这些资源类型可以扩展 Kubernetes 的功能，以满足特定的需求。用户可以创建自定义资源定义，定义自己的资源结构和行为，并编写自定义控制器来管理这些资源。

#### HPC
高性能计算（High Performance Computing，缩写HPC）指利用聚集起来的计算能力来处理标准工作站无法完成的数据密集型的计算任务。

#### tensorflow
TensorFlow是一个基于数据流编程的符号数学系统，被广泛应用于各类机器学习算法的编程实现。
PS-worker模型：Parameter Server执行模型相关业务，Work Server训练相关业务，推理计算、梯度计算等。

#### NPU
神经处理单元（Neural Processing Unit）的缩写，它是一种专门设计用于进行人工神经网络计算的处理器。NPU 的设计旨在加速深度学习任务，包括图像识别、语音识别、自然语言处理等


## volcano
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





## 调度GPU

#### k8s中使用GPU
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


nvidia-smi -L         ## 确认pod使用了GPU卡
或者kubectl describe pod 能够看到类似于 nvidia.com/gpu: 1 的资源分配信息；



## jenkins
持续集成----频繁将代码集成到主干；
持续交付----频繁将软件的新版本交付该质量团队或者用户，以供审批；
持续部署----代码评审或者测试通过后，将其自动部署到生产环境； 

