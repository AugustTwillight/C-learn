# [static函数与普通函数的区别](https://blog.csdn.net/drouin/article/details/112638024?ops_request_misc=elastic_search_misc&request_id=d6045bfb013eaca47d59528690174ec8&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-112638024-null-null.142^v102^pc_search_result_base6&utm_term=static%E5%87%BD%E6%95%B0%E5%92%8C%E6%99%AE%E9%80%9A%E5%87%BD%E6%95%B0&spm=1018.2226.3001.4187)
用static修饰的函数本限定在本源文件中 不能被本源码文件以外的代码文件调用 
而普通函数，默认是extern的，也就是说，可以被其他代码文件调用该函数
把局部变量改变为静态变量后是改变了他的存储方式即改变了他的生存期。
static函数在内存中只有一份，普通静态函数在每个被调用中维持一份拷贝程序的局部变量存在与（堆栈）中，全局变量存在于（静态区）、动态申请在（堆）
区别在于非静态的全局变量的作用域是整个源程序，当一个源程序由多个源文件组成时，非静态的全局变量在各个源文件中都是有效的。
静态全局变量则限制了其作用域，即只在定义该变量的源文件内有效，在同一源程序的其他文件中不能使用它。
static全局变量与普通的全局变量的区别是static全局变量只初始化一次，下一次依据上一次结果，防止在其他文件单元被引用。
## 普通函数
作用域：整个程序范围内可见（跨文件）。
链接属性：具有外部链接性（external linkage），可以在其他文件中通过声明调用。
## 静态函数
作用域：仅限于定义它的文件内（文件作用域）。
链接属性：具有内部链接性（internal linkage），无法被其他文件调用。
用途：主要用于实现文件内的辅助功能，避免命名冲突。
普通函数：需要在多个文件之间共享的功能。
static 函数：限制函数的作用域，避免命名冲突，增强封装性。
# [static变量与普通变量的区别](https://blog.csdn.net/weixin_45252056/article/details/147444743?ops_request_misc=&request_id=&biz_id=102&utm_term=static%E5%87%BD%E6%95%B0%E5%92%8C%E6%99%AE%E9%80%9A%E5%87%BD%E6%95%B0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-147444743.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)
## 全局变量
### (1) 普通全局变量
作用域：在整个程序范围内可见（跨文件）。
链接属性：具有外部链接性（external linkage），可以通过 extern 在其他文件中访问。
生命周期：从程序开始到程序结束。
### (2) static 全局变量
作用域：仅限于定义它的文件内（文件作用域）。
链接属性：具有内部链接性（internal linkage），无法被其他文件通过 extern 访问。
生命周期：从程序开始到程序结束。
普通全局变量：需要在多个文件之间共享数据时使用。
static 全局变量：限制变量的作用域，避免命名冲突，增强封装性。
## 局部变量
### (1) 普通局部变量
作用域：仅限于定义它的函数内。
生命周期：每次进入函数时创建，离开函数时销毁。
**存储位置：通常存储在栈上。**
### (2) static 局部变量
作用域：仅限于定义它的函数内。
生命周期：从程序开始到程序结束（类似于全局变量）。
**存储位置：存储在静态存储区（非栈）。**
**初始化：只在第一次调用时初始化，之后保留上次的值。**
![](./images/1764588115027_image.png)
普通局部变量：适用于临时计算，不需要保留状态。
static 局部变量：适用于需要在多次函数调用间保持状态的场景（如计数器）。
# static 的核心作用
限制作用域：将变量或函数的作用域限制在文件内或函数内。
延长生命周期：将局部变量的生命周期延长至程序运行期间。
应用场景：
使用 static 提高代码的封装性和可维护性，避免命名冲突。
根据需求选择合适的变量或函数类型，确保代码的清晰性和性能。

## 数据在内存中存储的三个区域

栈区   堆区  静态区

栈区：存放局部变量和函数参数等的地方。栈区的作用范围过了之后会自动回收栈区分配的内存，不需要手动管理。

堆区：由比如malloc，realloc等函数所主动申请的内存，使用完之后须用free释放内存，若申请完之后忘记释放内存，则很容易造成内存泄漏。

静态区：静态变量和全局变量所存储的区域，一旦静态区的内存被分配，直到程序全部结束之后才会被释放。

