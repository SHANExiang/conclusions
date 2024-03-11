<!-- vscode-markdown-toc -->
* [python概述](#python)
* [python2 与 python3 区别](#python2python3)
* [py2 项目如何迁移成 py3](#py2py3)
* [python 解释代码原理，函数怎么解析的](#python-1)
* [GIL](#GIL)
* [装饰器](#)
	* [1. 不带参数的装饰器](#-1)
	* [2. 带参数的装饰器](#-1)
* [可迭代对象、生成器、迭代器的区别](#-1)
* [copy 和 deepcopy 区别](#copydeepcopy)
* [进程、线程、协程](#-1)
* [python 中 IO 密集型为什么用多线程](#pythonIO)
* [简述__new__和__init__区别](#__new____init__)
* [python 内存管理机制](#python-1)
	* [python 内存池](#python-1)
	* [垃圾回收](#-1)
		* [引用计数 PyObject](#PyObject)
		* [标记清除](#-1)
	* [分代回收](#-1)
	* [哪些操作会导致 Python 内存泄露，怎么处理？](#Python)
* [python 中单例模式实现方式](#python-1)
* [python 编码](#python-1)
	* [python 三元运算子](#python-1)
	* [python 支持一个表达式进行多种比较操作，其实这个表达式本质是由多个隐式的 and](#pythonand)
	* [python 身份运算符 is 和 is not](#pythonisisnot)
	* [如何判断一个值是方法还是函数？](#-1)
	* [文档字符串](#-1)
	* [了解类型注解么？](#-1)
	* [猴子补丁](#-1)
	* [介绍 Cython，Pypy Cpython Numba 各有什么缺点](#CythonPypyCpythonNumba)
* [python 新式类和经典类的区别](#python-1)
	* [字典和集合解析](#-1)
	* [遍历字典两种方式](#-1)
	* [判断字典中是否有某一 key](#key)
	* [获得字典的最大深度：](#-1)
	* [字典如何删除键和合并两个字典](#-1)
	* [合并两个字典](#-1)
	* [列表嵌套字典的排序，分别根据年龄和姓名排序**](#-1)
	* [根据键对字典排序](#-1)
* [bytes 字节](#bytes)
* [set 集合](#set)
	* [创建非空元素](#-1)
	* [集合 set 不支持索引；也不支持元素删除，比如 del s[1]；](#setdels1)
	* [取两个列表的交集](#-1)
	* [集合并集](#-1)
	* [集合中添加元素](#-1)
	* [集合中删除元素](#-1)
	* [判断 set1 是否是 set2 的子集合](#set1set2)
	* [从 set1 中移除两个集合的交集；](#set1)
	* [取 set1 中的元素且不在 set2](#set1set2-1)
	* [对称差异，将两个集合的对称差作为新集合返回(即恰好在集合之一中的所有元素)](#-1)
* [函数](#-1)
	* [python 函数参数类型](#python-1)
	* [lambda 匿名函数](#lambda)
	* [内建函数](#-1)
		* [range](#range)
		* [sum](#sum)
		* [sorted](#sorted)
		* [reversed](#reversed)
		* [locals 与 globals](#localsglobals)
		* [ord 与 chr](#ordchr)
		* [all](#all)
		* [any](#any)
		* [map](#map)
		* [reduce](#reduce)
		* [zip](#zip)
		* [filter](#filter)
		* [slice](#slice)
		* [exec 与 eval](#execeval)
	* [python 函数参数传递方式](#python-1)
* [迭代器](#-1)
* [生成器](#-1)
	* [怎么获取 return 的值？](#return)
	* [yield 与 yield from](#yieldyieldfrom)
* [什么是闭包？](#-1)
* [python 中的魔法方法](#python-1)
* [封装](#-1)
* [继承](#-1)
* [多态](#-1)
	* [元类](#-1)
		* [元类控制器实例的创建](#-1)
* [正则表达式](#-1)
		* [多线程同步方式](#-1)
		* [threading.Event](#threading.Event)
		* [threading.Condition](#threading.Condition)
		* [threading.Semaphore信号量](#threading.Semaphore)
		* [threading.Lock](#threading.Lock)
		* [threading.RLock](#threading.RLock)
		* [线程池](#-1)
	* [多进程](#-1)
		* [进程间通信 Queue](#Queue)
		* [进程池 Pool](#Pool)
		* [daemon 和 join 的区别](#daemonjoin)
	* [协程](#-1)
		* [asyncio](#asyncio)
	* [queue](#queue)
	* [死锁](#-1)
		* [json.dump 显示中文](#json.dump)
* [快速排序](#-1)
		* [不用+号，两个数相加](#-1)
* [异常](#-1)
* [设计模式](#-1)
	* [单例模式的应用场景有那些？](#-1)
	* [代理模式](#-1)
	* [工厂模式](#-1)
	* [策略模式](#-1)
	* [访问者模式](#-1)
	* [抽象工厂模式](#-1)
	* [装饰器模式](#-1)
	* [模板方法模式](#-1)
	* [享元模式](#-1)
	* [责任链模式](#-1)
	* [适配器模式](#-1)
	* [观察者模式](#-1)
* [1. 合并两个无序链表](#-1)
* [2. 一次遍历获取列表第二大值 ok](#ok)
* [3. 快排](#-1)
* [4. 归并排序](#-1)

<!-- vscode-markdown-toc-config
	numbering=false
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->



# 1. python
## <a name='python'></a>python概述
 Python 是一种解释型语言，就是解释一行，运行一行；解释型语言使用解释器将源码逐行解释成机器码并立即执行，不会进行整体性的编译和链接处理，相当于把编译语言中的编译和解释混合到一起同时完成。 可解释 
 强类型、动态类型语言。不允许隐式类型转换，比如不允许"12"+34。在声明变量时，不需要说明变量的类型； 具有动态特性 
 非常适合面向对象的编程（OOP），因为它支持继承的方式定义类；面向对象 
 Python 中没有访问说明符（access specifier，类似 C++中的 public 和 private）； 
 在Python 语言中，函数是 first-class objects。这指的是它们可以被指定给变量，函数既能返回函数类型，也可以接受函数作为输入； 
 Python 代码编写快，但是运行速度比编译语言通常要慢。好在 Python 允许加入基于 C 语言编写的扩展，因此我们能够优化代码，消除瓶颈。numpy 就是一个很 好地例子，它的运行速度非常快，因为很多算术运算其实并不是通过 Python 实 现的； 
 Python 用途非常广泛——网络应用，自动化，科学建模，大数据应用等等； 
 python 开源，具有强大的社区支持； 

Python 最大的缺点： 
 执行速度：虽然 Cpython 也会先将代码编译为字节码，然后交给解释器执行， 但是速度还是比纯编译型语言慢。 

执行 python 脚本的两种方式 
a. ./run.py shell 直接调用 python 脚本 
b. python run.py 调用 python 解释器来调用 python 脚本 

## <a name='python2python3'></a>python2 与 python3 区别
1、Python3 使用 print 必须要以小括号包裹打印内容，比如 print('hi') Python2 既可以使用带小括号的方式，也可以使用一个空格来分隔打印内容，比如 print 'hi'  
2、python2 range(1,10)返回列表，python3 中返回迭代器，节约内存； 
3、python2 中使用 ascii 编码，python 中使用 utf-8 编码； 
4、python2 中 unicode 表示字符串序列，str 表示字节序列 python3 中 str 表示字符串序列，byte 表示字节序列； 
5、python2 中为正常显示中文，引入 coding 声明，python3 中不需要； 
6、python2 中是 raw_input()函数，python3 中是 input()函数； 

## <a name='py2py3'></a>py2 项目如何迁移成 py3 
1. 先备份原文件，然后使用 python3 自带工具 2to3.py 将 py2 文件转换位 py3 文件 
2. 手动将不兼容的代码改写成兼容 py3 的代码 
 
 
 
## <a name='python-1'></a>python 解释代码原理，函数怎么解析的
python 解释器由一个编译器和一个虚拟机构成，编译器负责将源代码转换成字节码文件，而虚拟机负责执行字节码。所以，解释型语言其实也有编译过程，只不
过这个编译过程并不是直接生成目标代码，而是中间代码（字节码），然后再通过虚拟机来逐行解释执行字节码

个人理解执行过程原理：执行 python XX.py 后，将会启动 Python 的解释器，python 解释器的编译器会将.py 源文件编译（解释）成字节码生成 PyCodeObject
字节码对象存放在内存中。python 解释器的虚拟机将执行内存中的字节码对象转化为机器语言，虚拟机与操作系统交互，使机器语言在机器硬件上运行。运行结
束后 python 解释器则将 PyCodeObject 写回到 pyc 文件中。当 python 程序第二次运行时，首先程序会在硬盘中寻找 pyc 文件，如果找到，则直接载入，否则就重
复上面的过程。 所以我们应该这样来定位 PyCodeObject 和 pyc 文件，我们说 pyc文件其实是 PyCodeObject 的一种持久化保存方式。 

.pyc文件通常在以下情况下会被更新：
1、当源代码文件（.py文件）被修改：如果您对源代码进行了修改，Python解释器会检测到源代码文件的时间戳发生了变化，它会重新编译源代码并生成新的.pyc文件。这确保了最新的代码变更可以得到正确的执行。
2、当.pyc文件不存在或已损坏：如果在执行Python程序时发现缺少或损坏了对应的.pyc文件，解释器会重新编译源代码并生成新的.pyc文件。
3、当Python解释器版本发生变化：不同版本的Python解释器可能使用不同的字节码格式，因此在切换Python解释器版本时，旧的.pyc文件可能会被认为不兼容并被重新生成。

## <a name='GIL'></a>GIL 
GIL 是 python 的全局解释器锁，同一进程中假如有多个线程运行，一个线程在运行 python 程序的时候会霸占 python 解释器（加了一把锁即 GIL），使该进程
内的其他线程无法运行，等该线程运行完后其他线程才能运行。如果线程运行过程中遇到耗时操作，则解释器锁解开，使其他线程运行。
所以在多线程中，线程的运行仍是有先后顺序的，并不是同时进行。 

Python GIL 底层实现原理 
图 1 GIL 工作流程示意图 上面这张图，就是 GIL 在 Python 程序的工作示例。
其中，Thread 1、2、3 轮流执行，每一个线程在开始执行时，都会锁住 GIL，以阻止别的线程执行；同样的，每一个线程执行完一段后，会释放 GIL，以允许别的线程开始利用资源。 
既然 CPython 能控制线程伪并行，为什么还需要 GIL 呢？其实，这和 CPython 的底层内存管理有关。 
CPython 使用引用计数来管理内容，所有 Python 脚本中创建的实例，都会配备一个引用计数，来记录有多少个指针来指向它。当实例的引用计数的值为 0 时，会自动释放其所占的内存。 
举个例子，看如下代码： 
>>> import sys 
>>> a = [] 
>>> b = a 
>>> sys.getrefcount(a) 3 
可以看到，a 的引用计数值为 3，因为有 a、b 和作为参数传递的 getrefcount 都引用了一个空列表。 
假设有两个 Python 线程同时引用 a，那么双方就都会尝试操作该数据，很有可能造成引用计数的条件竞争，导致引用计数只增加 1（实际应增加 2），这造成的后果是，当第一个线程结束时，会把引用计数减少 1，此时可能已经达到释放
内存的条件（引用计数为 0），当第 2 个线程再次视图访问 a 时，就无法找到有效的内存了。所以，CPython 引进 GIL，可以最大程度上规避类似内存管理这样复杂的竞争风险问题。 

 
## <a name=''></a>装饰器
装饰器本质上是一个 callable object，它可以让其他函数在不需要做任何代码变动的前提下增加额外功能，装饰器的返回值也是一个函数对象。
装饰器的实现原理是将原函数作为参数传递给装饰器函数，然后将原函数替换为装饰器函数返回的新函数。
这意味着当我们调用原函数时，实际上是在调用装饰器函数返回的新函数。这使得我们可以在不修改原函数代码的情况下，为函数增加额外的功能。 
### <a name='-1'></a>1. 不带参数的装饰器
```python
def wrap1(func): 
    def inner_func(*args, **kwargs): 
        print('I\'m inner_func', args, kwargs) 
        func() 
    return inner_func 

# 也就是说我们在进行不带参数的装饰器的调用时，相当于把下面的函数名当做参数传给了@后面的函数 
@wrap1   # 等价于 func1 = wrap(func1) 
def func1(): 
    print('I\'m func1...') 
func1() 
func1('dong', 'xiang', name='dongxiang')
# 输出： 
# I'm inner_func () {} 
# I'm func1... 
# I'm inner_func ('dong', 'xiang') {'name': 'dongxiang'} 
# I'm func1...
```

### <a name='-1'></a>2. 带参数的装饰器 
```python
def wrap2(type): 
    def outer(func): 
        def inner(*args, **kwargs): 
            if type == 'apple': 
                print('apple phone!!!') 
                func(*args, **kwargs) 
            else: 
                print('other phone!!!') 
        return inner 
    return outer 
# 如果要返回函数的话，带参数的装饰器就要写三层内嵌函数。 
# 等价于 func2 = wrap2('apple')(func) 
@wrap2('apple') 
def func2(): 
    print('func....') 

func2() 
# 输出： 
# apple phone!!! 
# func2.... 
```
 
装饰器----一个装饰器就是一个函数，它接受一个函数作为参数，并返回一个新的函数； 
任何时候使用装饰器的时候，都应该使用 functools 模块中的@wraps 装饰器来注解底层包装函数。如果忘记使用@wraps，你会发现被装饰函数丢失了所有有用的信息；wraps 作用就是将被包装函数的元信息复制到可调用实例中。 
@wraps 有一个重要特征是它可以让你通过属性__wrapped__直接访问被包装函数； 一个装饰器已经包装在了函数上，你想撤销他，直接访问原始的未包装的函数。可以使用:被包装函数名.__wrapped__(*args)访问； 
将装饰器定义为类，让装饰器可以同时工作在类定义的内部和外部； 
给类或静态方法提供装饰器；但是要确保装饰器在@classmethod 和@staticmethod 之前； 
通过装饰器给被装饰函数增加参数 
使用装饰器扩充类的功能； 
函数装饰器有什么作用？请列举说明？ 
1，引入日志  
2，函数执行时间统计 
3，执行函数前预备处理 
4，执行函数后清理功能 
5，权限校验等场景 
6，缓存 
7，事务处理 
一句话解释什么样的语言能够用装饰器? 
函数可以作为参数传递的语言，可以使用装饰器。 
多个装饰器装饰一个函数，其执行顺序是从下往上。 

对装饰器的理解，并写出一个计时器记录方法执行性能的装饰器？ 
```python
import time 
from functools import wraps 
 
def timeit(func): 
    @wraps(func) 
    def wrapper(*args, **kwargs): 
        start = time.clock() 
        ret = func(*args, **kwargs) 
        end = time.clock() 
        print('used:',end-start) 
        return ret 
    return wrapper 
 
@timeit 
def foo(): 
    print('in foo()')
    foo()
``` 
 
 
## <a name='-1'></a>可迭代对象、生成器、迭代器的区别 
可迭代对象：包含__iter__()方法的对象； 
迭代器：包含__iter__()和__next__()方法的对象；可以使用 iter() 以从任何序列
得到迭代器（如 list，tuple，dict，set 等） 
生成器：另一种形式的迭代器。要获取下一个元素，则使用成员函数 next()（Python 2）或函数 next() function（Python3）。
当没有元素时，则引发 StopIteration 异常。只是在需要返回数据的时候使用 yield 语句。每次 next()被调用时，生成器 会返回它脱离的位置（它记忆语句最后一次执行的位置和所有的数据值） 
区别：生成器能做到迭代器能做的所有事，而且因为自动创建 iter()和 next()方法，生成器显得特别简洁，而且生成器也是高效的，使用生成器表达式取代列表解析
可以同时节省内存。除了创建和保存程序状态的自动方法，当生成器终结时，还会自动抛出 StopIteration 异常。 
 
## <a name='copydeepcopy'></a>copy 和 deepcopy 区别
1、复制不可变数据类型，不管 copy 还是 deepcopy，都是同一个地址。当浅复制的值是不可变对象（数值，字符串，元组）时和=“赋值”的情况一样，对象的 id 值与浅复制原来的值相同。 
2、复制的值是可变对象（列表和字典）
浅拷贝 copy 有两种情况： 
第一种情况：复制的对象中无复杂子对象，原来值的改变并不会影响浅复制的值，同时浅复制的值改变也并不会影响原来的值。原来值的 id 值与浅复制原来的值不同。 
第二种情况：复制的对象中有复杂子对象（例如列表中的一个子元素是一个列表），改变原来的值中的复杂子对象的值，会影响浅复制的值。 
深拷贝 deepcopy：完全复制独立，包括内层列表和字典

浅拷贝：浅拷贝意味着构造一个新的集合对象，然后用原始对象中找到的子对象的引用来填充它。从本质上讲，浅层的复制只有一层的深度。复制过程不会递归，因此不会创建子对象本身的副本。a 和 b 是一个独立的对象，但他们的子对象还是指向统一对象 （是引用）。 
深拷贝：深拷贝使复制过程递归。这意味着首先构造一个新的集合对象，然后递归地用在原始对象中找到的子对象的副本填充它。以这种方式复制一个对象，遍历整个对象树，以创建原始对象及其所有子对象的完全独立的克隆。 

 
## <a name='-1'></a>进程、线程、协程 
1. 进程（Process），一个程序运行起来后，代码+用到的资源称之为进程，是系统进行资源分配和调度的基本单位； 
线程（Thread）是进程中的一个实体，是 CPU 调度和分派的基本单位； 
协程是一种用户态的轻量级线程，协程的调度完全由用户控制。在Python中使用asyncio库来实现。
2. 线程依赖于进程而存在，一个进程至少有一个线程；
进程有自己的独立地址空间，线程共享所属进程的地址空间； 
进程是拥有系统资源的一个独立单位，而线程自己基本上不拥有系统资源，只拥有一点在运行中必不可少的资源(如程序计数器,一组寄存器和栈)，和其他线程共享本进程的相关资源如内存、I/O、cpu 等； 
在进程切换时，涉及到整个当前进程 CPU 环境的保存环境的设置以及新被调度 运行的 CPU 环境的设置，而线程切换只需保存和设置少量的寄存器的内容，并不涉及存储器管理方面的操作，可见，进程切换的开销远大于线程切换的开销；
协程拥有自己的寄存器上下文和栈。协程调度切换时，将寄存器上下文和栈保存到其他地方，在切回来的时候，恢复先前保存的寄存器上下文和栈，直接操作栈则基本没有内核切换的开销，可以不加锁的访问全局变量，所以上下文的切换非常快。 
3. 线程之间的通信更方便，同一进程下的线程共享全局变量等数据，而进程之间的通信需要以进程间通信(IPC)的方式进行； 
多线程程序只要有一个线程崩溃，整个程序就崩溃了，但多进程程序中一个进程崩溃并不会对其它进程造成影响，因为进程有自己的独立地址空间，因此多进程更加健壮； 
当一个协程发生异常并崩溃时，通常只会影响当前的协程，其他协程可以继续执行。这是因为协程之间的切换是由事件循环控制的，事件循环会捕获协程的异常并处理，以确保其他协程的正常执行。

在 Python 中，协程不需要多线程的锁机制，因为只有一个线程，也不存在变量冲突。
Python2.x 对协程的支持比较有限，生成器 yield 实现了一部分但不完全，gevent 模块倒是有比较好的实现；
Python3.4 加入了 asyncio 模块，在 Python3.5 中又提供了 async/await 语法层面的支持，Python3.6 中 asyncio 模块更加完善和稳定。 

1. 线程和协程推荐在 IO 密集型的任务(比如网络调用)中使用，而在 CPU 密集型的任务中，表现较差。 
2. 对于 CPU 密集型的任务，则需要多个进程，绕开 GIL 的限制，利用所有可用的 CPU 核心，提高效率。 
3. 在高并发下的最佳实践就是多进程+协程，既充分利用多核，又充分发挥协程的高效率，可获得极高的性能。 
 
 
## <a name='pythonIO'></a>python 中 IO 密集型为什么用多线程
对于 IO 密集型任务，由于 CPU 经常需要等待 IO 操作完成，因此，在等待过程中，CPU 可以去做其他事情，单个 CPU 利用率很低，可能只有 10%，所以多线程可以提升 cpu 利用率，因此采用多线程可以提高程序执行效率，这实质是并发处理。
当然，由于多进程可以利用多核 CPU 真正实现并行处理，所以，采用多进程也可以提高 IO 密集型任务的执行效率。
但是考虑到多线程是共享资源的，因此通信更方便，而多进程之间的资源相互独立，通信不是很方便，因此，综合来看，IO 密集型任务，采用 python 多线程更合适。
对于计算密集型任务，由于 python 多线程为了实现同步锁安全机制，采用了全局锁机制，导致即使是多核 CPU 或多个 CPU，同一时间，也只有一个线程在执行，因为全局锁只有一个，即在多核 CPU 或多个 CPU 情况下，python 多线程仍然只能并发处理，而不能并行处理。
而因为计算密集型任务，CPU 几乎不存在空闲时间，使用 python 多线程，CPU 频繁在各个线程之间切换所需时间反而会大大降低程序执行效率。
而采用 python 多进程，可以实现真正意义上的并行处理，因此，计算密集型任务，采用 python 多进程更合适。 


## <a name='__new____init__'></a>简述__new__和__init__区别
1. __new__至少要有一个参数 cls，代表当前类，此参数在实例化时由 Python 解释器自动识别；__init__是初始化方法，创建对象后，就立刻被默认调用了，可接收参数； 
2. __new__必须要有返回值，返回实例化出来的实例，可以 return 父类（通过super(当前类名, cls)）__new__出来的实例，或者直接是 object 的__new__出来的实例；
__init__有一个参数 self，就是这个__new__返回的实例，__init__在__new__的基础上可以完成一些其它初始化的动作，__init__不需要返回值； 
3. 如果__new__创建的是当前类的实例，会自动调用_init__函数，通过 return 语句里面调用的__new__函数的第一个参数是 cls 来保证是当前类实例，如果是其他类的类名，那么实际创建返回的就是其他类的实例，其实就不会调用当前类的;
_init__函数，也不会调用其他类的__init__函数。 
4. __new__在创建一个实例的过程中必定会被调用，但__init__就不一定，比如通过 pickle.load 的方式反序列化一个实例时就不会调用__init__。 
5. 创建对象的动作有两步：1. 在内存中为对象分配空间，调用__new__()方法完成；2. 调用初始化方法__init__为对象初始化； 
 
 
## <a name='python-1'></a>python 内存管理机制 
### <a name='python-1'></a>python 内存池
当创建大量消耗小内存的对象时，频繁调用 new/malloc 会导致大量的内存碎片， 致使效率降低。 内存池的作用就是预先在内存中申请一定数量的，大小相等的内存块留作备用，当有新的内存需求时，就先从内存池中分配内存给这个需求，不够之后再申请新的内存。
这样做最显著的优势就是能够减少内存碎片，提升效率。 python 中的内存管理机制为 Pymalloc。
内存池是如果工作的（how）
首先，我们看一张 CPython(python 解释器)的内存架构图： 

python 的对象管理主要位于 Level+1~Level+3 层 
Level+3 层：对于 python 内置的对象（比如 int,dict 等）都有独立的私有内存池，对象之间的内存池不共享，即 int 释放的内存，不会被分配给 float 使用 
Level+2 层：当申请的内存大小小于 256KB 时，内存分配主要由 Python 对象分配器（Python’s object allocator）实施 
Level+1 层：当申请的内存大小大于 256KB 时，由 Python 原生的内存分配器进行分配，本质上是调用 C 标准库中的 malloc/realloc 等函数 
关于释放内存方面，当一个对象的引用计数变为 0 时，Python 就会调用它的析构函数。调用析构函数并不意味着最终一定会调用 free 来释放内存空间，如果真是这样的话，那频繁地申请、释放内存空间会使 Python 的执行效率大打折扣。
因此在析构时也采用了内存池机制，从内存池申请到的内存会被归还到内存池中， 以避免频繁地申请和释放动作。 

### <a name='-1'></a>垃圾回收 
垃圾回收机制：主要是以对象引用计数为主标记清除和分代技术为辅的那么一种方式； 

#### <a name='PyObject'></a>引用计数 PyObject 
python 里每一个东西都是对象，它们的核心就是一个结构体：PyObject。 PyObject 是每个对象必有的内容，其中 ob_refcnt 就是做为引用计数。
当一个对象有新的引用时，它的 ob_refcnt 就会增加，当引用它的对象被删除，它的 ob_refcnt就会减少，当引用计数降为 0 时，说明没有任何引用指向该对象，该对象就成为 要被回收的垃圾了。
不过如果出现循环引用（当对象 1 中的某个属性指向对象 2， 对象 2 中的某个属性指向对象 1 就会出现循环引用）的话，引用计数机制就不再起有效的作用了，del 语句可以减少引用次数，但是引用计数不会归 0，对象也就不会被销毁，从而造成了内存泄漏问题。 

#### <a name='-1'></a>标记清除
标记-清除机制，顾名思义，首先标记对象（垃圾检测），然后清除垃圾（垃圾回收）。首先初始所有对象标记为白色，并确定根节点对象（这些对象是不会被删除），标记它们为黑色（表示对象有效）。将有效对象引用的对象标记为灰色（表示对象可达，但它们所引用的对象还没检查），检查完灰色对象引用的对象后，将灰色标记为黑色。重复直到不存在灰色节点为止。最后白色结点都是需要清除的对象。 
解决循环引用： 
>>> a=[1,2] 
>>> b=[3,4] 
>>> sys.getrefcount(a) 
2 
>>> sys.getrefcount(b) 
2 
>>> a.append(b) 
>>> sys.getrefcount(b) 
3 
>>> b.append(a) 
>>> sys.getrefcount(a) 
3 
>>> del a 
>>> del b 
a 引用 b,b 引用 a,此时两个对象各自被引用了 2 次（去除 getrefcout()的临时引用） 
 
执行 del 之后，对象 a,b 的引用次数都-1，此时各自的引用计数器都为 1，陷入循环引用
标记：找到其中的一端 a,因为它有一个对 b 的引用，则将 b 的引用计数-1
标记：再沿着引用到 b,b 有一个 a 的引用,将 a 的引用计数-1，此时对象 a 和 b 的引用次数全部为 0，被标记为不可达（Unreachable）
清除: 被标记为不可达的对象就是真正需要被释放的对象 
 
### <a name='-1'></a>分代回收 
分代回收是基于这样的一个统计事实，对于程序，存在一定比例的内存块的生存周期比较短；而剩下的内存块，生存周期会比较长，甚至会从程序开始一直持续到程序结束。生存期较短对象的比例通常在 80%～90%之间。 
因此，简单地认为：对象存在时间越长，越可能不是垃圾，应该越少去收集。这样在执行标记-清除算法时可以有效减小遍历的对象数，从而提高垃圾回收的速度，是一种以空间换时间的方法策略。 
Python 将所有的对象分为年轻代（第 0 代）、中年代（第 1 代）、老年代（第 2 代）三代。所有的新建对象默认是 第 0 代对象。当在第 0 代的 gc 扫描中存活下来的对象将被移至第 1 代，在第 1 代的 gc 扫描中存活下来的对象将被移至第 2 代。 
gc 扫描次数（第 0 代>第 1 代>第 2 代） 
当某一代中被分配的对象与被释放的对象之差达到某一阈值时，就会触发当前一代的 gc 扫描。当某一代被扫描时，比它年轻的一代也会被扫描，因此，第 2 代的 gc 扫描发生时，第 0，1 代的 gc 扫描也会发生，即为全代扫描。 
>>> import gc  
>>> gc.get_threshold() ## 分代回收机制的参数阈值设置 
(700, 10, 10) 
700=新分配的对象数量-释放的对象数量，第 0 代 gc 扫描被触发 
第一个 10：第 0 代 gc 扫描发生 10 次，则第 1 代的 gc 扫描被触发 
第二个 10：第 1 代的 gc 扫描发生 10 次，则第 2 代的 gc 扫描被触发 


 
### <a name='Python'></a>哪些操作会导致 Python 内存泄露，怎么处理？
内存泄漏指由于疏忽或错误造成程序未能释放已经不再使用的内存。内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费。 有 
__del__() 函数的对象间的循环引用是导致内存泄露的主凶。不使用一个对象时使用: del object 来删除一个对象的引用计数就可以有效防止内存泄露问题。 
通过 Python 扩展模块 gc 来查看不能回收的对象的详细信息。 
可以通过sys.getrefcount(obj) 来获取对象的引用计数，并根据返回值是否为 0 来判断是否内存泄露 
 python 的程序会内存泄漏吗？ 
o会发生内存泄漏，在 Python 程序里，内存泄漏是由于一个长期持有的对象不断的往一个 dict 或者 list 对象里添加新的对象, 而又没有即时释放，就会导致这些对象占用的内存越来越多，从而造成内存泄漏。另外，对象的交叉引用也会造成内存无法释放的问题。 
 说说有没有什么方面阻止或者检测内存泄漏？ 
1. 程序员管理好每个 python 对象的引用，尽量在不需要使用对象的时候，断开所有引用 
2. 尽量少通过循环引用组织数据，可以改用 weakref 做弱引用或者用 id 之类的句柄访问对象 
3. 通过 gc 模块的接口可以检查出每次垃圾回收有哪些对象不能自动处理，再逐个逐个处理 

关于内存溢出和内存泄漏的区别 
内存溢出：（Out Of Memory---OOM） 
系统已经不能再分配出你所需要的空间，比如你需要 100M 的空间，系统只剩90M 了，这就叫内存溢出 
内存泄漏： (Memory Leak) 
强引用所指向的对象不会被回收，可能导致内存泄漏，虚拟机宁愿抛出 OOM 也不会去回收他指向的对象. 

Python 的内存管理机制及调优手段？ 
gc 模块 
gc.disable()  # 暂停自动垃圾回收. 
gc.collect()  # 执行一次完整的垃圾回收, 返回垃圾回收所找到无法到达的对象的数量. 
gc.set_threshold()  # 设置 Python 垃圾回收的阈值. 
gc.set_debug()  # 设置垃圾回收的调试标记. 调试信息会被写入 std.err. 
Python 有两种共存的内存管理机制: 引用计数和垃圾回收 
垃圾回收机制：主要是以对象引用计数为主标记清除和分代技术为辅的那么一种方式 

三种情况触发垃圾回收 1、调用 gc.collect() 2、GC 达到阀值时 3、程序退出时 
调优手段: 
1.手动垃圾回收 
2.调高垃圾回收阈值 
3.避免循环引用 

在 Python 中是如何管理内存的？ 
Python 有一个私有堆空间来保存所有的对象和数据结构。作为开发者，我们无法访问它，是解释器在管理它。但是有了核心 API 后，我们可以访问一些工具。 Python 内存管理器控制内存分配。 
另外，内置垃圾回收器会回收使用所有的未使用内存，所以使其适用于堆空间。 Python 引用了一个内存池(memory pool)机制，即 Pymalloc 机制(malloc:n.分配内存)，用于管理对小块内存的申请和释放。 
一、垃圾回收：python 不像 C++，Java 等语言一样，他们可以不用事先声明变量类型而直接对变量进行赋值。对 Python 语言来讲，对象的类型和内存都是在运行时确定的。
这也是为什么我们称 Python 语言为动态类型的原因（这里我们把动态类型可以简单的归结为对变量内存地址的分配是在运行时自动判断变量类型并对变量进行赋值）。 
二、引用计数：Python 采用了类似 Windows 内核对象一样的方式来对内存进行管理。每一个对象，都维护这一个对指向该对对象的引用的计数。当变量被绑定在一个对象上的时候，该变量的引用计数就是 1，(还有另外一些情况也会导致变量引用计数的增加),系统会自动维护这些标签，并定时扫描，当某标签的引用计数变为 0 的时候，该对就会被回收。 
三、内存池机制 Python 的内存机制以金字塔行，-1，-2 层主要有操作系统进行操作， 
第 0 层是 C 中的 malloc，free 等内存分配和释放函数进行操作； 
第 1 层和第 2 层是内存池，有 Python 的接口函数 PyMem_Malloc 函数实现，当对象小于 256K 时有该层直接分配内存； 
第 3 层是最上层，也就是我们对 Python 对象的直接操作； 
在 C 中如果频繁的调用 malloc 与 free 时,是会产生性能问题的.再加上频繁的分配与释放小块的内存会产生内存碎片. Python 在这里主要干的工作有: 
如果请求分配的内存在 1~256 字节之间就使用自己的内存管理系统,否则直接使用 malloc. 
这里还是会调用 malloc 分配内存,但每次会分配一块大小为 256k 的大块内存. 
经由内存池登记的内存到最后还是会回收到内存池,并不会调用 C 的 free 释放掉.以便下次使用.对于简单的 Python 对象，例如数值、字符串，元组（tuple 不允许被更改)采用的是复制的方式(深拷贝?)，也就是说当将另一个变量 B 赋值给变量 A 时，虽然 A 和 B 的内存空间仍然相同，但当 A 的值发生变化时，会重新给A 分配空间，A 和 B 的地址变得不再相同 



## <a name='python-1'></a>python 中单例模式实现方式 
第一种方法：使用装饰器 
```python
def singleton(cls): 
    instances = {} 
    def wrapper(*args, **kwargs): 
        if cls not in instances: 
            instances[cls] = cls(*args, **kwargs) 
        return instances[cls] 
    return wrapper 
@singleton 
class Foo(object): 
    pass 
foo1 = Foo() 
foo2 = Foo() 
print(foo1 is foo2) #True
```
第二种方法：使用基类__new__是真正创建实例对象的方法，所以重写基类的__new__ 方法，以此保证创建对象的时候只生成一个实例 
```python
class Singleton(object): 
    def __new__(cls,*args,**kwargs): 
        if not hasattr(cls,'_instance'): 
            cls._instance = super(Singleton,cls).__new__(cls,*args,**kwargs) 
        return cls._instance
``` 
第三种方法：元类，元类是用于创建类对象的类，类对象创建实例对象时一定要调用__call__方法，因此在调用__call__时候保证始终只创建一个实例即可，type 是 python 的元类 
```python
class Singleton(type): 
    def __call__(cls, *args, **kwargs): 
        if not hasattr(cls, '_instance'): 
            cls._instance = super(Singleton, cls).__call__(*args, **kwargs) 
        return cls._instance 

# Python2 
class Foo(object): 
    __metaclass__ = Singleton 
 
# Python3 
class Foo(metaclass=Singleton): 
    pass 
 
foo1 = Foo() 
foo2 = Foo() 
print(foo1 is foo2)  # True
```

第四种方法：import 方法 
作为 python 的模块是天然的单例模式 
```python
# mysingleton.py 
class My_Singleton(object): 
    def foo(self): 
        pass 
my_singleton = My_Singleton() 
# to use 
from mysingleton import my_singleton 
my_singleton.foo()
```
 
用double check（双重检查）实现单例模式
```python
import threading

class Singleton:
    _instance = None
    _lock = threading.Lock()

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            with cls._lock:
                if not cls._instance:
                    cls._instance = super().__new__(cls)
        return cls._instance
```



## <a name='-1'></a>函数 
函数名其实就是指向一段内存空间的地址，既然是地址，那么我们可以利用这种特性来： 
a. 函数名可以作为一个值; 
b. 函数名可以作为返回值; 
c. 函数名可以作为一个参数；
 
### <a name='python-1'></a>python 函数参数类型
python 函数传递参数类型比较多，按照是否确定参数数目可分为定长参数变长参数，按照是否引入关键字分为普通参数和关键字参，可以从以下五个方面进行介绍： 
 定长普通参数 
 定长关键字参数 
 变长普通参数 
 变长关键字参数 
 复合参数 
定长普通参数为最常见的参数传递方式，参数传递时要求实参和虚参的个数相同，顺序也必须相同，否则会出现错误。如果要设置默认参数，默认参数应该放在参数链表的尾部, 否则在函数调用时会给默认参数指定实参，从而使默认参数的设置失去意义。定长普通参数对参数位置要求严格，为了摆脱位置对参数的束缚， python 引入了关键字参数。 
定长关键字参数进行参数传递时，必须设定指定参数的名字，函数调用时将以键值对的方式进行传递，若要给某个参数设定缺省值，在函数定义时直接指定缺省键值对即可，且不受位置的束缚，在函数调用时则不需要设定指定参数的键值对。 
```python
def fun2(Name='Tom', Age=20, Sex="Male"): 
    print('Name:', Name) 
    print('Age:', Age)
    print('Sex:', Sex)


if __name__ == "__main__": 
    fun2(Name="Alice", Sex="Female") 
    fun2(Name="Bob", Age=22)
```
有些情况我们在定义一个函数时，并不能能够确定接受的参数的个数，只是知道需要对所接受的参数依次进行处理，这种情况下需要使用变长参数进行参数传递，
python 在定义变长参数时引入了*，定义变长普通参数时在参数名前面放在 ，如 
`args，定义变长关键字参数时在参数前面放两个*号，如**kargs`。 
变长普通参数只需要在参数前面放一个*号即可，在函数体内部把形参当作一个元组来处理。在函数调用时可以把一个元组当作参数，不过需要对元组进行unpack，即在相应参数前面加 * 即可。 
```python
def foo(*args): 
    for arg in args: 
        print(arg) 
     
if __name__ == "__main__": 
    foo(1, 2, 3, "good") 
    arglist = 1, 2, 3, "good" 
    foo(*arglist)
```
注意在函数定义时变长参数（包括后面介绍的变长关键字参数）必须设置在定长参数后面。 
变长关键字长参数类似于变长普通参数，只是变长关键字参数在函数定义时在参数前面加 ** 号，在函数体内部把参数当作一个参数字典来处理。在函数调用时可以把一个字典当作参数，不过需要对字典进行 unpack，即在字典参数前面加 **。 
```python
def foo(**kwargs): 
    for k, v in kwargs.items(): 
        print(k, v) 
if __name__ == "__main__": 
    foo(Name="Jim", Age=22, Sex="Male") 
    argDict = {"Name": "Jim", "Age": 22, "Sex": "Male"} 
    foo(**argDict)
```

对于变长关键字参数，我们可以任意指定关键字的名字，函数调用的内部会去获取关键字的名字和值。 
在实际编程的过程当中，我们可能面临这样一种情况，函数定义时不能够确定会接受到什么样参数，包括参数的数目，参数的类型等，这个时候，我们就会用到
这里介绍的复合参数，复合参数及前面介绍的四种参数的混搭形式。 
```python
def foo(firstname, lastname="John", *args, **kwargs): 
    print(firstname) 
    print("LastName: ", lastname) 
    for arg in args: 
        print(arg) 
    for k, v in kwargs.items(): 
        print(k, v) 
 
if __name__ == "__main__": 
    arglist = 1, 2, 3, "good" 
    argDict = {"Name": "Jim", "Sex": "Male"} 
    foo("Jim", "Jack", *arglist, **argDict)
```

只是在函数定义时对参数的顺序特定的要求： 
 关键字参数必须在普通参数后面 
 变长普通参数必须放在定长参数的后面 
 变长关键字参数必须放在变长普通参数的后面

函数调用时赋值的过程为： 
1. 依次为普通参数赋值 
2. 为关键字参数赋值 
3. 将多余出的零散实参打包成一个元组传递给赋给变长普通参数 *args 
4. 将多余的键值对形式的实参打包成一个字典传递给变长关键字参数 **kargs 
此外，我们还可以从一个函数传递变长普通参数和变长关键字参数到另一个函数中去，如下： 
def f(x, *args, **kwargs): 
    ... 
    kwargs['width'] = '14.3c' 
    ... 
    g(x, *args, **kwargs) 
fun(*args，**kwargs)中的*args，**kwargs 什么意思？ 
*args，**kwargs 主要用于函数定义，可以将不定数量的参数传递给函数，这里的
不定的意思是，预先并不知道函数使用者会传递多少个参数给你，所以在这种场
景下使用这两个关键字，*args 是用来发送一个非键值对的可变数量的参数列表
给一个函数，**kwargs 允许你将不定长度的键值对，作为参数传递给函数。 
 
默认参数值 
定义一个有可选参数的函数是非常简单的，直接在函数定义中给参数指定一个默认值，并放到参数列表最后就行了。但是要注意：默认参数的值仅仅在函数定义的时候赋值一次。看下面的例子： 
def f(a, L=[]): 
    L.append(a) 
    return L 
print f(1)  # [1] 
print f(2)  # [1, 2] 
print f(3)  # [1, 2, 3] 
关于这点，文档上着重给出警告，如下： 
Important warning: The default value is evaluated only once. This makes a difference 
when the default is a mutable object such as a list, dictionary, or instances of most 
classes. 
所以，如果默认参数是一个可修改的容器比如一个列表、集合或者字典，最好使
用 None 作为默认值。 
# Using a list as a default value 
def spam(a, b=None):  
    if b is None:  
        b = []  
    ... 
如果不想提供一个默认值，而是想仅仅测试下某个默认参数是不是有传递进来，
可以像下面这样写: 
_no_value = object() 
def spam(a, b=_no_value):  
    if b is _no_value:  
        print('No b value supplied') 
    ...  

### <a name='lambda'></a>lambda 匿名函数
Python 使用 lambda 关键字创造匿名函数。所谓匿名，意即不再使用 def 语句这样标准的形式定义一个函数。这种语句在调用时绕过函数的栈分配，可以提高效率。
其语法是： 
lambda [arg1[, arg2, ... argN]]: expression 
其中，参数是可选的，如果使用参数的话，参数通常也会在表达式之中出现。 
lambda 匿名函数捕获值 
lambda 表达式允许你定义简单函数,但是它的使用是有限制的。你只能指定单个
表达式,它的值就是最后的返回值。也就是说不能包含其他的语言特性了，包括多
个语句、条件表达式、迭代以及异常处理等等。 
>>> x = 10 
>>> a = lambda y: x + y 
>>> x = 20 
>>> b = lambda y: x + y  
那么 a(10) 和 b(10) 返回的结果是什么? 答案是 30，30。 
这其中的奥妙在于 lambda 表达式中的 x 是一个自由变量，在运行时绑定值，
而不是定义时就绑定，这跟函数的默认值参数定义是不同的。因此在调用这个 
lambda 表达式的时候，x 的值是执行时的值。 
如果想让某个匿名函数在定义时就捕获到值，可以将那个参数值定义成默认参数
即可，就像下面这样: 
>>> x = 10 
>>> a = lambda y, x=x: x + y 
>>> x = 20 
>>> b = lambda y, x=x: x + y  
这样结果就是 20，30 了。下面再看两个例子，加深对 lambda 变量捕获机制的了
解： 
>>> funcs = [lambda x: x+n for n in range(5)]  
>>> for f in funcs: 
... print(f(0))         # 4 4 4 4 4  这意味着内部函数被调用时，参数的值在闭包内进行查找。因此，当任何由 funcs()返回的函数被调用时，n 的值将在附近的范围进行查找。那时，不管返回的函数是否被调用，for 循环已经完成，n 被赋予了最终的值 4。 

>>> funcs = [lambda x, n=n: x+n for n in range(5)] 
>>> for f in funcs: 
... print(f(0))        # 0 1 2 3 4 

 
### <a name='-1'></a>内建函数 
#### <a name='range'></a>range 
range(1,10)---左开右闭， 
 

#### <a name='sum'></a>sum 
sum 可以接收一个容器，求其和 
sum([23, 23, 34,])       # 80 
 
sum(range(1,10))----1-9 之间的和 

 
#### <a name='sorted'></a>sorted 
sorted(iterable[, cmp[, key[, reverse]]]) 
Return a new sorted list from the items in iterable. 
它会从一个可迭代对象构建一个新的排序列表。 
 iterable：可迭代对象； 
 cmp：比较两个对象 x 和 y，如果 x > y 返回正数，x < y 返回负数；x == y，返回 0；比较什么由 key 决定。只适用于 python 2.x; 
 key：用来决定在排序算法中 cmp 比较的内容，key 可以是任何可被比较的内容，比如元组（python 中元组是可被比较的）; 
 reverse：排序规则, reverse = True(降序) 或者 reverse = False(升序，默认) 
例如有一个字典，根据字典中的属性值来排序，返回一个排好序的元组： 
>>> d = {"a":1, "c":3, "d":4, "b":2, "e": 5} 
>>> sorted_d = sorted(d.items(), key=lambda i: i[1]) 
>>> sorted_d 
[('a', 1), ('b', 2), ('c', 3), ('d', 4), ('e', 5)] 
或者有一个数组，它的每一个成员是一个字典，然后根据字典中的属性值来排序，
用 cmp 参数如下： 
>>> persons=[{'name':'zhang3','age':15},{'name':'li4','age':12}] 
>>> sorted_d = sorted(persons, lambda a,b: a['age']-b['age']) 
>>> sorted_d 
[{'age': 12, 'name': 'li4'}, {'age': 15, 'name': 'zhang3'}]  
再来看一个稍微复杂的例子。给定一个只包含大小写字母，数字的字符串，对其
进行排序，保证： 
 所有的小写字母在大写字母前面， 
 所有的字母在数字前面 
 所有的奇数在偶数前面 
像下面这样用 sorted 函数即可。 
>>> s = "Sorting1234" 
>>> "".join(sorted(s, key=lambda x: (x.isdigit(), x.isdigit() and int(x) % 2 == 0, x.isupper(), 
x))) 
'ginortS1324' 
这里，lambda 函数将输入的字符转换为一个元组，然后 sorted 函数将根据元组
（而不是字符）来进行比较，进而判断每个字符的前后顺序。这里可以理解为，
根据字符生成的元组重新定义了排序的依据。 
sorted(iterable, key=None, reverse=False)    # 返回重新排序的列表,新列表 
根据字典 value 进行排序 x[0]表示用键排序,x[1]表示用值进行排序 
 
d = {'354': 34, 'dong': 54, 'fh': 10} 
print(sorted(d.items(), key=lambda x:x[1]))          # [('fh', 10), ('354', 34), ('dong', 54)] 


#### <a name='reversed'></a>reversed
reversed(seq) 函数返回一个反转的迭代器。 
seq--要转换的序列，可以是 tuple, string, list 或 range。 


#### <a name='localsglobals'></a>locals 与 globals 
local()函数会以字典类型返回当前位置的全部局部变量。对于函数, 方法, lambda 
函式, 类, 以及实现了 call 方法的类实例, 它都返回 True。 
locals 函数可以得到一个局部变量字典，这样就可以从局部变量字典中取得修改
过后的变量值； 每次它被调用的时候，locals()会获取局部变量值中的值并覆盖
字典中相应的变量。 
globals  
函数会以字典类型返回当前位置的全部全局变量。 


#### <a name='ordchr'></a>ord 与 chr 
a. ord 与 chr 用法 
def test_ord_chr(): 
    print(ord('9')) 
    print(chr(69)) 
     
test_ord_chr()        # 57   E 
 
ord() 函数是 chr() 函数（对于 8 位的 ASCII 字符串）或 unichr() 函数（对于 Unicode
对象）的配对函数，它以一个字符（长度为 1 的字符串）作为参数，返回对应的 ASCII 
数值，或者 Unicode 数值 
返回值是对应的十进制整数。 
ord 是将一个字符转换成 ascii 值 
 
chr()函数是输入一个整数【0，255】返回其对应的 ascii 符号  
获得所有的小写字母列表，获得大写的字母列表 chr(i) + 65 
lower = [] 
for i in range(26): 
    lower.append(chr(i + 97))          # lower = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j', 'k', 'l', 'm', 'n', 'o', 'p', 'q', 'r', 's', 't', 'u', 'v', 'w', 'x', 'y', 'z'] 
 

#### <a name='all'></a>all 
all(iterable) -> bool 
all() 函数用于判断给定的可迭代参数 iterable 中的所有元素是否都为 TRUE，
如果是返回 True，否则返回 False。 
元素除了是 0、空、None、False 外都算 True。iterable -- 元组或列表。空元组、
空列表返回值为 True 
函数等价于： 
def all(iterable): 
    for element in iterable: 
        if not element: 
            return False 
    return True 
all 用法，接收一个可迭代对象，所有都为 True，结果才为 True 
>>> all([1, 2, 3]) 
True 
>>> all([0, 2, 3]) 
False 
 
def test_all(): 
    numbers = [23, 34, 12, 45] 
    print(all(x > 20 for x in numbers)) 
    print(all(x < 100 for x in numbers)) 
     
test_all()   # False  True 


#### <a name='any'></a>any 
Return True if any element of the iterable is true. If the iterable is empty, return False.  
any() 函数用于判断给定的可迭代参数 iterable 是否全部为 False，则返回 False，
如果有一个为 True，则返回 True。元素除了是 0、空、FALSE 外都算 TRUE。
iterable -- 元组或列表。如果都为空、0、false，则返回 false，如果不都为空、0、
false，则返回 true。 
函数等价于 Equivalent to: 
def any(iterable): 
    for element in iterable: 
        if element: 
            return True 
    return False 

st = 'A8238i823acdeOUEI' 
print(any(x.islower() for x in st))       # True 
st = 'DGRFRHR354DGHRYJH' 
print(any(x.islower() for x in st))       # False  
 

简述 any()和 all()方法 
any():只要迭代器中有一个元素为真就为真 
all():迭代器中所有的判断项返回都是真，结果才为真 
 
 
 
#### <a name='map'></a>map 
map(function, sequence[, sequence, ...]) -> list 
map() 会根据提供的函数对指定序列做映射。Python 2.x 返回列表。Python 3.x 
返回迭代器。 
第一个参数 function 以参数序列中的每一个元素调用 function 函数，返回包含
每次 function 函数返回值的新列表。 
具体看简单的例子： 
>>> Celsius = [39.2, 36.5, 37.3, 37.8] 
>>> Fahrenheit = map(lambda x: (float(9)/5)*x + 32, Celsius) 
>>> print Fahrenheit 
[102.56, 97.700000000000003, 99.140000000000001, 100.03999999999999] 
>>> a, b, c = [1,2,3,4], [17,12,11,10], [-1,-4,5,9] 
>>> map(lambda x,y,z:x+y+z, a,b,c) 
[17, 10, 19, 23] 
 
 
一行输入两个数字 
```python
def test_input_two_integers_in_single_line(): 
    print('input the value of x & y') 
    x, y = map(int, input().split()) 
    print('the value of x & y are---', x, y) 

test_input_two_integers_in_single_line()
# 输出
# input the value of x & y 
# 12  23 
# the value of x & y are--- 12 23 

``` 
 
map()函数第一个参数是 fun，第二个参数是一般是 list，第三个参数可以写 list，也可以
不写，根据需求 
>>> l = [1,2,3,4,5] 
>>> def fun(x): 
...     return x**2 
... 
>>> res = map(fun, l) 
>>> res 
<map object at 0x0000025B21B79790> 
>>> res = [x for x in res if x>10] 
>>> res 
[16, 25] 
 
 
 
 
#### <a name='reduce'></a>reduce 
reduce(function, sequence[, initial]) -> value 
reduce() 函数会对参数序列中元素进行累积。 
函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 
中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到
的结果再与第三个数据用 function 函数运算，最后得到一个结果。 
具体看简单的例子： 
>>> f = lambda a,b: a if (a > b) else b 
>>> reduce(f, [47,11,42,102,13]) 
102 
>>> reduce(lambda x, y: x+y, range(1,101)) 
5050 


#### <a name='zip'></a>zip 
zip(seq1 [, seq2 [...]]) -> [(seq1[0], seq2[0] ...), (...)] 
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元
组，然后返回由这些元组组成的列表。如果各个迭代器的元素个数不一致，则返
回列表长度与最短的对象相同，利用 * 号操作符，可以将元组解压为列表。解
压只支持元组列表，不支持 dict 直接解压 
具体看简单的例子： 
>>> z1=[1,2,3] 
>>> z2=[4,5,6] 
>>> result = zip(z1,z2) 
>>> a = [[1, 2, 3], [4, 5, 6], [7, 8, 9]] 
>>> zip(*a) 
[(1, 4, 7), (2, 5, 8), (3, 6, 9)] 
 
 
 
#### <a name='filter'></a>filter 
filter(function or None, sequence) -> list, tuple, or string 
Return those items of sequence for which function(item) is true. If function is None, 
return the items that are true. If sequence is a tuple or string, return the same type, else 
return a list. 
filter()函数用于过滤序列，过滤掉不符合条件的元素，返回由符合条件元素组成
的新列表。该接收两个参数，第一个为函数，第二个为序列，序列的每个元素作
为参数传递给函数进行判断，然后返回 True 或 False，最后将返回 True 的元素
放到新列表。 
具体看简单的例子： 
>>> def f(x): return x % 2 != 0 and x % 3 != 0  
>>> filter(f, range(2, 25))  
[5, 7, 11, 13, 17, 19, 23] 
>>> filter(lambda x: x!='a', "abcdef")  
'bcdef' 

>>> a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] 
>>> def fuc(l): 
...     return l%2 == 1 
... 
>>> newlist = filter(fuc, a) 
>>> newlist = [i for i in newlist] 
>>> newlist 
[1, 3, 5, 7, 9] 
 
 
 
#### <a name='slice'></a>slice 
内置的 slice()函数创建了一个切片对象，可以被用在任何切片允许使用的地方。 
>>> a = slice(1,10,2) 
>>> a 
>>> a.start 
1 
>>> a.stop 
10 
>>> a.step 
2 
>>> items = [0, 1, 2, 3, 4, 5, 6] 
>>> a = slice(2,5) 
>>> items[2:5] 
[2, 3, 4] 
>>> items[a] 
[2, 3, 4] 
>>> items[a] = [5,6,7] 
>>> items 
[0, 1, 5, 6, 7, 5, 6] 
>>> del items[a] 
>>> imtes 
[0, 1, 5, 6] 


#### <a name='execeval'></a>exec 与 eval 
exec(object[, globals[, locals]]) 
参数 
object：必选参数，表示需要被指定的 Python 代码。它必须是字符串或 code 对象。 
globals：可选参数，表示全局命名空间（存放全局变量），如果被提供，则必须是一个
字典对象。 
locals：可选参数，表示当前局部命名空间（存放局部变量），如果被提供，可以是任何
映射对象。 
如果该参数被忽略，那么它将会取与 globals 相同的值。 
exec()最佳实践----这样 b 才是修改后的值 
def test1(): 
    a = 10 
    loc = {'a': a} 
    glb = {} 
    exec('b = a + 1', glb, loc) 
    b = loc['b'] 
    print(loc) 
     
     
 
exec 没有返回值 
eval 有返回值,eval 还可以将字符串转化成对象 
class obj(object): 
    pass 
 
 
a = eval('obj()') 
print(a) 
对表达式进行求值 
print(eval('1 + 2'), type(eval('1 + 2')))  # 3, <class 'int'> 
print(eval('[1, 2, 3]'), type(eval('[1, 2, 3]'))) # [1, 2, 3] <class 'list'> 
print(eval('1' + '2')) # 12 
 
import ast 
my_list = ast.literal_eval('[1, 2, 3]')  
print(my_list, type(my_list))  # [1, 2, 3] <class 'list'> 
 
 
 
iter()与 next() 
M = ['a', 'b', 'c', 'd', 'e'] 
for x in M:  # 解释器隐式调用 
    print(x) 
 
for x in iter(M):  # 等价于 M.__iter()__   人为显示调用 
    print(x) 
     
iter()函数，就是将一个可迭代对象 M 变为迭代器也就是 M 调用__iter__()方法，然后内
部在调用__next__()方法。 
next(M)等价于 M.__next__ 
 
手动实现 for x in range(1, 4) print(x) 
```python
class Foo1(object): 
    def __init__(self, start, end): 
        self.start = start 
        self.end = end 
 
    def __iter__(self): 
        return self 
 
    def __next__(self): 
        num = self.start 
        if self.start == self.end: 
            raise StopIteration 
        self.start += 1 
        return num 
 
 
# 执行代码 
# foo1 = Foo1(1, 4) 
# for x in foo1: 
#     print(x) 
 
# output: 
# 1 
# 2 
# 3
```
 
### <a name='python-1'></a>python 函数参数传递方式 
python 中参数传递类似于引用传递。 
当函数参数为不可变对象（整数，字符串，元组）时，函数体内的参数在被改变之前，会一直持有该对象的引用，但当参数发生改变时，由于该对象为不可变对象，必须生成一份新的拷贝作为函数的本地变量，函数对该本地变量的修改不会影响函数调用者的变量值，这一点有点类似 C++ 函数的值传递。 
当函数参数为可变对象（列表，字典）时，除非发生赋值操作，函数体类的参数会一直持有该对象的引用，函数对该参数的修改也会影响到函数调用者的变量值，类似于 C++函数中的引用传递。但在函数体内发生赋值操作时，也会生成一份新的拷贝作为函数的本地变量，函数对该本地变量的修改不会影响到函数调用者的变量值。 
 

静态方法/类方法/实例方法 
```python
class Person(object): 
    def foo(self, num): 
        print('foo %s' % num) 
 
    @staticmethod 
    def foo_static(num): 
        print('foo_static %s' % num) 
 
    @classmethod 
    def foo_cls(cls, num): 
        print('foo_cls %s' % num) 
 
 
if __name__ == "__main__": 
    person = Person() 
    person.foo(1) 
    # Person.foo(2) 
    person.foo_cls(2) 
    Person.foo_cls(2) 
    person.foo_static(3) 
    Person.foo_static(3) 
     
# 类名.方式不能访问实例方法 
# 类名和实例对象都可以访问静态方法和类方法 

``` 
 
编写函数的 4 个原则 
1、函数设计要尽量短小，嵌套层次不宜过深。避免过长函数，嵌套最好能控制
在 3 层之内 
2、函数申明应该合理，简单，易于使用。除函数名能够够正确反映其大体功能
外，参数的设计也应该简洁明了，参数个数不宜太多 
3、函数参数设计应该考虑向下兼容。可以通过加入默认参数来避免退化 
4、一个函数只做一件事，就要尽量保证抽象层级的一致性，所有语句尽量在一
个粒度上。若在一个函数中处理多件事，不利于代码的重用 
 
 
## <a name='-1'></a>迭代器 
何为迭代：重复+继续 
第一，迭代需要重复进行某一操作； 
第二，本次迭代的要依赖上一次的结果继续往下做，如果中途有任何停顿，都不能算是迭代； 

可迭代对象---就是一个对象能够被迭代的使用。 
python 内部是如何知道一个对象是否为可迭代对象呢？答案是，在每一种数据类型对象中，都会有有一个__iter__()方法，正是因为这个方法，才使得这些基本数据类型变为可迭代。 
一个对象是否可迭代，关键看這个对象是否有__iter__()方法

迭代器---迭代器与可迭代对象区别在于:__next__()方法。迭代器有__next__()方法，可迭代对象没有。 
迭代器优点: 
1.节约内存 
2.不依赖索引取值 
3.实现惰性计算(什么时候需要，在取值出来计算) 
for 循环内部机制 
l = [1,2,3,4,5] 
 
for i in l: 
    print(i) 
l = [1,2,3,4,5]是一个可迭代对象，而且可迭代对象是不可以直接从其中取到元素。那么为啥我们还能从列表 l 中取到元素呢？这一切都是因为 for 循环内部实现。在 for 循环内部，首先 l 会调用__iter__()方法，将列表 l 变为一个迭代器，然后这个迭代器再调用其__next__()方法，返回取到的第一个值,这个元素就被赋值给了 i，接着就打印输出了。 
 
it = l.__iter__() 
print(type(it)) # <class 'list_iterator'> 
print(it.__next__()) # 1 
print(it.__next__()) # 2 
print(it.__next__()) # 3 
print(it.__next__()) # 4 
print(it.__next__()) # 5 
print(it.__next__()) # 什么都不输出 
 
 
from collections import Iterable, Iterator 
判断一个变量是否是可迭代的，isinstance(a, Iterable) 
判断一个变量是否是迭代器，isinstance(a, Iterator) 
 
__iter__方法返回一个迭代器； 
只要 raise StopIteration，for 循环自动就停了； 
 
并不是只有 for 循环能接收可迭代对象，list,tuple 等也能接收； 
判断对象是否是可迭代对象 
from collections.abc import Iterable 
 
f = open('test.txt', 'r') 
a = 10 
b = 'dong' 
c = {'name': 'dong'} 
d = ['2', 3, '56', 'ssha'] 
e = (2, 3, (3, 4, 6), 'sa') 
g = {2, 4, 9, 10} 
 
print(isinstance(f, Iterable))  # True 
print(isinstance(a, Iterable))  # False 
print(isinstance(b, Iterable))  # True 
print(isinstance(c, Iterable))  # True 
print(isinstance(d, Iterable))  # True 
print(isinstance(e, Iterable))  # True 
print(isinstance(g, Iterable))  # True 
除了整型之外，python 内的基本数据类型都是可迭代对象，包括文件对象。 
 
可迭代表明有__iter__方法 
print(hasattr(f, '__iter__')) # True 
print(hasattr(a, '__iter__')) # False 
print(hasattr(b, '__iter__')) # True 
print(hasattr(c, '__iter__')) # True 
print(hasattr(d, '__iter__')) # True 
print(hasattr(e, '__iter__')) # True 
print(hasattr(g, '__iter__'), '\n') # True 
 

迭代器与可迭代对象的区别就是，迭代器中多个__next__方法 
print(isinstance(f, Iterator)) # True 
print(isinstance(a, Iterator)) # False 
print(isinstance(b, Iterator)) # False 
print(isinstance(c, Iterator)) # False 
print(isinstance(d, Iterator)) # False 
print(isinstance(e, Iterator)) # False 
print(isinstance(g, Iterator)) # False 
 
 
 
## <a name='-1'></a>生成器 
将列表生成式中[]改成()之后数据结构是否改变？ 答案：是，从列表变为生成器。 
生成器： 
如果一个函数中有 yield 语句，表明这个函数就不再是一个函数了，而是一个生成器模板，如果调用这个函数的时候，就不再是调用这个函数了，而是创建一个生成器对象。
这个函数的返回值就是 yield 后面的内容，yield 表明执行到此语句的时候，函数停止，返回 yield 后面的内容；生成器是一个特殊的迭代器，它是可以迭代的，所以可以使用 next()语句调用。 
可以理解为一种数据类型，这种数据类型自动实现了迭代器协议（其他的数据类型需要调用自己内置的__iter__方法） 

Python 有两种不同的方式提供生成器: 
1.生成器函数（函数内部有 yield 关键字）：常规函数定义，但是，使用 yield 语句而不是 return 语句返回结果。yield 语句一次返回一个结果，在每个结果中间，挂起函数的状态，以便下次从它离开的地方继续执行 
2.生成器表达式：类似于列表推导，但是，生成器返回按需产生结果的一个对象，而不是一次构建一个结果列表Python 使用生成器对延迟操作提供了支持。所谓延迟操作，是指在需要的时候才产生结果，而不是立即产生结果。这也是生成器的主要好处。 

生成器小结： 
1.是可迭代对象 
2.实现了延迟计算,省内存啊 
3.生成器本质和其他的数据类型一样，都是实现了迭代器协议，只不过生成器附
加了一个延迟计算省内存的好处，其余的可迭代对象可没有这点好处！ 
def get_fibonacci(num): 
    a, b = 0, 1 
    current_num = 0 
    while current_num <= num: 
        yield a 
        a, b = b, a + b 
        current_num += 1 
    return "ok..." 
 
         
obj = get_fibonacci(10)  # 获得一个生成器对象 
next(obj)  # 调用生成器获取元素； 
### <a name='return'></a>怎么获取 return 的值？ 
 
while True: 
    try: 
        ret = next(obj) 
        print(ret) 
    except Exception as e: 
        print(e.value) 
        break 
迭代完，抛出异常 e 的 value 的值就是 return 的值； 
 
```python
def foo(): 
    while True: 
         x = yield 
         print('x==%s' % x) 
 
 
f = foo() 
print(next(f)) # 程序运动到 yield 就卡住，等待下一个 next 
f.send('dong') # 给 yield 发送值 dong,然后这个值被赋值给了 x，并且打印出来,然后继续下一次循环停在 yield 处 
f.send('xiang')  
print(next(f)) # 没有给 x 赋值，执行 print 语句，打印出 None,继续循环停在 yield 处 
 
# None 
# x==dong 
# x==xiang 
# x==None 
# None 

``` 
程序一旦执行到 yield 就会停在该处,并且将其返回值进行返回。上面的例子中，我们并
没有设置返回值，所有默认程序返回的是 None 
 
send()方法具有两种功能： 
第一，传值，send()方法，将其携带的值传递给 yield，注意，是传递给 yield，而不是 x,
然后再将其赋值给 x；第二，send()方法具有和 next()方法一样的功能，也就是说，传值
完毕后，会接着上次执行的结果继续执行，直到遇到 yield 停止。这也就为什么在调用
g.send()方法后，还会打印出 x 的数值 
 
 
执行下面的操作会报错： 
f = foo() 
f.send('dong') 
can't send non-None value to a just-started generator  # 错误提示:不能传递一个非空值给一个未启动的生成器。 
在一个生成器函数未启动之前，是不能传递数值进去。必须先传递一个 None 进去或者调用一次 next(g)方法，才能进行传值操作。 
 
```python
def deco(func): 
    def wrapper(): 
        res = func() 
        next(res) 
        return res 
    return wrapper 
 
 
@deco 
def func1(): 
    food_list = list() 
    while True: 
        x = yield food_list 
        food_list.append(x) 
        print('food_list==%s' % food_list) 
 
 
f1 = func1() 
f1.send('水蜜桃') 
f1.send('榴莲') 
f1.send('苹果') 
 
# food_list==['水蜜桃'] 
# food_list==['水蜜桃', '榴莲'] 
# food_list==['水蜜桃', '榴莲', '苹果'] 

``` 
 
ps: 
yield 的返回值是 yield 后面的值，food_list 
传给 yield 的值为 food 
send()既是传值，也是调用 next，执行下面的代码，直到下次 yield 停止； 
 
 
### <a name='yieldyieldfrom'></a>yield 与 yield from 
yield 将一个函数变成一个生成器 
yield 返回一个值 
yield from 后面跟的可以是生成器、元组、列表等可迭代对象以及 range()函数产生的序列; 

## <a name='-1'></a>什么是闭包？ 
作用域 
全局变量能够被文件任何地方引用，但修改只能在全局进行操作; 
如果局部没有找到所需的变量，就会往外进行查找，没有找到就会报错。 
函数闭包 
在计算机科学中，闭包（Closure）是词法闭包（Lexical Closure）的简称，是指引用了自由变量的函数。这个被引用的自由变量将和这个函数一同存在，即使已经离开了创造它的环境也不例外。所以，有另一种说法认为闭包是由函数和与其相关的引用环境组合而成的实体。 
Python 中如果调用函数 A，它返回函数 B。这个返回的函数 B 就叫做闭包，在调用函数 A 的时候传递的参数就是自由变量。举个例子： 
def func(name): 
    def inner_func(age): 
        print 'name:', name, 'age:', age 
    return inner_func 
 
bb = func('the5fire') 
bb(26)  # name: the5fire age: 26 
这里面调用 func 的时候就产生了一个闭包——inner_func,并且该闭包持有自由变量——name，因此这也意味着，当函数 func 的生命周期结束之后，name 这
个变量依然存在，因为它被闭包引用了，所以不会被回收。（闭包并不是 Python中特有的概念，所有把函数作为一等公民的语言均有闭包的概念。） 

解释一下什么是闭包？ 
在函数内部再定义一个函数，并且这个函数用到了外边函数的变量，那么将这个
函数以及用到的一些变量称之为闭包。闭包函数必须满足两个条件:1.函数内部定
义的函数 2.包含对外部作用域而非全局作用域的引用。 
 



## <a name='python-1'></a>python 中的魔法方法 
在 Python 中，所有以__双下划线包起来的方法，都统称为"魔术方法"。比如我们
接触最多的__init__. 
有些魔术方法,我们可能以后一辈子都不会再遇到了,这里也就只是简单介绍下; 
而有些魔术方法,巧妙使用它可以构造出非常优美的代码,比如将复杂的逻辑封装
成简单的 API。 
__doc__ ----类的描述信息; 
__module__ ----表示当前操作的对象在那个模块; 
__class__ ----表示当前操作的对象的类是什么; 
__name__ ----类调用的时候是类名，本 python 文件执行等于__main__，外部模块
调用本模块时，是模块名； 
__dic__ ----类或对象中的所有成员; 
__str__ ----如果一个类中定义了__str__方法，那么在打印对象时，默认输出该方
法的返回值; 
__getslice__、__setslice__、__delslice__ ----片操作时候被调用； 
__getitem__、__setitem__、__delitem__ ----用于索引操作，如字典。以上分别表示获
取、设置、删除数据 
__iter__ ----用于迭代器，之所以列表、字典、元组可以进行 for 循环，是因为类
型内部定义了 __iter__，一个类可迭代---类中定义__iter__方法，且方法返回一个
迭代器， 
__metaclass__ ----其用来表示该类由谁来实例化创建; 
__call__ ----对象后面加括号，触发执行。注：构造方法的执行是由创建对象触发
的，即：对象 = 类名() ；而对于 __call__ 方法的执行是由对象后加括号触发的，
即：对象() 或者 类()() 
__init__ __new__ __del__ 
构造和初始化  
__init__我们很熟悉了,它在对象初始化的时候调用,我们一般将它理解为"构造函
数". 
简化数据结构的初始化-----在基类的初始化方法__init__中对传参与每个实例的
可允许传参进行校验。 
定义多个构造器，定义__init__(*args)方法，另外的构造器--定义类方法，调用 cls(*args); 
__new__()----创建一个未初始化的实例，类方法中调用时，instance = cls.__new__(cls); 
实际上, 当我们调用 x = SomeClass()的时候调用,__init__并不是第一个执行的, 
__new__才是。所以准确来说,是__new__和__init__共同构成了"构造函数". 
__new__是用来创建类并返回这个类的实例, 而__init__只是将传入的参数来初始
化该实例. 
__new__在创建一个实例的过程中必定会被调用,但__init__就不一定，比如通过
pickle.load 的方式反序列化一个实例时就不会调用__init__。 
__new__方法总是需要返回该类的一个实例，而__init__不能返回除了 None 的任
何值。比如下面例子: 
class Foo(object): 
 
    def __init__(self): 
        print 'foo __init__' 
        return None  # 必须返回 None,否则抛 TypeError 
 
    def __del__(self): 
        print 'foo __del__' 
实际中,你很少会用到__new__，除非你希望能够控制类的创建。 如果要讲解
__new__，往往需要牵扯到 metaclass(元类)的介绍。 
在对象的生命周期结束时, __del__会被调用,可以将__del__理解为"析构函数". 
__del__定义的是当一个对象进行垃圾回收时候的行为。 
有一点容易被人误解, 实际上，x.__del__() 并不是对于 del x 的实现,但是往往执
行 del x 时会调用 x.__del__(). 
怎么来理解这句话呢? 继续用上面的 Foo 类的代码为例: 
foo = Foo() 
foo.__del__() 
print foo 
del foo 
print foo  # NameError, foo is not defined 
如果调用了 foo.__del__()，对象本身仍然存在. 但是调用了 del foo, 就再也没有
foo 这个对象了. 
请注意，如果解释器退出的时候对象还存在，就不能保证 __del__ 被确切的执行
了。所以__del__并不能替代良好的编程习惯。 比如，在处理 socket 时，及时关
闭结束的连接。 
 
 
 
__setattr__ __getattr__ __delattr__ 
属性访问控制 
希望 Python 能够定义私有属性，然后提供公共可访问的 getter 和 setter。 
代理----一种编程模式，将某个操作转移给另一个对象来实现； 
当需要代理的方法很多时，可以尝试使用__getattr__来实现； 
__getattr__是在属性不存在的时候才被调用； 
代 理 模 式 ： 代 理 类 中 实 例 化 时 传 被 代 理 类 的 实 例 ， 并 在 代 理 类 中 实 现
__setattr__/__getattr__/__delattr__方法； 
有个约定是：只代理那些不以下划线开头的属性；  
Python 其实可以通过魔术方法来实现封装。 
凡是对属性赋值或者修改操作，都触发__setattr__的执行 
__setattr__(self, name, value) 
__setattr__ 是实现封装的解决方案，它定义了你对属性进行赋值和修改操作时的
行为。 不管对象的某个属性是否存在,它都允许你为该属性进行赋值,因此你可以
为属性的值进行自定义操作。有一点需要注意，实现__setattr__时要避免"无限递
归"的错误，下面的代码示例中会提到。 
__delattr__(self, name) 
__getattr__:只有在调用属性时且属性不存在的情况下，触发這个函数的执行。 
__getattr__(self, name) 
该 方 法 定 义 了 你 试 图 访 问 一 个 不 存 在 的 属 性 时 的 行 为 。 因 此 ， 在 支 持
__getattribute__的 Python 版本，调用__getattr__前必定会调用 __getattribute__。重
载该方法可以实现捕获错误拼写然后进行重定向, 或者对一些废弃的属性进行
警告。 
__delattr__ 与 __setattr__ 很像，只是它定义的是你删除属性时的行为。实现
__delattr__是同时要避免"无限递归"的错误，__getattribute__同样要避免"无限递归
"的错误。 需要提醒的是，最好不要尝试去实现__getattribute__，因为很少见到这
种做法，而且很容易出 bug。 
例子说明__setattr__的无限递归错误: 
def __setattr__(self, name, value): 
    self.name = value 
    # 每一次属性赋值时, __setattr__都会被调用，因此不断调用自身导致无限递归了。 
因此正确的写法应该是: 
def __setattr__(self, name, value): 
    self.__dict__[name] = value 
下面的例子很好的说明了上面介绍的 4 个魔术方法的调用情况: 
class Access(object): 
 
    def __getattr__(self, name): 
        print '__getattr__' 
        return super(Access, self).__getattr__(name) 
 
    def __setattr__(self, name, value): 
        print '__setattr__' 
        return super(Access, self).__setattr__(name, value) 
 
    def __delattr__(self, name): 
        print '__delattr__' 
        return super(Access, self).__delattr__(name) 
 
    def __getattribute__(self, name): 
        print '__getattribute__' 
        return super(Access, self).__getattribute__(name) 
 
access = Access() 
access.attr1 = True  # __setattr__调用 
access.attr1  # 属性存在，只有__getattribute__调用 
try: 
    access.attr2  # 属性不存在, 先调用__getattribute__, 后调用__getattr__ 
except AttributeError: 
    pass 
del access.attr1  # __delattr__调用 
 
我们自己要重写方法，必须按照如下方式来做： 
def __setattr__(self, key, value): 
    self.__dict__[key] = value 
     
def __getattr__(self, item): 
    return self.__dict__[item] 
 
def __delattr__(self, item): 
    del self.__dict__[item] 
     
 
 
 
 
__getattribute__ 
__getattribute__(self, name) 
__getattribute__是属性访问拦截器，就是当这个类的属性被实例访问时，会自动调
用类的__getattribute__方法； 
```python
class TestObj(object): 
    cls_attr = 10 
 
    def __init__(self, name, age): 
        self.name = name 
        self.age = age 
 
    def __getattribute__(self, item): 
        print("__getattribute__被调用...") 
        try: 
            return super(TestObj, self).__getattribute__(item) 
        except Exception: 
            print("__getattribute__ error") 
 
 
if __name__ == '__main__': 
    t = TestObj(name='zhangsan', age=18) 
    print(t.name) 
    print(t.age) 
    print(TestObj.cls_attr) 
    print(t.email) 
 
# __getattribute__被调用... 
# zhangsan 
# __getattribute__被调用... 
# 18 
# 10 
# __getattribute__被调用... 
# __getattribute__ error 
# None 

``` 
 
总结要点： 
1.__getattribute__(self，*args，**kwgs)中传入的参数是属性名，不是属值； 
2.使用类名调用类属性时，不会经过__getattribute__方法，只拦截实例对象对属性
的调用，包括调用类属性 
3.__getattribute__是属性拦截器，属性调用会传入处理，最后要有返回值，将传入
属性处理后返回给调用者。 
 
__get__ __set__ __delete__ 
描述器就是一个实现了三个核心属性访问操作的类，分别为__get__，__set__和
__delete__方法； 这些方法接受一个实例作为输入，之后相应地操作实例底层的
字典；描述其只能在类级别被定义；__get__方法就是确保绑定方法对象能被正确
的创建； 
我们知道，距离既可以用单位"米"表示,也可以用单位"英尺"表示。 现在我们定
义一个类来表示距离,它有两个属性: 米和英尺。 
class Meter(object): 
    '''Descriptor for a meter.''' 
    def __init__(self, value=0.0): 
        self.value = float(value) 
    def __get__(self, instance, owner): 
        return self.value 
    def __set__(self, instance, value): 
        self.value = float(value) 
 
class Foot(object): 
    '''Descriptor for a foot.''' 
    def __get__(self, instance, owner): 
        return instance.meter * 3.2808 
    def __set__(self, instance, value): 
        instance.meter = float(value) / 3.2808 
 
class Distance(object): 
    meter = Meter() 
    foot = Foot() 
 
d = Distance() 
print d.meter, d.foot  # 0.0, 0.0 
d.meter = 1 
print d.meter, d.foot  # 1.0 3.2808 
d.meter = 2 
print d.meter, d.foot  # 2.0 6.5616 
在上面例子中，在还没有对 Distance 的实例赋值前, 我们认为 meter 和 foot 应该
是各自类的实例对象, 但是输出却是数值。这是因为__get__发挥了作用. 
我们只是修改了 meter，并且将其赋值成为 int，但 foot 也修改了。这是__set__发
挥了作用.  
描述器对象(Meter、Foot)不能独立存在, 它需要被另一个所有者类(Distance)所持
有。 描述器对象可以访问到其拥有者实例的属性，比如例子中 Foot 的
instance.meter。 
在面向对象编程时，如果一个类的属性有相互依赖的关系时，使用描述器来编写
代码可以很巧妙的组织逻辑。 在 Django 的 ORM 中, models.Model 中的
IntegerField 等, 就是通过描述器来实现功能的。 
一个类要成为描述器，必须实现__get__, __set__, __delete__ 中的至少一个方法。
下面简单介绍下: 
__get__(self, instance, owner) 
参数 instance 是拥有者类的实例。参数 owner 是拥有者类本身。__get__在其拥有
者对其读值的时候调用。 
__set__(self, instance, value) 
__set__在其拥有者对其进行修改值的时候调用。 
__delete__(self, instance) 

创建描述符 
对于实例 a，a.x 的查找顺序为 a.__dict__['x'],然后是 type(a).__dict__['x'] 
 
a. 基于类创建描述符 
class Email(object): 
    def __init__(self): 
        self._name = '' 
 
    def __get__(self, instance, owner): 
        return self._name 
 
    def __set__(self, instance, value): 
        m = re.match('\w+@\w+\.\w+', value) 
        if not m: 
            raise Exception('email not valid') 
        self._name = value 
 
    def __delete__(self, instance): 
        del self._name 
 
 
class Person1(object): 
    email = Email() 
 
 
p2 = Person1() 
p2.email = '234354@126.com' 
print(p2.email) # 234354@126.com 
p2.email = 'sfgdgfrgr' # Exception: email not valid 
 
b. 使用 property 函数创建描述符 
class Person2(object): 
    def __init__(self): 
        self._email = None 
 
    def get_email(self): 
        return self._email 
 
    def set_email(self, email): 
        m = re.match('\w+@\w+\.\w+', email) 
        if not m: 
            raise Exception('email not valid') 
        self._email = email 
 
    def delete_email(self): 
        del self._email 
 
    email = property(get_email, set_email, delete_email, 'email valid check') 
 
 
p3 = Person2() 
p3.email = '234354@126.com' 
print(p3.email) # 234354@126.com 
p3.email = 'sfgdgfrgr' # Exception: email not valid 
 
c. 使用@property 装饰器 
class Person3(object): 
    def __init__(self): 
        self._email = None 
 
    @property 
    def email(self): 
        return self._email 
 
    @email.setter 
    def email(self, email): 
        m = re.match('\w+@\w+\.\w+', email) 
        if not m: 
            raise Exception('email not valid') 
        self._email = email 
 
    @email.deleter 
    def email(self): 
        del self._email 
 
 
p4 = Person3() 
p4.email = 'dgfgrhtyjy@163.com' 
print(p4.email) 
p4.email = 'grfghoosf' # Exception: email not valid 
 
 
 
property 用法 
属性加个@property 装饰器，也就是 get 方法， 
set 方法加@name.setter， 
delete 方法加@name.deleter； 
如果子类扩展父类的 property 装饰的属性， 
get----return super().name 
set----super(SubClass, SubClass).name.__set__(self, value) 
delete----super(SubClass, SubClass).name.__delete__(self) 
如果只扩展一个方法的话，装饰器可以这样写 
@ParentClass.name.setter 和@ParentClass.name.getter; 
__delete__在其拥有者对其进行删除的时候调用。 
 
 
 
__getitem__ __setitem__ __delitem__ 
构造自定义容器  
列表，字典取元素的方式，lst = [1,2,3,4],取第一个元素 lst[0],d ={'name':'xiaohua'} 取元
素 dd['name'],与__setitem__，__getitem__，delitem__三个函数有关 
```python
# -*- coding: utf-8 -*- 
class FunctionalList: 
    ''' 实现了内置类型 list 的功能,并丰富了一些其他方法: head, tail, init, last, drop, take''' 
 
    def __init__(self, values=None): 
        if values is None: 
            self.values = [] 
        else: 
            self.values = values 
 
    def __len__(self): 
        return len(self.values) 
 
    def __getitem__(self, key): 
        return self.values[key] 
 
    def __setitem__(self, key, value): 
        self.values[key] = value 
 
    def __delitem__(self, key): 
        del self.values[key] 
 
    def __iter__(self): 
        return iter(self.values) 
 
    def __reversed__(self): 
        return FunctionalList(reversed(self.values)) 
 
    def append(self, value): 
        self.values.append(value) 
    def head(self): 
        # 获取第一个元素 
        return self.values[0] 
    def tail(self): 
        # 获取第一个元素之后的所有元素 
        return self.values[1:] 
    def init(self): 
        # 获取最后一个元素之前的所有元素 
        return self.values[:-1] 
    def last(self): 
        # 获取最后一个元素 
        return self.values[-1] 
    def drop(self, n): 
        # 获取所有元素，除了前 N 个 
        return self.values[n:] 
    def take(self, n): 
        # 获取前 N 个元素 
        return self.values[:n] 
 
if __name__ == '__main__': 
    l = [1, 2, 3] 
    f = FunctionalList(l) 
    f[2] = 10   # 调用__setitem__ 
    print(f.values)  # [1, 2, 10] 
    del f[0]  # 调用__delitem__ 
    print(f.values) # [2, 10] 
    print(f[0]) # 2 调用__getitem__
```
 
如果我们要自定义一些数据结构，使之能够跟以上的容器类型表现一样，那就需
要去实现某些协议。 
这里的协议跟其他语言中所谓的"接口"概念很像，一样的需要你去实现才行，只
不过没那么正式而已。 
如果要自定义不可变容器类型，只需要定义__len__ 和 __getitem__方法; 如果要
自定义可变容器类型，还需要在不可变容器类型的基础上增加定义__setitem__ 和 
__delitem__。 如果你希望你的自定义数据结构还支持"可迭代", 那就还需要定义
__iter__。 
__len__(self) 
需要返回数值类型，以表示容器的长度。该方法在可变容器和不可变容器中必须
实现。 
__getitem__(self, key) 
当你执行 self[key]的时候，调用的就是该方法。该方法在可变容器和不可变容器
中也都必须实现。 调用的时候,如果 key 的类型错误，该方法应该抛出 TypeError； 
如果没法返回 key 对应的数值时,该方法应该抛出 ValueError。 
__setitem__(self, key, value) 
当你执行 self[key] = value 时，调用的是该方法。 
__delitem__(self, key) 
当你执行 del self[key]的时候，调用的是该方法。 
__iter__(self) 
该方法需要返回一个迭代器(iterator)。当你执行 for x in container: 或者使用
iter(container)时，该方法被调用。 
__reversed__(self) 
如果想要该数据结构被內建函数 reversed()支持,就还需要实现该方法。 
__contains__(self, item) 
如果定义了该方法，那么在执行 item in container 或者 item not in container 时该方
法就会被调用。 如果没有定义，那么 Python 会迭代容器中的元素来一个一个比
较，从而决定返回 True 或者 False。 
__missing__(self, key) 
dict 字典类型会有该方法，它定义了 key 如果在容器中找不到时触发的行为。 比
如 d = {'a': 1}, 当你执行 d[notexist]时，d.__missing__('notexist')就会被调用。 
我们再举个例子，实现 Perl 语言的 AutoVivification,它会在你每次引用一个值未
定义的属性时为你自动创建数组或者字典。 
```python
class AutoVivification(dict): 
    """Implementation of perl's autovivification feature.""" 
    def __missing__(self, key): 
        value = self[key] = type(self)() 
        return value 
 
weather = AutoVivification() 
weather['china']['guangdong']['shenzhen'] = 'sunny' 
weather['china']['hubei']['wuhan'] = 'windy' 
weather['USA']['California']['Los Angeles'] = 'sunny' 
print(weather) 
 
# 结果输出:{'china': {'hubei': {'wuhan': 'windy'}, 'guangdong': {'shenzhen': 'sunny'}}, 'USA': {'California': {'Los Angeles': 'sunny'}}} 

```
在 Python 中，关于自定义容器的实现还有更多实用的例子，但只有很少一部分
能够集成在 Python 标准库中，比如 Counter, OrderedDict 等 
 
 
 
python 上下文管理器的两种方式？ 
1. __enter__ __exit__ 
with 声明是从 Python2.5 开始引进的关键词。你应该遇过这样子的代码: 
with open('foo.txt') as bar: 
    do something with bar 
在 with 声明的代码段中，我们可以做一些对象的开始操作和清除操作，还能对
异常进行处理。这需要实现两个魔术方法: __enter__ 和 __exit__。 
__enter__(self) 
__enter__会返回一个值，并赋值给 as 关键词之后的变量。在这里，你可以定义代
码段开始的一些操作。 
__exit__(self, exception_type, exception_value, traceback) 
__exit__定义了代码段结束后的一些操作，可以这里执行一些清除操作，或者做
一些代码段结束后需要立即执行的命令，比如文件的关闭，socket 断开等。如果
代码段成功结束，那么 exception_type, exception_value, traceback 三个参数传进
来时都将为 None。如果代码段抛出异常，那么传进来的三个参数将分别为: 异常
的类型，异常的值，异常的追踪栈。 如果__exit__返回 True, 那么 with 声明下的
代码段的一切异常将会被屏蔽。 如果__exit__返回 None, 那么如果有异常，异常
将正常抛出，这时候 with 的作用将不会显现出来。 
举例说明： 
这该示例中，IndexError 始终会被隐藏，而 TypeError 始终会抛出。 
class DemoManager(object): 
 
    def __enter__(self): 
        pass 
 
    def __exit__(self, ex_type, ex_value, ex_tb): 
        if ex_type is IndexError: 
            print(ex_value.__class__) 
            return True 
        if ex_type is TypeError: 
            print(ex_value.__class__) 
            return  # return None 
 
with DemoManager() as nothing: 
    data = [1, 2, 3] 
    data[4]  # raise IndexError, 该异常被__exit__处理了 
 
with DemoManager() as nothing: 
    data = [1, 2, 3] 
    data['a']  # raise TypeError, 该异常没有被__exit__处理 
 
''' 
输出: 
<type 'exceptions.IndexError'> 
<type 'exceptions.TypeError'> 
Traceback (most recent call last): 
  ... 
''' 
__slots__ __dict__ 
Python 是一门动态语言。通常，动态语言允许我们在程序运行时给对象绑定新的
属性或方法，当然也可以对已经绑定的属性和方法进行解绑定。但是如果我们需
要限定自定义类型的对象只能绑定某些属性，可以通过在类中定义__slots__变量
来进行限定。 
__slots__是什么:是一个类变量,变量值可以是列表,元祖,或者可迭代对象,也可以是一个
字符串(意味着所有实例只有一个数据属性) 
 
字典会占用大量内存,如果你有一个属性很少的类,但是有很多实例,为了节省内存可以使
用__slots__取代实例的__dict__ 
 
# 限定 Person 对象只能绑定_name, _age 和_gender 属性 
__slots__ = ('_name', '_age', '_gender') 
#当你定义 __slots__ 后，Python 就会为实例使用一种更加紧凑的内部表示。实例通过
一个很小的固定大小的数组来构建，而不是为每个实例定义一个字典，这跟元组或列表
很类似。在 __slots__ 中列出的属性名在内部被映射到这个数组的指定小标上，使用
__slots__一个不好的地方就是我们不能再给实例添加新的属性了,只能使用在__slots__
中定义的那些属性名。 
 
定义了__slots__后的类不再 支持一些普通类特性了,比如多继承。大多数情况下,你应该
只在那些经常被使用到 的用作数据结构的类上定义__slots__比如在程序中需要创建某
个类的几百万个实例对象。 
 
 
```python
class Person(object): 
    __slots__ = {'name': 'dong', 'age': 20} 
 
    def __init__(self, name): 
        self.name = name 
 
 
p1 = Person('p1') 
print('p1.__slot__==%s, 以及 p1.__slot__内存地址==%s, ' % 
      (p1.__slots__, id(p1.__slots__))) 
 
p2 = Person('p2') 
print('p2.__slot__==%s, 以及 p2.__slot__内存地址==%s, ' % 
      (p2.__slots__, id(p2.__slots__))) 
 
# 可以看到有类属性__slot__，所有实例共享一份此属性，且内存中此资源只有一份，并且类型定义了__slot__就不能没有__dict__属性了 
# p1.__slot__=={'name': 'dong', 'age': 20}, 以及 p1.__slot__内存地址==2776144564544,  
# p2.__slot__=={'name': 'dong', 'age': 20}, 以及 p2.__slot__内存地址==2776144564544,  
 
 
class Person1(object): 
    prop = 'wuming' 
 
    def __init__(self, name): 
        self.name = name 
 
 
p3 = Person1('p3') 
print('p3.__dict__==%s, 以及 p3.__dict__内存地址==%s, ' % 
      (p3.__dict__, id(p3.__dict__))) 
p4 = Person1('p4') 
print('p4.__dict__==%s, 以及 p4.__dict__内存地址==%s, ' % 
      (p4.__dict__, id(p4.__dict__))) 
# __dict__是每个实例都有一份不同的属性，且存储根据实例来 
# p3.__dict__=={'name': 'p3'}, 以及 p3.__dict__内存地址==2042154602752,  
# p4.__dict__=={'name': 'p4'}, 以及 p4.__dict__内存地址==2042155447744,  

``` 
 
 
__repr__ __str__ __format__ 
格式化代码  
__repr__返回一个实例的代码表示形式，通常用来重新构造这个函数，跟我们使
用交互式解释器显示的是同一个值；!r----格式化代码时表示使用__repr__代替
__str__输出； 
__format__自定义格式化； 
b、d、o、x 分别是二进制、十进制、八进制、十六进制。 
__str__(self)   # 对实例使用 str()时调用。 
__repr__(self)  # 对实例使用 repr()时调用。str()和 repr()都是返回一个代表该实例的字
符串， 主要区别在于: str()的返回值要方便人来看,而 repr()的返回值要方便计算机看。 
'Pair({0.x!r}, {0.y!r})'.format(self)   # {0.x!r}意思是第 1 个参数 self 的属性 x,!r 格式化代
码指明输出使用 __repr__() 来代替默认的 __str__() 。 
 
 
 
 
在打印对象的时候，会触发這个函数的执行，__str__：只能返回一个字符串类型，__repr__
先于__str__执行。 
 
```python
 
class Person(object): 
    def __init__(self, name): 
        self.name = name 
 
 
# p = Person('dong') 
# print(p)    # <__main__.Person object at 0x000001A7B682CEE0> 
 
 
class Person1(object): 
    def __init__(self, name): 
        self.name = name 
 
    def __str__(self): 
        return 'person name==%s' % self.name 
 
 
# p1 = Person1('dong') 
# print(p1)  # person name==dong 
 
# p1 = Person1('dong') 
# print(p1)  # str person name==dong 
 
 
class Person2(object): 
    def __init__(self, name): 
        self.name = name 
 
    def __repr__(self): 
        return 'repr person name==%s' % self.name 
 
 
# p2 = Person2('dong') 
# print(p2) # repr Person name==dong 
 
 
class Person3(object): 
    def __init__(self, name): 
        self.name = name 
 
    def __str__(self): 
        return 'str person name=%s' % self.name 
 
    def __repr__(self): 
        return 'repr person name==%s' % self.name 
 
 
# p3 = Person3('dong') 
# print(p3) 
# repr Person name==dong 
# str Person name=dong 
 
 
 
 
# 带关键字的格式化 
print('Your name %(name)s sounds nice!!' % {'name': 'dong'}) 
# Your name dong sounds nice!! 
print('My age %(age)i years old.' % {'age': 20}) 
# My age 20 years old. 
print('Hello {name}!!!'.format(name='James')) 
# hello James!!! 

``` 
 
 
__import__('字符串') 
CC = __import__(imp) 這种方式就是通过输入字符串导入你所想导入的模块  
CC.f1()  # 执行模块中的 f1 方法 
這种方式就是通过输入字符串导入你所想导入的模块 
模块名有可能不是在本级目录中存放着，这时可以通过以下方式处理： 
dd = __import__("lib.text.commons",fromlist = True)  #改用这种方式就能导入成功 


__class__ 
__class__它是一个类的引用， 
self.__class__ is a reference to the type of the current instance. 
 
name = 'dong' 
age = 20 
print(name.__class__)  # <class 'str'> 
print(age.__class__) # <class 'int'> 
 
 
def foo(): 
    pass 
 
 
print(foo.__class__)  # <class 'function'> 
 
 
class Bar(): 
    pass 
 
 
b = Bar() 
print(b.__class__)  # <class '__main__.Bar'> 
print(b.__class__()) # <__main__.Bar object at 0x0000029BE5D4F370> 又成为一个实例了 
print(name.__class__.__class__) # <class 'type'> 
print(age.__class__.__class__) # <class 'type'> 
print(foo.__class__.__class__) # <class 'type'> 
print(b.__class__.__class__) # <class 'type'> 
元类就是创建类这种对象的东西。 
 
 
当你写如下代码时 : 
class Foo(Bar): 
    pass 
Python 做了如下的操作： 
Foo 中有__metaclass__这个属性吗？如果是，Python 会在内存中通过__metaclass__创建
一个名字为 Foo 的类对象（我说的是类对象，请紧跟我的思路）。如果 Python 没有找
到__metaclass__，它会继续在 Bar（父类）中寻找__metaclass__属性，并尝试做和前面同
样的操作。如果 Python 在任何父类中都找不到__metaclass__，它就会在模块层次中去寻
找__metaclass__，并尝试做同样的操作。如果还是找不到__metaclass__,Python 就会用内
置的 type 来创建这个类对象。 
 
在用 class 语句自定义类时，默认 metaclass 是 type，我们也可以指定 metaclass 来创
建类。 由于 python3 和 python2 在指定类的 metaclass 语法不兼容，下面分别示例 
python2 和 python3 两个版本。 
 
python2 版本： 
class Bar(object): 
    __metaclass__ = MetaClass 
 
    def __init__(self, name): 
        self.name = name 
 
    def print_name(): 
        print(self.name) 
 
python3 版本： 
class Bar(object, metaclass=MetaClass): 
    def __init__(self, name): 
        self.name = name 
 
    def print_name(): 
        print(self.name) 
 
 
 
修改某个对象的__class__属性为另一个类，之后使用另一个类的方法         
class A(object): 
    def show(self): 
        print('A detail') 
 
 
class B(A): 
    def show(self): 
        print('B detail') 
 
 
obj = B() 
obj.__class__ = A 
obj.show()  
 
 
 
 
__call__ 
__call__()----特殊的类实例方法，使得类实例对象可以像调用普通函数那样，以"对象名
()"的形式调用； 
Python 中，凡是可以将()直接应用到自身并执行，都称为可调用对象。 
可调用对象包括自定义的函数、Python 内置函数以及本节所讲的类实例对象。 
 
 
可以调用的对象: 一个特殊的魔术方法可以让类的实例的行为表现的像函数一样 
class Entity: 
'''调用实体来改变实体的位置。''' 
 
def __init__(self, size, x, y): 
    self.x, self.y = x, y 
    self.size = size 
 
def __call__(self, x, y): 
    '''改变实体的位置''' 
    self.x, self.y = x, y 
 
e = Entity(1, 2, 3) // 创建实例 
e(4, 5) //实例可以象函数那样执行，并传入 x y 值，修改对象的 x y 
 
 
 
 
 
__all__ 
模块中定义__all__变量，当这个模块被导入到别处； 
from module import *----这样的导入将会导入所有不以下划线开头的属性和方法； 
from . import A----导入同级目录下的 A 模块 
from .module import A----打入同级目录下的模块中的类； 
 
  
一个包里有三个模块，mod1.py, mod2.py, mod3.py，但使用 from demopack import *导入模块时，如何保证只有 mod1、mod3 被导入了。 
答案:增加__init__.py 文件，并在文件中增加：__all__ = ['mod1','mod3'] 
 

 
对象的序列化 
Python 对象的序列化操作是 pickling 进行的。pickling 非常的重要，以至于 Python
对此有单独的模块 pickle，还有一些相关的魔术方法。使用 pickling, 你可以将数
据存储在文件中，之后又从文件中进行恢复。 
下面举例来描述 pickle 的操作。从该例子中也可以看出,如果通过 pickle.load 初
始化一个对象, 并不会调用__init__方法。 
```python
# -*- coding: utf-8 -*- 
from datetime import datetime 
import pickle 
 
class Distance(object): 
 
    def __init__(self, meter): 
        print 'distance __init__' 
        self.meter = meter 
 
data = { 
    'foo': [1, 2, 3], 
    'bar': ('Hello', 'world!'), 
    'baz': True, 
    'dt': datetime(2016, 10, 01), 
    'distance': Distance(1.78), 
} 
print 'before dump:', data 
with open('data.pkl', 'wb') as jar: 
    pickle.dump(data, jar)  # 将数据存储在文件中 
 
del data 
print 'data is deleted!' 
 
with open('data.pkl', 'rb') as jar: 
    data = pickle.load(jar)  # 从文件中恢复数据 
print 'after load:', data 

```

值得一提，从其他文件进行 pickle.load 操作时，需要注意有恶意代码的可能性。
另外，Python 的各个版本之间,pickle 文件可能是互不兼容的。 
pickling 并不是 Python 的內建类型，它支持所有实现 pickle 协议(可理解为接口)
的类。pickle 协议有以下几个可选方法来自定义 Python 对象的行为。 
__getinitargs__(self) 
如果你希望 unpickle 时，__init__方法能够调用，那么就需要定义__getinitargs__, 
该方法需要返回一系列参数的元组，这些参数就是传给__init__的参数。 
该方法只对 old-style class 有效。所谓 old-style class,指的是不继承自任何对象的类，
往往定义时这样表示: class A:, 而非 class A(object): 
__getnewargs__(self) 
跟__getinitargs__很类似，只不过返回的参数元组将传值给__new__ 
__getstate__(self) 
在调用 pickle.dump 时，默认是对象的__dict__属性被存储，如果你要修改这种行
为，可以在__getstate__方法中返回一个 state。state 将在调用 pickle.load 时传值给
__setstate__ 
__setstate__(self, state) 
一般来说,定义了__getstate__,就需要相应地定义__setstate__来对__getstate__返回
的 state 进行处理。 
__reduce__(self) 
如果 pickle 的数据包含了自定义的扩展类（比如使用 C 语言实现的 Python 扩展
类）时，就需要通过实现__reduce__方法来控制行为了。由于使用过于生僻，这里
就不展开继续讲解了。 
令人容易混淆的是，我们知道, reduce()是 Python 的一个內建函数, 需要指出
__reduce__并非定义了 reduce()的行为，二者没有关系。 
__reduce_ex__(self) 
__reduce_ex__ 是为了兼容性而存在的, 如果定义了 __reduce_ex__, 它将代替
__reduce__ 执行。 
下面的代码示例很有意思，我们定义了一个类 Slate(中文是板岩的意思)。这个类
能够记录历史上每次写入给它的值,但每次 pickle.dump 时当前值就会被清空，仅
保留了历史。 
```python
# -*- coding: utf-8 -*- 
import pickle 
import time 
 
class Slate: 
    '''Class to store a string and a changelog, and forget its value when pickled.''' 
    def __init__(self, value): 
        self.value = value 
        self.last_change = time.time() 
        self.history = [] 
 
    def change(self, new_value): 
        # 修改 value, 将上次的 valeu 记录在 history 
        self.history.append((self.last_change, self.value)) 
        self.value = new_value 
        self.last_change = time.time() 
 
    def print_changes(self): 
        print('Changelog for Slate object:') 
        for k, v in self.history: 
            print('%s    %s' % (k, v)) 
 
    def __getstate__(self): 
        # 故意不返回 self.value 和 self.last_change, 
        # 以便每次 unpickle 时清空当前的状态，仅仅保留 history 
        return self.history 
 
    def __setstate__(self, state): 
        self.history = state 
        self.value, self.last_change = None, None 
 
slate = Slate(0) 
time.sleep(0.5) 
slate.change(100) 
time.sleep(0.5) 
slate.change(200) 
slate.change(300) 
slate.print_changes()  # 与下面的输出历史对比 
with open('slate.pkl', 'wb') as jar: 
    pickle.dump(slate, jar) 
del slate  # delete it 
with open('slate.pkl', 'rb') as jar: 
    slate = pickle.load(jar) 
print('current value:', slate.value)  # None 
print(slate.print_changes())  # 输出历史记录与上面一致 

```

运算符相关的魔术方法 
运算符相关的魔术方法实在太多了，也很好理解，不打算多讲。在其他语言里，
也有重载运算符的操作，所以我们对这些魔术方法已经很了解了。 
比较运算符 
__cmp__(self, other) 
如果该方法返回负数，说明 self < other; 返回正数，说明 self > other; 返回 0 说明
self == other。 强烈不推荐来定义__cmp__, 取而代之, 最好分别定义__lt__等方法
从而实现比较功能。 __cmp__在 Python3 中被废弃了。 
__eq__(self, other) 
定义了比较操作符==的行为. 
__ne__(self, other) 
定义了比较操作符!=的行为. 
__lt__(self, other) 
定义了比较操作符<的行为. 
__gt__(self, other) 
定义了比较操作符>的行为. 
__le__(self, other) 
定义了比较操作符<=的行为. 
__ge__(self, other) 
定义了比较操作符>=的行为. 
下面我们定义一种类型 Word, 它会使用单词的长度来进行大小的比较, 而不是
采用 str 的比较方式。 但是为了避免 Word('bar') == Word('foo') 这种违背直觉的
情况出现,并没有定义__eq__, 因此 Word 会使用它的父类(str)中的__eq__来进行
比较。 
下面的例子中也可以看出: 在编程语言中, 如果 a >=b and a <= b, 并不能推导出 a == b 这样的结论。 
```python
# -*- coding: utf-8 -*- 
class Word(str): 
    '''存储单词的类，定义比较单词的几种方法''' 
    def __new__(cls, word): 
        # 注意我们必须要用到__new__方法，因为 str 是不可变类型 
        # 所以我们必须在创建的时候将它初始化 
        if ' ' in word: 
            print "Value contains spaces. Truncating to first space." 
            word = word[:word.index(' ')]  # 单词是第一个空格之前的所有字符 
        return str.__new__(cls, word) 
 
    def __gt__(self, other): 
        return len(self) > len(other) 
    def __lt__(self, other): 
        return len(self) < len(other) 
    def __ge__(self, other): 
        return len(self) >= len(other) 
    def __le__(self, other): 
        return len(self) <= len(other) 
 
print('foo < fool:', Word('foo') < Word('fool'))  # True 
print('foolish > fool:', Word('foolish') > Word('fool'))  # True 
print('bar >= foo:', Word('bar') >= Word('foo'))  # True 
print('bar <= foo:', Word('bar') <= Word('foo'))  # True 
print('bar == foo:', Word('bar') == Word('foo'))  # False, 用了 str 内置的比较方法来进行比较 
print('bar != foo:', Word('bar') != Word('foo'))  # True 

```

一元运算符和函数 
__pos__(self) 
实现了'+'号一元运算符(比如+some_object) 
__neg__(self) 
实现了'-'号一元运算符(比如-some_object) 
__invert__(self) 
实现了~号(波浪号)一元运算符(比如~some_object) 
__abs__(self) 
实现了 abs()內建函数. 
__round__(self, n) 
实现了 round()内建函数. 参数 n 表示四舍五进的精度. 
__floor__(self) 
实现了 math.floor(), 向下取整. 
__ceil__(self) 
实现了 math.ceil(), 向上取整. 
__trunc__(self) 
实现了 math.trunc(), 向 0 取整. 
算术运算符 
__add__(self, other) 
实现了加号运算. 
__sub__(self, other) 
实现了减号运算. 
__mul__(self, other) 
实现了乘法运算. 
__floordiv__(self, other) 
实现了//运算符. 
__div__(self, other) 
实现了/运算符. 该方法在 Python3 中废弃. 原因是 Python3 中，division 默认就是
true division. 
__truediv__(self, other) 
实现了 true division. 只有你声明了 from __future__ import division 该方法才会生效. 
__mod__(self, other) 
实现了%运算符, 取余运算. 
__divmod__(self, other) 
实现了 divmod()內建函数. 
__pow__(self, other) 
实现了**操作. N 次方操作. 
__lshift__(self, other) 
实现了位操作<<. 
__rshift__(self, other) 
实现了位操作>>. 
__and__(self, other) 
实现了位操作&. 
__or__(self, other) 
实现了位操作| 
__xor__(self, other) 
实现了位操作^ 
反算术运算符 
这里只需要解释一下概念即可。 假设针对 some_object 这个对象: 
some_object + other 
上面的代码非常正常地实现了 some_object 的__add__方法。那么如果遇到相反的
情况呢? 
other + some_object 
这时候，如果 other 没有定义__add__方法，但是 some_object 定义了__radd__, 那
么上面的代码照样可以运行。 这里的__radd__(self, other)就是__add__(self, other)的
反算术运算符。 
所以，类比的，我们就知道了更多的反算术运算符, 就不一一展开了: 
 
__rsub__(self, other) 
 
__rmul__(self, other) 
 
__rmul__(self, other) 
 
__rfloordiv__(self, other) 
 
__rdiv__(self, other) 
 
__rtruediv__(self, other) 
 
__rmod__(self, other) 
 
__rdivmod__(self, other) 
 
__rpow__(self, other) 
 
__rlshift__(self, other) 
 
__rrshift__(self, other) 
 
__rand__(self, other) 
 
__ror__(self, other) 
 
__rxor__(self, other) 
增量赋值 
这也是只要理解了概念就容易掌握的运算。举个例子: 
x = 5 
x += 1  # 这里的+=就是增量赋值，将 x+1 赋值给了 x 
因此对于 a += b, __iadd__ 将返回 a + b, 并赋值给 a。 所以很容易理解下面的魔
术方法了: 
 
__iadd__(self, other) 
 
__isub__(self, other) 
 
__imul__(self, other) 
 
__ifloordiv__(self, other) 
 
__idiv__(self, other) 
 
__itruediv__(self, other) 
 
__imod__(self, other) 
 
__ipow__(self, other) 
 
__ilshift__(self, other) 
 
__irshift__(self, other) 
 
__iand__(self, other) 
 
__ior__(self, other) 
 
__ixor__(self, other) 
类型转化 
__int__(self) 
实现了类型转化为 int 的行为. 
__long__(self) 
实现了类型转化为 long 的行为. 
__float__(self) 
实现了类型转化为 float 的行为. 
__complex__(self) 
实现了类型转化为 complex(复数, 也即 1+2j 这样的虚数)的行为. 
__oct__(self) 
实现了类型转化为八进制数的行为. 
__hex__(self) 
实现了类型转化为十六进制数的行为. 
__index__(self) 
在切片运算中将对象转化为 int, 因此该方法的返回值必须是 int。用一个例子来
解释这个用法。 
class Thing(object): 
    def __index__(self): 
        return 1 
 
thing = Thing() 
list_ = ['a', 'b', 'c'] 
print list_[thing]  # 'b' 
print list_[thing:thing]  # [] 
上面例子中, list_[thing]的表现跟 list_[1]一致，正是因为 Thing 实现了__index__方
法。 
可能有的人会想，list_[thing]为什么不是相当于 list_[int(thing)]呢? 通过实现 Thing
的__int__方法能否达到这个目的呢? 
显然不能。如果真的是这样的话，那么 list_[1.1:2.2]这样的写法也应该是通过的。 
而实际上，该写法会抛出 TypeError: slice indices must be integers or None or have an 
__index__ method 
下面我们再做个例子,如果对一个 dict 对象执行 dict_[thing]会怎么样呢? 
dict_ = {1: 'apple', 2: 'banana', 3: 'cat'} 
print dict_[thing]  # raise KeyError 
这个时候就不是调用__index__了。虽然 list 和 dict 都实现了__getitem__方法, 但
是它们的实现方式是不一样的。 如果希望上面例子能够正常执行, 需要实现
Thing 的__hash__ 和 __eq__方法. 
class Thing(object): 
    def __hash__(self): 
        return 1 
    def __eq__(self, other): 
        return hash(self) == hash(other) 
 
dict_ = {1: 'apple', 2: 'banana', 3: 'cat'} 
print dict_[thing]  # apple 
__coerce__(self, other) 
实现了混合模式运算。 
要了解这个方法,需要先了解 coerce()内建函数: 官方文档上的解释是, coerce(x, y)
返回一组数字类型的参数, 它们被转化为同一种类型，以便它们可以使用相同的
算术运算符进行操作。如果过程中转化失败，抛出 TypeError。 
比如对于 coerce(10, 10.1), 因为 10 和 10.1 在进行算术运算时，会先将 10 转为 10.0
再来运算。因此 coerce(10, 10.1)返回值是(10.0, 10.1). 
__coerce__在 Python3 中废弃了。 
其他魔术方法 
 
__unicode__(self) 
对实例使用 unicode()时调用。unicode()与 str()的区别在于: 前者返回值是 unicode, 
后者返回值是 str。unicode 和 str 都是 basestring 的子类。 
当你对一个类只定义了__str__但没定义__unicode__时,__unicode__会根据__str__
的返回值自动实现,即 return unicode(self.__str__()); 但返回来则不成立。 
class StrDemo2: 
    def __str__(self): 
        return 'StrDemo2' 
 
class StrDemo3: 
    def __unicode__(self): 
        return u'StrDemo3' 
 
demo2 = StrDemo2() 
print str(demo2)  # StrDemo2 
print unicode(demo2)  # StrDemo2 
 
demo3 = StrDemo3() 
print str(demo3)  # <__main__.StrDemo3 instance> 
print unicode(demo3)  # StrDemo3 
__format__(self, formatstr) 
"Hello, {0:abc}".format(a)等价于 format(a, "abc"), 等价于 a.__format__("abc")。 
这在需要格式化展示对象的时候非常有用，比如格式化时间对象。 
__hash__(self) 
对实例使用 hash()时调用, 返回值是数值类型。 
__nonzero__(self) 
对实例使用 bool()时调用, 返回 True 或者 False。 你可能会问, 为什么不是命名
为__bool__? 我也不知道。 我只知道该方法在 Python3 中改名为__bool__了。 
__dir__(self) 
对实例使用 dir()时调用。通常实现该方法是没必要的。 
__sizeof__(self) 
对实例使用 sys.getsizeof()时调用。返回对象的大小，单位是 bytes。 
__instancecheck__(self, instance) 
对实例调用 isinstance(instance, class)时调用。 返回值是布尔值。它会判断 instance
是否是该类的实例。 
__subclasscheck__(self, subclass) 
对实例使用 issubclass(subclass, class)时调用。返回值是布尔值。它会判断 subclass
否是该类的子类。 
__copy__(self) 
对实例使用 copy.copy()时调用。返回"浅复制"的对象。 
__deepcopy__(self, memodict={}) 
对实例使用 copy.deepcopy()时调用。返回"深复制"的对象。 
__call__(self, [args...]) 
该方法允许类的实例跟函数一样表现: 
class XClass: 
    def __call__(self, a, b): 
        return a + b 
 
def add(a, b): 
    return a + b 
 
x = XClass() 
print 'x(1, 2)', x(1, 2) 
print 'callable(x)', callable(x)  # True 
print 'add(1, 2)', add(1, 2) 
print 'callable(add)', callable(add)  # True 
Python3 中的差异 
 
Python3 中，str 与 unicode 的区别被废除了,因而__unicode__没有了，取而代之地
出现了__bytes__. 
 
Python3 中，division 默认就是 true division, 因而__div__废弃. 
 
__coerce__因存在冗余而废弃. 
 
__cmp__因存在冗余而废弃. 
 
__nonzero__改名为__bool__. 
字典运算 
在数据字典中执行一些计算操作（比如求最小值、最大值、排序等等） 
为了对字典值执行计算操作，通常需要使用 zip() 函数先将键和值反转过来。zip() 
函数方案通过将字典”反转”为 (值，键) 元组序列来解决了问题。当比较两个元
组的时候，值会先进行比较，然后才是键。这样的话你就能通过一条 简单的语
句就能很轻松的实现在字典上的求最值和排序操作了。 
prices = { 'ACME': 45.23, 'AAPL': 612.78, 'IBM': 205.55, 'HPQ': 37.20, 'FB': 10.75 } 
>>> [i for i in zip(prices.values(), prices.keys())] 
[(45.23, 'ACME'), (612.78, 'AAPL'), (205.55, 'IBM'), (37.2, 'HPQ'), (10.75, 'FB')] 
>>> min(zip(prices.values(), prices.keys())) 
(10.75, 'FB') 
>>> max(zip(prices.values(), prices.keys())) 
(612.78, 'AAPL') 
>>> sorted(zip(prices.values(), prices.keys())) 
[(10.75, 'FB'), (37.2, 'HPQ'), (45.23, 'ACME'), (205.55, 'IBM'), (612.78, 'AAPL')] 
注意：zip() 函数创建的是一个只能访问一次的迭代器。如下例子： 
>>> s = zip(prices.values(), prices.keys()) 
>>> min(s) 
(10.75, 'FB') 
>>> max(s) 
Traceback (most recent call last): 
  File "<stdin>", line 1, in <module> 
ValueError: max() arg is an empty sequence 
举例说明 zip()函数用法 
zip()函数在运算时，会以一个或多个序列（可迭代对象）做为参数，返回一个元
组的列表。同时将这些序列中并排的元素配对。 
zip()参数可以接受任何类型的序列，同时也可以有两个以上的参数;当传入参数
的长度不同时，zip 能自动以最短序列长度为准进行截取，获得元组。 
 
查找字典中的相同点（比如相同的键、相同的值等等） 
>>> a = { 
... 'x' : 1, 
... 'y' : 2, 
... 'z' : 3 
... } 
>>> b = { 
... 'w' : 10, 
... 'x' : 11, 
... 'y' : 2 
... } 
>>> a.keys() & b.keys() 
{'x', 'y'} 
>>> a.keys() - b.keys() 
{'z'} 
>>> a.items() & b.items() 
{('y', 2)} 
一个字典就是一个键集合与值集合的映射关系。字典的 keys() 方法返回一个展
现 键集合的键视图对象。键视图的一个很少被了解的特性就是它们也支持集合
操作，比如 集合并、交、差运算。 
字典的 items() 方法返回一个包含 (键，值) 对的元素视图对象。这个对象同样
也 支持集合操作，并且可以被用来查找两个字典有哪些相同的键值对。 
尽管字典的 values() 方法也是类似，但是它并不支持这里介绍的集合操作。 
使用 operator 模块的 itemgetter 函数，通过某个关键字排序一个字典列表 
>>> rows = [ 
... {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003}, 
... {'fname': 'David', 'lname': 'Beazley', 'uid': 1002}, 
... {'fname': 'John', 'lname': 'Cleese', 'uid': 1001}, 
... {'fname': 'Big', 'lname': 'Jones', 'uid': 1004} 
... ] 
>>> from operator import itemgetter 
>>> result = sorted(rows, key=itemgetter('fname')) 
>>> print(result) 
[{'fname': 'Big', 'lname': 'Jones', 'uid': 1004}, {'fname': 'Brian', 'lname': 'Jones', 'uid': 1003}, 
{'fname': 'David', 'lname': 'Beazley', 'uid': 1002}, {'fname': 'John', 'lname': 'Cl 
eese', 'uid': 1001}] 
itemgetter 也可以支持多个 keys，如 sorted(rows, itemgetter('fname','uid')) 
rows 被传递给接受一个关键字参数的 sorted() 内置函数。这个 参数是 callable 
类型，并且从 rows 中接受一个单一元素，然后返回被用来排序的值。 itemgetter() 
函数就是负责创建这个 callable 对象的。 operator.itemgetter() 函数有一个被 
rows 中的记录用来查找值的索引参数。可 以是一个字典键名称，一个整形值或
者任何能够传入一个对象的 __getitem__() 方法 的值。如果你传入多个索引参
数给 itemgetter() ，它生成的 callable 对象会返回一 个包含所有元素值的元组，
并且 sorted() 函数会根据这个元组中元素顺序去排序。但 你想要同时在几个字
段上面进行排序（比如通过姓和名来排序，也就是例子中的那样） 的时候这种
方法是很有用的。 
使用 itemgetter() 方式会运行的稍微快点。因此，如果 你对性能要求比较高的话
就使用 itemgetter() 方式。 
 
 
 
文件 
python 文件操作步骤：1. 打开；2. 读写；3. 关闭。 
第一次打开文件时，文件指针会指向文件的开始位置，当执行了 read 方法后，文
件指针会移动到读取内容的末尾。read 一次性全部读取到内存。 
file.readlines()----逐行读取，返回每行内容的列表； 
file.tell()----查找当前文件指针的位置并返回; 
file.seek(offset [, whence])----设置文件指针的位置并返回，whence 表明参考位置
0 表示文件开头，1 表示当前位置， 2 表示文件末尾，offset 表示按照参考位置
移动的字节; 
迭代文件对象，逐行读取；for line in file: 
 
文本文件与二进制文件。文本文件可以实用文本编辑器查看，二进制文件提供给
其他软件使用的文件，不能使用文本编辑器查看； 
 
python 单例模式； 
 
方法__new__()----为对象分配空间且返回对象引用。 
文件打开方式说明： 
 
文件/目录的常用管理操作： 
 
文件操作 
文件以什么方式编码的，就应该以什么方式解码； 
 
os.rename(current_file_name, new_file_name) 
 
os.remove(file_name) 
 
read 读取整个文件 
 
readline 读取下一行 
 
readlines 读取整个文件到一个迭代器以供我们遍历 

with open('somefile.txt', 'rt') as f:         # 使用带有 rt 模式的 open() 函数读取文本文件  

with open('somefile.txt', 'wt') as f:           # 使用带有 wt 模式的 open() 函数，如果之前文件内容存在则清除并覆盖掉。  # 如果是在已存在文件中添加内容，使用模式为 at 的 open() 函数。 

print(sys.getdefaultencoding()) # utf-8                 # 文件的读写操作默认使用系统编码，调用如下代码查看      
 
是关于换行符的识别问题，在 Unix 和 Windows 中是不一样的(分别是\n 和\r\n)。默认
情况下，Python 会以统一模式处理换行符。这种模式下，在读取文本的时候，Python 可
以识别所有的普通换行符并将其转换为单个\n 字符。类似的，在输出时会将换行符\n 转
换为系统默认的换行符。如果你不希望这种默认的处理方式，可以给 open()函数传入参
数 newline=''， 
with open('somefile.txt', 'rt', newline='') as f 
 
如果编码错误还是存在的话，你可以给 open() 函数传递一个可选的 errors 参数来处
理这些错误。 
open('sample.txt', 'rt', encoding='ascii', errors='replace') 
open('sample.txt', 'rt', encoding='ascii', errors='ignore') 
 
打印输出到文件中    有一点要注意的就是文件必须是以文本模式打开 
with open('d:/work/test.txt', 'wt') as f: 
    print('Hello World!', file=f) 
     
在 print() 函数中使用 sep 和 end 关键字参数，以你想要的方式输出 
print('ACME', 50, 91.5, sep=',', end='!!\n') 
 
使用模式为 rb 或 wb 的 open() 函数来读取或写入二进制数据 
with open('somefile.bin', 'wb') as f 
with open('somefile.bin', 'rb') as f: 
 
     
一个文件中写入数据，但前提必须是这个文件在文件系统上不存在。也就是不允许覆
盖已存在的文件内容。 
open() 函数中使用 x 模式来代替 w 模式的方法来解决这个问题, 
完美替代方案是： 
>>> import os 
>>> if not os.path.exists('somefile'): 
...     with open('somefile', 'wt') as f: 
...     f.write('Hello\n') 
... else: 
...     print('File already exists!') 
... 
File already exists! 
 
 
使用 io.StringIO() 和 io.BytesIO() 类来创建类文件对象操作字符串数据 
```python
def test_io(): 
    str_obj = io.StringIO() 
    str_obj.write('Hello World\n') 
    print(str_obj.getvalue()) # Get all of the data written so far 
    # # 将 This is a test_id 追加到文件对象 str_object 中 
    print('This is a test_io', file=str_obj) 
    print(str_obj.getvalue()) # Get all of the data written so far 
 
    str_obj1 = io.StringIO('dongxiang is s gaenius!') 
    print(str_obj1.read(4)) 
    print(str_obj1.read()) 
    byte_obj = io.BytesIO() 
    byte_obj.write(b'binary data') 
    print(byte_obj.getvalue()) 
 
# test_io() 
 
 
# output: 
# Hello World 
# 
# Hello World 
# This is a test_io 
# 
# dong 
# xiang is s gaenius! 
# b'binary data' 
 
 
# 读写一个 gzip 或 bz2 格式的压缩文件。 
# 以文本形式读取压缩文件 
with gzip.open('somefile.gz', 'rt') as f: 
    f.read() 
 
with bz2.open('somefile.bz2', 'rt') as f: 
    f.read() 
 
# 以文本形式写入压缩数据 
with gzip.open('somefile.gz', 'wt') as f: 
    f.write() 
 
with bz2.open('somefile.bz2', 'wt') as f: 
    f.write() 
 
 
# gzip.open() 和 bz2.open() 接受跟内置的 
# open() 函数一样的参数，包括 encoding，errors，newline 等等。 
 
# 它们可以作 
# 用在一个已存在并以二进制模式打开的文件上。比如，下面代码是可行的： 
f = open('somefile.gz', 'rb') 
with gzip.open(f, 'rt') as g: 
    text = g.read() 
     
     
# 读取固定大小的二进制数据 
from functools import partial 
 
# 如果文本文件，一行一行读取更普遍些 
with open('somefile.data', 'rt') as f: 
    f.read() 
 
 
# 如果是二进制文件，读取到固定大小的记录中更普遍 
RECODE_SIZE = 16 
with open('somefile.data', 'rb') as f: 
    records = iter(partial(f.read, RECODE_SIZE), b'') 
    for x in records: 
        print(x) 
# records 对象是一个可迭代对象，它会不断的产生固定大小的数据 
# 块，直到文件末尾。 
# 在例子中，functools.partial 用来创建一个每次被调用时从文件中读取固定数 
# 目字节的可调用对象。标记值 b'' 就是当到达文件结尾时的返回值。 
 
 
# iter() 函数有一个鲜为人知的特性就是，如果你给它传递一个可调用对象和一个标记值，它会创建一个迭代器。这个迭代器会一直调用传入的可调用对象直到它返回标记值为止，这时候迭代终止。 
 
 
# 获取文件名后缀名 
def get_extension_of_a_file2(file_name): 
    lis = file_name.split('.') 
    return lis[-1] 
 
# print(get_extension_of_a_file2('dong.xiang.java')) 
# output: java 

``` 
 
b. 判断文件路径是文件还是目录 
os.path.isfile(path) 
os.path.isdir(path) 
 
 
c. 获取文件的大小 
os.path.getsize("abc.txt") 
 
 
d. 获取文件所在的目录 
os.path.dirname(__file__) 
 
 
e. 判断文件或目录是否存在 
os.path.exists(file) 
 
 
f. 列出当前文件目录下的文件列表 
os.listdir(os.path.dirname(__file__))等价于 glob.glob("*") 
 
 
file 操作 
itertools.islice(file, n)----指的是获取文件的前 n 行，返回可迭代对象； 
seek()方法用于移动文件读取指针到指定位置--fileObject.seek(offset[, whence]) 
    offset--开始的偏移量，也就是代表需要移动偏移的字节数 
    whence：可选，默认值为 0。给 offset 参数一个定义，表示要从哪个位置开始偏移； 
            0 代表从文件开头开始算起，1 代表从当前位置开始算起，2 代表从文件末
尾算起。 
    由于程序中使用 seek()时，使用了非 0 的偏移量，因此文件的打开方式中必须包含
b，否则就会报 io.UnsupportedOperation 错误 
tell()方法返回文件的当前位置，即文件指针当前位置。 
f.readlines()----直接返回一个列表，每行+\n 是一个元素； 
f.read()----表示将文件所有内容读取出来，返回一个 str; 
f.write(string)----将 string 写入文件，打开文件写入，如果文件之前有内容，会清除掉，
新写入； 
shutil.copyfile(one_file, another_file)----将一个文件的内容拷贝到另一个文件； 
shutil.copy(src, dst)----# Copy src to dst. (cp src dst) 
shutil.copy2(src, dst)----# Copy files, but preserve metadata (cp -p src dst) 
shutil.copytree(src, dst)----# Copy directory tree (cp -R src dst) 
shutil.move(src, dst)----# Move src to dst (mv src dst) 
shutil.unpack_archive('Python-3.3.0.tgz')----解压文件； 
shutil.make_archive('py33','zip','Python-3.3.0')----将目录压缩成 py33.zip 文件； 
with open(f1) as f, open(f2) as g----可以使用 with 同时打开两个文件； 
f.read().splitlines()----返回一个列表，分裂每行； 
random.choice(original_list)----从一个列表中随机取出一个元素； 
f.closed()----返回文件是否关闭，True or False; 
string.ascii_uppercase----'ABCDEFGHIJKLMNOPQRSTUVWXYZ'; 
 
csv 
import csv 
with open('test.csv', 'w') as new_file: 
    filewriter = csv.writer(new_file): 
        for x in lis: 
            for key, value in x.items(): 
                filewriter.writerow([key, value]) 
 
Python 中的反射了解么? 
反射就是通过字符串的形式，导入模块；通过字符串的形式，去模块寻找指定函
数，并执行。利用字符串的形式去对象（模块）中操作（查找/获取/删除/添加）
成员，一种基于字符串的事件驱动！ 
自省就是面向对象的语言所写的程序在运行时,所能知道对象的类型.简单一句就
是运行时能够获得对象的类型.比如 type()，dir()，getattr()，hasattr()，isinstance()。 
getattr,hasattr,setattr,delattr 对模块的修改都在内存中进行，并不会影响文件中真
实内容。并且他们的入参可以是模块名； 
getattr(）---入参可是是模块名 
1. getattr()函数是 Python 自省的核心函数，具体使用大体如下： 
class A:  
def __init__(self):  
self.name = 'zhangjing' 
#self.age='24' 
def method(self):  
print"method print" 
   
Instance = A()  
print getattr(Instance , 'name, 'not find') #如果 Instance 对象中有属性 name 则打印 self.name
的值，否则打印'not find' 
print getattr(Instance , 'age', 'not find') #如果 Instance 对象中有属性 age 则打印 self.age 的
值，否则打印'not find' 
print getattr(a, 'method', 'default') #如果有方法 method，否则打印其地址，否则打印 default  
print getattr(a, 'method', 'default')() #如果有方法 method，运行函数并打印 None 否则打印
default  
 
2. hasattr(object, name) 
说明：判断对象 object 是否包含名为 name 的特性（hasattr 是通过调用 getattr(ojbect, name)
是否抛出异常来实现的） 
 
3. setattr(object, name, value) 
这是相对应的 getattr()。参数是一个对象,一个字符串和一个任意值。字符串可能会列出
一个现有的属性或一个新的属性。这个函数将值赋给属性的。该对象允许它提供。例
如,setattr(x,“foobar”,123)相当于 x.foobar = 123。 
 
4. delattr(object, name) 
与 setattr()相关的一组函数。参数是由一个对象(记住 python 中一切皆是对象)和一个字
符串组成的。string 参数必须是对象属性名之一。该函数删除该 obj 的一个由 string 指定
的属性。delattr(x, 'foobar')=del x.foobar 
 
 
 
 
面向对象--类 
类以及类中的方法在内存中只有一份，而根据类创建的每一个对象都在内存中需
要存一份。 
只要创建一个对象,其内存空间就会保存一份。  
 
 
普通字段属于对象 
 
静态字段属于类 
 
静态字段在内存中只保存一份 
 
普通字段在每个对象中都要保存一份 
class foo: 
 
    country = '中国'  # 静态字段,保存在类中，仅此一份 
    def __init__(self,name): 
 
        self.name = name  # 普通字段,封装到对象中 
 
    def show(self): 
        print(self.name) 
 
 
obj = foo("xiaoxiao") 
print(obj.name)  # 通过对象直接访问普通字段 
print(foo.country)  # 通过类名直接访问静态字段 
print(obj.country)  # 对象间接访问静态字段 
print(foo.name)  #报错, 
类中方法,除了类调用类中普通方法不一样,其余的全部都可以通过类或者对象来
调用。 
由属性的定义和调用要注意一下几点： 
 
定义时，在普通方法的基础上添加 @property 装饰器； 
 
定义时，属性仅有一个 self 参数 
 
调用时，无需括号 方法：foo_obj.func() 属性：foo_obj.prop 
属性存在意义是：访问属性时可以制造出和访问字段完全相同的假象 
Python 的属性的功能是：属性内部进行一系列的逻辑计算，最终将计算结果返回。 
property 的构造方法中有个四个参数 
 
第一个参数是方法名，调用 对象.属性 时自动触发执行方法 
 
第二个参数是方法名，调用 对象.属性 ＝ XXX 时自动触发执行方法 
 
第三个参数是方法名，调用 del 对象.属性 时自动触发执行方法 
 
第四个参数是字符串，调用 对象.属性.__doc__ ，此参数是该属性的描述信息 
```python
class Foo1(object): 
    def get_bar(self): 
        return 'dong is a genius' 
 
    def set_bar(self, name): 
        return '%s is a genius, yes, you are right.' % name 
 
    def del_bar(self): 
        return 'dong is a foolish man' 
 
    BAR = property(get_bar, set_bar, del_bar, 'description dong...') 
 
 
# 执行结果 
# foo = Foo1() 
# print(foo.BAR)  # 自动调用第一个参数中定义的方法：get_bar 
# foo.BAR = 'shane'  # 自动调用第二个参数中定义的方法：set_bar 方法，并将“shane”当作参数传入 
# print(foo.BAR) 
# del foo.BAR  # 自动调用第三个参数中定义的方法：del_bar 方法 
# print(foo.BAR.__doc__)  # 自动获取第四个参数中设置的值：description dong...
```
 
1.凡是类中的方法和函数，都是绑定给对象使用的； 
2.绑定方法都有自动传值的功能。传递进去的值，就是对象本身。 
3.如果类想调用绑定方法，就必须遵循函数的参数规则，有几个参数，就必须传
递几个参数。 
当对象在调用类的绑定方法时，也会默认把类当作参数传递进去 
类中方法默认都是绑定给对象使用，当对象调用绑定方法时，会自动将对象作为
第一个参数传递进去；而类来调用，则必须遵循函数参数一一对应的规则，有几
个参数，就必须传递几个参数。如果一个方法是用了@classmethod 装饰器，那么
這个方法绑定到类身上，不管是对象来调用还是类调用，都会将类作为第一个参
数传递进去。 
只要你使用关键字 class，Python 解释器在执行的时候就会创建一个对象。下面的代码段： 
>>> class ObjectCreator(object): 
...       pass 
...  
  将在内存中创建一个对象，名字就是 ObjectCreator。这个对象（类）自身拥
有创建对象（类实例）的能力，而这就是为什么它是一个类的原因。但是，它的
本质仍然是一个对象，于是乎你可以对它做如下的操作： 
  1) 你可以将它赋值给一个变量 
  2) 你可以拷贝它 
  3) 你可以为它增加属性 
  4) 你可以将它作为函数参数进行传递 


## <a name='-1'></a>封装 
 属性和方法封装到一个抽象的类中，外界使用类创建对象，然后让对象调用方法，对象方法的细节都被封装到类的内部；一个对象的属性可以是另外一个类创建的对象； 
 在对象方法的内部，是可以直接访问对象的属性；
 同一个类创建的多个对象之间，它们的属性是互不干扰的；
 身份运算符：用于比较两个对象的内存地址是否一致----是否是同一对象的引用；Python 中针对 None 的比较时建议使用 None 判断；is 用于判断两个变量引用对象是否是同一个，==判断引用变量的值是否相等。
 内部属性或方法(_internal)：任何以单下划线 _ 开头的名字都应该是内部实现。 同样适用于模块名和模块级别函数。 
 私有属性或方法(__private)：不希望外部访问的属性或方法，前面加上两个下划线，外界和子类都不能访问，使用双下划线开始会导致访问名称变成其他形式。 
```python
class A(object): 
    def __init__(self): 
        self._internal = 0 
        self.public = 1 
        self.__private_param = 100 
 
    def public_method(self): 
        print(self.__private_method) 
        print(self.__private_param) 
 
    def _internal(self): 
        pass 
 
    def __private_method(self): 
        pass 
     
class B(A): 
    def __init__(self): 
        super(B, self).__init__() 
        self.__private_param = 100 
 
    def __private_method(self): 
        pass 

a = A() 
a.public_method() 
print(a.__dict__) 
print(a._A__private_param) 
print(dir(a))    # '_A__private_method', '_A__private_param','_internal', 'public', 'public_method'  

b = B() 
print(dir(b))    # '_internal', 'public', 'public_method','_A__private_method', '_A__private_param', '_B__private_method', '_B__private_param', 
 

# 内部属性或方法就是：_internal 和_internal_method 
# 私 有 属 性 或 方 法 就 是 : _A_private_method ， _A_private_method ， 外 部 引 用就是a._A__private_param 
# 可以看到类 B 继承自 A，并在 B 中定义属性__private_param 和方法__private_method，
# 这个 B 中新定义的，并不是重写，而 B 中也继承了 A 中的私有属性和方法，也就是说python 这样命名私有属性这样重命名目的就是继承——这种属性通过继承是无法被覆盖的。 
 
# 使用下换线作为后缀可以避免与保留关键字冲突。 
# 如果想访问属性可以通过属性的 getter（访问器）和 setter（修改器）方法进行对应的操作。如果要做到这点，就可以考虑使用@property 包装器来包装 getter 和setter 方法，使得对属性的访问既安全又方便. 
```


## <a name='-1'></a>继承 
 继承用来实现代码的重用；子类拥有父类所有的属性和方法，可以直接使用父类的中封装好的方法，不需要再次开发；子类继承父类，子类的实例可以拥有父类的属性； 
 继承传递性； 
 重写父类方法： 
o 覆盖父类中的方法： 
实现方式就相当于在子类中定义一个和父类同名的方法并实现；运行时，只会调用子类中重写的方法，不会调用父类中封装的方法； 
o 对父类方法进行扩展： 
这种情况的意思是说，父类封装的方法实现是子类的一部分，具体实现方式就是在子类重写的父类方法体中，在需要的位置使用 super().父类方法来调用父类方法的执行，代码的其它位置编写子类特有的实现。 
o 多继承时，避免父类之间存在同名的属性或方法。如果一个类有多个父类时，子类调用方法的顺序是先子类，之后安装类名(A,B,C)中，先搜索 A，之后 B，再者C，最后 object 类，找到最后一个类没有这个方法的话，程序报错。Python 中提供一个内置属性__mro__可以查看方法的搜索顺序，MRO 是 method resolution order，主要用于多继承时判断方法属性的调用顺序。 

python 解释器中有一个 C3 算法，来计算通过 super()调用父类的一个顺序，C3 算法的结论的体现保存到 MRO 中去，一个类中调用 super()的时候，拿着当前类的名字，到 MRO 元组中匹配，匹配成功，就调用匹配到的下一个类，上图的
Grandson()调用顺序是 Son1-->Son2-->Parent-->object o 新式类，旧式类： 
新式类----以 object 为基类的类，旧式类----不以 object 为基类的类 
super 用法 
super().__setter(name, value     没有显式的指明某个类的父类，super()仍然可以有效的工作。 
 
super().parent_function()----调用父类方法； 
super().__init__()----在__init__()方法中确保父类被正确初始化； 
super.__setter__(key, value)----调用原生的方法； 
抽象基类 
python 通过引入模块 abc 提供官方的解决方案，这个模块为抽象基类提供了支
持。 


## <a name='-1'></a>多态 
 不同的子类对象调用相同的父类方法，产生不同的执行结果； 
 以继承和重写父类方法为前提； 
 类： 
o 创建对象的动作有两步：
1. 在内存中为对象分配空间，调用__new__()方法完成；
2. 调用初始化方法__init__为对象初始化； 
o Python 中一切皆对象，class A:定义的类属于类对象； 
o 程序运行时，类同样会被加载到内存，类是一个特殊的对象---类对象，类对象在内存够中只有一份； 
o 类属性或类方法： 
 类属性----给类对象定义的属性； 
 Python 中属性获取存在一个向上查找机制；对象调用属性时，如 person01.count，
1. 首先在对象内部查找对象的属性；2. 没有找到就会向上寻找类属性；
访问类属性的两种方式：1. 类名.类属性名；2. 对象.类属性。 
 如果使用对象.类属性 = 值赋值语句时，只会给对象添加一个属性，不会影响类属性的值； 
 类方法----修饰器@classmethod 标识； 
 静态方法----修饰器@staticmethod 标识； 

### <a name='-1'></a>元类 
元类就是用来创建这些类（对象）的，元类就是类的类。 
MyClass = type('MyClass', (), {}) 
type 实际上是一个元类，type 就是 python 在背后用来创建所有类的元类。 
 

#### <a name='-1'></a>元类控制器实例的创建 
Python 是一种动态语言，而动态语言和静态语言最大的不同，就是函数和类不是编译时定义的，而是运行时动态创建的。class 的定义是运行时动态创建的，而创建 class 的方法就是使用 type()函数 
定义：type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）) 
type()函数既可以返回一个对象的类型，又可以创建出新的类型 
metaclass 允许你创建类或者修改类。换句话说，你可以把类看成是 metaclass 创建出来的“实例”。 
一个类定义时 metaclass=定义的元类，元类中定义__call__()方法，当这个类实例化时会调用元类的__call__方法，再调用自己的__init__方法； 

单例模式：metaclass 的实现： 
class Singleton(type): 
    def __init__(self, *args, **kwargs): 
        self._instance = None 
        super().__init__(*args, **kwargs) 
 
    def __call__(self, *args, **kwargs): 
        if not self._instance: 
            self._instance = super().__call__(*args, **kwargs) 
        return self._instance 
 
 
class OnePerson(metaclass=Singleton): 
    def __init__(self): 
        print('create OnePerson instance') 
 
缓存实例： 
class Cached(type): 
    def __init__(self, *args, **kwargs): 
        super().__init__(*args, **kwargs) 
        self.__cached = weakref.WeakValueDictionary() 
 
    def __call__(self, *args): 
        if args in self.__cached: 
            return self.__cached[args] 
        else: 
            obj = super().__call__(*args) 
            self.__cached[args] = obj 
            return obj 
 
 
class CachedInstance(metaclass=Cached): 
    def __init__(self, name): 
        print('create cached instance({!r})'.format(name)) 
        self.name = name 
 
一个基本元类通常是继承 type 并重定义它的__new__()方法或者是__init__()方法； 
比如定义__new__()方法： 
class MyMeta(type): 
    def __new__(self, clsname, bases, clsdict): 
 
clsname is name of class being defined, bases is tuple of base classes, clsdict is 
class dictionary  
    return super().__new__(cls, clsname, bases, clsdict) 
另一种定义__init__()方法， 
class MyMeta(type):
    def __init__(self, clsname, bases, clsdict):
        super().__init__(clsname, bases, clsdict) 
为了使用这个元类，你通常要将它放到到一个顶级父类定义中，然后其他的类继承 
这个顶级父类。例如： 
class Root(metaclass=MyMeta):
    pass 
class A(Root):
    pass 
class B(Root):
    pass 
 
元类的一个关键特点是它允许你在定义的时候检查类的内容。在重新定义 init()方法中， 
你可以很轻松的检查类字典、父类等等。并且，一旦某个元类被指定给了某个类，那么
就会被继承到所有子类中去。 
 
使用 type.new_class()初始化新的类对象。 
你需要做的只是提供类的名字、父类元组、关键字参数，以及一个用成员变量填充类字
典的回调函数。 
一个类的定义如下： 
class Spam(Base, debug=True, typecheck=False):
    pass 
那么可以将其翻译成如下的 new class() 调用形式： 
Spam = types.new_class('Spam', (Base,),{'debug': True, 'typecheck': False},lambda ns: 
ns.update(cls_dict)) 
new class() 第四个参数最神秘，它是一个用来接受类命名空间的映射对象的函数。 
通常这是一个普通的字典，但是它实际上是 prepare () 方法返回的任意对象。 
 
另一种构造类对象： 
Stock = collections.nametuple('Stock', ['name', 'shares', 'price']) 
 
 
定义接口和抽象基类 
继承 metaclass=abc.ABCMeta，方法加@abstractmethod 
抽象类的定义就是让别的类继承它并实现特定的抽象方法，抽象类不能被实例化； 
python2 和 python3 定义元类的方式 
在用 class 语句自定义类时，默认 metaclass 是 type，我们也可以指定 metaclass 来创
建类。 由于 python3 和 python2 在指定类的 metaclass 语法不兼容，下面分别示例 
python2 和 python3 两个版本。 
python2 版本： 
class Bar(object): 
    __metaclass__ = MetaClass 
 
    def __init__(self, name): 
        self.name = name 
 
    def print_name(): 
        print(self.name) 
 
python3 版本： 
class Bar(object, metaclass=MetaClass): 
    def __init__(self, name): 
        self.name = name 
 
    def print_name(): 
        print(self.name) 
 
## <a name='-1'></a>正则表达式 
一是：检测某一段字符串是否符合规则，也就是我们常说的"校验"  
二是：从一大段字符串中找到符合规则的字符串，可以理解为"检索" 
正则表达式模块 
正则表达式模块标记：re.A 或 re.ASCII  
re.I 或 re.IGNORECASE 忽略大小写 
re.M 或 re.MULTILINE 使^在起始处并在每个换行符后匹配，使$在结尾处但在
每个换行符之前匹配  
re.S 或 re.DOTALL 使.匹配每个字符，包括换行符 
re.X 或 re.VERBOSE 使空白与注释包含在匹配中 
正则表达式模块的函数：  
re.compile(r,f) 返回编译后的正则表达式 r，如果指定，就将其标记设置为 f(即上
边的 re.A 等，且可同时有多个标记，用|分隔)；通常用作预编译，接下来重复使
用时不需再编译，返回 pattern 对象；然后 pattern 对象可以调用 search 方法，
pattern.search(string)----获得根据 pattern 搜索的字符串； 
re.escape(s) 返回字符串 s，其中所有非字母数字的字符都使用反斜线进行了转义
处理，因此，返回的字符串中没有特殊的正则表达式字符；  
re.findall(r,s,f) 返回正则表达式 r 在字符串 s 中所有非交叠的匹配（如果给定 f,就
受其制约）。如果正则表达式中有捕获，那么每次匹配都作为一个捕获元组返回； 
re.finditer(r,s,f) 对正则表达式 r 在字符串 s 中每个非交叠的匹配（如果给定了 f，
就受其制约），都返回一个匹配对象；  
re.match(r,s,f) 如果正则表达式 r 在字符串 s 的起始处匹配（如果给定 f，就受其
制约），就返回一个匹配对象(MatchObject)，否则返回 None;  
re.search(r,s,f) 如果正则表达式 r 在字符串 s 的任意位置匹配（如果给定 f,就受其
制约），就返回一个匹配对象，否则返回 None;  
re.split(r,s,m) 返回分割字符串 s(在正则表达式 r 每次出现处进行分割）所产生的
字符串的列表，至多分割 m 次（如果没有给定 m,就尽可能多的分割），如果正
则表达式中包含捕获，就被包含在分割的部分之间；  
>>> s="info:xiaoZhang 33 shandong" 
>>> import re 
>>> res = re.split(r':| ', s) 
>>> res 
['info', 'xiaoZhang', '33', 'shandong'] 
 
re.sub(r,x,s,m) 对正则表达式 r 的每次匹配（如果给定 m，那么至多 m 次），返
回字符串 s 的一个副本，并将其替换为 x--这可以是一个字符串，也可以是一个
函数；  
re.subn(r,x,s,m) 与 re.sub()函数相同，区别在于此函数返回一个二元组； 
 
贪婪模式和惰性模式 
在正则中，默认的匹配模式是“贪婪匹配”，也就是说，在符合匹配规则的前提下尽可能
多的去匹配字符，但是有些时候，我们不需要匹配太多的内容，只要得到需要的“片段”
内容就好了。 
而量词是匹配多次的，这时我们可以在量词的后面加上?就可以让匹配“适可而止”了，这
样的匹配规则就是惰性匹配 
这里举一个贪婪匹配与惰性匹配对比的例子： 
有字符串"aasdasdasdxxxxxxasdasd"，现在想匹配到 x 结束 
（1）用"惰性匹配"的话： 
 
语法：a.*?x 
 
说明：遇见一个先检测是不是 x，如果是的话就停止，不是的话就匹配 
 
结果：aasdasdasdx 
（2）默认的"贪婪匹配"： 
 
语法：a.*x 
 
说明：这里会匹配后面所有的 x 
 
结果：aasdasdasdxxxxxx 
 
贪婪模式： 
定义：正则表达式去匹配时，会尽量多的匹配符合条件的内容 
标识符：+，?，*，{n}，{n,}，{n,m} 
匹配时，如果遇到上述标识符，代表是贪婪匹配，会尽可能多的去匹配内容 
 
非贪婪模式： 
定义：正则表达式去匹配时，会尽量少的匹配符合条件的内容 也就是说，一旦发现匹
配符合要求，立马就匹配成功，而不会继续匹配下去(除非有 g，开启下一组匹配) 
标识符：+?，??，*?，{n}?，{n,}?，{n,m}? 
可以看到，非贪婪模式的标识符很有规律，就是贪婪模式的标识符后面加上一个? 
 
为了加深印象，看下下面的分别： 
<a name='PythonHTMLtag..'></a>用 Python 匹配 HTML tag 的时候，<.> 和 <.?> 有什么区别 
第一个代表贪心匹配，第二个代表非贪心； 
?在一般正则表达式里的语法是指的"零次或一次匹配左边的字符或表达式"相当于{0,1} 
而当?后缀于*,+,?,{n},{n,},{n,m}之后，则代表非贪心匹配模式，也就是说，尽可能少的
匹配左边的字符或表达式，这里是尽可能少的匹配.(任意字符) 
所以：第一种写法是，尽可能多的匹配，就是匹配到的字符串尽量长，第二中写法是尽
可能少的匹配，就是匹配到的字符串尽量短。 
比如<tag>tag>tag>end，第一个会匹配<tag>tag>tag>,第二个会匹配<tag>。 
re 模块实现正则的功能简单举例 
re 模块实现正则的功能  
re.match(正则表达式,要匹配的字符串，[匹配模式]) 
要匹配的字符串为 str1 = "Python's features"  
正则表达式 r'(.*)on(.*?) .*' 
r 表示后面的字符串是一个普通字符串（比如\n 会译为\和 n，而不是换行符） 
()符号包住的数据为要提取的数据，通常与.group()函数连用。 
.匹配单个任意字符 
*匹配前一个字符出现 0 次或无限次 
?匹配前一个字符出现 0 次或 1 次 
(.*)提取的数据为 str1 字符串中 on 左边的所有字符，即 Pyth 
(.*?)提取的数据为 str1 中 on 右边，空格前面，即's 
 
.group(0)输出的是匹配正则表达式整体结果 
.group(1) 列出第一个括号匹配部分 
.group(2) 列出第二个括号匹配部分 
 
re.match 与 re.search 的区别 
re.match 只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数
返回 None；而 re.search 匹配整个字符串，直到找到一个匹配。 
```python
import re
line = "Cats are smarter than dogs" 
matchObj = re.match(r'dogs', line, re.M | re.I) 
if matchObj: 
    print("match --> matchObj.group() : ", matchObj.group()) 
else: 
    print("No match!!") 
 
matchObj = re.search(r'dogs', line, re.M | re.I) 
if matchObj: 
    print("search --> searchObj.group() : ", matchObj.group()) 
else: 
    print("No match!!") 
# 输出 
# No match!! 
# search --> searchObj.group() :  dogs 

``` 
 
正则表达式中的特殊字符 
正则表达式中的特殊字符： 
$ 
匹配输入字符串的结尾位置。如果设置了 RegExp 对象的 Multiline 属性，则 
$ 也匹配 '\n' 或 '\r'。要匹配 $ 字符本身，请使用\$。 
( ) 
标记一个子表达式的开始和结束位置。子表达式可以获取供以后使用。要匹配
这些字符，请使用 \( 和 \)。 
* 
匹配前面的子表达式零次或多次。要匹配 * 字符，请使用 \*。例如，zo* 能
匹配 "z" 以及 "zoo"。* 等价于{0,}。 
+ 
匹配前面的子表达式一次或多次。要匹配 + 字符，请使用 \+。例如，'zo+' 能
匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。 
. 
匹配除换行符 \n 之外的任何单字符。要匹配 . ，请使用 \. 。 
[ 
标记一个中括号表达式的开始。要匹配 [，请使用 \[。 
? 
匹配前面的子表达式零次或一次，或指明一个非贪婪限定符。要匹配 ? 字符，
请使用 \?。例如，"do(es)?" 可以匹配 "do" 、 "does" 中的 "does" 、 "doxy" 中的 
"do" 。? 等价于 {0,1}。 
\ 
将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。
例如， 'n' 匹配字符 'n'。'\n' 匹配换行符。序列 '\\' 匹配 "\"，而 '\(' 则匹配 "("。 
^ 
匹配输入字符串的开始位置，除非在方括号表达式中使用，当该符号在方括号
表达式中使用时，表示不接受该方括号表达式中的字符集合。要匹配 ^ 字符本身，请使
用 \^。 
{ 
标记限定符表达式的开始。要匹配 {，请使用 \{。 
| 
指明两项之间的一个选择。要匹配 |，请使用 \|。 
{n} 
n 是一个非负整数。匹配确定的 n 次。例如，'o{2}' 不能匹配 "Bob" 中的 'o'，
但是能匹配 "food" 中的两个 o。 
{n,} 
n 是一个非负整数。至少匹配 n 次。例如，'o{2,}' 不能匹配 "Bob" 中的 'o'，
但能匹配 "foooood" 中的所有 o。'o{1,}' 等价于 'o+'。'o{0,}' 则等价于 'o*'。 
{n,m} 
m 和 n 均为非负整数，其中 n <= m。最少匹配 n 次且最多匹配 m 次。例
如，"o{1,3}" 将匹配 "fooooood" 中的前三个 o。'o{0,1}' 等价于 'o?'。请注意在逗号和
两个数之间不能有空格。 
[ABC] 
匹配 [...] 中的所有字符，例如 [aeiou] 匹配字符串 "google runoob taobao" 中
所有的 e o u a 字母。 
[^ABC] 匹配除了 [...] 中字符的所有字符，例如 [^aeiou] 匹配字符串 "google runoob 
taobao" 中除了 e o u a 字母的所有字母。 
[A-Z] 
[A-Z] 表示一个区间，匹配所有大写字母，[a-z] 表示所有小写字母。 
[\s\S] 
匹配所有。\s 是匹配所有空白符，包括换行，\S 非空白符，包括换行。 
\w 
匹配字母、数字、下划线。等价于 [A-Za-z0-9_] 
 
非打印字符也可以是正则表达式的组成部分。下表列出了表示非打印字符的转义序列： 
\cx 
匹配由 x 指明的控制字符。例如， \cM 匹配一个 Control-M 或回车符。x 的
值必须为 A-Z 或 a-z 之一。否则，将 c 视为一个原义的 'c' 字符。 
\f 
匹配一个换页符。等价于 \x0c 和 \cL。 
\n 
匹配一个换行符。等价于 \x0a 和 \cJ。 
\r 
匹配一个回车符。等价于 \x0d 和 \cM。 
\s 
匹配任何空白字符，包括空格、制表符、换页符等等。等价于 [ \f\n\r\t\v]。注
意 Unicode 正则表达式会匹配全角空格符。 
\S 
匹配任何非空白字符。等价于 [^ \f\n\r\t\v]。 
\t 
匹配一个制表符。等价于 \x09 和 \cI。 
\v 
匹配一个垂直制表符。等价于 \x0b 和 \cK。 
 
 
多线程、多进程、协程 
 
多任务： 
简单地说，就是操作系统可以同时执行多个任务。CPU 执行代码都是顺序执行
的，也就是说同一时间片 CPU 只能执行一个任务。单核 CPU 也可以执行多任务
----这是由于操作系统是轮流让多个任务交替执行，任务 A 执行 0.01 秒，切换到
任务 B 执行 0.01 秒，之后切换到任务 C 执行 0.01 秒，表面上看每个任务都是交
替执行的，但是 CPU 执行速度较快，感觉就像所有任务是同时在执行一样； 
 
并发：就是上述单核 CPU 执行多个任务的场景，就是同一时间片只能执行一个
任务，最主要判断方式就是 CPU 的数量小于任务的数量，一定是并发； 
 
并行：就是多个任务真的是在同时执行。 
用上课来举例就是，并发情况下是一个老师在同一时间段辅助不同的人功课。并
行则是好几个老师分别同时辅助多个学生功课。简而言之就是一个人同时吃三个
馒头还是三个人同时分别吃一个的情况，吃一个馒头算一个任务。 
全局锁全局解释锁(GIL),python 线程被限制到同一时刻只允许一个线程执行这样
的一个执行模型；实际上，解释器被一个全局解释器锁保护着，它确保任何时候
都只有一个 Python 线程执行。GIL 最大的问题就是 Python 的多线程程序并不能
利用多核 CPU 的优势（比如一个使用了多个线程的计算密集型程序只会在一个
单 CPU 上面运行）。 
<1> 多线程 
 
threading： 
thread01 = threading.Thread(target=函数名)----创建线程； 
可以通过继承 threading.Thread 类完成创建线程，实现 run()方法； 
threading.enumerate()----列出当前存活的线程对象，返回一个 list； 
多线程共享全局变量，多线程共享全局变量的问题（如下图显示的 demo）----为
什么结果不是 2000000 呢，这是由于 number += 1 不是原子性的操作，这个代码
可以分为几步：1. 内存获取 numbe 的值；2. number+1；3. number 写入内存；也
就是说可能的情况是线程 test01 执行完后 number=1 还没写入内存，线程 test02
取到值是 0，可能两个线程执行完后 number 的值才会变成 1，cpu 同一时间执行
执行一段代码，因此当执行次数多时，产生的混乱概率越大。 
从开销的角度来讲，开进程的开销要远远大于开线程的开销，因为开进程涉及到
空间的创建，而开启线程仅仅是在既有的空间的基础上进行的；其次，同一个进
程的各个线程之间是共享资源的，而不同的进程之间的资源是不共享的。 
对于多线程来说:，我们全局定义的 n=100，而经过子线程 t1 的处理，n 的值不管
是在主线程还是子线程都变成了 0。 
 
死锁：在线程间共享多个资源的时候，如果两个线程分别占用一部分资源并且同
时等待对方的资源，就会造成死锁； 
 
target=----指定这个线程将来去哪个函数去执行代码； 
args=----指定将来调用函数的时候传递什么数据过去；格式：args=(var, ) 
 

 
#### <a name='-1'></a>多线程同步方式 
```python
import os
import threading
# 找到当前线程的名字，用到 getName 方法 
from threading import current_thread   # 需要引入 current_thread 类 
print(current_thread().getName())   
 
# os 模块的 getpid 方法，获取线程 id： 
print(os.getpid())

from threading import active_count, enumerate 
print(active_count())    # 查看当前存活的线程的数量  
print(enumerate())       # 查 看 当 前 存活 的线 程 名字 ,[<_MainThread(MainThread, started 5940)>] 

def worker(): 
    print('os.get_pid==%s' % os.getpid()) 
    print('the thread is working...') 
    print(current_thread().getName, 'is in worker') 
    print(current_thread().is_alive()) 
     
thread1 = threading.Thread(target=worker, name='thread1') 
thread1.start() 
thread1.join() 
print(thread1.getName())    
print(thread1.is_alive()) 
thread1.setName('thread1_to_update') 
print(thread1.getName())   
# os.get_pid==9972 
# the thread is working... 
# thread1 is in worker 
# True 
# thread1 
# False 
# thread1_to_update 

``` 
 
 
#### <a name='threading.Event'></a>threading.Event
Event 对象包含一个可由线程设置的信号标志，它允许线程等待某些事件的发生。
在初始情况下，event 对象中的信号标志被设置为假。如果有线程等待一个 event 
对象，而 这个 event 对象的标志为假，那么这个线程将会被一直阻塞直至该标志
为真。一个线程 如果将一个 event 对象的信号标志设置为真，它将唤醒所有等待
这个 event 对象的线程。如果一个线程等待一个已经被设置为真的 event 对象，
那么它将忽略这个事件，继续执行。event 对象的一个重要特点是当它被设置为
真时会唤醒所有等待它的线程。 
判断一个线程是否已经启动 threading.Event 中 set()方法将 flag 置为 True,clear()
置为 False,wait()阻塞直到 flag 置为 True； 
一般用法： 
threading.Event().wait()---主线程等待着，那边线程结束将次标志置为真，即
threading.Event().set(); 

 
#### <a name='threading.Condition'></a>threading.Condition 
如果你只想唤醒单个线程，最好是使用信号量或者 Condition 对象来替代 Event。 
上下文管理器上使用 Condition: 
   
with threading.Condition(): 
    threading.Condition().wait()        # 等待 

with threading.Condition(): 
    threading.Condition().notify_all()   # 唤醒  
 
 
 
#### <a name='threading.Semaphore'></a>threading.Semaphore信号量 
threading.Semaphore(0).acquire()        # 线程等待获取信号量
threading.Semaphore(0).release()       # 信号量被释放  
 

#### <a name='threading.Lock'></a>threading.Lock
Lock 对象和 with 语句块一起使用可以保证互斥执行，就是每次只有一个线程
可 以执行 with 语句包含的代码块。with 语句会在这个代码块执行前自动获取
锁，在执行结束后自动释放锁。 
with threading.Lock(): 
    do something

Lock 对象和 with 语句块一起使用可以保证互斥执行，就是每次只有一个线程可以执行 with 语句包含的代码块。with 语句会在这个代码块执行前自动获取锁，在执行结束后自动释放锁。
from threading import Lock 

class ShareCounter(): 
    def __init__(self, init_num): 
        self._init_num = init_num 
        self._lock = Lock() 
 
    def incr(self, delta=1): 
        with self._lock: 
            self._init_num += delta 
 
    def decr(self, delta): 
        with self._lock: 
            self._init_num -= delta 
线程调度本质上是不确定的，因此，在多线程程序中错误地使用锁机制可能会导致随机数据损坏或者其他的异常行为，我们称之为竞争条件。为了避免竞争条
件，最好只在临界区（对临界资源进行操作的那部分代码）使用锁。 
“互斥锁”是将代码的“局部变成串行。互斥锁的意思就是互相排斥，如果把多个进程比喻为多个人，互斥锁的工作原理就是多个人都要去争抢同一个资源：卫生间，一个人抢到卫生间后上一把锁，其他人都要等着，等到这个完成任务后释放锁，其他人才有可能有一个抢到......所以互斥锁的原理，就是把并发改成串行， 降低了效率，但保证了数据安全不错乱。 
import json 
import time 
from multiprocessing import Process, Lock 
 
 
def remain(ticket_name, ticket_buyer): 
    time.sleep(1) 
    with open('tickets.json', 'r', encoding='utf-8') as f: 
        tickets_dict = json.load(f) 
        tickets_remain = tickets_dict.get(ticket_name) 
        if tickets_remain <= 0: 
            print('<%s>查看%s 余票不足...' % (ticket_buyer, ticket_name)) 
        else: 
            print('<%s>查看 remain tickets numbers==%s' % (ticket_buyer, 
                                                        tickets_remain)) 
 
def buy_tickets(ticket_name, ticket_buyer): 
    time.sleep(1) 
    with open('tickets.json', 'r', encoding='utf-8') as f: 
        tickets_dict = json.load(f) 
        tickets_remain = tickets_dict.get(ticket_name) 
    tickets_remain -= 1 
    with open('tickets.json', 'w', encoding='utf-8') as f: 
        if tickets_remain >= 0: 
            tickets_dict = {'%s' % ticket_name : tickets_remain} 
            json.dump(tickets_dict, f, ensure_ascii=False) 
            print('<%s>购买到票' % ticket_buyer) 
        else: 
            tickets_dict = {'%s' % ticket_name: 0} 
            json.dump(tickets_dict, f, ensure_ascii=False) 
            print('<%s>未购买到票' % ticket_buyer) 
 
 
def task(ticket_name, ticket_buyer, mutex): 
    remain(ticket_name, ticket_buyer) 
    mutex.acquire() 
    buy_tickets(ticket_name, ticket_buyer) 
    mutex.release() 
 
 
if __name__ == '__main__': 
    mutex = Lock() 
    for i in range(4): 
        p = Process(target=task, args=('速度与激情 7', '学生%s' % (i + 1), mutex)) 
        p.start() 
         
 
    # with open('tickets.json', 'w', encoding='utf-8') as f: 
    #     json.dump({'速度与激情 7': 4}, f, ensure_ascii=False) 
    # file_new = open('tickets.json', 'r', encoding='utf-8') 
    # print(file_new.read()) 
    # file_new.close() 
 
tickets.json  ---- {"速度与激情 7": 3} 
 
output: 
<学生 1>查看 remain tickets numbers==3 
<学生 3>查看 remain tickets numbers==3 
<学生 2>查看 remain tickets numbers==3 
<学生 4>查看 remain tickets numbers==3 
<学生 1>购买到票 
<学生 3>购买到票 
<学生 2>购买到票 
<学生 4>未购买到票 
 
 
 
#### <a name='threading.RLock'></a>threading.RLock 
可重入锁 
一个 RLock （可重入锁）可以被同一个线程多次获取，主要用来实现基于监测对象模式的锁定和同步。在使用这种锁的情况下，当锁被持有时，只有一个线程可以使用完整的函数或者类中的方法。 
 

#### <a name='-1'></a>线程池 
创建一个线程池，用来相应客户端请求或者执行其它的工作； 
concurrent.furtures 函数库中有一个 ThreadPoolExecutor 类可以达到这种效果； 
pool = ThreadPoolExecutor(128) 
pool.submit(func, args) 



### <a name='-1'></a>多进程 
一个程序运行起来后，代码+用到的资源称之为进程，它是操作系统分配资源的最小单元。 
python 的多进程的模块是 multiprocessing，创建进程的方式和线程类似，multiprocessing.Process(target=, args=())，另一个种方式就是继承 Processing，重写run 方法，创建实例 start； 
start 仅仅是向操作系统发出信号，具体谁先执行不一定，由操作系统决定。在主进程运行过程中如果想并发地执行其他的任务，我们可以开启子进程，此时主进程的任务与子进程的任务分两种情况 
情况一：在主进程的任务与子进程的任务彼此独立的情况下，主进程的任务先执行完毕后，主进程还需要等待子进程执行完毕，然后统一回收资源。 
情况二：如果主进程的任务在执行到某一个阶段时，需要等待子进程执行完毕后才能继续执行，就需要有一种机制能够让主进程检测子进程是否运行完毕，在子进程执行完毕后才继续执行，否则一直在原地阻塞，这就是 join 方法的作用。 

#### <a name='Queue'></a>进程间通信 Queue 
使用 multiprocessing 模块中的 Queue 实现多进程之间的数据传递，Queue 本身是一个消息队列程序。 
from multiprocessing import Queue
from multiprocessing import Manager 
queue = Manager().Queue() 


#### <a name='Pool'></a>进程池 Pool 
from multiprocessing import Pool 

pool = multiprocessing.Pool(3)，3 表明进程池中初始化 3 个进程，pool.apply_async(worker)
表明进程池中进程执行任务，每次循环将会用空闲出来的子进程去调用目标； 
pool.close() # 关闭进程池，关闭后进程池将不再接受请求 
pool.join()   # 等待 pool 的子进程执行完成，必须放在 pool.close()之后； 


def show_video_stats(options): 
    pool = Pool(8) 
    video_page_urls = get_video_page_urls() 
    results = pool.map(get_video_data, video_page_urls) 
multiprocessing.Pool 类开启了 8 个工作进程等待分配任务运行。 

调用 pool.map()类似于调用常规的 map()，它将会对第二个参数指定的迭代变量中的每个元素调用一次第一个参数指定的函数。最大的不同是，它将发送这些给进程池所拥有的进程运行，所以在这个例子中八个任务将会并行运行。 

concurrent.futures.ProcessPoolExecutor    # 计算密集型函数
简单用法： 
with ProcessPoolExecutor() as pool: 
    do work in parallel using pool 
其原理是，一个 ProcessPoolExecutor 创建 N 个独立的 Python 解释器，N 是系统上面可用 CPU 的个数。 
你可以通过提供可选参数给 ProcessPoolExecutor(N)来修改处理器数量。
这个处理池会一直运行到 with 块中最后一个语句执行完成，然后处理池被关闭。
不过，程序会一直等待直到所有提交的工作被处理完成。被提交到池中的工作必须被定义为一个函数。 
有两种方法去提交。如果你想让一个列表推导或一个 map()操作并行执行的话，可使用pool.map(): 
 
 
#### <a name='daemonjoin'></a>daemon 和 join 的区别 
守护进程(daemon) 
主进程创建守护进程 
其一：守护进程会在主进程代码执行结束后就终止 
其二：守护进程内无法开启子进程，否则抛出异常：AssertionError: daemonic processes are not allowed to have children 
注意：进程之间是相互独立的，主进程的代码运行结束，守护进程随即终止 p.join([timeout]): 主进程等待相应的子进程 p 终止（强调：是主线程处于等的状态，而 p 是处于运行的状态）。需要强调的是 p.join()只能 join 住 start 开始的进程，而不能 join住 run 开启的进程 
 
 

### <a name='-1'></a>协程 
协程实现多任务，意思就是说任务完成或者等待某些条件时，可以在此期间完成其它的任务，也就说一个线程等待某些条件，可以充分利用这段时间去完成其它事情，这就是协程。 
生成器替换线程 yield 语句会让它的生成器挂起它的执行，这样可以编写一个调度器，将生成器当作某种任务并使用任务协作切换来替换它们执行； 
def task01(): 
    while True: 
        print("----------task01---------") 
        time.sleep(0.1) 
        yield 
def task02(): 
    while True: 
        print("-----------task02---------") 
        yield 
 
def test_coroutine(): 
    t01 = task01() 
    t02 = task02() 
    while True: 
        next(t01) 
        next(t02) 
         
     
greenlet 
为了更好地使用协程来完成任务，python 中的 greenlet 模块对其进行封装，从而
使得切换任务变得更加简单。 
from greenlet import greenlet 
import time 
 
 
def func1(): 
    while True: 
        print("---A---") 
        gr2.switch() 
        time.sleep(0.5) 
 
 
def func2(): 
    while True: 
        print('---B---') 
        gr1.switch() 
        time.sleep(0.5) 
 
 
gr1 = greenlet(func1) 
gr2 = greenlet(func2) 
 
gr1.switch() 
gevent 
更强大的并且能够自动切换任务的模块---gevent 
其原理是当一个 gevent 遇到 IO(指的是 input output 输入输出，比如网络，文件
操作等)操作时，比如访问网络，就自动切换到其他的 gevent，等到 IO 操作完成，
再在适当的时候切换回来继续执行。 
import gevent 
 
 
def func1(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        gevent.sleep(0.5)   # gevent 只有遇到 sleep()的时候才会切换任务 
 
 
def func2(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        gevent.sleep(0.5) 
 
 
def func3(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        gevent.sleep(0.5) 
 
 
g1 = gevent.spawn(func1, 5) 
g2 = gevent.spawn(func2, 5) 
g3 = gevent.spawn(func3, 5) 
 
g1.join() 
g2.join() 
g3.join() 
 
# <Greenlet at 0x1da0fa898c0: func1(5)> 0 
# <Greenlet at 0x1da0fa89ae0: func2(5)> 0 
# <Greenlet at 0x1da0fa899d0: func3(5)> 0 
# <Greenlet at 0x1da0fa898c0: func1(5)> 1 
# <Greenlet at 0x1da0fa89ae0: func2(5)> 1 
# <Greenlet at 0x1da0fa899d0: func3(5)> 1 
# <Greenlet at 0x1da0fa898c0: func1(5)> 2 
# <Greenlet at 0x1da0fa89ae0: func2(5)> 2 
# <Greenlet at 0x1da0fa899d0: func3(5)> 2 
# <Greenlet at 0x1da0fa898c0: func1(5)> 3 
# <Greenlet at 0x1da0fa89ae0: func2(5)> 3 
# <Greenlet at 0x1da0fa899d0: func3(5)> 3 
# <Greenlet at 0x1da0fa898c0: func1(5)> 4 
# <Greenlet at 0x1da0fa89ae0: func2(5)> 4 
# <Greenlet at 0x1da0fa899d0: func3(5)> 4 
 
 
以上代码等同于如下： 
from gevent import monkey 
import gevent 
import time 

gevent.monkey.patch_all()  # 将程序中用到的耗时操作代码，换位 gevent 中自己实现的代码 
 
 
def func1(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        time.sleep(0.5)    
 
 
def func2(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        time.sleep(0.5) 
 
 
def func3(n): 
    for i in range(n): 
        print(gevent.getcurrent(), i) 
        time.sleep(0.5) 
 
 
g1 = gevent.spawn(func1, 5) 
g2 = gevent.spawn(func2, 5) 
g3 = gevent.spawn(func3, 5) 
 
g1.join() 
g2.join() 
g3.join() 
 
以上 6 行代码可以用如下代替： 
gevent.joinall([ 
    gevent.spawn(func1, 5), 
    gevent.spawn(func2, 5), 
    gevent.spawn(func3, 5), 
]) 
 
 
 
#### <a name='asyncio'></a>asyncio 
asyncio 是 Python 语言内置的异步编程库，它提供了一种协程（coroutine）基础架构，使得开发者能够编写高效的异步代码。
asyncio 的核心是事件循环（event loop）、协程和任务（task）。 
事件循环负责运行异步任务，协程是异步任务的执行单元，任务则是协程的高层封装，包含协程的状态信息以及其他必要属性。通过 async/await 语法，开发者可以编写类似于同步代码的异步程序。当遇到 IO 操作等阻塞调用时，协程会主动
挂起并切换到其他协程，直到 IO 操作完成后再切换回来继续执行。这样就避免了线程切换时的资源浪费和上下文切换带来的额外消耗。 
同步，是指操作一个接一个地执行，下一个操作必须等上一个操作执行完成之后才能开始执行； 
异步，是指不同操作间可以相互交替执行，如果其中地某个操作被堵塞，程序并不会等待，而是会找出可执行的操作继续执行。 
 
什么是 Asyncio? 
Asyncio 和其他 Python 程序一样，是单线程的，它只有一个主线程，但可以进行多个不
同的任务。这里的任务，指的就是特殊的 future 对象，我们可以把它类比成多线程版本
里的多个线程。 
这些不同的任务，被一个叫做事件循环（Event Loop）的对象所控制。所谓事件循环，
是指主线程每次将执行序列中的任务清空后，就去事件队列中检查是否有等待执行的任
务，如果有则每次取出一个推到执行序列中执行，这个过程是循环往复的。 
 
为了简化讲解这个问题，可以假设任务只有两个状态：分别是预备状态和等待状态： 
预备状态是指任务目前空闲，但随时待命准备运行； 
等待状态是指任务已经运行，但正在等待外部的操作完成，比如 I/O 操作。 
 
在这种情况下，事件循环会维护两个任务列表，分别对应这两种状态，并且选取预备状
态的一个任务（具体选取哪个任务，和其等待的时间长短、占用的资源等等相关）使其
运行，一直到这个任务把控制权交还给事件循环为止。 
 
当任务把控制权交还给事件循环对象时，它会根据其是否完成把任务放到预备或等待状
态的列表，然后遍历等待状态列表的任务，查看他们是否完成：如果完成，则将其放到
预备状态的列表；反之，则继续放在等待状态的列表。而原先在预备状态列表的任务位
置仍旧不变，因为它们还未运行。 
 
这样，当所有任务被重新放置在合适的列表后，新一轮的循环又开始了，事件循环对象
继续从预备状态的列表中选取一个任务使其执行…如此周而复始，直到所有任务完成。 
 
值得一提的是，对于 Asyncio 来说，它的任务在运行时不会被外部的一些因素打断，因
此 Asyncio 内的操作不会出现竞争资源（多个线程同时使用同一资源）的情况，也就不
需要担心线程安全的问题了。 
 
 
 
event_loop 事件循环 
程序开启一个无限的循环，程序员会把一些函数注册到事件循环上。当满足事件发生的
时候，调用相应的协程函数。 
 
coroutine 协程 
协程对象，指一个使用 async 关键字定义的函数，它的调用不会立即执行函数，而是会
返回一个协程对象。协程对象需要注册到事件循环，由事件循环调用。 
def test_define_coroutine(): 
    async def do_some_work(x): 
        print('waiting: {}'.format(x)) 
 
    now = lambda: time.time() 
 
    start = now() 
    coroutine = do_some_work(4) 
    loop = asyncio.get_event_loop() 
    loop.run_until_complete(coroutine) 
    print('Time lost: %s' % (now() - start)) 
 
 
task 任务 
一个协程对象就是一个原生可以挂起的函数，任务则是对协程进一步封装，其中包含任
务 的 各 种 状 态 。 协 程 对 象 不 能 直 接 运 行 ， 在 注 册 事 件 循 环 的 时 候 ， 其 实 是
run_until_complete 方法将协程包装成为了一个任务（task）对象。所谓 task 对象是 Future
类的子类。保存了协程运行后的状态，用于未来获取协程的结果。 
```python
def test_task(): 
    async def do_some_work(x): 
        print('waiting: {}'.format(x)) 
 
    now = lambda :time.perf_counter() 
    start = now() 
    coroutine = do_some_work(4) 
    loop = asyncio.get_event_loop() 
    # task = asyncio.ensure_future(coroutine) 
    task = loop.create_task(coroutine) 
    print(task) 
    loop.run_until_complete(task) 
    print(task) 
    print('Time lost: %s' % (now() - start)) 
# <Task pending name='Task-1' coro=<test_task.<locals>.do_some_work() running at F:/projects/python/test_coroutine/test_asyncio.py:52>> 
# waiting: 4 
# <Task finished name='Task-1' coro=<test_task.<locals>.do_some_work() done, defined at F:/projects/python/test_coroutine/test_asyncio.py:52> result=None> 
# Time lost: 0.0006648999999999683 
 
# 创建 task 后，task 在加入事件循环之前是 pending 状态，因为 do_some_work 中没有耗时的阻塞操作，task 很快就执行完毕了。后面打印的 finished 状态。 
# asyncio.ensure_future(coroutine) 和 loop.create_task(coroutine)都可以创建一个 task，run_until_complete 的参数是一个 futrue 对象。当传入一个协程，其内部会自动封装成task，task 是 Future 的子类。isinstance(task, asyncio.Future)将会输出 True。
# task 的 result,run_until_complete()执行后，task.result()可以获取协程 return 结果; 

```
 
 
future 代表将来执行或没有执行的任务的结果。它和 task 上没有本质的区别 
 
 
async/await 关键字 python3.5 用于定义协程的关键字，async 定义一个协程，await 用于挂起阻塞的异步调用接口。 
Async 和 await 关键字是 Asyncio 的最新写法，表示这个语句（函数）是非阻塞的，
正好对应前面所讲的事件循环的概念，即如果任务执行的过程需要等待，则将其放入等待状态的列表中，然后继续执行预备状态列表里的任务; 
使用 async 可以定义协程对象，使用 await 可以针对耗时的操作进行挂起，就像生成器里的 yield 一样，函数让出控制权。协程遇到 await，事件循环将会挂起该协程，执行
别的协程，直到其他的协程也挂起或者执行完毕，再进行下一个协程的执行。 
```python
def test_await(): 
    async def do_some_work(x): 
        print('Waiting: {}'.format(x)) 
        await asyncio.sleep(x) 
        return "Done after {}s".format(x) 
 
    now = lambda: time.perf_counter() 
    start = now() 
    coroutine = do_some_work(4) 
    loop = asyncio.get_event_loop() 
    task = loop.create_task(coroutine) 
    loop.run_until_complete(task) 
    print('Task ret: ', task.result()) 
    print('Time lost: %s' % (now() - start)) 
     
# output: 
# Waiting: 4 
# Task ret:  Done after 4s 
# Time lost: 4.00298 
 
```
 
 
asyncio.ensure_future(coro) 表示对输入的协程 coro 创建一个任务，安排它的执行，并返回此任务对象。 
asyncio.gather() 表示在事件循环对象中运行 aws 序列的所有任务。 
 
asyncio 实现并发 
def test_concurrent_using_asyncio(): 
    async def do_some_work(x): 
        print('Waiting: {}'.format(x)) 
        await asyncio.sleep(x) 
        return "Done after {}s".format(x) 
 
    now = lambda: time.perf_counter() 
    start = now() 
    coroutine1 = do_some_work(1) 
    coroutine2 = do_some_work(2) 
    coroutine3 = do_some_work(4) 
    tasks = [asyncio.ensure_future(coroutine1), 
             asyncio.ensure_future(coroutine2), 
             asyncio.ensure_future(coroutine3)] 
    loop = asyncio.get_event_loop() 
    loop.run_until_complete(asyncio.wait(tasks)) 
    for task in tasks: 
        print("Task ret: ", task.result()) 
    print('Time lost: %s' % (now() - start)) 
  
output: 
Waiting: 1 
Waiting: 2 
Waiting: 4 
Task ret:  Done after 1s 
Task ret:  Done after 2s 
Task ret:  Done after 4s 
Time lost: 4.0084599999999995 
asyncio.wait(tasks)也可以使用 asyncio.gather(*tasks),前者接受一个 task 列表，后者接收一堆 task。 
 
 
 
 
### <a name='queue'></a>queue 
队列是将数据存到内存中处理，这就满足了“效率高”这个要求，另外，队列是基
于“管道+锁”设计的，所以另外一点也满足了。事实上，队列才是进程间通信（IPC）
的最佳选择！ 
另外需要大家注意的是：队列是一种先进先出的数据结构。 
```python
def producer(queue): 
    for i in range(4): 
        res = '商品%s' % (i + 1) 
        time.sleep(0.5) 
        print('生产者%s 生产%s' % (os.getpid(), res)) 
        queue.put(res) 
 
 
def consumer(queue): 
    while True: 
        res = queue.get() 
        if not res: 
            break 
        time.sleep(1) 
        print('消费者%s 消费了%s' % (os.getpid(), res)) 
 
# 执行代码 
#     q = Queue() 
#     p1 = Process(target=producer, args=(q, ), name='p1') 
#     p2 = Process(target=producer, args=(q, ), name='p2') 
#     c1 = Process(target=consumer, args=(q, ), name='c1') 
#     c2 = Process(target=consumer, args=(q, ), name='c2') 
#     p1.start() 
#     p2.start() 
#     c1.start() 
#     c2.start() 
#     p1.join() 
#     p2.join() 
    # 以下 put 两个 None 是为了让两个消费者停止消费，跳出循环 
    # q.put(None) 
    # q.put(None) 
output: 
生产者 15292 生产商品 1 
生产者 13788 生产商品 1 
生产者 15292 生产商品 2 
生产者 13788 生产商品 2 
消费者 15120 消费了商品 1 
生产者 15292 生产商品 3 
消费者 13816 消费了商品 1 
生产者 13788 生产商品 3 
生产者 15292 生产商品 4 
生产者 13788 生产商品 4 
消费者 15120 消费了商品 2 
消费者 13816 消费了商品 2 
消费者 15120 消费了商品 3 
消费者 13816 消费了商品 3 
消费者 15120 消费了商品 4 
消费者 13816 消费了商品 4
``` 
 
创建队列----queue.Queue(),此对象不可迭代，要迭代使用 queue.Queue().queue； 
队列的元素个数----queue.Queue().qsize(); 
判断队列是否为空----queue.Queue().empty(); 
往 queue.Queue()中 put 元素时是 insert 队列的后面; 
往 queue.LifoQueue()中 put 元素是 insert 队列的前面； 
 
 
 
 
解释一下什么是锁，有哪几种锁？ 
锁(Lock)是 python 提供的对线程控制的对象。有互斥锁，可重入锁，死锁。 


### <a name='-1'></a>死锁
在两个或者多个并发进程中，每个进程持有某种资源而又等待其它进程释放它们现在保持着的资源，在未改变这种状态之前都不能向前推进，称这一组进程产生了死锁(deadlock)。通俗的讲就是两个或多个进程无限期的阻塞、相互等待的一种状态。 

死锁产生的必要条件？
互斥：一个资源一次只能被一个进程使用； 
占有并等待：一个进程至少占有一个资源，并在等待另一个被其它进程占用的资源； 
非抢占：已经分配给一个进程的资源不能被强制性抢占，只能由进程完成任务之后自愿释放； 
循环等待：若干进程之间形成一种头尾相接的环形等待资源关系，该环路中的每个进程都在等待下一个进程所占有的资源。 

死锁有哪些处理方法？
鸵鸟策略 
直接忽略死锁。因为解决死锁问题的代价很高，因此鸵鸟策略这种不采取任务措施的方案会获得更高的性能。当发生死锁时不会对用户造成多大影响，或发生死锁的概率很低，可以采用鸵鸟策略。 

死锁预防 
基本思想是破坏形成死锁的四个必要条件： 
破坏互斥条件：允许某些资源同时被多个进程访问。但是有些资源本身并不具有
这种属性，因此这种方案实用性有限； 
破坏占有并等待条件：实行资源预先分配策略（当一个进程开始运行之前，必须
一次性向系统申请它所需要的全部资源，否则不运行）；或者只允许进程在没有
占用资源的时候才能申请资源（申请资源前先释放占有的资源）； 
缺点：很多时候无法预知一个进程所需的全部资源；同时，会降低资源利用率，
降低系统的并发性； 
破坏非抢占条件：允许进程强行抢占被其它进程占有的资源。会降低系统性能；破坏循环等待条件：对所有资源统一编号，所有进程对资源的请求必须按照序号递增的顺序提出，即只有占有了编号较小的资源才能申请编号较大的资源。这样避免了占有大号资源的进程去申请小号资源。 
死锁避免 
动态地检测资源分配状态，以确保系统处于安全状态，只有处于安全状态时才会
进行资源的分配。所谓安全状态是指：即使所有进程突然请求需要的所有资源，
也能存在某种对进程的资源分配顺序，使得每一个进程运行完毕。 
死锁解除 
如何检测死锁：检测有向图是否存在环；或者使用类似死锁避免的检测算法。 
死锁解除的方法 
利用抢占：挂起某些进程，并抢占它的资源。但应防止某些进程被长时间挂起而
处于饥饿状态； 
利用回滚：让某些进程回退到足以解除死锁的地步，进程回退时自愿释放资源。
要求系统保持进程的历史信息，设置还原点； 
利用杀死进程：强制杀死某些进程直到死锁解除为止，可以按照优先级进行。





多线程交互访问数据，如果访问到了就不访问了？ 
怎么避免重读？
创建一个已访问数据列表，用于存储已经访问过的数据，并加上互斥锁，在多线程访问数据的时候先查看数据是否在已访问的列表中，若已存在就直接跳过。 


什么是线程安全，什么是互斥锁？
每个对象都对应于一个可称为互斥锁的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。 
同一进程中的多线程之间是共享系统资源的，多个线程同时对一个对象进行操作，一个线程操作尚未结束，另一线程已经对其进行操作，导致最终结果出现错误，此时需要对被操作对象添加互斥锁，保证每个线程对该对象的操作都得到正确的结果。 



说说下面几个概念：同步，异步，阻塞，非阻塞？ 
同步： 多个任务之间有先后顺序执行，一个执行完下个才能执行。 
异步： 多个任务之间没有先后顺序，可以同时执行，有时候一个任务可能要在
必要的时候获取另一个同时执行的任务的结果，这个就叫回调！ 
阻塞： 如果卡住了调用者，调用者不能继续往下执行，就是说调用者阻塞了。 
非阻塞： 如果不会卡住，可以继续执行，就是说非阻塞的。 
同步异步相对于多任务而言，阻塞非阻塞相对于代码执行而言。 



什么是僵尸进程和孤儿进程？怎么避免僵尸进程？
ok 
孤儿进程： 父进程退出，子进程还在运行的这些子进程都是孤儿进程，孤儿进
程将被 init 进程（进程号为 1）所收养，并由 init 进程对他们完成状态收集工作。 
僵尸进程：进程使用 fork 创建子进程，如果子进程退出，而父进程并没有调用
wait 获 waitpid 获取子进程的状态信息，那么子进程的进程描述符仍然保存在系
统中的这些进程是僵尸进程。 
避免僵尸进程的方法： 
1.fork 两次用孙子进程去完成子进程的任务 
2.用 wait()函数使父进程阻塞 
3.使用信号量，在 signal handler 中调用 waitpid,这样父进程不用阻塞 
 
 
 
 
 
线程是并发还是并行，进程是并发还是并行？ 
线程是并发，进程是并行; 
进程之间互相独立，是系统分配资源的最小单位，同一个线程中的所有线程共享
资源。 
 
 
 
并行(parallel)和并发（concurrency)? 
并行：同一时刻多个任务同时在运行 
并发：不会在同一时刻同时运行，存在交替执行的情况。 
实现并行的库有：multiprocessing 
实现并发的库有: threading 
程序需要执行较多的读写、请求和回复任务的需要大量的 IO 操作，IO 密集型操
作使用并发更好。 
CPU 运算量大的程序，使用并行会更好 
 
 
 
IO 密集型和 CPU 密集型区别？ 
IO 密集型： 系统运行，大部分的状况是 CPU 在等 I/O（硬盘/内存）的读/写 
CPU 密集型： 大部分时间用来做计算，逻辑判断等 CPU 动作的程序称之 CPU
密集型。 
 
 
 
python asyncio 的原理？ 
asyncio 这个库就是使用 python 的 yield 这个可以打断保存当前函数的上下文的
机制， 封装好了 selector 摆脱掉了复杂的回调关系


简述乐观锁和悲观锁 
悲观锁, 就是很悲观，每次去拿数据的时候都认为别人会修改，所以每次在拿数
据的时候都会上锁，这样别人想拿这个数据就会 block 直到它拿到锁。传统的关
系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，
都是在做操作之前先上锁。 
乐观锁，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，
但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版
本号等机制，乐观锁适用于多读的应用类型，这样可以提高吞吐量。 
生产者消费者模型的应用场景 
说明 
生产者只在仓库未满时进行生产，仓库满时生产者进程被阻塞；消费者只在仓库
非空时进行消费，仓库为空时消费者进程被阻塞； 
应用场景：处理数据比较消耗时间，线程独占，生产数据不需要即时的反馈等。
比如说写入日志，将多线程产生的日志放在队列中，然后写入。 
python 模块 
就好比工具包，模块中定义的全局变量、函数、类都是提供给外界直接使用的工
具；一个独立的 python 文件就是一个模块，在导入文件时，文件中所有没有任
何缩进的代码都会被执行一遍。 
random---产生随机数； 
requests---网络请求； 
functools---functools.reduce()函数会对参数序列中元素进行累积；functool 中的
wraps 在类中定义装饰器，并将其作用在其它函数和方法上；如果需要减少某个函
数的参数个数，可以使用 functools.partial()； 
collections---collections.Counter 序列的元素出现频率；collections.deque 创建双向
链表； 
pkg_resources---获取本地安装的所有模块； 
argparse---解析命令行参数； 
json---解析 json 格式内容； 
contextlib---实现上下文管理器； 
 
导入模块 import 
 
导入模块，每个导入都应该独占一行； 
 
模块别名应该符合大驼峰命名法，如 DogModule； 
 
import 模块名----一次性把模块中的所有工具全部导入； 
 
from...import...----导入部分工具； 
import common 
from common import num_list 
# 比如 num_list 是 common 中的全局变量， 
# 在块中使用变量 num_list，common.num_list.append(1)  ----这个修改会使 common 模
块中的 num_list 增加元素 1； 
#  from common import num_list 导入到本模块中，要是修改的话，不会同步到 common
模块了； 
 
如果两个模块，存在同名的函数，后导入的模块的函数会覆盖掉先导入的函数； 
 
模块的搜索顺序： 
Python 解释器在导入模块时，1. 先搜索当前目录制定模块名的文件，如果有就
直接导入；2. 如果没有再搜索系统目录 sys.path； 
Python 中每个模块都有一个内置属性__file__查看模块的完整路径。 
 
程序执行时添加新的模块路径： 
sys.path.append('/home/plugins') 
sys.path.insert(0, '/home/plugins') 
 
重新导入模块 
重新加载已经加载的模块 
使用 imp 模块，imp.reload(module)----reload()擦除了模块底层字典的内容，并通过重新
执行模块的源代码来刷新它。 
模块对象本身的身份保持不变。因此，该操作在程序中所有已经被导入了的地方更新了
模块。也就是说再使用这个 reload 之前这个模块已经被导入过了。 
 
 
 
 
name 属性： 
内置属性，记录着一个字符串； 
如果是被其它文件导入的，__name__就是模块名；如果是当前执行的程序，
__name__就是"main"。 
 
包： 
包含多个模块的特殊目录，目录下有一个特殊的文件 init.py，包名命名方式：小
写字母+“_“； 
import 包名可以一次性导入包中的所有模块； 
init.py 中是用来指定对外界暴露的模块列表。格式：from . import 模块名 
 
发布模块： 
步骤：1. 创建 setup.py，用来添加一些描述信息,放置在和模块路径一样的位置： 
2. 构建模块：python setup.py build 
3. 生成发布压缩版本：python setup.py sdist，产生一个压缩文件； 
4. 安 装 模 块 ： 解 压 ， 执 行 python setup.py install 进 行 安 装 ， 安 装 路 径 在
/usr/local/lib/python/dist-packages/  
__init__.py 用法 
文件__init__.py 的目的是要包含不同运行级别的包的可选的初始化代码。 
举个例子 
如果你执行了语句 import graphics，文件 graphics/init.py 将被导入, 建立 graphics 命名
空间的内容。 
像 import graphics.format.jpg 这样导入， 
文 件
graphics/init.py
和 文 件
graphics/graphics/formats/init.py
将 在 文 件
graphics/formats/jpg.py 导入之前导入。 
init.py 能够用来自动加载子模块: 
# graphics/formats/__init__.py 
from . import jpg 
from . import png 
像 这 样 一 个 文 件 , 用 户 可 以 仅 仅 通 过 import grahpics.formats 来 代 替 import 
graphics.formats.jpg 以及 import graphics.formats.png。 
<1> random 
# random 模块使用 
import random 
# 生成一个范围内的随机数 
print(random.randrange(1, 10))   # 左开右闭,不包括 10 
print(random.randrange(1, 20, 3))   # 1-20 之间，步长为 3 的数字 
print(random.randint(1, 10))   # 1-10 之间的整数，包括 10 
print(random.choice('dongxiang1243'))   # 在一个字符串中间选择一个 
print(random.choice((1, 2, 34, '34')))   # 返回一个给定数据集合的随意数据 
print(random.sample('shanedfgv2335', 4))   # 返回给定数量的数据集合的列表 
print(random.sample([2, 34, '34', 3], 4))   # 返回给定数量的数据集合的列表 
 
 
 
###### **15. python 中生成随机整数、随机小数、0--1 之间小数方法** 
随机整数：random.randint(a,b),生成区间内的整数 
随机小数：习惯用 numpy 库，利用 np.random.randn(5)生成 5 个随机小数 
0-1 随机小数：random.random(),括号中不传参 
 
 
# 打乱数据集合 
l = [3, 4, 5, 6, 7] 
random.shuffle(l) 
print(l)  
 
 
<2> requests 
在 requests 模块中，requests.content 和 requests.text 什么区别 
.content 中间存的是字节码 .text 存的是.content 编码后的字符串 
操作方式就是，如果想取得文本就用.text，如果想获取图片，就用.content 
<3> glob 
glob.glob(pathname) 
返回所有匹配的文件路径列表。它只有一个参数 pathname，定义了文件路径匹配规则，
这里可以是绝对路径，也可以是相对路径。 
 
# 获取当前程序所在路径下的以.py 扩展结尾的文件列表 
files = glob.glob('*.py') 
 
 
# 获得一个目录下的文件； 
files = glob.glob(dir + filenames) 
<4> functools 
functools.reduce() 
reduce() 函数会对参数序列中元素进行累积。 
函数将一个数据集合（链表，元组等）中的所有数据进行下列操作： 
用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作， 
得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。 
语法 
reduce()函数语法： 
reduce(function, iterable[, initializer]) 
参数 
function -- 函数，有两个参数 
iterable -- 可迭代对象 
initializer -- 可选，初始参数 
 
 
### 求数字列表的乘积 
```python
lis = [2, 34, 10] 
product = reduce((lambda x, y : x * y), lis) 
# 680 
``` 

 
### functions.wraps
在类中定义装饰器，并将其作用在其它函数和方法上； 
```python
from functools import wraps 
 
 
class Decorate(object): 
    def decorate1(self, func): 
        @wraps(func) 
        def wrapper(*args, **kwargs): 
            print('decorate1, instance method') 
            func(*args, **kwargs) 
        return wrapper 
 
    @classmethod 
    def decorate2(cls, func): 
        @wraps(func) 
        def wrapper(*args, **kwargs): 
            print('decorate2, classmethod') 
            func(*args, **kwargs) 
        return wrapper 
 
 
d = Decorate() 
 
 
@d.decorate1 
def add(x, y): 
    return x + y 
 
 
@Decorate.decorate2 
def spam(): 
    print('spam') 

``` 
 
 
 
     
### paritial
如果需要减少某个函数的参数个数，可以使用 functools.partial() 。 partial() 函数允许给
一个或多个参数设置固定的值，减少接下来被调用时的参数个数。假设有下面的函数： 
def spam(a, b, c, d): 
    print(a, b, c, d) 
     
现在使用 partial() 函数来固定某些参数值：   
```shell
>>> from functools import partial 
>>> s1 = partial(spam, 1) # a = 1 
>>> s1(2, 3, 4) 
1 2 3 4 
>>> s2 = partial(spam, d=42) # d = 42 
>>> s2(1, 2, 3) 
1 2 3 42 
>>> s3 = partial(spam, 1, 2, d=42) # a = 1, b = 2, d = 42 
>>> s3(3) 
1 2 3 42
``` 
可以看出，partial() 固定某些参数并返回一个新的 callable 对象。这个新的 callable 接受
未赋值的参数，然后跟之前已经赋值过的参数合并起来，最后将所有参数传递给原始函
数。 


### collections 
collections 定义了很多抽象类，如果想自定义容器类的时候可以继承其中的类；
使用 collections 中的抽象基类，可以确保自定义的容器实现了所有必要的方法，
并且还能简化类型检查； 
```python
num = [2, 3, 4, 3, 3, 0, 10, 4, 4, 4] 
c = collections.Counter(num) 
print(c, c.items(), type(c.items())) 
print(sum(c.values())) # 求数字出现次数的和 
 
# Counter({4: 4, 3: 3, 2: 1, 0: 1, 10: 1}) dict_items([(2, 1), (3, 3), (4, 4), (0, 1), (10, 1)]) <class 'dict_items'> 
# 10

import collections 
import pprint 
 
 
# Write a Python program to count the number of each character of a text file. 
file_path = input('file is --') 
with open(file_path, 'r') as info: 
    count = collections.Counter(info.read().upper()) 
    print(count.items()) 
    pprint.pprint(count) 
 
 
# file is --abc.txt 
# dict_items([('G', 13), ('E', 64), ('R', 29), ('M', 17), ('A', 42), ('N', 45), (' ', 86), ('U', 14), ('I', 36), 
# ('T', 40), ('Y', 17), ('D', 19), ('\n', 10), ('F', 15), ('O', 31), ('W', 5), ('K', 1), ('P', 4), (',', 7), ('H', 25), 
# ('C', 13), ('L', 15), ('(', 1), (':', 1), ('S', 12), (')', 1), ('B', 6), ('3', 2), ('.', 4), ('V', 1), ('1', 3), ('9', 5), 
# ('0', 2), ('-', 1)]) 
# Counter({' ': 86, 
#          'E': 64, 
#          'N': 45, 
#          'A': 42, 
#          'T': 40, 
#          'I': 36, 
#          'O': 31, 
#          'R': 29, 
#          'H': 25, 
#          'D': 19, 
#          'M': 17, 
#          'Y': 17, 
#          'F': 15, 
#          'L': 15, 
#          'U': 14, 
#          'G': 13, 
#          'C': 13, 
#          'S': 12, 
#          '\n': 10, 
#          ',': 7, 
#          'B': 6, 
#          'W': 5, 
#          '9': 5, 
#          'P': 4, 
#          '.': 4, 
#          '1': 3, 
#          '3': 2, 
#          '0': 2, 
#          'K': 1, 
#          '(': 1, 
#          ':': 1, 
#          ')': 1, 
#          'V': 1, 
#          '-': 1}) 

```
 
 
 
 
 
创建双向链表----collections.deque('abcdef'); 
双向链表操作： 
deque_obj.pop()--移除最右边的元素，deque_obj.popleft()--移除最做左边的元素； 
deque_obj.reverse()--反转链表；deque_obj.appendleft(element)--左边添加元素； 
deque_obj.append(element)--右边添加元素； 
deque_obj.extend(iterable_obj)；deque_obj.clear()--移除所有的元素； 
deque_obj.count(element)--返回 element 在 deque_obj 中出现的次数； 
deque_obj.rotate(2)--双向链表正向转动 2 次；deque_obj.rotate(-2)--反向转动 2 次； 
获得一个text出现最多的10个word的列表----collections.Counter(text).most_common(10); 
两个 Counter 可以取相加，可以取交集; 
collections.OrderDict(original_dict)---记住插入的顺序； 
 
 
###### 利用 collections 库的 Counter 方法统计字符串每个单词出现的次数 
 
Counter 类中的 most_common()可以给出序列中出现次数最多的元素 
```shell
>>> s = "kjalfj;ldsjafl;hdsllfdhg;lahfbl;hl;ahlf;h" 
>>> from collections import Counter 
>>> res = Counter(s) 
>>> res 
Counter({'l': 9, ';': 6, 'h': 6, 'f': 5, 'a': 4, 'j': 3, 'd': 3, 's': 2, 'k': 1, 'g': 1, 'b': 1}) 
>>> print(res.most_common(3))    # 出现最多的三个元素是哪些 
[('l', 9), (';', 6), ('h', 6)] 
>>> res['l']    # 在底层实现上，一个 Counter 对象就是一个字典，将元素映射到它出现
的次数上。 
Counter 实例一个鲜为人知的特性是它们可以很容易的跟数学运算操作相结合。比如两个 Counter 实例的相加减等。 
``` 




### inspect # 获取给定对象的实际模块对象 
import inspect 
import math 
from python.w3resource import sort_tests_69 
 
print(inspect.getmodule(math.pow))          # <module 'math' (built-in)> 
 
print(inspect.getmodule(sort_tests_69))            # <module 'python.w3resource.sort_tests_69' from 'F:\\projects\\python\\w3resource\\sort_tests_69.py'> 
 
inspect.signature(func)----获得函数的参数信息； 
from inspect import Signature, Parameter 
 
 
params = [Parameter('x', Parameter.POSITIONAL_OR_KEYWORD), 
          Parameter('y', Parameter.POSITIONAL_OR_KEYWORD, default=24), 
          Parameter('z', Parameter.KEYWORD_ONLY, default=None)] 
 
sig = Signature(params) 
print(sig)  # (x, y=24, *, z=None) 
sig 就是签名对象，可以使用它的 bind()将它绑定到*args, **kwargs 中，bound_args = 
sig.bind(*args, **kwargs), 
参数名称和参数值可以通过 bound_args.arguments.items()获得； 
通过将签名和传递的参数绑定起来，可以强制函数调用遵循特定的规则，比如必填、默
认、重复等等 
### pkg_resources 获取本地安装的所有模块 
import pkg_resources 
 
 
installed_packages = pkg_resources.working_set 
installed_packages_list = sorted( 
    ['%s==%s' % (x.key, x.version) for x in installed_packages]) 
for p in installed_packages_list: 
    print(p) 
 
<8> array 
导入 array---from array improt *; 
定义一个 Integer 类型的 array----arr = array('i', original_list)； 
数组最后添加元素----arr.append(10); 
数组反转---arr.reverse(); 
数组中一个元素的字节长度----arr.itemsize; 
缓存取的内存地址以及所有元素的长度---arr.buffer_info(); 
append() -- append a new item to the end of the array 
buffer_info() -- return information giving the current memory info 
byteswap() -- byteswap all the items of the array 
count() -- return number of occurrences of an object 
extend() -- extend array by appending multiple elements from an iterable 
fromfile() -- read items from a file object 
fromlist() -- append items from the list 
frombytes() -- append items from the string 
index() -- return index of first occurrence of an object 
insert() -- insert a new item into the array at a provided position 
pop() -- remove and return item (default last) 
remove() -- remove first occurrence of an object 
reverse() -- reverse the order of the items in the array 
tofile() -- write all items to a file object 
tolist() -- return the array converted to an ordinary list 
tobytes() -- return the array converted to a string 
<9> itertools 
itertools.product(*iterables, repeart=1) 
获取所有迭代器的组合，默认重复一次 
```shell
>>>from itertools import product 
>>> x = product([1,2,3], [4,5]) 
>>> for a in x: 
...     print(a) 
... 
(1, 4) 
(1, 5) 
(2, 4) 
(2, 5) 
(3, 4) 
(3, 5) 
 
>>> x = product('34', '23', '34') 
>>> for a in x: 
...     print(a) 
... 
('3', '2', '3') 
('3', '2', '4') 
('3', '3', '3') 
('3', '3', '4') 
('4', '2', '3') 
('4', '2', '4') 
('4', '3', '3') 
('4', '3', '4') 

```
 
 
### islice(iterable, [start, ] stop [, step])
创建一个迭代器： iterable[start : stop : step]，跳过前 start 个项， 
迭代在 stop 所指定的位置停止，step 指定用于跳过项的步幅。迭代默认将从 0 开始，步幅默认 1 
用 python 快速写斐波那契数列(延迟计算：生成器表达式，仅在需要计算的时候才通过
yield 产生所需的元素)： 
def get_fibonacci_numbers(a=0, b=1): 
     yield a 
     while True: 
         yield b 
         a, b = b, a + b 
result = list(itertools.islice(get_fibonacci_numbers(), n)) 
 

# itertools.starmap(func, iterables) 
第一个参数是函数对象，后一个参数是可迭代对象，意思将迭代对象中的参数传给函数。 
def test_itertools_starmap(): 
    map = itertools.starmap(str.isdigit, '34dfgg43dfd') 
    print('type(map)==%s', type(map)) 
    print(list(map)) 
 
 
test_itertools_starmap() 
# type(map)==%s <class 'itertools.starmap'> 
# [True, True, False, False, False, False, True, True, False, False, False] 
 
<10> argparse 
parser = argparse.ArgumentParser() 
parser.add_argument('-b', 
nargs='+', 
help='backup 
log 
path', 
default=['/var/log/mongodb_backup.log']) 
parser.add_argument('-r', 
nargs='+', 
help='restore 
log 
path', 
default=['/var/log/mongodb_restore.log']) 
# parser.print_help() 
args = parser.parse_args() 
mongo_data_integrity_check(args.b[0], args.r[0]) 
 
parser = arsparse.ArgumentParser(prog='python mongodb_data_interity_check,.py') 
# 参数 prog 是程序的名字（默认 sys.argv[0]） 
 
# 对象的 add_argument 函数增加参数，以下是其常用的参数： 
default: 没有设置值情况下的默认参数； 
required: 表示这个参数是都一定需要设置，如果设置了 required=True，则在实际运行的
时候不设置该参数将报错； 
type: 参数类型； 
默认的参数类型是 str 类型，如果你的程序需要一个整数或者布尔型参数，你需要设置
type=int 或者 type=bool; 
choices: 参数值只能从几个选项里面选择； 
help: 指定参数的说明信息； 
dest: 设置参数在代码中的变量； 
argparse 默认的变量名是--或-后面的字符串，但是你也可以通过 dest=xxx 来设置参数的
变量名，然后在代码中用 args.xxx 来获取参数的值； 
nargs: 设置参数在使用可以提供的个数； 
parse.add_argument('-name', nargs=x) 
-----N,参数的绝对个数(列如 3)，其中 x 的候选值和含义如下："?"--0 或 1 个参数，"*"--
0 或所有参数，"+"--所有，并且至少一个参数； 
 
 
 
为了解析命令行参数，首先要创建一个 ArgumentParser 实例，并使用 add_argument()方
法声明想要支持的选项； 
在每个 add_argument()调用中，dest 参数指定解析结果被指派给属性的名字。metavar 参
数被用来生成帮助信息； 
action 参数指定跟属性对应的处理逻辑，通常的值为 store，被用来存储某个值或将多个
参数值收集到一个列表中。 
parse = argparse.ArgumentParser(description='Search some file') 
# 用来构造一个文件名列表 
parse.add_argument(dest='filenames', metavar='filename', nargs='*') 
# 参数说明允许某个参数重复出现多次，并将它们追加到一个列表中去。required 标志
表示该参数至少要有一个。 
-p 和 --pat 表示两个参数名形式都可使用。 
parse.add_argument('-p', '--pat', metavar='pattern', required=True, 
                   dest='patterns', action='append', 
                   help='text pattern to search for') 
# 下面的参数根据参数是否存在来设置一个 Boolean 标志 
parse.add_argument('-v', dest='verbose', action='store_true', 
                   help='verbose mode') 
# 下面的参数接受一个单独值并将其存储为一个字符串 
parse.add_argument('-o', dest='outfile', action='store', help='output file') 
# 下面的参数说明接受一个值，但是会将其和可能的选择值做比较，以检测其合法性 
parse.add_argument('--speed', dest='speed', action='store', 
                   choices={'slow', 'fast'}, default='slow', 
                   help='search speed') 
# 一旦参数选项被指定，你就可以执行 parser.parse() 方法了。它会处理 sys.argv 的值并
返回一个结果实例。 
每个参数值会被设置成该实例中 add_argument()方法的 dest 参数指定的属性值 
args = parse.parse_args() 
<11> os 
# 获取 os 的位数，32bit 还是 64bit 
import struct 
def get_os_mode_32bit_or_64bit(): 
    return struct.calcsize("P") * 8 
     
 
# 获取系统的名称，系统所在平台，系统的发行版     
def get_os_name(): 
    import platform 
    return os.name, platform.system(), platform.release()    
 
# 获得 python 的 site-packages 所在目录 
def get_python_site_packages(): 
    import site 
    return site.getsitepackages() 
 
 
# 调用一个外部命令 
def command_to_call(): 
    import subprocess 
    return subprocess.call(['python', '--version']) 
 
 
# 获得当前执行程序的文件路径，__file__绝对路径 
def get_file_path_the_current_execute(): 
    print(__file__) 
    return os.path.realpath(__file__) 
 
 
# 获取当前正在使用的 cpu 个数 
def get_cup_numbers_using(): 
    import multiprocessing 
    return multiprocessing.cpu_count() 
 
 
# 获得一个目录下的所有文件 
def get_all_files_list_from_direction(): 
    files_list = [f for f in os.listdir('F:\projects\python') 
                  if os.path.isfile(os.path.join('F:\projects\python', f))] 
    directions_list = [f for f in os.listdir('F:\projects\python') 
                       if not os.path.isfile(os.path.join('F:\projects\python', f))] 
    return files_list, directions_list 
 
# 系统变量 
os.environ 
os.environ['PATH']  # 获取指定的环境变量 
 
# 获取当前的用户名   
import getpass 
print(getpass.getuser())  # Administrator 
 
# 获取绝对文件路径 
os.path.abspath(path_fname) 
 
 
# 获取版权信息 
import sys 
print(sys.copyright) 
# Copyright (c) 2001-2019 Python Software Foundation. 
All Rights Reserved. 
 
# 查看系统是小端平台还是大端平台 
sys.byteorder   # 两个值 little 和 big 
 
 
# 清屏或者清除终端 
os.system('cls') 
 
 
# 从给定路径中提取文件名 
os.path.basename('/users/system1/student1/homework-1.py') 
# homework-1.py 
 
import os 
print("\nEffective group id: ",os.getegid()) 
print("Effective user id: ",os.geteuid()) 
print("Real group id: ",os.getgid()) 
print("List of supplemental group ids: ",os.getgroups()) 
print() 
#  
# Effective group id:  1007                                           
# Effective user id:  1006                                            
# Real group id:  1007                                                
# List of supplemental group ids:  [1007]   
 
 
# 将一个路径按照扩展名分离成元组 
def divide_path_on_extension_separator(): 
    import os.path 
    for path in ['test.txt', 'filename', '/user/system/test.txt', '/', '']: 
        print('"%s" :' % path, os.path.splitext(path)) 
 
 
divide_path_on_extension_separator() 
# "test.txt" : ('test', '.txt') 
# "filename" : ('filename', '') 
# "/user/system/test.txt" : ('/user/system/test', '.txt') 
# "/" : ('/', '') 
# "" : ('', '') 
 
 
# 检索文件属性 
def retrieve_file_properties(): 
    import os.path 
    import time 
    print('file is', __file__)  # 文件名 
    print('Access time', time.ctime(os.path.getatime(__file__)))  # 执行程序时间 
    print('Modify time', time.ctime(os.path.getmtime(__file__))) # 修改时间 
    print('create time', time.ctime(os.path.getctime(__file__))) # 创建时间 
    print('size of file', os.path.getsize(__file__)) # 文件大小 
 
 
retrieve_file_properties() 
# file is F:/projects/python/w3resource/retrieve_file_properties_107.py 
# Access time Fri Sep 11 22:59:53 2020 
# Modify time Fri Sep 11 22:59:53 2020 
# create time Fri Sep 11 22:54:19 2020 
# size of file 376 
 
 
# display some information about the OS 
import platform as pl 
 
 
def test_list_os_profile(): 
    os_profile = [ 
        'architecture', 
        'linux_distribution', 
        'mac_ver', 
        'machine', 
        'node', 
        'platform', 
        'processor', 
        'python_build', 
        'python_compiler', 
        'python_version', 
        'release', 
        'system', 
        'uname', 
        'version', 
    ] 
    for x in os_profile: 
        if hasattr(pl, x): 
            print(x + ": " + str(getattr(pl, x)())) 
 
             
 
# 测试文件是否存在： 
try: 
    file = open('a.txt', 'r') 
    text = file.read() 
    file.close() 
except IOError: 
    print('not accessed or problem in reading:', 'a.txt') 
文件是否可读----os.access(file, os.R_OK); 
文件是否可写----os.access(file, os.W_OK); 
文件是否存在----os.access(file, os.F_OK); 
文件是否可执行----os.access(file, os.X_OK); 
在给定路径上执行统计系统调用----stat_info = os.stat('D:\python_projects\python') 
大小---print(stat_info.st_size) 
权限---print(stat_info.st_mode) 
owner---print(stat_info.st_uid) 
device---print(stat_info.st_dev) 
创建时间---print(time.ctime(stat_info.st_ctime)) 
最后一次修改时间---print(time.ctime(stat_info.st_mtime)) 
最后一次操作时间---print(time.ctime(stat_info.st_atime)) 
获得当前执行文件---os.path.basename(__file__) 
获得当前文件的目录名---os.path.dirname(__file__); 
文件的全路径名---__file__; 
创建软链接---os.symlink(src_file, des_file); 
返回软链接的指向---os.readllink(path)； 
解除软链接---os.unlink(path); 
父进程 id----os.getppid(); 
当前进程的用户 id---os.getuid(); 
change 用户 id---os.setuid(); 
change 路径到上级目录---os.pardir 
change 当前路径到指定的路径下：os.chdir(os.pardir)--从当前路径转到上级目录； 
获得所有环境变量----os.environ; 
获得指定环境变量---os.environ[var]; 
获得环境变量键的值---os.getenv(key); 
返回主路径，主路径下的子路径名，文件名---os.walk(path); 
判断文件是否存在---os.path.exists(file); 
文件被 split 成目录和文件---os.path.split(path); 
一个文件被添加到一个目录中---os.path.join(dir, file); 
它接受一个路径，可能是相对路径，最后返回绝对路径---os.path.abspath(); 
用来返回正常路径，可以解决双斜杆、对目录的多重引用的问题等---os.path.normpath(); 
将字符串写入缓存中---io.StringIO().write(string); 
从缓存中取值---io.StringIO().getvalue(); 
使用 os 模块执行命令---os.system(command); 
获得纪元时间---time.gmtime(0); 
将一个结构时间转换成 string--- 
t = (2020, 1, 22, 2, 34, 6, 6, 362, 0) 
result = time.asctime(t) #  Sun Jan 22 02:34:06 2020 
将一个 string 格式的时间转换成结构时间： 
time_string = '04/11/15 11:55:23' 
result 
= 
time.strptime(time_string, 
"%m/%d/%y 
%H:%M:%S") 
# 
time.struct_time(tm_year=2015, 
tm_mon=4, 
tm_mday=11, 
tm_hour=11, 
tm_min=55, 
tm_sec=23, tm_wday=5, tm_yday=101, tm_isdst=-1) 
当前终端的大小以便正确的格式化输出----size=os.get_terminal_size() 
>>> size.columns 
184 
>>> size.lines 
16 
 
 
 
# os.remove()删除文件 
# os.rename()重命名文件 
# os.walk()生成目录树下的所有文件名 
# os.chdir()改变目录 
# os.mkdir/makedirs 创建目录/多层目录 
# os.rmdir/removedirs 删除目录/多层目录 
# os.listdir()列出指定目录的文件 
# os.getcwd()取得当前工作目录 
# os.chmod()改变目录权限 
# os.path.basename()去掉目录路径，返回文件名 
# os.path.dirname()去掉文件名，返回目录路径 
# os.path.join()将分离的各部分组合成一个路径名 
# os.path.split()返回（dirname(),basename())元组 
# os.path.splitext()(返回 filename,extension)元组 
# os.path.getatime\ctime\mtime 分别返回最近访问、创建、修改时间 
# os.path.getsize()返回文件大小 
# os.path.exists()是否存在 
# os.path.isabs()是否为绝对路径 
# os.path.isdir()是否为目录 
# os.path.isfile()是否为文件 
<12> json 
json.dumps()   # 将 python 对象编码成 json 字符串； 
json.loads()   # 将已编码的 json 字符串解码成 python 对象； 
json.dump()    # 编码处理文件 
json.load()    # 解码处理文件 
 
 
json.dump 显示中文 
json dump 有一个 ensure_ascii 参数，当它为 True 的时候，所有非 ASCII 码字符显示为
\uXXXX 序列，只需在 dump 时将 ensure_ascii 设置为 False 即可，此时存入 json 的中文
即可正常显示。 
with open('tickets.json', 'w', encoding='utf-8') as f: 
        json.dump({'速度与激情 7': 4}, f, ensure_ascii=False) 
    file_new = open('tickets.json', 'r', encoding='utf-8') 
    print(file_new.read()) 
    file_new.close() 
     
     
>>> d = {'name': 'dx', 'age': '20'} 
>>> d01 = json.dumps(d) 
>>> print(d01, type(d01)) 
{"name": "dx", "age": "20"} <class 'str'> 
>>> d02 = json.loads(d01) 
>>> print(d02, type(d02)) 
{'name': 'dx', 'age': '20'} <class 'dict'>     
<13> bisect 
bisect 模块 bisect.insort(lis, item)-----在排序列表中插入元素，保证元素插入后还保持顺
序；bisect.bisect_left(list, item)----返回元素 item 插入 list 中的索引； 
 
 
 
<14> heapq 
heapq.nlargest(num, dict1)----表示获取最大 num 的 dict1 的键； 
>>> nlargest(3, {'item1': 45.50, 'item2':35, 'item3': 41.30, 'item4':55, 'item5': 24}.items(), 
key=lambda x: x[1]) 
[('item4', 55), ('item1', 45.5), ('item3', 41.3)] 
heapq.nsmallest(num, list1)----表示湖区最大的 num 个的 list 的值，字典同样是键； 
heapq.heappush(iterable_obj, value)----向可迭代对象中 push 值 value; 
heapq.heappop(iterable_obj)----从 iterable_obj 中 pop 值，从小到大 pop; 
heapq.heapify(original_list)---将一个 list 转换成 heap; 
heapq.heappushpop(iterable_obj, item)---pop 最小的元素，push 一个 item; 
 
 
 
<15> enum 
枚举类： 
class Country(enum.Enum): 
    Afghanistan = 93 
    Albania = 355 
    Algeria = 213 
    Andorra = 376 
    Angola = 244 
    Antarctica = 672 
print('Member name:', Country.Albania.name) 
print("Member value:", Country.Albania.value) 
# Member name: Albania 
# Member value: 355 
 
迭代一个枚举类，并独立展示成员以及其值： 
# for data in Country: 
#     print(data.name, data.value) 
 
获取一个枚举类的值列表： 
枚举类继承自 enum.IntEnum,返回 list(map(int, Country)); 
 
 
 
<16> getpass 
弹出密码输入提示，并且不会再任务终端回显密码。 
user = getpass.getuser() # 不会弹出用户名的输入提示。它会根据该用户的 shell 环境或者
会依据本地系统的密码库（支持 pwd 模块的平台）来使用当前用户的登录名 
password = getpass.getpass() 
<17> subprocess 模块 
执行外部命令并获取它的输出 
默认情况下，check_output()仅仅返回输入到标准输出的值。如果你需要同时收集标准输
出和错误输出，使用 stderr 参数 
output_bytes = subprocess.check_output(['cmd', 'arg1', 'arg2'], 
                                       stderr=subprocess.STDOUT) 
# 如果你需要用一个超时机制来执行命令，使用 timeout 参数： 
try: 
    output_bytes = subprocess.check_output(['cmd', 'arg1', 'arg2'], timeout=5) 
except subprocess.TimeoutExpired as e: 
    pass 
# 如果你想让命令被一个 shell 执行，传递一个字符串参数，并设置参数 shell=True 
output_bytes = subprocess.check_output('grep python |wc > out', shell=True) 
 
 
 
<> subprocess 
# 系统命令输出 
import subprocess 
 
# 执行 dir 命令，打印到终端 
returned_text = subprocess.check_output('dir', shell=True, 
                                        universal_newlines=True) 
print(returned_text) 
 
 
 
<17> sys 
你无法导入你的 python 代码因为它所在的目录不在 sys.path 里； 
两种常见的方式将新目录添加到 sys.path 中： 
1. 可以使用 PYTHONPATH 环境变量来添加； 
env PYTHONPATH=/some/dir:/other/dir python3 
>>> import sys 
>>> sys.path 
['', '/some/dir', '/other/dir', ...] 
这样的环境变量可在程序启动时设置或通过 shell 脚本 
2. 第二种方法是创建一个.pth 文件，将目录列举出来，像这样： 
# myapplication.pth 
/some/dir 
/other/dir 
这 个 .pth
文 件 需 要 放 在 某 个
Python
的
site-packages
目 录 ， 通 常 位 于
/usr/local/lib/python3.3/site-packages 
或者 ~/.local/lib/python3.3/sitepackages, 
当解释器启动时，.pth 文件里列举出来的存在于文件系统的目录将被添加到 sys.path。安
装一个.pth 
文件可能需要管理员权限，如果它被添加到系统级的 Python 解释器。 
3. 手动调节 sys.path 的值； 
sys.path.insert(0, /some/dir) 
 
 
 
# 向标准错误打印一条消息并返回某个非零状态码来终止程序运行 
raise SystemExit('It failed!')----将消息打印在 sys.stderr 中,然后程序以状态码 1 退出； 
 
 
python 1.py 22 33 命令行启动程序并传参，print(sys.argv)会输出什么数据？ 
文件名和参数构成的列表 
['1.py', '22', '33'] 
sys.argv[0]  -----文件名 
sys.argv[1:] -----参数 
 
 
def test_get_object_size_in_bytes(): 
    st1 = 'one' 
    st2 = 'two' 
    st3 = 'three' 
    print('st1 is %s, ' % st1, "it\'s size %s bytes" % sys.getsizeof(st1)) 
    print('st1 is %s, ' % st2, "it\'s size %s bytes" % sys.getsizeof(st2)) 
    print('st1 is %s, ' % st3, "it\'s size %s bytes" % sys.getsizeof(st3)) 
 
 
# test_get_object_size_in_bytes() 
# output: 
# st1 is one,  it's size 52 bytes 
# st1 is two,  it's size 52 bytes 
# st1 is three,  it's size 54 bytes 
 
 
# sys.getrecursionlimit()   递归深度 
# sys.argv      命令行参数 List，第一个元素是程序本身路径 
# sys.modules.keys() 返回所有已经导入的模块列表 
# sys.exc_info()   获取当前正在处理的异常类,exc_type、exc_value、exc_traceback 当前
处理的异常详细信息 
# sys.exit(n)    退出程序，正常退出时 exit(0) 
# sys.hexversion   获取 Python 解释程序的版本值，16 进制格式如：0x020403F0 
# sys.version    获取 Python 解释程序的版本信息 
# sys.maxint     最大的 Int 值 
# sys.maxunicode   最大的 Unicode 值 
# sys.modules    返回系统导入的模块字段，key 是模块名，value 是模块 
# sys.path      返回模块的搜索路径，初始化时使用 PYTHONPATH 环境变量的值 
# sys.platform    返回操作系统平台名称 
# sys.stdout     标准输出 
# sys.stdin     标准输入 
# sys.stderr     错误输出 
# sys.exc_clear()  用来清除当前线程所出现的当前的或最近的错误信息 
# sys.exec_prefix  返回平台独立的 python 文件安装的位置 
# sys.byteorder   本地字节规则的指示器，big-endian 平台的值是'big',little-endian 平台的
值是'little' 
# sys.copyright   记录 python 版权相关的东西 
# sys.api_version  解释器的 C 的 API 版本 
# sys.version_info  python 版本 
 
 
 
<18> contextlib 
实现上下文管理器 
contextlib 模块中@contextmanager 装饰器 
@contextmanager 
def timethis(label): 
    start_time = time.time() 
    try: 
        yield 
    finally: 
        end_time = time.time() 
        print("{}: {}".format(label, (end_time - start_time))) 
 
 
with timethis('counting'): 
    n = 100000 
    while n > 0: 
yield 之前的代码会在上下文管理器中作为__enter__()方法执行，所有再 yield 之后的代
码会作为__exit__()方法执行， 
如果出现了异常，代码会在 yield 那里抛出； 
 
 
 
<19> importlib 
对字符串调用导入命令 
import importlib 
mod = importlib.import_module('urllib.request') 
>>> u = mod.urlopen('http://www.python.org') 
import module 只是简单地执行和 import 相同的步骤，但是返回生成的模块对象。 
你只需要将其存储在一个变量，然后像正常的模块一样使用。 
 
 
 
<20> pickle 
持久化存储对象，模块为 pickle 或 cpickle 
pickle.dump() ----用来把对象存储在打开的文件中，成为存储； 
pickle.load() ----用来取对象，成为取存储； 
 
 
 
<21> urllib 
params = { 
    'name1': 'value1', 
    'name2': 'value2' 
} 
querystring = parse.urlencode(params) 
# querystring 变成了这种形式 name1=value1&name2=value2 
发送一个简单的 HTTP GET 请求到远程服务器上----urllib.request.urlopen(url + '?' + 
querystring) 
将 参 数 编 码 后 作 为 可 选 参 数 提 供 给 urlopen() 函 数 ----urllib.request.urlopen(url, 
querystring.encode('ascii')) 
你需要在发出的请求中提供一些自定义的 HTTP 头，例如修改 user-agent 字段,  
 
 
 
<22> xml_rpc 
xml_rpc 构造简单远程调用服务 
远程调用理解：在本地调用远程机器上的服务； 
xmlrpc 模块的实现---创建一个服务器实例，通过它的方法 registry_function()来注册函数， 
                  然后使用方法 server_forever()启动它。 
python 代码笔记 
1. 解压序列赋值给多个变量 
 
 
----任何的序列（或者是可迭代对象）可以通过一个简单的赋值语句解压
并赋值给多个变量； 
 
 
----*号变量可以用来接收多个元素；  
# 函数解包以及元组解包 
def unpack(x, y): 
    print('x==%s, y==%s' % (x, y)) 
 
 
lis1 = [1, 2] 
unpack(*lis1)  
dic = {'x': 'dong', 'y': 20} 
unpack(**dic) 
 
# output: 
# x==1, y==2 
# x==dong, y==20 
 
a, b, *c = range(6) 
print('a==%s, b==%s, c==%s' % (a, b, c)) 
# a==0, b==1, c==[2, 3, 4, 5] 
x, *y, z = range(5) 
print('x==%s, y==%s, z==%s' % (x, y, z)) 
# x==0, y==[1, 2, 3], z==4 
 
 
 
2. global 在函数声明修改全局变量 
>>> a=3 
>>> def change(): 
...     global a 
...     a=5 
... 
>>> change() 
>>> a 
5 
 
 
 
 
with 方法打开处理文件做了什么? 
打开文件在进行读写的时候可能会出现一些异常状况，如果按照常规的 f.open 
写法，我们需要 try,except,finally，做异常判断，并且文件最终不管遇到什么情况，
都要执行 finally f.close()关闭文件，with 方法帮我们实现了 finally 中 f.close 
with 语句适用于对资源进行访问的场合，确保不管使用过程中是否发生异常都会
执行必要的“清理”操作，释放资源，比如文件使用后自动关闭、线程中锁的自动
获取和释放等。 
with 语句即“上下文管理器”，在程序中用来表示代码执行过程中所处的前后环境
上下文管理器：含有__enter__和__exit__方法的对象就是上下文管理器。 
__enter__：在执行语句之前，首先执行该方法，通常返回一个实例对象，如果 with
语句有 as 目标，则将对象赋值给 as 目标。 
__exit__：执行语句结束后，自动调用__exit__方法，用户释放资源，若此方法返
回布尔值 True，程序会忽略异常。 使用环境：文件读写、线程锁的自动释放等。 
 
 
 
6. 可变和不可变数据类型，简述原理 
# 不可变数据类型：数值型、字符串型 string 和元组 tuple 
# 不允许变量的值发生变化，如果改变了变量的值，相当于是新建了一个对象，而对于
相同的值的对象，在内存中则只有一个对象（一个地址） 
# 可变数据类型：列表 list 和字典 dict； 
# 允许变量的值发生变化，即如果对变量进行 append、+=等这种操作后，只是改变了变
量的值，而不会新建一个对象，变量引用的对象的地址也不会变化，不过对于相同的值
的不同对象，在内存中则会存在不同的对象，即每个对象都有自己的地址，相当于内存
中对于同值的对象保存了多份，这里不存在引用计数，是实实在在的对象 
 
可变 list，dict，set ，不可变 int string tuple 
当一个引用传递给函数的时候,函数自动复制一份引用,这个函数里的引用和外边的引用
没有半毛关系了； 
当函数内的引用指向的是可变对象,对它的操作就和定位了指针地址一样,在内存里进行
修改. 
 
1,可变类型有 list,dict.不可变类型有 string，number,tuple. 
2,当进行修改操作时，可变类型传递的是内存中的地址，也就是说，直接修改内存中的
值，并没有开辟新的内存。 
3,不可变类型被改变时，并没有改变原内存地址中的值，而是开辟一块新的内存，将原
地址中的值复制过去，对这块新开辟的内存中的值进行操作。 
 
 
# 整数对象和字符串对象是不可变对象，python 会高效地缓存它们，python 缓存的整数
范围为(-1, 100); 
# 浮点数对象是可变对象； 
 
 
 
7. a="hello"和 b="你好"编码成 bytes 类型 
>>> a = b'Hello' 
>>> b = '你好'.encode() 
>>> print(a, b) 
b'Hello' b'\xe4\xbd\xa0\xe5\xa5\xbd' 
>>> print(type(a), type(b)) 
<class 'bytes'> <class 'bytes'> 
 
 
 
9. 简述同源策略 
同源策略需要同时满足以下三点要求： 
1）协议相同 
2）域名相同 
3）端口相同 
http://www.test.com 与 http://www.test.com 不同源——协议不同 
http://www.test.com 与 http://www.admin.com 不同源——域名不同 
http://www.test.com 与 http://www.test.com:8081 不同源——端口不同 
只要不满足其中任意一个要求，就不符合同源策略，就会出现“跨域” 
 
 
 
 
关于 list tuple copy 和 deepcopy 的区别是什么？ 
tuple： 
a = (1, 2, 3, [4, 5, 6, 7], 8) 
a[3] = 3  //typeerror 
a[3][3] = 9 // a (1, 2, 3, [4, 5, 6, 9], 8) 
列表是可变数据类型，数据的值可以修改的这里只是修改了元祖子对象的值，而
不是修改了元祖的值 修改可变类型的值不会改变内存 id，因此元祖的引用还是
没有发生变化 可以这么理解，只要不修改元祖中值的内存 id，那么就可以进行
“修改元祖”操作扩展， 面试官可能会问到：元祖是否可以被修改？ 答：元祖是
不可变数据类型，因此不能修改元祖中的值，但是如果元组中有可变数据类型，
那么可以修改可变数据类型中的值，修改可变数据类型的值并不会使其内存 id
发生变化，所以元祖中元素中的内存 id 也没有改变，因此就做到了“修改元祖”
操作 
list: 
a = [1,2,[3,4]] 
b = a 
c = a[:] 
d = a.copy() 
e = copy.deepcopy(a) 
 
id(a), id(b), id(c), id(d), id(e)  // 只有 a b 是同一个指向 
 >>> (4398429512, 4398429512, 4398398664, 4398429576, 4398429192) 
 
a.append(5) // 只有 b 跟着改变 
a, b, c,d,e 
([1, 2, [3, 4], 5], [1, 2, [3, 4], 5], [1, 2, [3, 4]], [1, 2, [3, 4]], [1, 2, [3, 4]]) 
 
a[2][1] = 7 // 除了 deepcopy， 其他都跟着变了 
a, b, c,d,e 
([1, 2, [3, 7], 5], [1, 2, [3, 7], 5], [1, 2, [3, 7]], [1, 2, [3, 7]], [1, 2, [3, 4]]) 
copy 仅拷贝对象本身，而不拷贝对象中引用的其它对象。 deepcopy 除拷贝对象
本身，而且拷贝对象中引用的其它对象。（子对象） 
 
 
 
11. sort 和 sorted 对列表排序区别 
sort 是在原列表的基础上修改，无返回值，sort()不能对 dict 字典进行排序； 
sorted 有返回值，是新的 list，对 dict 排序默认会按照 dict 的 key 值进行排序，最
后返回的结果是一个对 key 值排序好的 list。 
sorted 对 tuple， dict 依然有效，而 sort 不行。 
 
 
 
12. 列表推导式、字典推导式、生成器 
>>> l = [i for i in range(4)] 
>>> print(l, type(l)) 
[0, 1, 2, 3] <class 'list'> 
>>> g = (i for i in range(4)) 
>>> print(g, type(g)) 
<generator object <genexpr> at 0x000001C813ED7270> <class 'generator'> 
>>> import random 
>>> d = {i:random.randint(0,4) for i in ['a', 'b', 'c', 'd']} 
>>> print(d, type(d)) 
{'a': 2, 'b': 2, 'c': 4, 'd': 1} <class 'dict'> 
 
 
 
13. int("1.4"),int(1.4)输出结果？ 
int("1.4")报错-->float("1.4")输出 1.4，int(1.4)输出 1 
 
 
 
17. Python 对象的命名规范，例如方法或者类等 
变量命名：字母数字下划线，不能以数字开头 
_ 受保护的 
__ 私有的 
__init__ 内置变量 
函数和方法（类中叫做方法，模块中称作函数）命名 
下划线开头的命名方式被常用于模块中，在一个模块中以单下划线开头的变量和
方法会被默认划入模块内部范围。 
当使用 from my_module import * 导入时，单下划线开头的变量和方法是不会被
导入的。但使用 import my_module 导入的话，仍然可以用 my_module._var 这
样的形式访问属性或方法。 
双下划线开头和结尾的是一些 python 的“魔术”对象 
class A 中定义的属性__cont ，这样的变量获取时需要用 A._A__cont 
 
 
 
18. 编码为 GBK 的字符串 S，转成 UTF-8 编码的字符串 
demo_str = "demo".encode("gbk")  
demo=demo_str.decode('gbk').encode('utf-8'） 
 
 
 
19. 哪些不能作为字典的健 
字典中的键是不可变类型，可变类型 list 和 dict 不能作为字典键 一个对象能不
能作为字典的 key，就取决于其有没有__hash__方法; 
 
 
 
21. 如何判断一个对象是函数还是方法 
在类外声明 def 为函数 
类中声明 def：使用类调用为函数，使用实例化对象调用为方法 
可以使用 isinstance()判断 
class Work(object): 
    def show(self): 
        print("执行 show 方法") 
 
work = Work() 
print(Work.show) 
print(work.show) 
 
结果： 
<function Work.show at 0x000001CC55BC5268> 
<bound method Work.show of <__main__.Work object at 0x000001CC55C2F240>> 
 
from types import MethodType,FunctionType 
print(isinstance(Work.show,FunctionType)) 
print(isinstance(work.show,MethodType)) 
 
结果： 
True 
True 
 
 
 
22. python 实现接口 
接口只是定义了一些方法，而没有去实现，多用于程序设计时，只是设计需要有
什么样的功能，但是并没有实现任何功能，这些功能需要被另一个类（B）继承
后，由类 B 去实现其中的某个功能或全部功能。 
遵循：开放封闭原则，依赖导致原则，接口隔离原则，继承多态。 
编程思想：为子类做规范； 归一化设计：几个类 实现了相同的方法 抽象类：
最好单继承，且可以简单的实现功能，接口类：可以多继承，且最好不实现具体
功能 
在 python 中接口由抽象类和抽象方法去实现，接口是不能被实例化的，只能被
别的类继承去实现相应的功能。 
个人觉得接口在 python 中并没有那么重要，因为如果要继承接口，需要把其中
的每个方法全部实现，否则会报编译错误，还不如直接定义一个 class，其中的方
法实现全部为 pass，让子类重写这些函数。 
方法一：用抽象类和抽象函数实现方法（适用于单继承） 方法二：用普通类定
义接口（推荐） 
 
 
 
23. Python 中变量的作用域？（变量查找顺序) 
函数作用域的 LEGB 顺序 
1.什么是 LEGB? 
L： local 函数内部作用域 
E: enclosing 函数内部与内嵌函数之间 
G: global 全局作用域 
B：build-in 内置作用 
python 在函数里面的查找分为 4 种，称之为 LEGB，也正是按照这是顺序来查找
的。 
 
 
25. python 判断对象相等 
python 中的 == python 中的对象包含三要素:id, type, value id 用来标识唯一一个
对象，type 标识对象的类型，value 用来设置对象的值。  
is 判断是否是一个对象，使用 id 来判断的，比较的是两个对象的 id 值是否相等，
也就是比较俩对象是否为同一个实例对象。是否指向同一个内 存地址。  
== 是判断 a 对象的值是否是 b 对象的值，默认调用它的__eq__方法。 
 
 
 
26. not A 与 A is None 
not A 就是 True,但是这并不代表该对象没有定义，也不代表该对象没有其它的属
性，它只是代表 A 中元素为空，仅此而已； 
如果要看对象是否又定义，就要使用 is None 来判断； 
 
 
 
28. 通过字符串调用某个对象对应的方法 
1. 直接 getattr(obj, 'func')(*args)；    
2. operator.methodcaller('func', *args)(obj)； 
29. 如何提高 python 的运行效率 
使用生成器； 
关键代码使用外部功能包（Cython，pylnlne，pypy，pyrex）； 
针对循环的优化–尽量避免在循环中访问变量的属性。 
 
 
 
2. 如果你想让某个匿名函数在定义时就捕获到值，可以将那个参数值定义成默
认参 数即可 
a = lambda y, x=x: x + y 
b = lambda y, x=x: x + y 
funcs = [lambda x: x+n for n in range(5) 
funcs = [lambda x, n=n: x+n for n in range(5)] 
4. sys.modules[__name__] 
def function(): 
    print('test sys.modules') 
 
 
print(sys.modules[__name__])   # 此 python 文件中的 module 'main' 
print(function.__name__)    # 函数名 
function_obj = getattr(sys.modules[__name__], function.__name__)# 获得此函数名对象 
function_obj() 
9. 各种加密方式 
Base64 编码 
Base64 是一种用 64 个字符来表示任意二进制数据的方法。 
Base64 编码可以称为密码学的基石。可以将任意的二进制数据进行 Base64 编码。
所有的数据都能被编码为并只用 65 个字符就能表示的文本文件。（ 65 字符：
A~Z a~z 0~9 + / = ）编码后的数据~=编码前数据的 4/3，会大 1/3 左右。 
原理 
 
1. 将所有字符转化为 ASCII 码。 
2. 将 ASCII 码转化为 8 位二进制 。 
3. 将二进制 3 个归成一组(不足 3 个在后边补 0)共 24 位，再拆分成 4 组，每组 6
位。 
4. 统一在 6 位二进制前补两个 0 凑足 8 位。 
5. 将补 0 后的二进制转为十进制。 
6. 从 Base64 编码表获取十进制对应的 Base64 编码。 
说明 
1. 转换的时候，将三个 byte 的数据，先后放入一个 24bit 的缓冲区中，先来的 byte
占高位。 
2. 数据不足 3byte 的话，于缓冲区中剩下的 bit 用 0 补足。然后，每次取出 6 个 bit，
按照其值选择查表选择对应的字符作为编码后的输出。 
3. 不断进行，直到全部输入数据转换完成。 
4. 如果最后剩下两个输入数据，在编码结果后加 1 个“=”。 
5. 如果最后剩下一个输入数据，编码结果后加 2 个“=”。 
6. 如果没有剩下任何数据，就什么都不要加，这样才可以保证资料还原的正确性。 
Python 内置的 base64 模块可以直接进行 base64 的编解码 
*注意：用于 base64 编码的，要么是 ASCII 包含的字符，要么是二进制数据* 
import base64 
 
 
def test_base64(): 
    encode_str = base64.b64encode(b'dongxiang is a genius!!!') 
    print('encode_str==%s' % encode_str) 
    decode_str = base64.b64decode(encode_str) 
    print('decode_str==%s' % decode_str) 
     
output: 
# encode_str==b'ZG9uZ3hpYW5nIGlzIGEgZ2VuaXVzISEh' 
# decode_str==b'dongxiang is a genius!!!' 
MD5（信息-摘要算法） 
message-digest algorithm 5（信息-摘要算法）。经常说的“MD5 加密”，就是信息
摘要算法。 
md5，其实就是一种算法。可以将一个字符串，或文件，或压缩包，执行 md5 后，
就可以生成一个固定长度为 128bit 的串。这个串，基本上是唯一的。 
特点 
1. 压缩性：任意长度的数据，算出的 MD5 值长度都是固定的。 
2. 容易计算：从原数据计算出 MD5 值很容易。 
3. 抗修改性：对原数据进行任何改动，哪怕只修改 1 个字节，所得到的 MD5 值都
有很大区别。 
4. 强抗碰撞：已知原数据和其 MD5 值，想找到一个具有相同 MD5 值的数据（即
伪造数据）是非常困难的。 
5. 不可逆性：每个人都有不同的指纹，看到这个人，可以得出他的指纹等信息，并
且唯一对应，但你只看一个指纹，是不可能看到或读到这个人的长相或身份等信
息。 
举个栗子：世界上只有一个我，但是但是妞却是非常非常多的，以一个有限的我
对几乎是无限的妞，所以可能能搞定非常多（100+）的妞，这个 理论上的确是
通的，可是实际情况下.... 
python 使用 
由于 MD5 模块在 python3 中被移除，在 python3 中使用 hashlib 模块进行 md5 操
作。 
import hashlib 
 
 
def test_md5(string): 
    h1 = hashlib.md5() 
    # 若写法为 hl.update(str)  报错为： Unicode-objects must be encoded before hashing 
    # 此处必须声明 encode 
    h1.update(string.encode('utf-8')) 
    print('string 加密前==%s' % string) 
    print('string 加密后==%s' % h1.hexdigest()) 
 
test_md5('dong xiang is a genius!!!') 
output: 
# string 加密前==dong xiang is a genius!!! 
# string 加密后==c025c1458ea539328a8b0b38de686049 
AES 
高级加密标准（英语：Advanced Encryption Standard，缩写：AES），在密码
学中又称 Rijndael 加密法，是美国联邦政府采用的一种区块加密标准。这个标准
用来替代原先的 DES，已经被多方分析且广为全世界所使用。经过五年的甄选流
程，高级加密标准由美国国家标准与技术研究院（NIST）于 2001 年 11 月 26 日
发布于 FIPS PUB 197，并在 2002 年 5 月 26 日成为有效的标准。2006 年，高级
加密标准已然成为对称密钥加密中最流行的算法之一。 
AES 在软件及硬件上都能快速地加解密，相对来说较易于实作，且只需要很少的
存储器。作为一个新的加密标准，目前正被部署应用到更广大的范围。 
特点 
1. 抵抗所有已知的攻击。 
2. 在多个平台上速度快，编码紧凑。 
3. 设计简单。 
 
AES 为分组密码，分组密码也就是把明文分成一组一组的，每组长度相等，每次
加密一组数据，直到加密完整个明文。在 AES 标准规范中，分组长度只能是 128
位，也就是说，每个分组为 16 个字节（每个字节 8 位）。密钥的长度可以使用
128 位、192 位或 256 位。密钥的长度不同，推荐加密轮数也不同。 
一般常用的是 128 位 
PyCrypto 是 Python 中密码学方面最有名的第三方软件包，提供了许多加密算法
的使用。可惜的是，它的开发工作于 2012 年就已停止。 幸运的是，有一个该项
目的分支 PyCrytodome 取代了 PyCrypto 。 
在 Linux 上安装，可以使用以下 pip 命令： pip install pycryptodome import Crypto 
在 Windows 系统上安装则稍有不同： pip install pycryptodomex 导入：import 
Cryptodome 
from Cryptodome.Cipher import AES 
from Cryptodome import Random 
from binascii import b2a_hex 
 
 
 
# 传入要加密的明文 
def test_AES(string): 
    # 密钥 key 长度必须为 16（AES-128）、24（AES-192）、或 32（AES-256）Bytes 
长度. 
    key = b'xiangdong1234567' 
    # 生成长度等于 AES 块大小的不可重复的密钥向量 
    iv = Random.new().read(AES.block_size) 
    # 使用 key 和 iv 初始化 AES 对象, 使用 MODE_CFB 模式 
    mycipher = AES.new(key, AES.MODE_CFB, iv) 
    # 加密的明文长度必须为 16 的倍数，如果长度不为 16 的倍数，则需要补足为 16
的倍数 
    # 将 iv（密钥向量）加到加密的密文开头，一起传输 
    cipher_text = iv + mycipher.encrypt(string.encode()) 
    # 解密的话要用 key 和 iv 生成新的 AES 对象 
    mydecrypt = AES.new(key, AES.MODE_CFB, cipher_text[:16]) 
    # 使用新生成的 AES 对象，将加密的密文解密 
    decrypt_text = mydecrypt.decrypt(cipher_text[16:]) 
    print('密匙是==%s' % key) 
    print('iv==%s, len(iv)==%s' % (iv, len(iv))) 
    print('加密后数据为==%s' % b2a_hex(cipher_text[16:]).decode()) 
    print('解密后数据为==%s' % decrypt_text.decode()) 
 
test_AES('dong xiang 0816') 
output: 
密匙是==b'xiangdong1234567' 
iv==b'\xba_0\x86\xc4\xa1\xa3B\x00\xf9\xb9_\xab\xe1\xc2#', len(iv)==16 
加密后数据为==94690bf43b12b63e0cc61bc90049c5 
解密后数据为==dong xiang 0816 
 
 
import hashlib 
 
def hash_code(s, salt='mysite'):# 加点盐 
    h = hashlib.sha256() 
    s += salt 
    h.update(s.encode())  # update 方法只接收 bytes 类型 
    return h.hexdigest() 
 
 
# hash a word 
soundx = [0, 1, 2, 3, 0, 1, 2, 0, 0, 2, 2, 4, 5, 5, 0, 1, 
          2, 6, 2, 3, 0, 1, 0, 2, 0, 2] 
 
 
def hash_word(): 
    word = input('input the word be hashed:') 
    word = word.upper() 
    coded = word[0] 
    for a in word[1:len(word)]: 
        i = 65 - ord(a) 
        coded = coded + str(soundx[i]) 
    print('the coded word is ', coded) 
  
 
11. logging 模块使用记录日志 
# DEBUG   # 级别最低，详细的信息，通常只出现在诊断问题上； 
# INFO    # 确认一切按预期进行； 
# WARNING # 一个迹象表明，一些意想不到事情发生了，或表明一些问题在不久将来，
这个软件能按预期进行；  
# ERROR # 更严重的问题，软件没能执行一些功能； 
# CRITICAL # 一个严重的错误，这表明程序本身可能无法继续运行； 
 
# 默认的是 WARNING，当在 WARNING 或者之上时才被追踪； 
 
 
# 日志打印在文件中； 
logging.BasicConfig(level=logging.WARNING,  
                    filename="./log.txt", 
                    filemode='w', format='%(asctime)s' - %(filename)s[line:%(lineno)d] 
- %(levelname)s: %(message)s') 
 
 
import logging 
 
 
# logging 模块使用 
def get_logger_object(): 
    # 生成 logger 对象 
    logger = logging.getLogger('dx.log') 
    logger.setLevel(logging.INFO) 
    # 获取文件 handler 对象，用于写入日志文件 
    file_handler = logging.FileHandler('dx.log') 
    file_handler.setLevel(logging.INFO) 
                     
    # 创建一个 handler,用于输出到控制台 
    ch_handler = logging.StreamHandler() 
    ch.setLevel(logging.WARNING)    
                     
    # 定义 handler 的输出格式，生成 formatter 对象,asctime--类似于这样 2020-07-28 
22:17:02,875,name--就是日志文件名 
    file_formatter = logging.Formatter('%(asctime)s - %(name)s - ' 
                                       '%(levelname)s - %(message)s ') 
    # 把 formatter 绑定到 handler 对象中 
    file_handler.setFormatter(file_formatter) 
    ch_handler.setFormatter(file_formatter)                 
    # 把 handler 对象绑定到 logger 对象中 
    logger.addHandler(file_handler) 
    return logger 
 
 
def write_log(msg): 
    logger = get_logger_object() 
    logger.info(msg) 
    logger.handlers.pop() 
 
 
if __name__ == '__main__': 
    while True: 
        msg = input('输入日志信息').strip() 
        write_log(msg) 
12. iter 方法取文件方式 
def iter_file(file, size=1024): 
    with open(file, 'r') as f: 
        for data in iter(lambda: f.read(size), b''): 
            yield data 
 
 
def test_iter(): 
    for row in iter_file('test.txt'): 
        print(row) 
13. type 
isinstance 不止可以判断对象与实例化这个对象类的关系，还能接受“继承关系” 
只有直接实例化这个对象的类才是 type(对象)的类，即使有继承关系，type 也不会“承认”
这个类的父类的； 
type 创建类的方式： 
type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）) 
 
type 方法创建类： 
Student = type( 
   'Student',  
  (object,), 
  { 
       'country': 'China', 
       'add': lambda self, x, y: x+y 
  } 
) 
 
等价于 
class Student(object): 
   country = "China" 
   def add(self,x:int,y:int)->int: 
       return x+y 
 
type(self)---在对象中使用类的方法 
class People1(object): 
   def __init__(self, name): 
       self.name = name 
       # type(self)，在对象方法中使用类的方法 
       self.age = type(self).get_default_age() 
 
   @classmethod 
   def get_default_age(cls): 
       return 18 
 
   def get_age(self): 
       # type(self)，在对象方法中使用类的方法 
       return type(self).get_default_age() 
    
type(self)---在对象中使用对象的方法 
class People2(object): 
   def __init__(self, name): 
       self.name = name 
       # type(self)，在对象方法中使用类的方法 
       self.age = type(self).get_default_age(self) 
 
   @classmethod 
   def get_default_age(cls): 
       return 18 
 
   def get_age(self): 
       # type(self)，在对象方法中使用类的方法 
       return type(self).get_default_age(self) 
 
   # 起一个与类方法同名的对象方法 
   def get_default_age(self): 
       return 20 
 
p1 = People1('shane') 
print(p1.get_age())  # 18 
# 对象也可以直接调用“类方法”，但是为了“规范”，我们不这么直接让对象直接调用“类
方法” 
print(p1.get_default_age())  # 18 
 
# 如果对象方法与类方法同名的话，对象会优先调用自己的方法 
p2 = People2('shane') 
print(p2.get_age())  # 20 
print(p2.get_default_age()) # 20 
 
 
# type 使变量输出为空 
x = 90 
y = [2, 3, 5] 
z = {12, '23', b'78'} 
print(type(x)(), x) 
print(type(y)(), y) 
print(type(z)(), z) 
# 0 90 
# [] [2, 3, 5] 
# set() {b'78', 12, '23'} 
 
 
 
type(('a'))--表示的是 str 类型，不是 tuple 类型； 
27. 时间格式化 
import datetime 
# 时间戳转时间 
d = datetime.datetime.fromtimestamp(23343555) 
print(d) # 1970-09-28 12:19:15 
# 时间字符串格式化 
d = d.strftime("%Y-%m-%d %H-%M-%S") 
print(d) # 1970-09-28 12-19-15 
 
# 计算两个日期之间差的天数 
from datetime import date 
f_date = date(2014, 7, 2) 
l_date = date(2014, 7, 11) 
delta = l_date - f_date 
 
 
# 获取当前日期 
from datetime import datetime 
datetime.now() 
31. 闭包使用外围作用域的变量 
# 自定义比较器，根据列中的元素的索引为 2 的元素大小进行排序 
# students_tuples = [('join','a',15),('kane','b',20),('pole','c',30)] 
# students_tuples.sort(key=lambda student:student[2]) 
# print(students_tuples) 
# output ---> [('join', 'a', 15), ('kane', 'b', 20), ('pole', 'c', 30)] 
 
 
def sort_priority(values, group): 
    def comparator(x): 
        if x in group: 
            return (0, x) 
        return (1, x) 
    values.sort(key=comparator) 
# 说明：闭包是一种定义在某个作用域中的函数，这种函数引用了那个作用域的变量。 
 
numbers = [8, 3, 1, 2, 5, 4, 7, 6] 
group = {2, 3, 7, 5} 
# sort_priority(numbers, group) 
# print(numbers) 
# output ---> [2, 3, 5, 7, 1, 4, 6, 8] 
 
# lis =  [(1, 8), (0, 3), (1, 1), (0, 2), (0, 5), (1, 4), (0, 7), (1, 6)] 
# lis.sort() 
# print(lis) 
# output ---> [(0, 2), (0, 3), (0, 5), (0, 7), (1, 1), (1, 4), (1, 6), (1, 8)] 
# 说明：元组可以进行比较，先比较元组中下标为 0 的元素，如果相等再比较下标为 1
的对应元素。 
 
def sort_priority1(values, group): 
    found = False 
 
    def comparator(x): 
        if x in group: 
            found = True 
            return (0, x) 
        return (1, x) 
    values.sort(key=comparator) 
    return found 
 
 
# found = sort_priority1(numbers, group) 
# print('found==%s' % found) 
# print(numbers) 
# 说明：在表达式中引用变量时，python 解释器按照如下顺序遍历各作用域，以解析该
引用 
# a. 当前函数的作用域； 
# b. 任何外围作用域(例如，包含当前函数的其它函数); 
# c. 包含当前代码的那个模块的作用域(也叫全局作用域，global scope); 
# d. 内置作用域(包含 len 与 str 等函数的那个作用域); 
# 给变量赋值时，规则又有所不同，如果当前作用域已经定义了这个变量，那么该变量
就会具备新值；若是当前作用域没有这个变量，python 把这次赋值视为对该变量的定义。
而新变量的这个变量，其作用域就是包含赋值操作的这个函数。 
 
def sort_priority2(values, group): 
    found = False 
    print(id(found)) # 140720592160624 
 
    def comparator(x): 
        nonlocal found 
        if x in group: 
            found = True 
            print(id(found))  # 140720592160592 
            return (0, x) 
        return (1, x) 
    values.sort(key=comparator) 
    return found 
 
 
found = sort_priority2(numbers, group) 
print('found==%s' % found) 
print(numbers) 
# output 
# 140720592160624 
# 140720592160592 
# 140720592160592 
# 140720592160592 
# 140720592160592 
# found==True 
# [2, 3, 5, 7, 1, 4, 6, 8] 
# 说明：nonlocal 作用就是给相关变量赋值时，应该再上层作用域中查找该变量，当时
它不能延伸到模块级别。nolocal 允许用来修改闭包中的变量； 
 
 
nonl = ['gfd', 'g', 546] 
 
 
# def test_nonlocal(): 
#     nonlocal nonl 
#     nonl = 4354 
#     print(nonl) 
# 会报错，说明 nonlocal 不能延伸到模块级别，这是为了防止它污染全局作用域。 
#     nonlocal nonl 
#     ^ 
# SyntaxError: no binding for nonlocal 'nonl' found 
 
def sort_priority3(values, group): 
    found = [False] 
    print(id(found)) # 140720592160624 
 
    def comparator(x): 
        if x in group: 
            found[0] = True 
            print(id(found))  # 140720592160592 
            return (0, x) 
        return (1, x) 
    values.sort(key=comparator) 
    return found[0] 
 
 
found = sort_priority3(numbers, group) 
print('found==%s' % found) 
print(numbers) 
# output:  
found==True 
[2, 3, 5, 7, 1, 4, 6, 8] 
# 说明：可以使用可变值来实现 nonlocal 语句相仿的机制。 
 
34. 函数的默认参数 
def foo(x=[]): 
    x.append(1) 
    print(x) 
 
 
# foo() # [1] 
# foo() # [1, 1] 
# 更安全的做法 
 
 
def foo_safe(x=None): 
    if x is None: 
        x = [] 
    x.append(1) 
    print(x) 
 
 
foo_safe() # [1] 
foo_safe() # [1] 
 
36. isinstance 可以接收一个元组 
print(isinstance(1, (int, float))) # 判断 1 是不是 int 类型或者 float 类型 
# True 
print(isinstance(1.3, (float, int))) 
# True 
print(isinstance('2.5', (str, int))) 
# True 
38. 打印一年中的某一个月的日历 
calendar.month(year, month) 
42. python 重定向 
# 标准输入、标准输出和错误输出。 
sys.stdin、sys.stdout 和 sys.stderr 
 
def stderr_test(*args, **kwargs): 
    print(*args, file=sys.stderr, **kwargs) 
 
# stderr_test('abc', 'def', 'xyz', sep='-*-')   # abc-*-def-*-xyz 
 
 
def stdin_test(): 
    print('please input your name:') 
    name = sys.stdin.readline() 
    print(name) 
 
 
# 执行代码 
# stdin_test() 
 
# output: 
# please input your name: 
# dongx 
# dongx 
 
 
def stdout_test(): 
    sys.stdout.write('hello' + '\n') 
 
 
stdout_test() # 等价于 print('hello' + '\n') 
44. 调用栈的打印 
import traceback 
 
 
def abc(): 
    traceback.print_stack() 
 
 
def func(): 
    abc() 
 
 
func() 
#   File "F:/projects/python/w3resource/traceback_use_96.py", line 12, in <module> 
#     func() 
#   File "F:/projects/python/w3resource/traceback_use_96.py", line 9, in func 
#     abc() 
#   File "F:/projects/python/w3resource/traceback_use_96.py", line 5, in abc 
#     traceback.print_stack() 
 
 
 
写一个类，并让它尽可能多的支持操作符? 
class Array: 
 
__list = [] 
    def __init__(self): 
        print "constructor" 
 
    def __del__(self): 
        print "destruct" 
 
    def __str__(self): 
        return "this self-defined array class" 
 
    def __getitem__(self,key): 
        return self.__list[key] 
 
    def __len__(self): 
        return len(self.__list) 
 
    def Add(self,value): 
        self.__list.append(value) 
 
    def Remove(self,index): 
        del self.__list[index] 
 
    def DisplayItems(self): 
        print "show all items---" 
        for item in self.__list: 
        print item 
 
 
 
###### 46.请描述抽象类和接口类的区别和联系 1.抽象类： 规定了一系列的方
法，并规定了必须由继承类实现的方法。由于有抽象方法的存在，所以抽 象类
不能实例化。可以将抽象类理解为毛坯房，门窗，墙面的样式由你自己来定，所
以抽象类与作为基 类的普通类的区别在于约束性更强 
 
2.接口类：与抽象类很相似，表现在接口中定义的方法，必须由引用类实现，但
他与抽象类的根本区别 在于用途：与不同个体间沟通的规则，你要进宿舍需要
有钥匙，这个钥匙就是你与宿舍的接口，你的舍 友也有这个接口，所以他也能
进入宿舍，你用手机通话，那么手机就是你与他人交流的接口  
3.区别和关联： 1.接口是抽象类的变体，接口中所有的方法都是抽象的，而抽象
类中可以有非抽象方法，抽象类是声明方法的存在而不去实现它的类 2.接口可
以继承，抽象类不行 3.接口定义方法，没有实现的代码，而抽象类可以实现部分
方法 4.接口中基本数据类型为 static 而抽象类不是 
 
 
 
pycharm 使用 
支持并行执行程序 
Editor Configurations 
 
 
pycharm 设置护眼绿 
 
爬虫 
常见的反爬虫和应对方法 
1）.通过 Headers 反爬虫 
从用户请求的 Headers 反爬虫是最常见的反爬虫策略。很多网站都会对 Headers
的 User-Agent 进行检测，还有一部分网站会对 Referer 进行检测（一些资源网站
的防盗链就是检测 Referer）。如果遇到了这类反爬虫机制，可以直接在爬虫中添
加 Headers，将浏览器的 User-Agent 复制到爬虫的 Headers 中；或者将 Referer 值
修改为目标网站域名。对于检测 Headers 的反爬虫，在爬虫中修改或者添加
Headers 就能很好的绕过。 
2）.基于用户行为反爬虫 
还有一部分网站是通过检测用户行为，例如同一 IP 短时间内多次访问同一页面，
或者同一账户短时间内多次进行相同操作。 
大多数网站都是前一种情况，对于这种情况，使用 IP 代理就可以解决。可以专
门写一个爬虫，爬取网上公开的代理 ip，检测后全部保存起来。这样的代理 ip 爬
虫经常会用到，最好自己准备一个。有了大量代理 ip 后可以每请求几次更换一
个 ip，这在 requests 或者 urllib2 中很容易做到，这样就能很容易的绕过第一种反
爬虫。 
对于第二种情况，可以在每次请求后随机间隔几秒再进行下一次请求。有些有逻
辑漏洞的网站，可以通过请求几次，退出登录，重新登录，继续请求来绕过同一
账号短时间内不能多次进行相同请求的限制。 
3）.动态页面的反爬虫 
上述的几种情况大多都是出现在静态页面，还有一部分网站，我们需要爬取的数
据是通过 ajax 请求得到，或者通过 JavaScript 生成的。首先用 Fiddler 对网络请
求进行分析。如果能够找到 ajax 请求，也能分析出具体的参数和响应的具体含
义，我们就能采用上面的方法，直接利用 requests 或者 urllib2 模拟 ajax 请求，对
响应的 json 进行分析得到需要的数据。 
能够直接模拟 ajax 请求获取数据固然是极好的，但是有些网站把 ajax 请求的所
有参数全部加密了。我们根本没办法构造自己所需要的数据的请求。这种情况下
就用 selenium+phantomJS，调用浏览器内核，并利用 phantomJS 执行 js 来模拟人
为操作以及触发页面中的 js 脚本。从填写表单到点击按钮再到滚动页面，全部都
可以模拟，不考虑具体的请求和响应过程，只是完完整整的把人浏览页面获取数
据的过程模拟一遍。 
用这套框架几乎能绕过大多数的反爬虫，因为它不是在伪装成浏览器来获取数据
（上述的通过添加 Headers 一定程度上就是为了伪装成浏览器），它本身就是浏
览器，phantomJS 就是一个没有界面的浏览器，只是操控这个浏览器的不是人。
利 selenium+phantomJS 能干很多事情，例如识别点触式（12306）或者滑动式的
验证码，对页面表单进行暴力破解等。 
分布式爬虫主要解决什么问题？ 
1)ip 
2)带宽 
3）cpu 
4）io 
爬虫过程中验证码怎么处理？ 
1.scrapy 自带 
2.付费接口 
写爬虫是用多进程好？还是多线程好？ 为什么？ 
IO 密集型代码(文件处理、网络爬虫等)，多线程能够有效提升效率(单线程下有
IO 操作会进行 IO 等待，造成不必要的时间浪费，而开启多线程能在线程 A 等待
时，自动切换到线程 B，可以不浪费 CPU 的资源，从而能提升程序执行效率)。
在实际的数据采集过程中，既考虑网速和响应的问题，也需要考虑自身机器的硬
件情况，来设置多进程或多线程 
 
 
 
抓取网站中有两个基本的任务： 
1. 加载网页到一个 string 里。 
2. 从网页中解析 HTML 来定位感兴趣的位置。 
Python 为上面两个任务提供了两个超棒的工具。用 requests 去加载网页，用 
BeautifulSoup 去做解析。 
# 伪装成浏览器访问，直接访问的话 csdn 会拒绝 
user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)' 
headers = {'User-Agent': user_agent} 
# 构造请求 
req = urllib.request.Request(blog_url, headers=headers) 
# 访问页面 
response = urllib.request.urlopen(req) 
page = response.read().decode('utf-8') 
# 利用正则表达式来获取博客的内容 
blog_content = re.findall('<div id=\"topics\">(.*?)<script src=', page, re.S) 
title = re.findall('<span>(.*?)</span>', blog_content[0]) 
with open('%s.txt' % title[0].lstrip().rstrip(), 'w+', encoding='utf-8') as f: 
    # 把这些 html 标签去掉 
    dr = re.compile(r'<[^>]+>', re.S)    
    blog_content = dr.sub('', blog_content[0]) 
     
     
 
 
# http.client 访问 url，获取 content 
from http.client import HTTPConnection 
 
 
conn = HTTPConnection('www.baidu.com') 
conn.request('GET', "/") 
result = conn.getresponse() 
content = result.read() 
print(content) 
 
 
# bs4.BeautifulSoup 
soup_page = BeautifulSoup(xml_page, 'xml') 
news_list = soup_page.findAll('item') 
 
soup = BeautifulSoup(html, features='html.parser') 
all_blogs = soup.main.find_all('li') 
scrapy 
制作 Scrapy 爬虫 一共需要 4 步： 
1. 新建项目 (scrapy startproject xxx)：新建一个新的爬虫项目 
2. 明确目标 （编写 items.py）：明确你想要抓取的目标 
3. 制作爬虫 （spiders/xxspider.py）：制作爬虫开始爬取网页 
4. 存储内容 （pipelines.py）：设计管道存储爬取内容 
新建项目 scrapy startproject 
其中， mySpider 为项目名称，可以看到将会创建一个 mySpider 文件夹，目录
结构大致如下： 
下面来简单介绍一下各个主要文件的作用： 
mySpider/ 
    scrapy.cfg 
    mySpider/ 
        __init__.py 
        items.py 
        pipelines.py 
        settings.py 
        spiders/ 
            __init__.py 
            ... 
这些文件分别是: 
 
scrapy.cfg: 项目的配置文件。 
 
mySpider/: 项目的 Python 模块，将会从这里引用代码。 
 
mySpider/items.py: 项目的目标文件。 
 
mySpider/pipelines.py: 项目的管道文件。 
 
mySpider/settings.py: 项目的设置文件。 
 
mySpider/spiders/: 存储爬虫代码目录。 
 
 
 
明确目标(mySpider/items.py) 
我们打算抓取 http://www.itcast.cn/channel/teacher.shtml 网站里的所有讲师的
姓名、职称和个人信息。 
1. 打开 mySpider 目录下的 items.py。 
2. Item 定义结构化数据字段，用来保存爬取到的数据，有点像 Python 中的 dict，
但是提供了一些额外的保护减少错误。 
3. 可以通过创建一个 scrapy.Item 类， 并且定义类型为 scrapy.Field 的类属性来定
义一个 Item（可以理解成类似于 ORM 的映射关系）。 
4. 接下来，创建一个 ItcastItem 类，和构建 item 模型（model）。 
5. import scrapy 
6.  
7. class ItcastItem(scrapy.Item): 
8.    name = scrapy.Field() 
9.    title = scrapy.Field() 
10.    info = scrapy.Field() 
11.  
 
 
 
制作爬虫 （spiders/itcastSpider.py） 
爬虫功能要分两步： 
1. 爬数据 
在当前目录下输入命令，将在 mySpider/spider 目录下创建一个名为 itcast 的爬虫，
并指定爬取域的范围： 
scrapy genspider itcast "itcast.cn" 
打开 mySpider/spider 目录里的 itcast.py，默认增加了下列代码: 
import scrapy 
 
class ItcastSpider(scrapy.Spider): 
    name = "itcast" 
    allowed_domains = ["itcast.cn"] 
    start_urls = ( 
        'http://www.itcast.cn/', 
    ) 
 
    def parse(self, response): 
        pass 
其实也可以由我们自行创建 itcast.py 并编写上面的代码，只不过使用命令可以免
去编写固定代码的麻烦 
要建立一个 Spider， 你必须用 scrapy.Spider 类创建一个子类，并确定了三个强
制的属性 和 一个方法。 
name = "" ：这个爬虫的识别名称，必须是唯一的，在不同的爬虫必须定义不同
的名字。 
allow_domains = [] 是搜索的域名范围，也就是爬虫的约束区域，规定爬虫只爬
取这个域名下的网页，不存在的 URL 会被忽略。 
start_urls = () ：爬取的 URL 元祖/列表。爬虫从这里开始抓取数据，所以，第一
次下载的数据将会从这些 urls 开始。其他子 URL 将会从这些起始 URL 中继承性
生成。 
parse(self, response) ：解析的方法，每个初始 URL 完成下载后将被调用，调用的
时候传入从每一个 URL 传回的 Response 对象来作为唯一参数，主要作用如下： 
负责解析返回的网页数据(response.body)，提取结构化数据(生成 item) 生成需要
下一页的 URL 请求。 将 start_urls 的值修改为需要爬取的第一个 url 
start_urls = ("http://www.itcast.cn/channel/teacher.shtml",) 
修改 parse()方法 
def parse(self, response): 
    filename = "teacher.html" 
    open(filename, 'w').write(response.body) 
然后运行一下看看，在 mySpider 目录下执行： 
scrapy crawl itcast 
是的，就是 itcast，看上面代码，它是 ItcastSpider 类的 name 属性，也就是使
用 scrapy genspider 命令的唯一爬虫名。 
运行之后，如果打印的日志出现 [scrapy] INFO: Spider closed (finished)，代表执
行完成。 之后当前文件夹中就出现了一个 teacher.html 文件，里面就是我们刚
刚要爬取的网页的全部源代码信息。 
注意: Python2.x 默认编码环境是 ASCII，当和取回的数据编码格式不一致时，可
能会造成乱码；我们可以指定保存内容的编码格式，一般情况下，我们可以在代
码最上方添加 
import sys 
reload(sys) 
sys.setdefaultencoding("utf-8") 
这三行代码是 Python2.x 里解决中文编码的万能钥匙，经过这么多年的吐槽后 
Python3 学乖了，默认编码是 Unicode 了...(祝大家早日拥抱 Python3) 
2. 取数据 
爬取整个网页完毕，接下来的就是的取过程了，首先观察页面源码： 
<div class="li_txt"> 
    <h3>  xxx  </h3> 
    <h4> xxxxx </h4> 
    <p> xxxxxxxx </p> 
是不是一目了然？直接上 XPath 开始提取数据吧。 
这里给出一些 XPath 表达式的例子及对应的含义: 
 
/html/head/title: 选择 HTML 文档中 标签内的 元素 
 
/html/head/title/text(): 选择上面提到的 `` 元素的文字 
 
//td: 选择所有的 ` 元素 
 
//div[@class="mine"]: 选择所有具有 class="mine" 属性的 div 元素 
举例我们读取网站 http://www.itcast.cn/ 的网站标题，修改 itcast.py 文件代码
如下：： 
# -*- coding: utf-8 -*- 
import scrapy 
 
# 以下三行是在 Python2.x 版本中解决乱码问题，Python3.x 版本的可以去掉 
import sys 
reload(sys) 
sys.setdefaultencoding("utf-8") 
 
class Opp2Spider(scrapy.Spider): 
    name = 'itcast' # 定义爬虫名 
   
    allowed_domains = ['itcast.com'] #搜索的域名范围，也就是爬虫的约束区域，规定爬
虫只爬取这个域名下的网页 
    start_urls = ['http://www.itcast.cn/'] 
 
    def parse(self, response): 
        # 获取网站标题 
        context = response.xpath('/html/head/title/text()')    
        
        # 提取网站标题 
        title = context.extract_first()   
        print(title)  
        pass 
执行以下命令： 
$ scrapy crawl itcast 
... 
... 
传智播客官网-好口碑 IT 培训机构,一样的教育,不一样的品质 
... 
... 
我们之前在 mySpider/items.py 里定义了一个 ItcastItem 类。 这里引入进来: 
from mySpider.items import ItcastItem 
然后将我们得到的数据封装到一个 ItcastItem 对象中，可以保存每个老师的属性： 
from mySpider.items import ItcastItem 
 
def parse(self, response): 
    #open("teacher.html","wb").write(response.body).close() 
 
    # 存放老师信息的集合 
    items = [] 
 
    for each in response.xpath("//div[@class='li_txt']"): 
        # 将我们得到的数据封装到一个 `ItcastItem` 对象 
        item = ItcastItem() 
        #extract()方法返回的都是 unicode 字符串 
        name = each.xpath("h3/text()").extract() 
        title = each.xpath("h4/text()").extract() 
        info = each.xpath("p/text()").extract() 
 
        #xpath 返回的是包含一个元素的列表 
        item['name'] = name[0] 
        item['title'] = title[0] 
        item['info'] = info[0] 
 
        items.append(item) 
 
    # 直接返回最后数据 
    return items 
我们暂时先不处理管道，后面会详细介绍。 
保存数据 
scrapy 保存信息的最简单的方法主要有四种，-o 输出指定格式的文件，命令如
下： 
scrapy crawl itcast -o teachers.json 
json lines 格式，默认为 Unicode 编码 
scrapy crawl itcast -o teachers.jsonl 
csv 逗号表达式，可用 Excel 打开 
scrapy crawl itcast -o teachers.csv 
xml 格式 
scrapy crawl itcast -o teachers.xml 
Xpath 
路径表达式 
结果 
body 
选取 body 元素的所有子节点。 
/head 
选取根元素下 head。假如路径起始于正斜杠( / )，则此路径始终代表到某元素的
div/a 
选取属于 div 的子元素的所有 a 元素。 
//a 
选取所有 a 子元素，而不管它们在文档中的位置。 
路径表达式 
结果 
div//a 
选择属于 div 元素的后代的所有 a 元素，而不管它们位于 bookstore 之下的什
//@class 
选取名为 claa 的所有属性。 
./a 
选取当前元素下的 a 
…/a 
选取父元素下的 a 
a/@href 
选取 a 标签的 href 属性 
a/text() 
选取 a 标签下的文本 
response.follow 直接支持相对 URL-无需调用 URLJOIN。注意 response.follow 只
返回一个请求实例；您仍然需要生成这个请求。 
response.url---爬取时请求的 url 
scrapy 提高速率 
scrapy 在单机跑大量数据的时候，在对 settings 文件不进行设置的时候，scrapy
的爬取速度很慢，再加上多个页面层级解析，往往导致上万的数据可能爬取要半
个小时之久，这还不包括插入数据到数据库的操作。下面是我在实验中测试并且
验证爬取速度大幅度提升，不过前提你要注意到你爬取的目标网站有没有反 IP
的可能。 
settings 文件设置以下参数 
DOWNLOAD_DELAY = 0 
 
CONCURRENT_REQUESTS = 100 
 
CONCURRENT_REQUESTS_PER_DOMAIN = 100 
 
CONCURRENT_REQUESTS_PER_IP = 100 
 
COOKIES_ENABLED = False 
123456789 
1. 降低下载延迟 DOWNLOAD_DELAY = 0 将下载延迟设置为 0，同时加入随机
User-Agent 是所必要的，这个是一开始就要进行设置的 
2. 多线程 
3. CONCURRENT_REQUESTS = 100 
4. CONCURRENT_REQUESTS_PER_DOMAIN = 100 
5. CONCURRENT_REQUESTS_PER_IP = 100 
6. 123 
scrapy 框架是基于多线程 Twisted，当然 scrapy 也是通过多线程进行数据请求的，
并且支持多核 CPU 的并发，我们就可以通过设置并发请求数来提高爬取速度。 
7. 禁止使用 Cookies COOKIES_ENABLED = False 大部分情况下静止使用 Cookies 可
以防止被 ban。 --------------------------------------- 下面是个人信息 -------------------
----------------------------- 
 
 
 
描述下 scrapy 框架运行的机制？ 
从 start_urls 里获取第一批 url 并发送请求，请求由引擎交给调度器入请求队列，
获取完毕后，调度器将请求队列里的请求交给下载器去获取请求对应的响应资源，
并将响应交给自己编写的解析方法做提取处理：1. 如果提取出需要的数据，则交
给管道文件处理；2. 如果提取出 url，则继续执行之前的步骤（发送 url 请求，并
由引擎将请求交给调度器入队列…)，直到请求队列里没有请求，程序结束。 
 
 
 
scrapy 和 scrapy-redis 有什么区别？为什么选择 redis 数据库？ 
（1）scrapy 是一个 Python 爬虫框架，爬取效率极高，具有高度定制性，但是不
支持分布式。而 scrapy-redis 一套基于 redis 数据库、运行在 scrapy 框架之上的组
件，可以让 scrapy 支持分布式策略，Slaver 端共享 Master 端 redis 数据库里的
item 队列、请求队列和请求指纹集合。 
（2）为什么选择 redis 数据库，因为 redis 支持主从同步，而且数据都是缓存在
内存中的，所以基于 redis 的分布式爬虫，对请求和数据的高频读取效率非常高。 
 
 
 
你用过的爬虫框架或者模块有哪些？谈谈他们的区别或者优缺
点？ 
Python 自带：urllib，urllib2 第 三 方：requests 框 架：Scrapy urllib 和 urllib2 模
块都做与请求 URL 相关的操作，但他们提供不同的功能。 urllib2.：urllib2.urlopen
可以接受一个 Request 对象或者 url，（在接受 Request 对象时候，并以此可以来
设置一个URL 的 headers），urllib.urlopen 只接收一个 url urllib 有urlencode,urllib2
没有，因此总是 urllib，urllib2 常会一起使用的原因 scrapy 是封装起来的框架，
他包含了下载器，解析器，日志及异常处理，基于多线程， twisted 的方式处理，
对于固定单个网站的爬取开发，有优势，但是对于多网站爬取 100 个网站，并发
及分布式处理方面，不够灵活，不便调整与括展。 
request 是一个 HTTP 库， 它只是用来，进行请求，对于 HTTP 请求，他是一个
强大的库，下载，解析全部自己处理，灵活性更高，高并发与分布式部署也非常
灵活，对于功能可以更好实现. 
Scrapy 优缺点： 
优点：scrapy 是异步的 
采取可读性更强的 xpath 代替正则 
强大的统计和 log 系统 
同时在不同的 url 上爬行 
支持 shell 方式，方便独立调试 
写 middleware,方便写一些统一的过滤器 
通过管道的方式存入数据库 
缺点：基于 python 的爬虫框架，扩展性比较差 
基于 twisted 框架，运行中的 exception 是不会干掉 reactor，并且异步框架出错后
是不会停掉其他任务的，数据出错后难以察觉。 
 
 
 
算法与数据结构 
算法 
 
 
网络编程 
<1>. socket 
 
# python 获取本机主机名和 ip 地址` 
import socket 
hostname = socket.gethostname()   # PC201910292213 
print(socket.gethostbyname(hostname)) # 192.168.31.58 
print(socket.gethostbyname_ex(socket. ()))  # 获得当前主机的所有 ip 列表 
# ('PC201910292213', [], ['172.30.64.1', '192.168.176.17', '192.168.73.1', '192.168.49.1', 
'192.168.31.58']) 
 
# 使用 python 标准库写出本地 ip 地址 
[l for l in (lis, [[(s.connect(('8.8.8.8', 53)),s.getsockname()[0], s.close()) for s in 
[socket.socket(socket.AF_INET,socket.SOCK_DGRAM)]][0][1]]) if l][0][0] 
 
 
 
# 使用 socketserver 库创建一个 TCP 服务器 
class EchoHandler(BaseRequestHandler): 
    def handle(self) -> None: 
        print('Got connection from', self.client_address) 
        while True: 
            msg = self.request.recv(8192) 
            if not msg: 
                break 
            self.request.send(msg) 
 
 
if __name__ == "__main__": 
    server = TCPServer(('', 20000), EchoHandler) 
    server.serve_forever() 
定义了一个特殊的处理类，实现了一个 handler 方法，用来为客户端连接服务。 
request 属性是客户端 socket,client_address 有客户端地址; 
打开另外的 python 进程可以访问这个服务器; 
s = socket.socket(AF_INET, SOCK_STREAM) 
s.connect(('localhost', 20000)) 
s.send(b'hello')---必须是字节类型的; 
s.recv(8192) 
# s.sendto(b'hello', ('localhost', 20000)) 
# s.recvfrom(128) 
注意：一般情况下这种服务器是单线程的，一次只能为一个客户端提供服务； 
如果想处理多个客户端，可以初始化一个 ForkingTCPServer 或者 ThreadingTCPServer 对
象； 
 
from socket import socket, AF_INET, SOCK_DGRAM 
s = socket(AF_INET, SOCK_DGRAM) 
s.sendto(b'', ('localhost', 20000)) 
 
 
# ipaddress 使用 
CIDR 网络地址转换成所代表的所有 IP----net = ipaddress.ip_network("10.42.115.0/24"); 
允许像数组一样索引取值----net[0],net.num_address---地址数; 
创建一个 ip----address = ipaddress.ip_address('10.42.115.99'); 
执行网路成员检查----address in net--True; 
一个 ip 地址和网络地址能通过一个 ip 接口来指定: 
inet = ipaddress.ip_interface('10.42.115.23/24'); 
inet.network = '10.42.115.23/24'; 
inet.ip = '10.42.115.23'; 
 
 
# ssl 模块为底层 socket 连接添加 SSL 的支持 
 
 
# 事件驱动 I/O 
事件驱动 I/O 本质上来讲就是将基本 I/O 操作（比如读和写）转化为你程序需要处理
的事件。 
例如，当数据在某个 socket 上被接受后，它会转换成一个 receive 事件， 
然后被你定义的回调方法或函数来处理。 
select.select()调用，它会不断轮询文件描述符从而激活它。 
在调用 select()之前，时间循环会询问所有的处理器来决定哪一个想接受或发生。 
然后它将结果列表提供给 select()。然后 select()返回准备接受或发送的对象组成的列表。 
 
 
# 创建一个简单 tcp 服务器需要的流程 
1.socket 创建一个套接字 
2.bind 绑定 ip 和 port 
3.listen 使套接字变为可以被动链接 
4.accept 等待客户端的链接 
5.recv/send 接收发送数据 
 
 
 
####  
server 端： 
import socket 
import threading 
from concurrent.futures import ThreadPoolExecutor 
 
 
def communicate(conn): 
    while True: 
        try: 
            data = conn.recv(1024) 
            if not data: 
                break 
            print('Client Data:', data.decode('utf-8')) 
            conn.send(data.upper()) 
        except ConnectionResetError: 
            break 
 
 
def server(ip, post): 
    server = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
    server.bind((ip, post)) 
    server.listen(5) 
    while True: 
        conn, addr = server.accept() 
        # t = threading.Thread(target=communicate, args=(conn, )) 
        # t.start() 
        pool.submit(communicate, conn) 
 
 
if __name__ == '__main__': 
    # 最多可开 client 端为 2 个 
    pool = ThreadPoolExecutor(2) 
    server('127.0.0.1', 8000) 
 
 
 
client 端： 
import socket 
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM) 
client.connect(('127.0.0.1', 8000)) 
 
while True: 
    msg = input('>>>:').strip() 
    if not msg: 
        continue 
    client.send(msg.encode('utf-8')) 
    data = client.recv(1024) 
    print('Server Data:', data.decode('utf-8')) 
 
     
     
# 判断 ip 地址是否有效 
import socket 
def test_valid_ip_address(addr): 
 
    try: 
        socket.inet_aton(addr) 
        print('valid ip address') 
    except socket.error: 
        print('invalid ip address') 
 
 
addr1 = '125.23.22.2356' 
addr2 = '125.23.22.215' 
test_valid_ip_address(addr1) # invalid ip address 
test_valid_ip_address(addr2) # valid ip address 
 
**我们在线程池设置了最多可以有 2 个客户端与服务器端通信，所以当第三个客户端试
图去与服务器端建立链接时是没有用的，只有当其中的一个客户端停掉才能通信** 
 
 
 
## <a name='-1'></a>异常 
AttributeError 试图访问一个对象没有的树形，比如 foo.x，但是 foo 没有属性 x 
IOError 输入/输出异常；基本上是无法打开文件 
ImportError 无法引入模块或包；基本上是路径问题或名称错误 
IndentationError 语法错误（的子类） ；代码没有正确对齐 
IndexError 下标索引超出序列边界，比如当 x 只有三个元素，却试图访问 x[5] 
KeyError 试图访问字典里不存在的键 
KeyboardInterrupt Ctrl+C 被按下 
NameError 使用一个还未被赋予对象的变量 SyntaxError Python 代码非法，代码不能编
译(个人认为这是语法错误，写错了）TypeError 传入对象类型与要求的不符合 
UnboundLocalError 试图访问一个还未被设置的局部变量，基本上是由于另有一个同名
的全局变量，导致你以为正在访问它 
ValueError 传入一个调用者不期望的值，即使值的类型是正确的 
SyntaxError:Python 代码逻辑语法出错，不能执行 


 
## <a name='-1'></a>设计模式 
### <a name='-1'></a>单例模式的应用场景有那些？ 
单例模式应用的场景一般发现在以下条件下：资源共享的情况下，避免由于资源操作时导致的性能或损耗等，如日志文件，应用配置。 控制资源的情况下，方便资源之间的互相通信。如线程池等， 
1.网站的计数器 
2.应用配置 
3.多线程池 
4.数据库配置 数据库连接池 
5.应用程序的日志应用... 

### <a name='-1'></a>代理模式
代理模式是给某一个对象提供一个代理，并由代理对象控制对原对象的引用。 
优点： 代理模式能够协调调用者和被调用者，在一定程度上降低了系统的耦合度；可以灵活地隐藏被代理对象的部分功能和服务，也增加额外的功能和服务。 
缺点： 由于使用了代理模式，因此程序的性能没有直接调用性能高； 使用代理模式提高了代码的复杂度。 
黄牛卖火车票：没有流行网络购票的年代是很喜欢找黄牛买火车票的，因为工作忙的原因，没时间去买票，然后就托黄牛给你买张回家过年的火车票。 这个过程中黄牛就是代理人，火车票就是被代理的对象。
婚姻介绍所：婚姻介绍所的工作人员，搜集单身人士信息，婚介所的工作人员为这个单身人士找对 象，这个过程也是代理模式的生活案例。对象就是被代理的对象。

静态代理模式中，代理类在编译时就已经确定了；代理类和被代理类实现同一个接口或继承同一个父类，以便代理类可以代理被代理类的行为。
在动态代理模式中，代理类在运行时动态生成。动态代理模式通过反射或字节码操作等方式，在运行时创建代理对象，无需预先定义代理类。

代码实现就是代理类和被代理类继承同一类，代理类实例化时带上被代理类，实际调用时就是在代理类中调用被代理类的方法；
```python
from abc import ABC, abstractmethod 
 
 
class Subject(ABC): 
    @abstractmethod 
    def request(self) -> None: 
        pass 
 
 
class RealSubject(Subject): 
    def request(self) -> None: 
        print("RealSubject: Handling request.") 
 
 
class Proxy(Subject): 
    def __init__(self, real_subject: RealSubject) -> None: 
        self._real_subject = real_subject 
 
    def request(self) -> None: 
        if self.check_access(): 
            self._real_subject.request() 
            self.log_access() 
 
    def check_access(self) -> bool: 
        print("Proxy: Checking access prior to firing a real request.") 
        return True 
 
    def log_access(self) -> None: 
        print("Proxy: Logging the time of request.", end="") 
 
 
def client_code(subject: Subject) -> None: 
    # ... 
    subject.request() 
    # ... 
 
 
if __name__ == "__main__": 
    print("Client: Executing the client code with a real subject:") 
    real_subject = RealSubject() 
    client_code(real_subject) 
 
    print("") 
 
    print("Client: Executing the same client code with a proxy:") 
    proxy = Proxy(real_subject) 
    client_code(proxy)
```

### <a name='-1'></a>工厂模式
该模式旨在创建对象而无需指定具体类。我们可以通过定义一个工厂函数或者类来实现它。
例如，假设我们正在创建一个游戏，需要创建多个不同类型的角色对象，可以使用工厂模式来创建这些对象。工厂模式的优点是可以将对象的创建和使用分离，使得代码更加灵活和可扩展；缺点是增加了代码的复杂度和工作量，需要额外编写工厂类。 
```python
from abc import ABC, abstractmethod 
 
class Character(ABC): 
    @abstractmethod 
    def say_hello(self): 
        pass 
 
class Knight(Character): 
    def say_hello(self): 
        print("I'm a knight!") 
 
class Mage(Character): 
    def say_hello(self): 
        print("I'm a mage!") 
 
class CharacterFactory: 
    def create_character(self, character_type): 
        if character_type == 'knight': 
            return Knight() 
        elif character_type == 'mage': 
            return Mage() 
        else: 
            raise ValueError(f'Unknown character type {character_type}') 
 
# Create some characters using the factory 
factory = CharacterFactory() 
knight = factory.create_character('knight') 
mage = factory.create_character('mage') 
 
knight.say_hello()  # Output: "I'm a knight!" 
mage.say_hello()    # Output: "I'm a mage!"
```


### <a name='-1'></a>策略模式
在 Python 中实现策略模式，可以通过定义抽象策略类和具体策略类，以及包含策略对象的上下文类来完成。
具体代码实现如下：
```python
# 抽象策略类
class Strategy: 
    def do_strategy(self): 
        pass
 
# 具体策略类 1
class StrategyA(Strategy): 
    def do_strategy(self): 
        print("执行策略 A") 
 
# 具体策略类 2 
class StrategyB(Strategy): 
    def do_strategy(self): 
        print("执行策略 B") 
 
# 上下文类 
class Context: 
    def __init__(self, strategy): 
        self.strategy = strategy 
 
    def set_strategy(self, strategy): 
        self.strategy = strategy 
 
    def execute_strategy(self): 
        self.strategy.do_strategy()
```

在上述实现中，抽象策略类 Strategy 定义了一个抽象方法 do_strategy()，具体策略类 StrategyA 和 StrategyB 分别实现了该方法，上下文类 Context 包含一个策略对象，并提供了设置策略和执行策略方法。
策略模式的优点是能够更轻松地对算法进行切换和替换，使得代码更加灵活和可扩展；同时不需要创建额外的类，因此较为简洁。缺点是会使得代码变得更加分散和复杂，需要开发人员有较高的设计能力和抽象能力。 


说说策略模式在我们生活的场景？  
答：策略模式是指定义一系列算法，将每个算法都封装起来，并且使他们之间可以相互替换。 
优点：遵循了开闭原则，扩展性良好。 缺点：随着策略的增加，对外暴露越来越多。 条条大路通罗马，条条大路通北京。 我们去北京的交通方式（策略）很多，比如说坐飞机、坐高铁、自己开车等方式。每一种方式就可以理解为每一种策略。 这就是生活中的策略模式。  


### <a name='-1'></a>访问者模式
```python
from abc import ABC, abstractmethod


class Element(ABC):
    @abstractmethod
    def accept(self, visitor):
        pass


class ConcreteElementA(Element):
    def accept(self, visitor):
        visitor.visit_concrete_element_a(self)

    def operation_a(self):
        return "ConcreteElementA operation A."


class ConcreteElementB(Element):
    def accept(self, visitor):
        visitor.visit_concrete_element_b(self)

    def operation_b(self):
        return "ConcreteElementB operation B."


class Visitor(ABC):
    @abstractmethod
    def visit_concrete_element_a(self, element):
        pass

    @abstractmethod
    def visit_concrete_element_b(self, element):
        pass


class ConcreteVisitor1(Visitor):
    def visit_concrete_element_a(self, element):
        print("ConcreteVisitor1 visited ConcreteElementA.")
        print(element.operation_a())

    def visit_concrete_element_b(self, element):
        print("ConcreteVisitor1 visited ConcreteElementB.")
        print(element.operation_b())


class ConcreteVisitor2(Visitor):
    def visit_concrete_element_a(self, element):
        print("ConcreteVisitor2 visited ConcreteElementA.")
        print(element.operation_a())

    def visit_concrete_element_b(self, element):
        print("ConcreteVisitor2 visited ConcreteElementB.")
        print(element.operation_b())


class ObjectStructure:
    def __init__(self):
        self._elements = []

    def attach(self, element):
        self._elements.append(element)

    def detach(self, element):
        self._elements.remove(element)

    def accept(self, visitor):
        for element in self._elements:
            element.accept(visitor)


if __name__ == "__main__":
    object_structure = ObjectStructure()
    object_structure.attach(ConcreteElementA())
    object_structure.attach(ConcreteElementB())

    visitor1 = ConcreteVisitor1()
    visitor2 = ConcreteVisitor2()

    object_structure.accept(visitor1)
    print("-" * 20)
    object_structure.accept(visitor2)

```
这个例子中，我们先定义了两个元素类 ConcreteElementA 和 ConcreteElementB，它们都继承于 Element 抽象基类，并实现了 accept() 方法。接着定义了两个访问者类 ConcreteVisitor1 和 ConcreteVisitor2，它们都继承于 Visitor 抽象基类，并实现了 visit_concrete_element_a() 和 visit_concrete_element_b() 方法。
最后我们定义了一个对象结构类 ObjectStructure，用于管理元素对象，并向外提供访问接口。在这个类中，我们定义了 attach()、detach() 和 accept() 方法，分别用于添加/删除元素对象和接受访问者访问元素对象。
当我们创建完元素对象并添加到对象结构中后，可以通过调用 accept() 方法接受访问者的访问，这个过程中通过动态多态性调用相应的 visit_concrete_element_a() 和 visit_concrete_element_b() 方法，实现了访问者对元素的操作。
在这个示例中，我们创建了两个不同的访问者对象 visitor1 和 visitor2，它们分别对相同的元素对象进行不同的操作。最后我们通过调用 accept() 方法，分别接受两个访问者的访问，并输出结果。


### <a name='-1'></a>抽象工厂模式
答：抽象工厂模式是在简单工厂的基础上将未来可能需要修改的代码抽象出来，通过继承的方式让子类去做决定。 
比如，以上面的咖啡工厂为例，某天我的口味突然变了，不想喝咖啡了想喝啤酒，这个时候如果直接修改简单工厂里面的代码，这种做法不但不够优雅，也不符合软件设计的“开闭原则”，因为每次新增品 类都要修改原来的代码。
这个时候就可以使用抽象工厂类了，抽象工厂里只声明方法，具体的实现交给子类（子工厂）去实现，这个时候再有新增品类的需求，只需要新创建代码即可。

抽象工厂模式包含以下几个核心角色：
抽象工厂（Abstract Factory）：声明了一组用于创建产品对象的方法，每个方法对应一种产品类型。抽象工厂可以是接口或抽象类。
具体工厂（Concrete Factory）：实现了抽象工厂接口，负责创建具体产品对象的实例。
抽象产品（Abstract Product）：定义了一组产品对象的共同接口或抽象类，描述了产品对象的公共方法。
具体产品（Concrete Product）：实现了抽象产品接口，定义了具体产品的特定行为和属性。

```python
# Python原生默认不支持接口，默认多继承，所有的方法都必须不能实现
from abc import  abstractmethod,ABCMeta

# 创建一个接口Shape
class Shape(metaclass=ABCMeta):
    @abstractmethod
    def draw(self):
        pass
#创建Shape的实体类
class Rectangle(Shape):
    def draw(self):
        print("Inside Rectangel:draw() method.")

class Square(Shape):
    def draw(self):
        print("Inside Square:draw() method.")

class Circle(Shape):
    def draw(self):
        print("Inside Circle:draw() method.")

# 创建一个接口Color
class Color(metaclass=ABCMeta):
    @abstractmethod
    def fill(self):
        pass
# 创建Color的实体类
class Red(Color):
    def fill(self):
        print("Inside Red.fill() method.")

class Green(Color):
    def fill(self):
        print("Inside Green.fill() method.")

class Blue(Color):
    def fill(self):
        print("Inside Blue.fill() method.")

#创建抽象工厂
class AbstractFactory(metaclass=ABCMeta):
    @abstractmethod
    def getColor(self,color):
        pass

    @abstractmethod
    def getShape(self,shape):
        pass

#创建抽象工厂实例 ShapeFactory,ColorFactory
class ShapeFactory(AbstractFactory):
    def getShape(self,shapeType):
        if shapeType == None :
            return None
        elif shapeType.upper() == "CIRCLE":
            return Circle()
        elif shapeType.upper() == "RECTANGLE":
            return Rectangle()
        elif shapeType.upper() == "SQUARE":
            return Square()
        return None
    def getColor(self,colorType):
        pass

class ColorFactory(AbstractFactory):
    def getShape(self,shapeType):
        pass
    def getColor(self,colorType):
        if colorType == None:
            return None
        elif colorType.upper() == "RED":
            return Red()
        elif colorType.upper() == "GREEN":
            return Green()
        elif colorType.upper() == "BLUE":
            return Blue()
        return None

# 创建工厂创造器/生产器类
class FactoryProducer(object):
    @staticmethod
    # 这里不能写成 def getFactory(self,choiceType): 否则会报错
    # 因为是静态方法，被直接调用，所以不能带self参数
    # 如果不是静态方法，必须加self参数，且需要先实例化对象，再用实例化的对象调用方法
    def getFactory(choiceType):
        if choiceType.upper() == "SHAPE":
            return ShapeFactory()
        elif choiceType.upper() == "COLOR":
            return ColorFactory()
        return None

# 调用输出
if __name__ == '__main__':
    shapeFactory = FactoryProducer.getFactory('SHAPE')
    shape1 = shapeFactory.getShape("CIRCLE")
    shape1.draw()
    shape2 = shapeFactory.getShape("RECTANGLE")
    shape2.draw()
    shape3 = shapeFactory.getShape("SQUARE")
    shape3.draw()

    colorFactory = FactoryProducer.getFactory("COLOR")
    color1 = colorFactory.getColor("RED")
    color1.fill()
    color2 = colorFactory.getColor("Green")
    color2.fill()
    color3 = colorFactory.getColor("BLUE")
    color3.fill()
```

### <a name='-1'></a>装饰器模式
装饰器模式是指动态地给一个对象增加一些额外的功能，同时又不改变其结构。 优点：装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。 
装饰器模式的关键：装饰器中使用了被装饰的对象。比如，创建一个对象“laowang”，给对象添加不同的装饰，穿上夹克、戴上帽子......，这个执行过程就是装饰者模式。
代理模式和装饰器模式有什么区别？
答：都是结构型模式，代理模式重在访问权限的控制，而装饰器模式重在功能的加强。  
装饰器模式通过将对象包装在装饰器类中，以便动态地修改其行为。
```python
# Decorator Pattern with Python Code
from abc import  abstractmethod,ABCMeta

# 创建Shape接口
class Shape(metaclass=ABCMeta):
    @abstractmethod
    def draw(self):
        pass
# 实现Shape的实体类：Rectangle、Circle
class Rectangle(Shape):
    def draw(self):
        print("Shape: Rectangle")
class Circle(Shape):
    def draw(self):
        print("Shape: Circle")

# 创建实现了Shape接口的抽象装饰类ShapeDecorator类
class ShapeDecorator(Shape):
    _decoratedShape = None
    def __init__(self,inDecoratedShape):
        self._decoratedShape = inDecoratedShape
    def draw(self):
        self._decoratedShape.draw()

# 创建扩展了ShapeDecorator类的实体装饰类对象
class RedShapeDecorator(ShapeDecorator):
    def __init__(self,inDecoratedShape):
        ShapeDecorator.__init__(self,inDecoratedShape)
    def draw(self):
        self._decoratedShape.draw()
        self.setRedBorder(self._decoratedShape)
    def setRedBorder(self,inDecoratedShape):
        print("Border Color: Red")

# 调用输出
if __name__ == '__main__':
    aCircle = Circle()
    aRedCircle = RedShapeDecorator(Circle())
    aRedRectangle = RedShapeDecorator(Rectangle())

    print("Circle with normal border")
    aCircle.draw()
    print("\nCircle of red border")
    aRedCircle.draw()
    print("\nRectangle of red border")
    aRedRectangle.draw()
```


### <a name='-1'></a>模板方法模式
答：模板方法模式是指定义一个算法骨架，将具体内容延迟到子类去实现。 
优点： 提高代码复用性：将相同部分的代码放在抽象的父类中，而将不同的代码放入不同的子类中； 
实现了反向控制：通过一个父类调用其子类的操作，通过对子类的具体实现扩展不同的行为，实现了反向控制并且符合开闭原则。 
喝茶茶：烧水----放入茶叶---喝茶。放入的茶叶每个人自己的喜好不一样，有的是普洱、有的是铁观 音等。 每日工作：上班打卡----工作---下班打卡。每个人工作的内容不一样，后端开发的、前端开发、测 试、产品每个人的工作内容不一样。  
```python
# Template Pattern code with Python
from abc import abstractmethod,ABCMeta

# 创建一个game父类
class Game(metaclass=ABCMeta):

    # 把几乎不变的公共部分代码集中在父亲类，比如showcopyright
    # 此例子中，初始化、开始、结束三个方法，除了游戏名以外，都一样，所以把共性部分放在父类
    def initialize(self):
        print("%s Game Initialized! Start Playing." %self.setGameName())
    def startPlay(self):
        print("%s Game Started. Enjoy the game!" % self.setGameName())
    def endPlay(self):
        print("%s Game Finished! \n" % self.setGameName())
    def play(self):
        self.initialize()
        self.startPlay()
        self.endPlay()

    # 每个游戏子类的抽象部分仅仅是游戏名称的设定
    @abstractmethod
    def setGameName(self):
        pass

class Cricket(Game):
    def setGameName(self):
        return  "Cricket"

class Football(Game):
    def setGameName(self):
        return "Football"

# 调用输出
if __name__ == '__main__':
    game1 = Cricket()
    game1.play()
    game2 = Football()
    game2.play()
```

### <a name='-1'></a>享元模式
答：顾名思义就是被共享的单元。享元模式的意图是复用对象，节省内存，前提是享元对象是不可变对象。 
具体来讲，当一个系统中存在大量重复对象的时候，如果这些重复的对象是不可变对象，我们就可以利用享元模式将对象设计成享元，在内存中只保留一份实例，供多处代码引用。
这样可以减少内存中对象的数量，起到节省内存的目的。 
享元模式和单例模式的区别？  
答：单例模式是创建型模式，重在只能有一个对象。而享元模式是结构型模式，重在节约内存使用，提升程序性能。 享元模式：把一个或者多可对象霍村起来，用的时候，直接从缓存里获取。也就是说享元模式不一 定只有一个对象。
```python
# Flyweight Pattern with Python Code
from abc import abstractmethod,ABCMeta
import random

#创建一个Shape接口
class Shape(metaclass=ABCMeta):
    @abstractmethod
    def draw(self):
        pass
# 创建实现接口的实体类Circle
class Circle(Shape):
    _color=""
    _x =0
    _y =0
    _radius = 0

    def __init__(self, inColor):
        self._color = inColor
    def setX(self, inX):
        self._x = inX
    def setY(self,inY):
        self._y = inY
    def setRadius(self,inRadius):
        self._radius = inRadius
    def draw(self):
        print("Circle: Draw() [Color : {0}, x : {1}, y : {2}, radius : {3}]".format(self._color,self._x,self._y,self._radius))

# 创建一个工厂，生成基于给定信息的实体类的对象
class ShapeFactory():
    circleMap = {}
    @staticmethod
    def getCircle(inColor):
        circle = None
        if inColor in ShapeFactory.circleMap :
            circle = ShapeFactory.circleMap[inColor]
        else:
            circle = Circle(inColor)
            aCircle = {inColor:circle}
            ShapeFactory.circleMap.update(aCircle)
            print("Creating circle of color : " + inColor)
        return circle
# 调用输出
if __name__ == '__main__':
    colors = ["Red","Green","Blue","White","Black"]

    def getRandomColor():
        return colors[random.randint(0,len(colors)-1)]
    def getRandom():
        return random.randint(1,100)

    for i in range(20):
        ranColor = getRandomColor()
        circle = ShapeFactory.getCircle(ranColor)
        circle.setX(getRandom())
        circle.setY(getRandom())
        circle.setRadius(getRandom())
        circle.draw()
```

### <a name='-1'></a>责任链模式
是行为型设计模式之一，其将链中每一个节点看作是一个对象，每个节点处理的请求均不同，且内部自动维护一个下一节点对象。
当一个请求从链式的首端发出时，会沿着链的路径依次传递给 每一个节点对象，直至有对象处理这个请求为止。 
优点：解耦了请求与处理；请求处理者（节点对象）只需关注自己感兴趣的请求进行处理即可，对于不感兴趣的请求，直接转发给下一级节点对象； 具备链式传递处理请求功能，请求发送者无需知晓链路结构，只需等待请求处理结果； 链路结构灵活，可以通过改变链路结构动态地新增或删减责任； 易于扩展新的请求处理类（节点），符合 开闭原则；
缺点 责任链路过长时，可能对请求传递处理效率有影响； 如果节点对象存在循环引用时，会造成死循环，导致系统崩溃； 
生活案列：我们在公司内部发起一个 OA 审批流程，项目经理审批、部门经理审批。老板审批、人力审批。这就是生活中的责任链模式，每个角色的责任是不同。 SpringMVC 中的拦截器和 Mybatis 中的插件机制，都是拦截器经典实现。  
```python
# Chain of Responsibility Pattern with Python Code
from abc import  abstractmethod,ABCMeta


# 创建抽象的记录器类
class AbstractLogger(metaclass=ABCMeta):
    INFO = 1
    DEBUG = 2
    ERROR = 3
    _level = 0
    #责任链中的下一个元素
    _nextLogger = None

    def setNextLogger(self,inNextLogger):
        self._nextLogger = inNextLogger

    def logMessage(self,inLevel,inMessage):
        if self._level <= inLevel :
            self.write(inMessage)
        if self._nextLogger is not None :
            self._nextLogger.logMessage(inLevel,inMessage)

    @abstractmethod
    def write(self,inMessage):
        pass


# 创建扩展了该记录器类的实体类
class ConsoleLogger(AbstractLogger):
    def __init__(self,inLevel):
        self._level = inLevel

    def write(self,inMessage):
        print("Standard Console::Logger: "+inMessage)


class ErrorLogger(AbstractLogger):
    def __init__(self,inLevel):
        self._level = inLevel

    def write(self,inMessage):
        print("Error Console::Logger: "+inMessage)


class FileLogger(AbstractLogger):
    def __init__(self,inLevel):
        self._level = inLevel

    def write(self,inMessage):
        print("File::Logger: "+inMessage)


# 创建不同类型的记录器，赋予它们不同的错误级别，并在每个记录器中设置下一个记录器。每个记录器中的下一个记录器代表的是链的一部分
if __name__ == '__main__':
    errorLogger = ErrorLogger(AbstractLogger.ERROR)
    fileLogger = FileLogger(AbstractLogger.DEBUG)
    consoleLogger = ConsoleLogger(AbstractLogger.INFO)

    errorLogger.setNextLogger(fileLogger)
    fileLogger.setNextLogger(consoleLogger)

    loggerChain = errorLogger
    loggerChain.logMessage(AbstractLogger.INFO,"This is an information.")
    loggerChain.logMessage(AbstractLogger.DEBUG,"This is a debug level information")
    loggerChain.logMessage(AbstractLogger.ERROR,"This is an error information.")
    
# Standard Console::Logger: This is an information.
# File::Logger: This is a debug level information
# Standard Console::Logger: This is a debug level information
# Error Console::Logger: This is an error information.
# File::Logger: This is an error information.
# Standard Console::Logger: This is an error information.

```


### <a name='-1'></a>适配器模式
答：适配器模式是将一个类的接口变成客户端所期望的另一种接口，从而使原本因接口不匹配而无法一起工作的两个类能够在一起工作。 
优点： 可以让两个没有关联的类一起运行，起着中间转换的作用； 灵活性好，不会破坏原有的系统。 
缺点：过多地使用适配器，容易使代码结构混乱，如明明看到调用的是 A 接口，内部调用的却是 B 接口的实现。 
生活中的插座，为了适应各种插头，然后上面有两个孔的，三个空的，基本都能适应。还有万能充电器、USB 接口等。
```python
#Adapter Pattern with Python Code
from abc import abstractmethod,ABCMeta

# 为媒体播放器和更高级的媒体播放器创建接口
class MediaPlayer(metaclass=ABCMeta):
    @abstractmethod
    def play(self, strAudioType, strFilename):
        pass

class AdvancedMediaPlayer(metaclass=ABCMeta):
    @abstractmethod
    def playVlc(self,strFilename):
        pass
    @abstractmethod
    def playMp4(self,strFilename):
        pass

# 实现AdvancedMediaPlayer接口的实体类
class VlcPlayer(AdvancedMediaPlayer):
    def playVlc(self,strFilename):
        print("Playing vlc file. Name: "+strFilename)
    def playMp4(self,strFilename):
        pass

class Mp4Player(AdvancedMediaPlayer):
    def playVlc(self,strFilename):
        pass
    def playMp4(self,strFilename):
        print("Playing MP4 file. Name: " + strFilename)

# 实现MediaPlayer的MediaAdapter实体类
class MediaAdapter(MediaPlayer):
    advancedMusicPlayer = None

    def __init__(self,strAudioType):
        strAudioType = str.lower(strAudioType)
        if strAudioType == "vlc" :
            self.advancedMusicPlayer = VlcPlayer()
        elif strAudioType == "mp4" :
            self.advancedMusicPlayer = Mp4Player()
    def play(self,strAudioType,strFilename):
        if strAudioType == "vlc" :
            self.advancedMusicPlayer.playVlc(strFilename)
        elif strAudioType == "mp4" :
            self.advancedMusicPlayer.playMp4(strFilename)

# 实现MediaPlayer的AudiPlayer实体类
class AudioPlayer(MediaPlayer):
    mediaAdapter = None
    def play(self,strAudioType,strFilename):
        strAudioType = str.lower(strAudioType)
        # 播放MP3音乐文件
        if strAudioType == "mp3" :
            print("Playing mp3 file. Name: "+ strFilename)
        elif (strAudioType == "vlc") or (strAudioType == "mp4") :
            self.mediaAdapter = MediaAdapter(strAudioType)
            self.mediaAdapter.play(strAudioType,strFilename)
        else :
            print("Invalid media. "+ strAudioType + " format not supported.")

# 调用输出
if __name__ == '__main__':
    audioPlayer = AudioPlayer()

    audioPlayer.play("mp3","beyond the horizon.mp3")
    audioPlayer.play("mp4","alone.mp4")
    audioPlayer.play("vlc", "far far away.vlc")
    audioPlayer.play("avi", "mind me.avi")
```


### <a name='-1'></a>观察者模式
答：观察者模式是定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。
观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。 
优点： 观察者模式可以实现表示层和数据逻辑层的分离，并定义了稳定的消息更新传递机制，抽象了更新接口，使得可以有各种各样不同的表示层作为具体观察者角色； 
观察者模式在观察目标和观察者之间建立一个抽象的耦合； 观察者模式支持广播通信； 
观察者模式符合开闭原则（对拓展开放，对修改关闭）的要求。
缺点： 如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间； 
如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃； 
观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。 

在观察者模式中有如下角色： 
Subject：抽象主题（抽象被观察者），抽象主题角色把所有观察者对象保存在一个集合里，每个主题都可以有任意数量的观察者，抽象主题提供一个接口，可以增加和删除观察者对象；
ConcreteSubject：具体主题（具体被观察者），该角色将有关状态存入具体观察者对象，在具体主题的内部状态发生改变时，给所有注册过的观察者发送通知； 
Observer：抽象观察者，是观察者者的抽象类，它定义了一个更新接口，使得在得到主题更改通知时更新自己； 
ConcrereObserver：具体观察者，实现抽象观察者定义的更新接口，以便在得到主题更改通知 时更新自身的状态。

```python
# Observer Pattern with Python Code
from abc import abstractmethod,ABCMeta

# 创建一个目标对象Subject，如果有多种不同的目标，可以抽象subject，用子对象实现
class Subject:
    # 建立一个私有集合，存放观察者对象
    _observers = []
    _state = ""
    def getState(self):
        return self._state
    def setState(self,inState):
        self._state = inState
        self.notifyAllObservers()
    # 追加观察者
    def attach(self, inObserver):
        self._observers.append(inObserver)
    # 通知观察者
    def notifyAllObservers(self):
        for aObser in self._observers:
            aObser.update()

# 创建观察者抽象类
class Observer(metaclass=ABCMeta):
    subject = Subject()
    @abstractmethod
    def update(self):
        pass
    def __init__(self):
        self.subject = Subject()

# 实现具体观察者
class BinaryObserver(Observer):
    def __init__(self, inSubject):
        self.subject = inSubject
        self.subject.attach(self)

    def update(self):
        print("Binary String : " + str(bin(self.subject.getState())))

class OctalObserver(Observer):
    def __init__(self,inSubject):
        self.subject = inSubject
        self.subject.attach(self)
    def update(self):
        print("Octal String : " + str(oct(self.subject.getState())))

class HexaObserver(Observer):
    def __init__(self,inSubject):
        self.subject = inSubject
        self.subject.attach(self)
    def update(self):
        print("Hex String : " + str(hex(self.subject.getState())))

# 调用输出
if __name__ == '__main__':
    aSubject = Subject()

    HexaObserver(aSubject)
    OctalObserver(aSubject)
    BinaryObserver(aSubject)

    print("First state change : 15")
    aSubject.setState(15)
    print("======================")
    print("First state change : 10")
    aSubject.setState(10)
``` 



 
# python基础



## <a name='python-1'></a>python 编码 
文件以什么编码保存的，就以什么编码方式打开. 
python2 中默认使用 ascii，python3 中默认使用 utf-8。 
x="hello",其中的 x，等号，引号，地位都一样，都是普通字符而已，都是以 unicode
编码的二进制形式存放与内存中的.但是程序在执行过程中，会申请内存（与程序
代码所存在的内存是俩个空间），可以存放任意编码格式的数据。 比如 x="hello",
会被 python 解释器识别为字符串，会申请内存空间来存放"hello"，然后让 x 指向
该内存地址，此时新申请的该内存地址保存也是 unicode 编码的 hello； 如果代
码换成 x="hello".encode('utf-8'),那么新申请的内存空间里存放的就是 utf-8 编码
的字符串 hello 了。不同编码的字符串就是存放在不同的内存地址。 
python2 中有两种字符串类型 str 和 unicode 
str 类型 
   当 python 解释器执行到产生字符串的代码时（例如 s='林'），会申请新的
内存地址，然后将'林'编码成文件开头指定的编码格式，这已经是 encode 之后的
结果了，所以 s 只能 decode。再次 encode 就会报错。 
#_*_coding:gbk_*_ 
 
x='林' 
print x.encode('gbk') #报错 
print x.decode('gbk') #结果：林 
在 python2 中，str 就是编码后的结果 bytes，str=bytes,所以在 python2 中，unicode 字符
编码的结果是 str/bytes 
#coding:utf-8 
s='林' #在执行时,'林'会被以 conding:utf-8 的形式保存到新的内存空间中 
 
print repr(s) #'\xe6\x9e\x97' 三个 Bytes,证明确实是 utf-8 
print type(s) #<type 'str'> 
 
s.decode('utf-8') 
s.encode('utf-8') #报错，s 为编码后的结果 bytes，所以只能 decode 
  Unicode 类型 
  当 python 解释器执行到产生字符串的代码时（例如 s=u'林'），会申请新的
内存地址，然后将'林'以 unicode 的格式存放到新的内存空间中，所以 s 只能 encode，
不能 decode. 
s=u'林' 
print repr(s) #u'\u6797' 
print type(s) #<type 'unicode'> 
 
 
s.decode('utf-8') #报错，s 为 unicode，所以只能 encode 
s.encode('utf-8')  
  特别说明: 
  当数据要打印到终端时，要注意一些问题。 
  当程序执行时，比如:x='林';print(x) #这一步是将 x 指向的那块新的内存空间
（非代码所在的内存空间）中的内存，打印到终端，而终端仍然是运行于内存中
的，所以这打印可以理解为从内存打印到内存，即内存->内存，unicode->unicode.
对于 unicode 格式的数据来说，无论怎么打印，都不会乱码.python3 中的字符串
与 python2 中的 u'字符串'，都是 unicode，所以无论如何打印都不会乱码.在
windows 终端（终端编码为 gbk，文件编码为 utf-8，乱码产生） 
#分别验证在 pycharm 中和 cmd 中下述的打印结果 
s=u'林' #当程序执行时，'林'会被以 unicode 形式保存新的内存空间中 
 
 
2. <a name='unicodeencode'></a>指向的是 unicode，因而可以编码成任意格式，都不会报 encode 错误 
s1=s.encode('utf-8') 
s2=s.encode('gbk') 
print s1 #打印正常否？ 
print s2 #打印正常否 
 
 
print repr(s) #u'\u6797' 
print repr(s1) #'\xe6\x9e\x97' 编码一个汉字 utf-8 用 3Bytes 
print repr(s2) #'\xc1\xd6' 编码一个汉字 gbk 用 2Bytes 
 
print type(s) #<type 'unicode'> 
print type(s1) #<type 'str'> 
print type(s2) #<type 'str'> 
python3 中也有两种字符串类型 str 和 bytes 
python2 中，str 类型和 bytes 类型是同一种类型。以下语句在 python2 中等效： 
a = 'ab' 
a = b'ab' 
python3 中，str 类型和 unicode 类型是同一种类型。以下语句在 python3 中等效： 
a = 'ab' 
a = u'ab' 
python2 unicode 类似于 python3 str; 
python2 str 类似于 python3 bytes。 
str 类型变为 unicode 类型 
#coding:utf-8 
s='林' #当程序执行时，无需加 u，'林'也会被以 unicode 形式保存新的内存空间中, 
 
3. <a name='encode'></a>可以直接 encode 成任意码格式 
s.encode('utf-8') 
s.encode('gbk') 
 
print(type(s)) #<class 'str'> 
 
bytes 类型 
```python
x = '春香' 
print(x)  # 春香 
s = u'vdgfdghf 董' 
print('type(s)==%s' % type(s), 'id(s)==%s' % id(s), s) 
# type(s)==<class 'str'> id(s)==1782182021696 vdgfdghf 董 
s1 = s.encode('gbk') 
s2 = s.encode('utf-8') 
s3 = s2.decode() 
print('type(s1)==%s' % type(s1), 'id(s1)==%s' % id(s1), s1) 
# type(s1)==<class 'bytes'> id(s1)==1782182900816 b'vdgfdghf \xb6\xad' 
print('type(s2)==%s' % type(s2), 'id(s2)==%s' % id(s2), s2) 
# type(s2)==<class 'bytes'> id(s2)==1782182903408 b'vdgfdghf \xe8\x91\xa3' 
print('type(s3)==%s' % type(s3), 'id(s3)==%s' % id(s3), s3) 
# type(s3)==<class 'str'> id(s3)==1993071642976 vdgfdghf 董 

# 可以看到不同编码的字符串，存放在不同的内存地址，bytes 类型字符串在 python3 中，是什么就打印什么 
# s encode()之后再 decode()已经不是之前的 s 了，重新放置再另一块内存 

# python 进制 
# python 中二进制用 0b 加相应数字表示，八进制用 0o 加相应数字表示，十六进制用 0x加相应数字表示；bin()方法可以将其他进制的数转换成二进制，oct()将其他进制的数转换成八进制，hex()将其他进制的数转换成十六进制；int()转换成十进制；  
 
# 数字转换成二进制，并且指定位数，前面用 0 填充 
print(format(10, '08b')) # 00001010 
print(format(10, '010b')) # 0000001010 
 
# 十进制数字转换成十六进制 
print(format(30, '02x')) # 1e 
print(format(100, '02x')) # 64 
 
# python 运算 
print(20 // 8)  # 2   20 除以 8 的商 
 
# 在 Python3 中，/操作符是做浮点除法，而//是做整除（即商没有余数，比如 10//3 其结果就为 3，余数会被截除掉，而(-7)//3 的结果却是-3。这个算法与其它很多编程语言不一样，需要注意，它们的整除运算会向 0 的方向取值。 
# 在 Python2 中，/就是整除，即和 Python3 中的//操作符一样） 
 
# python 中的正无穷或负无穷，使用 float("inf")或 float("-inf")来表示。 
# 这里有点特殊，写成：float("inf")，float("INF")或者 float('Inf')都是可以的。 
# 当涉及 > 和 < 比较时，所有数都比无穷小 float("-inf")大，所有数都比无穷大 float("inf")小。 
# 相等比较时，float("+inf")与 float("+inf")、float("inf")三者相等。 
 

# 特别地，0 * float('inf') 结果为：nan 
float('inf') / float('inf')    # 结果为：nan 
float('inf') - float('inf')    # 结果为：nan 
float('-inf') - float('-inf')  # 结果也为：nan 
 
# nan 代表 Not A Number（不是一个数），它并不等于 0 因为 nan 不是一个数，所以相关计算都无法得到数字。 所有涉及 nan 的操作，返回的都是 nan。 

``` 
 
 
### <a name='python-1'></a>python 三元运算子 
[on true] if [expression] else [on false] 

### <a name='pythonand'></a>python 支持一个表达式进行多种比较操作，其实这个表达式本质是由多个隐式的 and
连接起来的多个表达式； 
```python
3<4<7  # same as "(3<4) and (4<7)" 

# 在不加括号时候, and 优先级大于 or 
# x or y 的值只可能是 x 或 y. x 为真就是 x, x 为假就是 y 
# x and y 的值只可能是 x 或 y. x 为真就是 y, x 为假就是 x
```

### <a name='pythonisisnot'></a>python 身份运算符 is 和 is not  
类型注解 
def add(x:int, y:int) -> int: 
   return x + y 
    
用 : 类型 的形式指定函数的参数类型，用 -> 类型 的形式指定函数的返回值类型。 
然后特别要强调的是，Python 解释器并不会因为这些注解而提供额外的校验，没有任何
的类型检查工作。也就是说，这些类型注解加不加，对你的代码来说没有任何影响，只
是类型检查，高亮显示； 
 
变量类型进行注解的方法： 
a: int = 123 
b: str = 'hello' 
python 比较 
复数不支持比较大小 
类似元组、字符串、列表这类格式，在进行两者之间的比较时，先从第一个元素
开始比较 ASCII 码值大小，如果相等，则依次向后比较，如果全部相等，则比
较数量大小。 
ASCII 码值大小： 
数字： 
0-9: 48-57 
字母： 
A-Z：65-90. 
a-z： 97-122 
一串数字、字符的 ASCII 码值大小取决于最后一位的 ASCII 码值。 
 
python 方法与函数 
_foo----用来指定私有变量的一种方式.不能用 from module import * 导入； 
__foo----这个有真正的意义:解析器用_classname__foo 来代替这个名字，以区别
和其他类相同的命名，它无法直接像公有成员一样随便访问，通过对象名._类名
__xxx 这样的方式可以访问. 


### <a name='-1'></a>如何判断一个值是方法还是函数？ 
1、 使用 type()来判断，如果是 method 为方法，如果是 function 则是函数。 
2、 与类和实例无绑定关系的 function 都属于函数（function） 
3、 与类和实例有绑定关系的 function 都属于方法 
 
 
 
### <a name='-1'></a>文档字符串 
在函数的第一个逻辑行的字符串是这个函数的文档字符串。
文档字符串的惯例是一个多行字符串，它的首行以大写字母开始，句号结尾。第二行是空行，从第三行开始是详细的描述，在函数中使用文档字符串时尽量遵循这个惯例。 
文档字符串是一个重要工具，用于解释文档程序 ，帮助你的程序文档更加简单易懂。 我们可以在函数体的第一行使用一对三个单引号 或者一对三个双引号来定义文档字符串。 你可以使用 __doc__调用函数中的文档字符串属性;


### <a name='-1'></a>了解类型注解么？ 
def list_to_str (param_list:list,connect_str: str = " ") - > str: 
    paas 
python3 中注解用来给参数， 返回值，变量的类型加上注解，对代码没影响 
Python 提供了一个工具方便我们测试类型注解的正确性 
pip install mypy mypy demo.py 若无错误则无输出 

 
### <a name='-1'></a>猴子补丁 
“猴子补丁”(monkey patching)就是指，在函数或对象已经定义之后，再去改变它们的行为。
指在运行时动态修改类或模块。运行时动态修改模块、类或函数，通常是添加功能或修正缺陷。猴子补丁在代码运行时内存中）发挥作用，不会修改源码，因此只对当前运行的程序实例有效。因为猴子补丁破坏了封装，而且容易导致程序与补丁代码的实现细节紧密耦合，所以被视为临时的变通方案，不是集成代码的推荐方式。 
举个例子： 
import datetime 
datetime.datetime.now = lambda: datetime.datetime(2012, 12, 12) 
 

 
### <a name='CythonPypyCpythonNumba'></a>介绍 Cython，Pypy Cpython Numba 各有什么缺点 
CPython 是使用最广的 Python 解释器。 
IPython 是基于 CPython 之上的一个交互式解释器，也就是说，IPython 只是在交互方式上有所增强 
PyPy 是另一个 Python 解释器，它的目标是执行速度。PyPy 采用 JIT 技术，对 Python 代码进行动态编译（注意不是解释），所以可以显著提高 Python 代码的执行速度。 绝大部分 Python 代码都可以在 PyPy 下运行，但是 PyPy 和 CPython
有一些是不同的，这就导致相同的 Python 代码在两种解释器下执行可能会有不同的结果。如果你的代码要放到 PyPy 下执行，就需要了解 PyPy 和 CPython 的。

不同点 
Jython Jython 是将 Python code 在 JVM 上面跑和调用 java code 的解释器。 

 
提高 python 运行效率的方法 
1、使用生成器，因为可以节约大量内存 
2、循环代码优化，避免过多重复代码的执行 
3、核心模块用 Cython PyPy 等，提高效率 
4、多进程、多线程、协程 
5、多个 if elif 条件判断，可以把最有可能先发生的条件放到前面写，这样可以减
少程序判断的次数，提高效率 
 
 
 
## <a name='python-1'></a>python 新式类和经典类的区别
a. 在 python 里凡是继承了 object 的类，都是新式类 
b. Python3 里只有新式类 
c. Python2 里面继承 object 的是新式类，没有写父类的是经典类 
d. 经典类目前在 Python 里基本没有应用 
e. 保持 class 与 type 的统一对新式类的实例执行 a.__class__与 type(a)的结果是一致的，对于旧式类来说就不一样了。 
f. 对于多重继承的属性搜索顺序不一样新式类是采用广度优先搜索(C3 算法)， 旧式类采用深度优先搜索; 
g. 新式类多继承搜索顺序(广度优先)：先在水平方向查找，然后再向上查找；经典类多继承搜索顺序(深度优先)：先深入继承树左侧查找，然后再返回，开始查找右侧； 
h. 新式类除了拥有经典类的全部特性之外，还有一些新的特性。比如__init__发生了变化，新增了静态方法__new__； 

python 之禅 
通过 import this 语句可以获取其具体的内容。它告诉大家如何写出高效整洁的代码。 
如何给变量加注释？ 
可以通过变量名：类型的方式如下 
a： str = "this is string type" 
例举几个规范 Python 代码风格的工具 
pylint 和 flake8 
列举 3 条以上 PEP8 编码规范 
《Python Enhancement Proposal #8》（8 号 Python 增强提案）又叫 PEP8 
1、顶级定义之间空两行，比如函数或者类定义； 
2、方法定义、类定义与第一个方法之间，都应该空一行； 
3、三引号进行注释； 
4、使用 Pycharm、Eclipse 一般使用 4 个空格来缩进代码。 
 
 
 
容器类型 
容器是一种把多个元素组织在一起的数据结构，容器中的元素可以逐个地迭代获
取。简单来说，就好比一个盒子,我们可以往里面存放数据，也可以从里面一个一
个地取出数据。 
在 python 中，属于容器类型地有:list,dict,set,str,tuple.....，容器仅仅只是用来存放
数据的，我们平常看到的 l = [1,2,3,4]等等，好像我们可以直接从列表这个容器
中取出元素，但事实上容器并不提供这种能力，而是可迭代对象赋予了容器这种
能力。 
python 内置数据类型 
a.数值类型： 
整型 int、长整型 long(Python3 中没有 long，只有无限精度的 int)、浮点型 float、 
复数 complex、布尔型 bool； 
b.序列对象 
字符串 str、列表 list、元祖 tuple； 
c.键值对 
字典 dict 、集合 set； 
list 列表 
有序可重复的元素集合，可嵌套、迭代、修改、分片、追加、删除，成员判断。 
Python 的列表是一个可变长度的顺序存储结构，每一个位置存放的都是对象的
指针。 
 
del lis[0] 
lis.pop()----pop 的是从后往前元素； 
map(str, list)----将列表中的元素转为 str，生成新的 list； 
list(dict)----使用 list()函数，一个字典作为参数，得到的是字典键值的列表； 
original_list[-1:] = target_list----将最后一个元素替换成一个列表； 
l=[1,2,3,4,5],执行 l[1:3]='abc'后，l 变成[1,'a','b','c',4,5]； 
list = ['a', 'b', 'c', 'd', 'e'] 
print list[10:] # 打印[]  尝试获取 list[10]和之后的成员，会导致 IndexError. 然而，尝试
获取列表的切片，开始的 index 超过了成员个数不会产生 IndexError,而是仅仅返回一个
空列表。 
列表中的元素可以根据位置传递给几个变量，这样使用必须变量数和元素数量一致： 
x, y, z = [1, 2, 3] 
print(x, y, z) 
[[5 * x + res for res in range(1, 6)] for x in range(0, 5)]----再一个列表中产生 5 组 5 个元素
的 list； 
ast.literal_eval(str)----将一个 str(类似与这样的 str--"['Red', 'Green', 'White']")转换成 list; 
all('target' == element for element in list)----判断 list 中是否有元素等于 target 
list(set().union(*original_list))----[[10, 20], [40], [30, 56, 25], [10, 20], [33], [40]]-->[33, 40, 
10, 20, 56, 25, 30] 
list(itertools.chain(*original_list))----将[[2,4,3],[1,5,6], [9], [7,9,0]]-->[2, 4, 3, 1, 5, 6, 9, 7, 9, 
0],也可以用如下方法： 
从一个列表中随机选择 n 个元素作为新的 list----random.sample(original_list, n) 
```python
list = [ [ ] ] * 5 
list  # output? 
list[0].append(10) 
list  # output? 
list[1].append(20) 
list  # output? 
list.append(30) 
list  # output? 
 
# [[], [], [], [], []] 
# [[10], [10], [10], [10], [10]] 
# [[10, 20], [10, 20], [10, 20], [10, 20], [10, 20]] 
# [[10, 20], [10, 20], [10, 20], [10, 20], [10, 20], 30] 
```
第一行的输出结果直觉上很容易理解，例如 list = [ [ ] ] * 5 就是简单的创造了 5 个空列
表。然而，理解表达式 list=[ [ ] ] * 5 的关键一点是它不是创造一个包含五个独立列表的
列表，而是它是一个创建了包含对同一个列表五次引用的列表。只有了解了这一点，我
们才能更好的理解接下来的输出结果。 
 
list[0].append(10) 将 10 附加在第一个列表上。 
但由于所有 5 个列表是引用的同一个列表，所以这个结果将是： 
[[10], [10], [10], [10], [10]] 
 
同理，list[1].append(20)将 20 附加在第二个列表上。但同样由于 5 个列表是引用的同一
个列表，所以输出结果现在是： 
[[10, 20], [10, 20], [10, 20], [10, 20], [10, 20]] 
 
作为对比， list.append(30)是将整个新的元素附加在外列表上，因此产生的结果是： [[10, 
20], [10, 20], [10, 20], [10, 20], [10, 20], 30] 
 
对 list 排序 foo = [-5,8,0,4,9,-4,-20,-2,8,2,-4],使用 lambda 函数从小到大排序 
>>> foo =  [-5,8,0,4,9,-4,-20,-2,8,2,-4] 
>>> res = sorted(foo, key=lambda x:x) 
>>> res 
[-20, -5, -4, -4, -2, 0, 2, 4, 8, 8, 9] 
 
 
使用 lambda 函数对 list 排序 foo = [-5,8,0,4,9,-4,-20,-2,8,2,-4]，输出结果为
[0,2,4,8,8,9,-2,-4,-4,-5,-20]，正数从小到大，负数从大到小 
（传两个条件，x<0 和 abs(x) 
 
>>> foo =  [-5,8,0,4,9,-4,-20,-2,8,2,-4] 
>>> res = sorted(foo, key=lambda x:(x<0, abs(x))) 
>>> res 
[0, 2, 4, 8, 8, 9, -2, -4, -4, -5, -20] 
 
python 实现列表去重的方法,先通过集合去重，再转列表
>>> l = [10,23,12,10,24,10,34,12] 
>>> s = set(l) 
>>> s 
{34, 10, 12, 23, 24} 
>>> [x for x in s] 
[34, 10, 12, 23, 24] 
```python
# 用 list 类的 sort 方法 
l1 = ['b','c','d','c','a','a'] 
l2 = list(set(l1)) 
l2.sort(key=l1.index) 
print(l2) 
# 用 sorted()函数 
l1 = ['b','c','d','c','a','a'] 
l2 = sorted(set(l1),key=l1.index) 
print(l2) 
# 也可以遍历 
l1 = ['b','c','d','c','a','a'] 
l2 = [] 
for i in l1: 
    if not i in l2: 
        l2.append(i) 
print(l2) 

```

list.extend(iterable)----表示将一个可迭代对象中的元素追加到列表中； 
itertools.combinations(list, num)----表示获取一个列表的元素个数为 num 的子 list; 
itertools.tee(iterable, num)----表示从一个迭代器中对象返回 num 个独立的迭代器； 
map(str, list)----将列表中的元素转为 str，生成新的 list； 
itertools.groupby(list, key=)----groupby()把迭代器中相邻的，指定特征重复的元素挑出来
放在一起; 
operator.itemgetter()----使用 itemgetter()参数规定分组依据; 
 
 
tuple 元组 
元组也是序列结构，但是是一种不可变序列，你可以简单的理解为内容不可变的
列表。 
元组对象进行重新赋值或者更新时会导致运行时异常； 
元组中不允许的操作，确切的说是元组没有的功能： 
修改、新增元素、删除某个元素（但可以删除整个元组）、所有会对元组内部元
素发生修改动作的方法。例如，元组没有 remove，append，pop 等方法。 
元组只保证它的一级子元素不可变，对于嵌套的元素内部，不保证不可变！ 
color1 = "Red", "Green", "Orange"   # 可以这样定义，color1 表示的是一个 tuple； 
x = () # 创建一个空元组； 
tuplex = 5 # 创建一个元素的元组 (5,) 
tuple.count(element) # 得到元组中 element 元素出现的次数； 
```python
for vlan_min, vlan_max in [(200, 300)]: 
      print(vlan_min, vlan_max)    
# output:200 300 
```
 
str 字符串 
字符串是不可变的序列数据类型，不能直接修改字符串本身，和数字类型一样！
Python3 全面支持 Unicode 编码，所有的字符串都是 Unicode 字符串。 

Python 的转义字符 
字符串前加 u、r、b 
u"中文字符组成的字符串" 
作用：以 Unicode 格式 进行编码，一般用在中文字符串前面，防止因为源码储存格式
问题，导致再次使用时出现乱码。 
r"\n\n\n\n”  # 表示一个普通生字符串 \n\n\n\n，而不表示换行 
作用：去掉反斜杠的转义机制，常用于正则表达式，对应着 re 模块。 
b’Hello World’   # 表示这是一个 bytes 对象 
作用：b" "前缀表示：后面字符串是 bytes 类型。在网络编程中，服务器和浏览器只认
bytes 类型数据。在 Python3 中，bytes 和 str 的互相转换方式是 str.encode(‘utf-8’)和
bytes.decode(‘utf-8’)。 
 
str.translate({32: None})----去掉 str 中的空格； 
s.translate(table)-----table 是字符映射转换表表，是通过 maketrans()方法转换而来的。 
intab = "aeiou" 
outtab = "12345" 
trantab1 = str.maketrans(intab,outtab) # 创建字符映射转换表,将 intab 映射成 outtab; 
之后使用 s.translate(trantabl1),将 s 转换； 
 
 
string.title()---所有单词的首字母大写，其余小写，非字母后的第一个字母将装换成大写字母； 
 
 
字符串的 strip()方法是用，当 strip()有参数时，这个参数可以理解成要删除的字符序列，是否被删除的前提是字符串的开头和结尾是不是包含要删除的字符，如果有就继续处理，没有的话就不会删除中间的字符的； 
 

a. count 使用 
```python
def test_count(): 
   s = 'The quick brown fox jumps over the lazy dog.' 
   print('the occurrence times of character %s is %s' % ('e', s.count('e'))) 
 
 
test_count() 
# the occurrence times of character e is 3 

``` 
 
b. 判断字符串是不是数字 
def test_string_whether_numeric(): 
   st = 'sd234' 
   try: 
       num = float(st) 
   except (ValueError, TypeError): 
       print('not numeric') 
        
        
c. 字符装换成一个 int 列表 
```python
by = b'ABm' 
print(list(by)) 
# [65, 66, 109]
``` 
 
d. unicode 字符 
print(u'\u0050\u0079\u0074\u0068\u006f\u006e')  # Python   
 
 
e. 两个相同的字符串指向同一内存地址 
```python
st1 = 'dong' 
st2 = 'dong' 
print('st1 的内存地址==%s\nst2 的内存地址==%s' % (hex(id(st1)), hex(id(st2)))) 
# st1 的内存地址==0x21b3f5dc4f0 
# st2 的内存地址==0x21b3f5dc4f0 

``` 
 
f. 字符串中添加尾随和前导零 
```python
def test_add_trailing_and_leading_zeroes_to_a_string(): 
   st = 'dgfr45sfry4'
   print('origin string---%s, len(st)---%s' % (st, len(st))) 
   st1 = st.ljust(15, '0')
   print('add trailing zeroes---', st1)
   st2 = st.ljust(15, '*') 
   print('add trailing *---', st2)
   st3 = st.rjust(15, '0')
   print('add leading zeroes---', st3)
   st4 = st.rjust(15, '*') 
   print('add leading zeroes---', st4) 


test_add_trailing_and_leading_zeroes_to_a_string() 
# origin string---dgfr45sfry4, len(st)---11 
# add trailing zeroes--- dgfr45sfry40000 
# add trailing *--- dgfr45sfry4**** 
# add leading zeroes--- 0000dgfr45sfry4 
# add leading zeroes--- ****dgfr45sfry4 
```
 
 
g. zfill 使用，接收一个数字，表示讲字符串前边填充 0.     
```python
def test_combination_3_digit(): 
   nums = [] 
   for x in range(15): 
       num = str(x).zfill(3) 
       nums.append(num) 
   return nums 
 
print(test_combination_3_digit()) 
# ['000', '001', '002', '003', '004', '005', '006', '007', '008', '009', '010', '011', '012', '013', '014'] 
```
 
 
h. 字符串中的 replace 方法 
st = string1.replace(old, new[, max])  # 会生成一个新对象返回,原来的字符串 string1 还是原来的值 
 
 
i. split 
```python
def get_last_part_string(st): 
   print(st.split('/')) 
   print(st.rsplit('/')) 
   print(st.split('/', 1)) 
   print(st.split('/', 2)) 
   print(st.split('/', 3)) 
   return st.rsplit('/', 1)[0], st.rsplit('-', 1)[0] 
# split(" ")解决不了单词间多空格的问题，s.split()可以解决 
# s = "a good   example" 
# s.split(" ") 
# ['a', 'good', '', '', 'example'] 
# s.split() 
# ['a', 'good', 'example'] 

# print(get_last_part_string('https://www.w3resource.com/python-exercises/string')) 
# output: 
# ['https:', '', 'www.w3resource.com', 'python-exercises', 'string'] 
# ['https:', '', 'www.w3resource.com', 'python-exercises', 'string'] 
# ['https:', '/www.w3resource.com/python-exercises/string'] 
# ['https:', '', 'www.w3resource.com/python-exercises/string'] 
# ['https:', '', 'www.w3resource.com', 'python-exercises/string'] 
# ('https://www.w3resource.com/python-exercises', 'https://www.w3resource.com/python') 
```
 
j. upper()与 lower() 
st.upper()  # 字符串全大写 
st.lower()  # 字符串全小写 
 
 
k. startswith() 
Python startswith() 方法用于检查字符串是否是以指定子字符串开头，如果是则返回 
True，否则返回 False。如果参数 beg 和 end 指定值，则在指定范围内检查。 
语法: 
startswith()方法语法： 
str.startswith(str, beg=0,end=len(string)); 
参数: 
str -- 检测的字符串。 
strbeg -- 可选参数用于设置字符串检测的起始位置。 
strend -- 可选参数用于设置字符串检测的结束位置。 
str = "this is string example....wow!!!"; 
print str.startswith( 'this' ); # True 
print str.startswith( 'is', 2, 4 );  # True
print str.startswith( 'this', 2, 4 );  # False
 
dic 字典 
映射是一种关联式的容器类型，它存储了对象与对象之间的映射关系，字典是
python 里唯一的映射类型，它存储了键值对的关联，是由键到键值的映射关系。 
Python 的字典数据类型是基于 hash 散列算法实现的，采用键值对(key:value)的形
式，根据 key 的值计算 value 的地址，具有非常快的查取和插入速度。 
字典包含的元素个数不限，值的类型可以是任何数据类型！但是字典的 key 必须
是不可变的对象，例如整数、字符串、bytes 和元组，最常见的还是将字符串作
为 key。列表、字典、集合等就不可以作为 key。同时，同一个字典内的 key 必
须是唯一的，但值则不必。 
注意：从 Python3.6 开始，字典是有序的！它将保持元素插入时的先后顺序！请
务必清楚！ 
字典可精确描述为不定长、可变、散列的集合类型。 
字典中的键映射多个值 
如果你想要一个键映射多个值，那么你 就需要将这多个值放到另外的容器中，比如列
表或者集合里面。 
你可以很方便的使用 collections 模块中的 defaultdict 来构造这样的字典。 
 
defaultdict 的一个特征是它会自动初始化每个 key 刚开始对应的值，所以你只需要 关注
添加元素操作了。 
>>> from collections import defaultdict 
>>> d = defaultdict(list) 
>>> d 
defaultdict(<class 'list'>, {}) 
>>> d['a'].append(1) 
>>> d["a"].append(2) 
>>> d 
defaultdict(<class 'list'>, {'a': [1, 2]}) 
 
defaultdict 会自动为将要访问的键（就算目前字典中并不存在 这样的键）创建映射实体。
如果你并不需要这样的特性，你可以在一个普通的字典上使用 setdefault()方法来代替。 
>>> d = {} 
>>> d.setdefault("a", []).append(1) 
>>> d.setdefault('a', []).append(2) 
>>> d 
{'a': [1, 2]} 
 
 
### <a name='-1'></a>字典和集合解析 
```python
my_dict = {i: i * i for i in range(10)} 
my_set = {i * i for i in range(10)} 
print('dict==%s, set==%s' % (my_dict, my_set)) 
# dict=={0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81},  
# set=={0, 1, 64, 4, 36, 9, 16, 49, 81, 25}
``` 
 
### <a name='-1'></a>遍历字典两种方式 
```python
dic = {'name': 'dong', 'age': 20, 'gender': (0, 1)} 
for k in dic: 
    print('key==%s, value==%s' % (k, dic[k])) 
 
# key==name, value==dong 
# key==age, value==20 
# key==gender, value==(0, 1) 
 
for key, value in dic.items(): 
    print('key==%s, value==%s' % (key, value)) 

```     
     
### <a name='key'></a>判断字典中是否有某一 key 
print('name' in dic) # True 
print('ag' not in dic) # True 
 
 
 
### <a name='-1'></a>获得字典的最大深度： 
def get_depth_dictionary(d): 
    if isinstance(d, dict): 
        return 1 + (max(map(get_depth_dictionary, d.values())) if d else 0) 
    return 0 
排序一个字典,根据值排序----sorted(original_dict.items(), key=itemgetter(1)) 
几个字典合并到一块----迭代执行 dict().update(dict1), 或者此方式：{**dict1, **dict2}; 
字典中删除一个 key----del dict[key] 
两个字典值相加----collections.Counter(dict1) + collection.Counter(dict2) 
itertools.product('ab', range(3)) --> ('a',0) ('a',1) ('a',2) ('b',0) ('b',1) ('b',2) 
dict(collections.Counter(string))----将一个 string 中某个字符以及其出现的次数构造成字
典:{'w': 1, '3': 1, 'r': 2}; 
defaultdict()用法----定义一个普通的 dict 时，取值时，如果键不在 dict 中，会直接报
keyError， 
但是如果是 defaultdict(factory_function)定义的字典，就取工厂函数的默认值，此工厂函
数值得是 set,int,list,str: 
```python
dict1 = defaultdict(set) 
dict2 = defaultdict(int) 
dict3 = defaultdict(str) 
dict4 = defaultdict(list) 
 
print(dict1[1]) 
print(dict2[1]) 
print(dict3[1]) 
print(dict4[1]) 
# set() 
# 0 
#  
# []
``` 
dict1.pop(key)函数-----意思是将 key-value 对从 dict 移除，并且返回值未此 key 对应的
value； 
取两个字典的交集可以按此进行操作-----set(dict1.items()) & set(dict2.items())-得到的是
类似于此形式的集合{('key1', 1)}； 
dict(x=list(range(11, 20)), y=list(range(21, 30)), z=list(range(31, 40))----得到{x: [11,...19], y: 
[21,...29], z: [30,...39]} 
collections.defaultdict(list)----用来将同一键的值放在一个列表中； 
collections.Counter(list1)  == collections.Counter(list2)---用来比较两个 list； 
 
### <a name='-1'></a>字典如何删除键和合并两个字典 
>>> d01 = {"name":"dx", "age":20} 
>>> del d01['name'] 
>>> d01 
{'age': 20} 
>>> d02 = {'name':'shane'} 
>>> d01.update(d02) 
>>> d01 
{'age': 20, 'name': 'shane'} 
      
      
### <a name='-1'></a>合并两个字典 
# 第一种方式 
```python
import uuid
x = {'name': 'dong', 'age': 10}  
y = {'host': 'compute', 'id': uuid.uuid4()} 
 
z = x.copy() 
print('z==%s' % z, 'id(x)==%s' % id(x), 'id(z)==%s' % id(z)) 
# z=={'name': 'dong', 'age': 10} id(x)==2342080434368 id(z)==2342110319872 
z.update(y) 
print('z==%s' % z, 'id(z)==%s' % id(z)) 
# z=={'name': 'dong', 'age': 10, 'host': 'compute', 
# 'id': UUID('e4379eb9-5d36-4018-baf6-b6d0b0572441')} id(z)==2689673090240 
# update()是就地和并字典，z 还是同样的内存地址 
```
 
# 第二种方式，python3.5 以上版本 
```python
import uuid
x = {'name': 'dong', 'age': 10}  
y = {'host': 'compute', 'id': uuid.uuid4()} 
z = {**x, **y} 
print('z==%s' % z, 'id(z)==%s' % id(z)) 
# z=={'name': 'dong', 'age': 10, 'host': 'compute', 
# 'id': UUID('2f06eb3b-b1f2-4b4b-9321-4a42f5b5f2d5')} id(z)==1376163448128 
# z 又存储到另一块内存 
```
 
# 第三种方式 
python3.9 可以使用 "|" 操作符合并两个字典 
>>> z3 = z|y 
{'a': 1, 'b': 2, 'c': 3, 'd': 4} 
      
      
### <a name='-1'></a>列表嵌套字典的排序，分别根据年龄和姓名排序** 
 
foo = [{"name":"zs","age":19},{"name":"ll","age":54}, 
{"name":"wa","age":17},{"name":"df","age":23}] 
 
>>> foo = [{"name":"zs","age":19},{"name":"ll","age":54}] 
>>> res = sorted(foo, key=lambda x:x['age'], reverse=False) 
>>> res 
[{'name': 'zs', 'age': 19}, {'name': 'll', 'age': 54}] 
 
>>> res = sorted(foo, key=lambda x:x['name'], reverse=False) 
>>> res 
[{'name': 'll', 'age': 54}, {'name': 'zs', 'age': 19}] 
 
 
 
### <a name='-1'></a>根据键对字典排序 
 
**方法一，zip 函数** 
 
>>> d = {'name': 'dx', 'age': 20, 'address': 'nj'} 
>>> z = zip(d.keys(), d.values()) 
>>> foo = [i for i in z] 
>>> res = sorted(foo, key=lambda x:x[0], reverse=False) 
>>> res 
[('address', 'nj'), ('age', 20), ('name', 'dx')] 
 
**方法二,不用 zip** 
 
dic.items 和 zip(dic.keys(),dic.values())都是为了构造列表嵌套字典的结构，方便后面用
sorted()构造排序规则 


>>> res = sorted(d.items(), key=lambda x:x[0], reverse=False) 
>>> res 
[('address', 'nj'), ('age', 20), ('name', 'dx')] 


## <a name='bytes'></a>bytes 字节
在 Python3 以后，字符串和 bytes 类型彻底分开了。字符串是以字符为单位进行处理的，bytes 类型是以字节为单位处理的。 
bytes 数据类型在所有的操作和使用甚至内置方法上和字符串数据类型基本一样，也是不可变的序列对象。 
bytes 对象只负责以二进制字节序列的形式记录所需记录的对象，至于该对象到底表示什么（比如到底是什么字符）则由相应的编码格式解码所决定。
Python3中，bytes 通常用于网络数据传输、二进制图片和文件的保存等等。可以通过调用 bytes()生成 bytes 实例，其值形式为 b'xxxxx'，其中 'xxxxx' 为一至多个转义的十六进制字符串（单个 x 的形式为：\x12，其中\x 为小写的十六进制转义字符，12 为二位十六进制数）组成的序列，每个十六进制数代表一个字节（八位二进制数，取值范围 0-255），对于同一个字符串如果采用不同的编码方式生成 bytes对象，就会形成不同的值. 
b = b''          # 创建一个空的 bytes 
b = bytes()      # 创建一个空的 bytes 

## <a name='set'></a>set 集合 
set 集合是一个无序不重复元素的集，基本功能包括关系测试和消除重复元素。
集合使用大括号({})框定元素，并以逗号进行分隔。但是注意：如果要创建一个空集合，必须用set()而不是{}，因为后者创建的是一个空字典。集合除了在形式上最外层用的也是花括号外，其它的和字典没有一毛钱关系。 
集合数据类型的核心在于自动去重。 
### <a name='-1'></a>创建非空元素 
set([1, 2, 3, 4, 5]) 

### <a name='setdels1'></a>集合 set 不支持索引；也不支持元素删除，比如 del s[1]； 

### <a name='-1'></a>取两个列表的交集 
set(list1) & set(list2)  
集合的交集 
set1 & set2； 
### <a name='-1'></a>集合并集 
set1 | set2  
color_list_2.union(color_list_1) 
两集合并集-交集-----set1 ^ set2；
set().union(*L)----L 为一个列表，其中包含元组元素，指的是获取一个列表中的独一无二的元素； 
 
### <a name='-1'></a>集合中添加元素 
original_set.add("red") 
original_set.update(["blue", "black"]) 
 
### <a name='-1'></a>集合中删除元素 
original_set.pop()  # 从前往后移除 
original_set.remove(num)以及 original_set.discard(num)  # 都是移除元素 num； 
 
### <a name='set1set2'></a>判断 set1 是否是 set2 的子集合 
set1.issubset(set2); 
同理父集合----set1.issuperset(set2) 
集合的拷贝----set2 = set1.copy(); 
集合的清理----set1.clear(); 
 
### <a name='set1'></a>从 set1 中移除两个集合的交集； 
set1.difference_update(set2) 

print(s1 - s2) # s1 中有 s2 中没有的元素     # print(s1 + s2)# TypeError: unsupported operand type(s) for +: 'set' and 'set' 
print(s2 - s1) # s2 中有 s1 中没有的元素 
 
### <a name='set1set2-1'></a>取 set1 中的元素且不在 set2 
>>> color_list_1 = set(["White", "Black", "Red"]) 
>>> color_list_2 = set(["Red", "Green"]) 
>>> color_list_1 - color_list_2 
{'Black', 'White'} 
>>> color_list_1.difference(color_list_2) 
{'Black', 'White'} 
 
 
 
### <a name='-1'></a>对称差异，将两个集合的对称差作为新集合返回(即恰好在集合之一中的所有元素) 
n = [9,8,3,2,2,0,9,7,6,3] 
all_nums = set([0,1,2,3,4,5,6,7,8,9]) 
n = set([int(i) for i in n]) 
n = n.symmetric_difference(all_nums)     # [1, 4, 5] 
