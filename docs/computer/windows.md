# Windows

windows smart screen

When Windows Defender SmartScreen worked?
When you download a executeful app


## 压缩算法
- zip
- gzip
- compress
- deflate



句柄

handle

cpu 通过寻址来访问内存，32位操作系统，也就是32位cpu，支持的最大物理内容容量是0-0xffffffff 4G。

可是程序运行期间，可能用到的内存不止4G，这时候就要用虚拟内存，cpu 通过向虚拟内存寻址，虚拟内存和物理内存对应，通过页表映射。

如果物理内存不够，虚拟内存就映射到磁盘上。

使用虚拟内存的程序，叫做 MMU memory management unit 内存管理单元。

如何进行内存管理呢？

将虚拟内存和物理内存，进行分页，按固定大小（比如4k）分割成页 page 或 页帧 page frame。

这种映射表存储在内存中，叫做页表 page table

logical address
physical address


程序员使用的 malloc 分配的都是逻辑地址，c 指针指向的也是逻辑地址



Get-ChildItem -Recurse  -Path E:\electron-demo | measure -Property Length -Sum

先递归获取所有目录和子目录中的文件属性，包含文件名，文件大小，LastWriteTime，再通过管道符传递给 measure 命令，统计 length 属性的合计值，就能得到文件的总大小，，但不是所占存储空间的大小，我估计是文件夹也要占存储空间。



开发一个实用工具，里面汇总了很多 Windows 和 mac 上的常用功能，可以方便用户快速得到操作系统信息，再搭配好看的 UI，以及 AI 的问答，就可以通过询问，来获取本机信息，多完美的工具。