静态区这个换而言之：静态区的生命周期和程序的生命周期是一样的，出了作用范围不会被销毁，相当于作用范围不变，但生命周期延长了。

那么上面那个例子我们也可以进行解释原理了：

static修饰局部变量时，实际改变的是变量的存储位置，原来在栈区，被修饰后放在了静态区。

所以说除了作用范围之后不会被销毁。
# const 
如果把const放在变量类型名前，说明这个变量的值是保持不变的，该变量必须在定义时初始化，初始化后对它进行的任何赋值都是非法的。
# 核心转储（core dump）：
参考：
[Linux核心转储（Core Dump）原理、配置与调试实践](https://blog.csdn.net/2302_80871796/article/details/149787347?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%A0%B8%E5%BF%83%E8%BD%AC%E5%82%A8%EF%BC%88core%20dump%EF%BC%89&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-149787347.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)
[核心转储（core dump）](https://blog.csdn.net/picway/article/details/71036610?ops_request_misc=&request_id=&biz_id=102&utm_term=%E6%A0%B8%E5%BF%83%E8%BD%AC%E5%82%A8%EF%BC%88core%20dump%EF%BC%89&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-71036610.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)：

在UNIX系统中，常将“主内存”(main memory) 称为核心(core)，而核心映像(core image) 就是 **“进程”(process)**执行当时的内存内容。当**进程**发生错误或收到“信号”(signal) 而终止执行时，系统会将核心映像写入一个文件，以作为调试之用，这就是所谓的核心转储(core dump)。
通常情况下core dump包含了程序运行时的内存，寄存器状态，堆栈指针，内存管理信息等。可以理解为把程序工作的当前状态存储成一个文件。许多程序和操作系统出错时会自动生成一个core文件。
## Core Dump文件存储位置
默认位置与可执行程序在同一目录下，文件名是core.XXX，其中XXX是一个数字。core dump文件名的模式保存在/proc/sys/kernel/core_pattern中，缺省值是core。通过以下命令可以更改core dump文件的位置(如希望生成到/tmp/cores目录下)

```
echo “/tmp/cores/core” > /proc/sys/kernel/core_pattern
```
设置Core Dump的核心转储文件目录和命名规则
/proc/sys/kernel/core_uses_pid可以控制产生的core文件的文件名中是否添加pid作为扩展，如果添加则文件内容为1，否则为0
proc/sys/kernel/core_pattern可以设置格式化的core文件保存位置或文件名，比如原来文件内容是core-%e
可以这样修改:
echo “/corefile/core-%e-%p-%t” > core_pattern
将会控制所产生的core文件会存放到/corefile目录下，产生的文件名为core-命令名-pid-时间戳
以下是参数列表:
%p - insert pid into filename 添加pid
%u - insert current uid into filename 添加当前uid
%g - insert current gid into filename 添加当前gid
%s - insert signal that caused the coredump into the filename 添加导致产生core的信号
%t - insert UNIX time that the coredump occurred into filename 添加core文件生成时的unix时间
%h - insert hostname where the coredump happened into filename 添加主机名
%e - insert coredumping executable name into filename 添加命令名
## 分析core dump
现在大部分类unix操作系统都提供了分析core文件的工具，比如 GNU Binutils Binary File Descriptor library (BFD),GNU Debugger (gdb），mdb等。较流行的是gdb。
在core文件所在目录下键入:

```
gdb -c core
```
它会启动GNU的调试器，来调试core文件，并且会显示生成此core文件的程序名，中止此程序的信号等等。
如果你已经知道是由什么程序生成此core文件的，比如MyServer崩溃了生成core.12345，那么用此指令调试:
```
gdb -c core MyServer
```
在进入gdb后, 可以用bt命令查看backtrace以检查发生程序运行到哪里, 来定位core dump的文件->行。
## 造成程序core dump的原因
1. 内存访问越界
a) 由于使用错误的下标，导致数组访问越界
b) 搜索字符串时，依靠字符串结束符来判断字符串是否结束，但是字符串没有正常的使用结束符
c) 使用strcpy, strcat, sprintf, strcmp, strcasecmp等字符串操作函数，将目标字符串读/写爆。应该使用strncpy, strlcpy, strncat, strlcat, snprintf, strncmp, strncasecmp等函数防止读写越界。

