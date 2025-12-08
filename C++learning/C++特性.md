# C++内存管理
## [ELF文件](https://leetcode.cn/leetbook/read/cmian-shi-tu-po/vv6a76/)：
Executable and Linkable Format，是一种用于可执行文件，目标代码，共享库和核心转储（core dump）的标准文件格式，每个ELF文件都是由一个ELF header和紧跟其后的文件数据部分组成：

[ELF头](https://www.cnblogs.com/AndroidBinary/p/15364043.html#%E4%B8%80%E4%B8%B6%E7%AE%80%E4%BB%8B)：ELF 程序头是对二进制文件中段的描述，是程序装载必须的一部分。段（segment) 是在内核装载时被解析的。主要作用就是描述磁盘上可执行文件的内存布局以及如何映射到内存中。可以通过引用原始的ELF头中名为： **e_phoff**(程序头表的偏移量)的偏移量来得到程序头表。

[节头](https://www.cnblogs.com/AndroidBinary/p/15366466.html)：ELF节头表（Program Header Table）是 ELF文件格式 中的一个关键结构，它是一个描述文件运行时段（Segment）信息的数组，告诉操作系统如何加载和映射可执行文件或共享库到内存中。每个节头条目（Entry）都包含诸如节的类型、虚拟地址、文件偏移、大小等信息，是程序加载器（Loader）理解和执行 ELF 文件所必需的
批注iSsue：section不应该翻译成「段」而应该翻译成「节」，segment才是『段』。Section 是文件里的，Segment是内存里的。
段（segment) 和 节（section)是有区别的。 节不是段。 段是程序执行的必要组成部分。 在每个段中会有代码或者数据被划分为不同的节。 而 节头表 则是对这些节的位置和大小的描述，主要是用于链接和调试。