2. 多线程程序使用了线程不安全的函数。
应该使用下面这些可重入的函数 它们很容易被用错：

```
asctime_r(3c) gethostbyname_r(3n) getservbyname_r(3n) ctermid_r(3s) gethostent_r(3n) getservbyport_r(3n) ctime_r(3c) getlogin_r(3c) getservent_r(3n) fgetgrent_r(3c) getnetbyaddr_r(3n) getspent_r(3c) fgetpwent_r(3c) getnetbyname_r(3n) getspnam_r(3c) fgetspent_r(3c) getnetent_r(3n) gmtime_r(3c) gamma_r(3m) getnetgrent_r(3n) lgamma_r(3m) getauclassent_r(3) getprotobyname_r(3n) localtime_r(3c) getauclassnam_r(3) etprotobynumber_r(3n) nis_sperror_r(3n) getauevent_r(3) getprotoent_r(3n) rand_r(3c) getauevnam_r(3) getpwent_r(3c) readdir_r(3c) getauevnum_r(3) getpwnam_r(3c) strtok_r(3c) getgrent_r(3c) getpwuid_r(3c) tmpnam_r(3s) getgrgid_r(3c) getrpcbyname_r(3n) ttyname_r(3c) getgrnam_r(3c) getrpcbynumber_r(3n) gethostbyaddr_r(3n) getrpcent_r(3n)
```
3. 多线程读写的数据未加锁保护。
对于会被多个线程同时访问的全局数据，应该注意加锁保护，否则很容易造成core dump

4. 非法指针
a) 使用空指针
b) 随意使用指针转换。一个指向一段内存的指针，除非确定这段内存原先就分配为某种结构或类型，或者这种结构或类型的数组，否则不要将它转换为这种结构或类型的指针，而应该将这段内存拷贝到一个这种结构或类型中，再访问这个结构或类型。这是因为如果这段内存的开始地址不是按照这种结构或类型对齐的，那么访问它 时就很容易因为bus error而core dump.

5. 堆栈溢出
不要使用大的局部变量（因为局部变量都分配在栈上），这样容易造成堆栈溢出，破坏系统的栈和堆结构，导致出现莫名其妙的错误。

## System Dump和Core Dump的区别
许多没有做过UNIX系统级软件开发的人士，可能只听说过Dump，而并不知道系统Dump和Core Dump的区别，甚至混为一谈。
### 系统Dump（System Dump）
所有开放式操作系统，都存在系统DUMP问题。
产生原因：
由于系统关键/核心进程，产生严重的无法恢复的错误，为了避免系统相关资源受到更大损害，操作系统都会强行停止运行，并将当前内存中的各种结构,核心进程出错位置及其代码状态，保存下来，以便以后分析。**最常见的原因是指令走飞，或者缓冲区溢出，或者内存访问越界**。走飞就是说代码流有问题，导致执行到某一步指令混乱，跳转到一些不属于它的指令位置去执行一些莫名其妙的东西（没人知道那些地方本来是代码还是数据，而且是不是正确的代码开始位置），或者调用到不属于此进程的内存空间。写过C程序及汇编程序的人士，对这些现象应当是很清楚的。
#### 系统DUMP生成过程的特点：
在生成DUMP过程中，为了避免过多的操作结构，导致问题所在位置正好也在生成DUMP过程所涉及的资源中，造成DUMP不能正常生成，操作系统都用尽量简单的代码来完成，所以避开了一切复杂的管理结构，如文件系统）LVM等等，所以这就是为什么几乎所有开放系统，都要求DUMP设备空间是物理连续的——不用定位一个个数据块，从DUMP设备开头一直写直到完成，这个过程可以只用BIOS级别的操作就可以。这也是为什么在企业级UNIX普遍使用LVM的现状下，DUMP设备只可能是裸设备而不可能是文件系统文件，而且[b]只[/b]用作DUMP的设备，做LVM镜像是无用的——系统此时根本没有LVM操作，它不会管什么镜像不镜像，就用第一份连续写下去。
所以UNIX系统也不例外，它会将DUMP写到一个裸设或磁带设备。在重启的时候，如果设置的DUMP转存目录（文件系统中的目录）有足够空间，它将会转存成一个文件系统文件，缺省情况下，对于AIX来说是/var/adm/ras/下的vmcore*这样的文件，对于HPUX来说是/var/adm/crash下的目录及文件。当然，也可以选择将其转存到磁带设备。
#### 会造成系统DUMP的原因主要是：
系统补丁级别不一致或缺少（系统内核扩展有BUG（例如Oracle就会安装系统内核扩展））驱动程序有 BUG（因为设备驱动程序一般是工作在内核级别的），等等。所以一旦经常发生类似的系统DUMP，可以考虑将系统补丁包打到最新并一致化）升级微码）升级设备驱动程序（包括FC多路冗余软件））升级安装了内核扩展的软件的补丁包等等。
进程Core Dump
进程Core Dump产生的技术原因，基本等同于系统DUMP，就是说从程序原理上来说是基本一致的。
但进程是运行在低一级的优先级上（此优先级不同于系统中对进程定义的优先级，而是指CPU代码指令的优先级），被操作系统所控制，所以操作系统可以在一个进程出问题时，不影响其他进程的情况下，中止此进程的运行，并将相关环境保存下来，这就是core dump文件，可供分析。
如果进程是用高级语言编写并编译的，且用户有源程序，那么可以通过在编译时带上诊断用符号表（所有高级语言编译程序都有这种功能），通过系统提供的分析工具，加上core文件，能够分析到哪一个源程序语句造成的问题，进而比较容易地修正问题，当然，要做到这样，除非一开始就带上了符号表进行编译，否则只能重新编译程序，并重新运行程序，重现错误，才能显示出源程序出错位置。
如果用户没有源程序，那么只能分析到汇编指令的级别，难于查找问题所在并作出修正，所以这种情况下就不必多费心了，找到出问题的地方也没有办法。
**进程Core Dump的时候，操作系统会将进程异常终止掉并释放其占用的资源，不可能对系统本身的运行造成危害。这是与系统DUMP根本区别的一点，系统DUMP产生时，一定伴随着系统崩溃和停机，进程Core Dump时，只会造成相应的进程被终止，系统本身不可能崩溃。**当然如果此进程与其他进程有关联，其他进程也会受到影响，至于后果是什么，就看相关进程对这种异常情况（与自己相关的进程突然终止）的处理机制是什么了，没有一概的定论。
## Term和core
功能：Term和core的功能都是终止进程。
区别：Term->终止进程，没有多余动作；core->会先进行核心转储，然后再进行终止进程。
所以我们向进程发送kill -1 进程pid和kill -6 进程pid是有区别的。当我们发送kill -1 进程pid，会直接终止进程，没有多余动作；发送kill -6 进程pid，会在异常错误后面显示（core dumped文件转储）的标志，并会在当前目录下生成核心转储文件。
## 总结：用于进程
核心转储的作用
1. 事后调试：无需重现崩溃场景即可分析问题
2. 远程诊断：可将核心文件发送给开发者分析
3. 长期保存：保存崩溃现场供后续分析
4. 自动化分析：可用于自动化崩溃报告系统

核心转储的工作原理
1. 继承机制：子进程会继承父进程的资源限制（Resource Limit），包括核心转储设置
2. 信号触发：特定信号（如SIGQUIT、SIGSEGV等）会触发核心转储
3. 信息保存：系统保存进程地址空间内容、寄存器状态、内存映射等信息

核心转储的用途
1. 核心转储在程序调试和问题诊断中具有重要作用：
2. 事后调试：即使程序已经终止，仍可通过core文件分析崩溃时的状态
3. 问题复现：保存了程序崩溃瞬间的完整内存状态，便于复现问题
4. 错误定位：结合调试器（如gdb）可以：
    - 查看崩溃时的调用栈
	- 检查变量值
	- 分析内存状态
	- 定位段错误(Segmentation Fault)的具体位置

# ulimit
## 参考：
[ulimit最详解](https://blog.csdn.net/FreeApe/article/details/101058393?ops_request_misc=&request_id=&biz_id=102&utm_term=ulimit%20&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-101058393.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)
[Linux资源限制命令—ulimit](https://blog.csdn.net/qtm_gitee/article/details/127992049?ops_request_misc=elastic_search_misc&request_id=3d924d9664da1ef84b12745a2d41770d&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-127992049-null-null.142^v102^pc_search_result_base6&utm_term=ulimit%20&spm=1018.2226.3001.4187)

ulimit通过一些参数选项来管理不同种类的系统资源。
## ulimit命令：
1. ulimit可以限制使用系统资源的范围。是一个内置BASH命令。
2. ulimit设置项仅在当前shell作用(类似export命令，永久生效可以写入相关配置文件)，即Shell Session级别作用
3. 写入~/.profile或~/.bashrc只对当前用户持久性生效
4. 写入/etc/security/limits.conf可针对性配置，系统级持久性生效
5. 调整相关硬限制值(Hard Limit)，设置一次后，以后的值只能小于上一次设置的值
6. 如果不加S或H修饰，则默认同时修改Soft Limit和Hard Limit值

```
$ help limit
Syntax
      ulimit [-acdfHlmnpsStuv] [limit]

Key(设置项)
   -S   Set a soft limit for the given resource(设置软资源限制，设置后可以增加，但是不能超过硬资源设置)
   -H   Set a hard limit for the given resource(设置硬资源限制，一旦设置不能增加)

   -a   All current limits are reported(显示当前所有的 limit 信息). 
   -c   The maximum size of core files created(最大的 core 文件的大小， 以 blocks 为单位). 
   -d   The maximum size of a process's data segment(进程最大的数据段的大小，以 Kbytes 为单位). 
   -f   The maximum size of files created by the shell(default option,进程可以创建文件的最大值，以 blocks 为单位)
   -l   The maximum size that can be locked into memory(最大可加锁内存大小，以 Kbytes 为单位). 
   -m   The maximum resident set size(最大内存大小，以 Kbytes 为单位). 
   -n   The maximum number of open file descriptors(可以打开最大文件描述符的数量). 
   -p   The pipe buffer size(管道缓冲区的大小，以 Kbytes 为单位). 
   -s   The maximum stack size(线程栈大小，以 Kbytes 为单位). 
   -t   The maximum amount of cpu time in seconds(最大的 CPU 占用时间，以秒为单位). 
   -u   The maximum number of processes available to a single user(用户最大可用的进程数). 
   -v   The maximum amount of virtual memory available to the process(进程最大可用的虚拟内存，以 Kbytes 为单位).
```
注：所有设置blocks均指文件系统的最小分配单位block，典型值将介于1k和4k之间，对于超大型文件系统，最高可达16k及以上。
实例：

```
# 示例1：查看所有默认设置项值
$ ulimit -a
    core file size          (blocks, -c) 0
    data seg size           (kbytes, -d) unlimited
    scheduling priority             (-e) 0
    file size               (blocks, -f) unlimited
    pending signals                 (-i) 7823
    max locked memory       (kbytes, -l) 64
    max memory size         (kbytes, -m) unlimited
    open files                      (-n) 1024
    pipe size            (512 bytes, -p) 8
    POSIX message queues     (bytes, -q) 819200
    real-time priority              (-r) 0
    stack size              (kbytes, -s) 8192
    cpu time               (seconds, -t) unlimited
    max user processes              (-u) 7823
    virtual memory          (kbytes, -v) unlimited
    file locks                      (-x) unlimited

# 示例2：软限制设置core dump文件最大大小为2048
$ ulimit -Sc 51200

# 示例3：core dump文件大小无限制
$ ulimit -c unlimited

# 限制管道缓冲区大小为512Kbytes(KB)
$ ulimit -p 512

# 示例5：查看当前终端进程limits
$ echo $$ | cat /proc/`awk '{print $1}'`/limits
    Limit                     Soft Limit           Hard Limit           Units
    Max cpu time              unlimited            unlimited            seconds
    Max file size             unlimited            unlimited            bytes
    Max data size             unlimited            unlimited            bytes
    Max stack size            8388608              unlimited            bytes
    Max core file size        0                    unlimited            bytes
    Max resident set          unlimited            unlimited            bytes
    Max processes             8041                 8041                 processes
    Max open files            1024                 4096                 files
    Max locked memory         65536                65536                bytes
    Max address space         unlimited            unlimited            bytes
    Max file locks            unlimited            unlimited            locks
    Max pending signals       8041                 8041                 signals
    Max msgqueue size         819200               819200               bytes
    Max nice priority         40                   40
    Max realtime priority     0                    0
    Max realtime timeout      unlimited            unlimited            us
```
