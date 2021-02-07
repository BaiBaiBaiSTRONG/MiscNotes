# OperatingSystemReview

> https://www.bilibili.com/video/BV1kJ411E7AQ?from=search&seid=16010825604667189028
>
> https://www.bilibili.com/video/BV18X4y1u7aU?from=search&seid=13603946897090647412
>
> Exercise
>
> https://www.zhihu.com/column/c_1242426573364064256

## 第一部分　概论

### 第1章　导论 2

#### 　==1.1操作系统的功能 2==

#####==1.1.1　用户视角 2==

#####==1.1.2　系统视角 3==

#####==1.1.3　操作系统的定义 4==

####==1.2　计算机系统的组成 4==

##### ==1.2.1　计算机系统的运行 5==

#####==1.2.2　存储结构 6==

#####==1.2.3　I/O结构 8==

####==1.3　计算机系统的体系结构 9==

##### ==1.3.1　单处理器系统 9==

#####==1.3.2　多处理器系统 10==

#####==1.3.3　集群系统 12==

####==1.4　操作系统的结构 13==

####==1.5　操作系统的执行 14==

#####==1.5.1　双重模式与多重模式的执行 15==

#####==1.5.2　定时器 16==

####==1.6　进程管理 17==

####==1.7　内存管理 17==

####==1.8　存储管理 18==

#####==1.8.1　文件系统管理 18==

#####==1.8.2　大容量存储器管理 19==

#####==1.8.3　高速缓存 19==

#####==1.8.4　I/O系统 21==

####==1.9　保护与安全 21==

####==1.10　内核数据结构 22==

#####==1.10.1　列表、堆栈及队列 22==

#####==1.10.2　树 23==

#####==1.10.3　哈希函数与哈希表 23==

#####==1.10.4　位图 24==

####==1.11　计算环境 24==

#####==1.11.1　传统计算 24==

#####==1.11.2　移动计算 25==

#####==1.11.3　分布计算 26==

#####==1.11.4　客户机-服务器计算 26==

##### ==1.11.5　对等计算 27==

##### ==1.11.6　虚拟化 28==

##### ==1.11.7　云计算 29==

##### ==1.11.8　实时嵌入式系统 29==

#### ==1.12　开源操作系统 30==

##### ==1.12.1　历史 31==

##### ==1.12.2　Linux 31==

##### ==1.12.3　BSD UNIX 32==

##### ==1.12.4　Solaris 32==

##### ==1.12.5　用作学习的开源操作系统 33==

#### ==1.13　小结 33==

### ==第2章　操作系统结构 38==

#### ==2.1　操作系统的服务 38==

#### ==2.2　用户与操作系统的界面 40==

##### ==2.2.1　命令解释程序 40==

##### ==2.2.2　图形用户界面 41==

##### ==2.2.3　界面的选择 42==

#### ==2.3　系统调用 43==

#### ==2.4　系统调用的类型 46==

##### ==2.4.1　进程控制 46==

##### ==2.4.2　文件管理 49==

##### ==2.4.3　设备管理 50==

##### ==2.4.4　信息维护 50==

##### ==2.4.5　通信 50==

##### ==2.4.6　保护 51==

#### ==2.5　系统程序 51==

#### ==2.6　操作系统的设计与实现 52==

##### ==2.6.1　设计目标 52==

##### ==2.6.2　机制与策略 53==

##### ==2.6.3　实现 53==

#### ==2.7　操作系统的结构 54==

##### ==2.7.1　简单结构 54==

##### ==2.7.2　分层方法 55==

##### ==2.7.3　微内核 56==

##### ==2.7.4　模块 57==

##### ==2.7.5　混合系统 58==

#### ==2.8　操作系统的调试 60==

##### ==2.8.1　故障分析 60==

##### ==2.8.2　性能优化 60==

##### ==2.8.3　DTrace 61==

#### ==2.9　操作系统的生成 63==

#### ==2.10　系统引导 64==

#### 

## 第二部分　进程管理

### 第3章　进程 72

#### 3.1　进程概念 72

###### 1. 如何理解进程？

进程在直观的概念上，进程就是运行起来的程序。



```undefined
  强调：程序的本身不是进程，程序只是被动实体，它是磁盘上包含的一系列的指令的文件。
       进程是活动的实体，具有一个程序计数器用于表示下个执行命令和一组可以描述当前进程状态的信息。
                                                  -------《操作系统概念精要》
```

个人理解：程序好比一个机器人，只是一堆机械，进程好比一个安装了电池的机器人，他们有了自己运行的一些状态信息。

###### 2.操作系统是如何把CPU 抽象成进程的？

回答这个问题，一定要搞懂两个概念，就是物理CPU和逻辑CPU。
 物理CPU好理解，它就是实际运行在物理机上的可以运行指令的东西。
 当一个程序在运行的时候，他在CPU上运行一般分为，取指，解码和运行三个部分。和这三个在运行的时候需要用到一些寄存器，和程序计数器一起其他在运行时需要的一些信息。
 当在它的运行时间周期我们可以进行一个快照，这个可以认为CPU处于运行状态。
 我们假设这个时候将这个世界静止下来，把这些寄存器和程序计数器一起其他的堆栈信息，一起它的运行状态进行记录，那么我们用包含这些信息的结构体就可以 抽象出来CPU的状态，从而对CPU进行了一个抽象。我们把保存这个CPU状态的结构成为程序控制块（PCB）。

###### 3. 如何理解内核态和用户态？如何理解 用户线程和内核线程？

根据书上和字面上的意思，内核态可以执行CPU的指令集的全部，用户态是只可以执行CPU指令的集的部分子集，比如，内部和I/O以及PSW（程序计数器寄存器）是不可以修改。他要访问物理硬件必须通过系统调用，切换到内核态。
 了解了内核态和用户态，就以此类推。类比用户线程和内核线程。
 用户线程就是运行在用户态的，内核线程是运行在内核态的。

###### 重要：

在讨论进程的时候没有用户进程和内核进程的讨论，其实它是隐式存在的，因为当用户进程执行系统调用的时候，他就要切换到内核态，进行指令的操作。如果细分的话，他在进程内是有一个用户线程和内核线程的，内核线程是专门为了执行系统调用而服务的。
 其实上面说的那个就是最基本的一对一的线程模型。
 所以在理解这个用户线程和内核线程的时候，只要记住，内核线程是用来为用户线程执行的系统调用来服务的就会对很多问题有所理解。
 类似的也就可以理解线程的 多对一，和多对多模型进行理解。

###### 4. 当父进程创建子进程后，父子进程会共享那些数据？

父进程在创建子进程的时候，一般子进程是父进程的一个副本，当执行数据的时候
 他会有写时拷贝这么一说，这时，他的变量和堆栈，会被重新拷贝出来一份，但是有编程的同学都应该知道有深浅拷贝， 这里的拷贝就类似与浅拷贝，它指针指向的地方不会进行复制，所以对于它的PCB里面包含的文件描述符指向的I/O设备的是不会被复制一份的。理解了深浅拷贝和PCB 里面包含的数据就很会很好的理解这个问题。

###### 5.同一个进程内的线程之间共享哪些数据？

每个线程都有自己的程序计数器，和寄存器以及堆栈信息。但是他们都是共享的进程的地址空间，堆内存，代码段，数据段和打开的文件以及信号。



![img](https:////upload-images.jianshu.io/upload_images/4793176-19edd78ecae49647.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

thread_1.png

###### 6. 如何理解进程是系统分配资源的基本单位，线程是CPU执行的最小单位？

地址空间是在进程的层级提出的，它是在进程创建的时候进行分配的。线程的创建时在线程库的基础上实现的，并不存在资源的分配。
 CPU的执行，都是根据程序计数器和周边的寄存器共同决定那一条质量来执行的，每个线程有自己的程序计数器和寄存器，那说明只有一个线程就能完全确定CPU执行哪条指令。

##### 3.1.1　进程 72

**进程**是执行的程序，这是一种非正式的说法。进程不只是程序代码，程序代码有时被成为**文本段(text section)**（或者代码段(code section))。进程还包括当前活动，如**程序计数器**的值和处理器**寄存器**的内容等。另外进程通常还包括：**进程的堆栈**（包括临时数据，比如函数参数，返回地址和局部变量）和**数据段**（全局变量，常量，静态变量）,除此之外还有动态分配的内存**堆**(heap)。如图所示：

![img](https:////upload-images.jianshu.io/upload_images/4793176-28795f86cfd8519d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### 3.1.2　进程状态 73

进程在执行时会改变状态。进程状态部分取决于进程的当前活动。
 每个进程可能处于一下状态：

1. **新的（new）**： 进程正在创建。
2. **运行（running）**: 指令正在运行。
3. **等待（waiting）**: 进程等待某个事件发生（如I/O完成或者某个信号）。
4. **就绪（ready）**: 进程等待分配处理器。
5. **终止（terminated）** : 进程已经完成执行。

在这些状态中，一次只有一个进程可以在处理去上运行，但是许多进程可处于就绪或者等待状态。

他们的状态转移图如图所示:

![img](https:////upload-images.jianshu.io/upload_images/4793176-e7606150661cb71d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

process_state.png

##### 3.1.3　进程控制块 73

操作系统内的每个进程都是由**进程控制块**(Process Control Block, PCB)来进行表示的。
 PCB中包含了很多的特定的进程相关信息：

+ **进程状态(state)**:  状态可以包括，新的，就绪，运行，等待和停止。
+ **程序计数器(PC)**:  计数器表示进程将要执行的下一个指令的地址。CPU到进程和线程抽象的重要CPU物理寄存器。
+ **CPU寄存器(CPU register)**: 根据计算机体系结构的不同，寄存器的类型和数量也会不同。包括累加器，索引寄存器，堆栈指针，通用寄存器和其他条件码信息寄存器。在发生终端时，这些状态信息和程序计数器一起保存到PCB中，一边进行上下文的切换。
+ **CPU调度信息(CPU scheduling information)**: 这类信息包括进程的优先级和调度队列的指针和其他调度参数。
+ **内存管理制度(memory-management information)**: 根据操作系统使用的内存系统，这类信息可以包括基地址和界限寄存器的值，页表和段表。
+ **记账信息(accounting information)**：这类信息包括CPU时间，实际使用时间时间期限，作业和进程数量等。
+ **I/O状态信息(I/O status information)**: 这类信息包括分配给进程的I/O设备列表，打开的文件列表。

![img](https:////upload-images.jianshu.io/upload_images/4793176-d577f6d3751216f0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### 在Linux中的进程表示：

linux 操作系统的进程控制块是采用C语言的结构体 task_struct 来表示，它位于内核的源代码目录内的头文件<linux/sched.h>。 这个结构体包含用于表示进程的所有必要信息，包括进程状态，调度和内存
 管理信息，打开的文件列表，指向父进程的指针以及子进程，兄弟进程列表的指针。

##### ==3.1.4　线程 74==

#### 3.2　进程调度 75

##### 3.2.1　调度队列 75

#### 调度队列

进程在进入系统时，会被加入**作业队列**(job queue)，这个队列包括系统内所有的进程。除此之外，系统还有一个**就绪队列**。它保存了驻留在内存中的，就绪的，等待运行的进程， 一般是一个**链表结构**来实现的。其头节点有两个指针，用于指向链表的第一个和最后一个PCB块；每个PCB有个指针指向队列里的下一个PCB。
 系统还有其他队列。当一个进程被分配了CPU以后，它执行了一段时间，最终退出，或被中断，或等待特定事件发生(I/O事件完成)。假设进程向一个共享设备发出请求，由于具有许多进程，磁盘可能忙于其他进程的I/O进程，因此该进程可能需要等待磁盘。等待I/O设备的进程列表，也被放到了一个队列中，这个队列就是**设备队列**。每个设备都有属于自己的设备队列。

![img](https:////upload-images.jianshu.io/upload_images/4793176-d4374b6ba8e9eb5d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

ready_queue_device_queue.png

CPU的调度可以用下面的图进行来进行表示：



![img](https:////upload-images.jianshu.io/upload_images/4793176-672e12bc2f666027.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

进程调度实际表示.png

进程从开始到进入系统到结束一般都是在这个图中进行的，

1. 进程进入系统后，加入到就绪队列中，这时进程处于就绪状态。
2. 进程被调入到CPU中后，就进入到了运行状态。
3. 运行时需要访问I/O设备，进程被放入设备队列中，进入挂起状态(或者等待状态)，直到I/O事件就绪后，在进入到就绪对列中。

##### 3.2.2　调度程序 77

在上面的进程的整个生命周期中，进程会在各种调度队列中迁移。
 操作系统为了调度必须按照一定的方式从这些队列中选择进程。进程选择通过**调度程序（调度器）**进行调度。
 通常，对于有大量的需要执行进程的系统中。可能进程的任务不能全部加载到内存中，这个时候会有部分的进程保存在磁盘中。这部分磁盘的数据，他作为一个虚拟的内存池被称为**虚拟内存**。
 调度程序可以分为**长期调度程序**和**短期调度程序**。
 **长期调度程序** 会把在磁盘上的进程加载到内存，把最近不会被执行的进程从内存中挪出。
 **短期调度程序(CPU调度程序)** 从就绪队列中选择进程分配CPU。

为什么会有两个调度程序？
 这两种调度程序的主要区别是执行效率。短期调度程序必须经常为CPU选择新的进程。长期调度程序执行并不频繁，但是它控制着内存中进程的数量，也就是并发进程的程度。
 长期调度程序他会进行认真的选择。一般根据进程的类型分为：**I/O密集型进程**和**CPU密集型进程**。
 如果所有进程都是I/O密集型进程，那么I/O设备队列一直都是满的，而就绪队列是空的，短期调度程序就一直处于空闲。
 当所有进程都是CPU密集型进程，那么I/O 设备对列都是空的，I/O设备就没有得到使用。所以长期调度程序是保证系统使用CPU和I/O设备平衡的。

##### 3.2.3　上下文切换 78

进程间的切换是由调度器完成的。它们进行切换的过程如图所示：



![img](https:////upload-images.jianshu.io/upload_images/4793176-b6ba86baac73f27a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

其中状态保存和状态回复的这一个过程成为**上下文切换**。
 上下文的切换的时间与硬件支持是分不开的。

#### 3.3　进程运行 79

##### 3.3.1　进程创建 79

进程在执行的过程中可以创建多个新的进程。创建进程成为**父进程**，而新的进程成为**子进程**。每个子进程又可以创建其他进程，从而形成**进程树**。下图为一个Linux系统的典型的进程树。

![img](https:////upload-images.jianshu.io/upload_images/4793176-21d5edaa6fba7c6f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

process_tree.png



在linux 下可以通过 pstree 查看进程树。



![img](https:////upload-images.jianshu.io/upload_images/4793176-3f93c4d9e37e9abe.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

linux_process_tree.png

#### 父子进程

一般来说，当一个进程创建子进程时，该子进程会需要一定的资源来完成任务。子进程可以从操作系统那里直接获取资源，也可以只从父进程里获得。父进程可能在子进程之间非配资源或者共享资源（主要是打开的I/O设备，也有可能是内存）。

在进程创建新进程时，可能有两种情况：

+ 父进程与子进程并发进行。
+ 父进程等待，直到某个子进程执行完成。

新进程的地址空间也有两种可能：

+ 子进程是父进程的复制品。
+ 子进程加载另外一个程序。

这里看下Unix和Linux的具体实现吧。
 每个进程都有一个唯一的整型进程标识符来标识。进程是通过*fork()*系统调用来创建的。每个进程的地址空间复制了原来的父进程的地址空间。这种机制允许父子进程之间可以轻松的通信。
 这两个进程都继续执行*fork()*之后的指令。但是对于子进程,系统调用*fork()*的返回值是0，对于父进程，返回的是子进程的进程标识符。(具体实现参考《linux 内核源码剖析》) 根据返回返回值的不同对父子进程进行不同的操作。

通常*fork()*系统调用之后，子进程一般会使用*exec()*系统调用，以新的程序来进行替换新进程的内存空间，系统调用*exec()*加载二进制文件到内存中（破坏包含系统调用exec()的原来进程内存内容），并开始执行新的进程。
 父进程能够创建更多的子进程，或者如果在子进程运行时没有什么可做，那么他就会使用系统调用*wait() * 把自己移出就绪队列，等待子进程完成，这里是任意一个子进程。



```cpp
  #include <sys/types.h>      
  #include <sys/wait.h>
  #include <stdio.h>                  
  #include <unistd.h>                 
  int main() {
     pid_t pid;
     // 调用fork系统调用创建新的进程
     pid = fork();
     if (pid < 0) {
         fprintf(stderr, "Fork Failed.\n");                                       
         return 1;
     } else if (pid == 0) {
         printf("I am child process.\n");
         execlp("/bin/ls", "ls", NULL);
     } else {
         wait(NULL);
         printf("I am parent process.\n");
    }
    return 0;
} 
```

执行命令: ./fork



![img](https:////upload-images.jianshu.io/upload_images/4793176-b191f7dab2331fa5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### 3.3.2　进程终止 82

当进程完成执行最后语句并通过系统调用*exit()*请求操作系统删除自身时，进程终止。这时，进程可以返回状态值到父进程（如果父进程调用了wait())。所有进程资源，如物理和虚拟内存，文件表，I/O缓冲区等，都由操作系统释放。
 在其他情况下，进程也有可能出现进程终止。进程通过适当的系统调用，可以终止另一个进程。通常，只有终止进程的父进程才能执行这一系统调用，否则用户可以任意终止彼此的进程。

父进程终止子进程的原因有很多，如：

+ 子进程使用了超过他分配的资源
+ 分配给子进程的任务，不再需要
+ 父进程正在退出，而且有些系统不支持无父进程的子进程继续运行。
     第三种的情况，是在某些系统中存在，那么，父进程创建的所有的子进程都应该终止，比如关机，这种株连九族的现象叫做 **级联终止**，一般由操作系统来启动。

在Unix和Linux系统中，父进程可以通过系统调用*wait()* 等待子进程的终止，系统调用*wait()*可以通过参数，让父进程获取子进程的终止状态；*wait()* 也返回终止子进程的标识符，让父进程知道哪个子进程被终止了。



```cpp
  pid_t pid;    
  int status;  
  pid = wait(&status); 
```

当一个进程终止时， 操作系统会释放其资源，不过，它位于进程表中的条目还是在的，知道父进程调用*wait()*;  这是因为进程表包含了进程的退出状态。

当进程已经终止，父进程没有*wait()*的进程，称为 **僵尸进程**。
 所有进程在中的时候都会进入到这个状态，但一般僵尸进程只是很短暂的存在，一旦父进程调用了*wait()*, 僵尸进程的进程描述符和它的进程表中的条目都会被释放。

如果父进程没有执行*wait()*就终止了，那么它的子进程就变成了**孤儿进程**， 在Unix和Linux 系统，他们的父进程就会变成 pid = 1的init 进程。 进程init 会定期进行*wait()* 操作。以便收集孤儿进程的状态，并释放孤儿进程标识符和进程表条目。

#### 3.4　进程间通信 83

操作系统的并发执行的进程可以是独立的也可以是协作的。一个进程的运行不会影响其他进程的运行，那么这个进行时**独立的**。显然，不与任何其他进程共享数据的进程是独立的。相反的，进程之前就是相互**协作的**。协作的进程之间会有共享的数据。

协作进程需要进行进程间通信（InterProcess Communication, IPC）机制，以允许进程相互交换数据与信息。
 就目前大多数系统来说，进程间的通信一般由两种基本模型：**共享内存** 和 **消息传递**。
 消息传递在交换少量的数据量来说很有用，而且在分布式系统中，消息传递是比共享内存更容易实现。如业界的kafka。
 共享内存的优点是快于消息队列，这是因为消息传递经常使用系统调用，而共享内存是在内存里映射一个共享内存区域，访问他就和内存的速度一样，不需要内核进行干预。

![img](https:////upload-images.jianshu.io/upload_images/4793176-5fa60f22d6c83ea6.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

IPC.png

对于消息传递和共享内存，会在后面的文章中详细去讲解，这里只做最基本的概念整理。

##### 3.4.1　共享内存系统 85

采用共享内存进行通信，需要通信的进程公共的共享内存区域。通常，一个进程创建一个共享内存区域，这块内存会驻留在创建共享内存段的进程的地址空间里，其他希望使用共享内存的进程需要将这块地址附加到自己的地址空间里。

##### 3.4.2　消息传递系统 86

消息传递提供一种机制，以便允许进程不必共享地址空间来实现通信和同步。对分布式系统特别有用。
 假如两个进程，P和Q需要进程通信，他们需要互相发送和接受消息：他们必须有**通信链路**。该通信链路有多种实现方法。但这里不关心物理实现。
 一般消息传递工具，至少提供两种操作：

```undefined
 send (messge)     
 receive(message)
 
```

##### 3.4.3 同步

进程间通信可以通过调用原语 send() 和receive()来进行。实现这些原语有很多种设计方案。
 消息在传递的时候可以是***阻塞的（blocking）***，也可以是***非阻塞的***(nonblocking), 也称为***同步的(synchronous)*** 和***异步的(asynchronous)***。

+ 阻塞发送： 发送进程阻塞，直到消息由接收进程或者邮箱所接收。
+ 非阻塞发送： 发送进程发送消息，不用等待接收进程回复，继续自己的操作。
+ 阻塞接收：接收进程阻塞，直到有消息可用。
+ 非阻塞接收： 接收进程收到一个有效消息或者空消息。

#### ==3.5　IPC系统例子 89==

##### ==3.5.1　例子：POSIX共享内存 89==

##### ==3.5.2　例子：Mach 91==

##### ==3.5.3　例子：Windows 92==

#### 3.6　客户机/服务器通信 93

了解了使用共享内存和消息传递进行通信。但是现在使用最多的还是基于客户端/服务器通信的方式。
 这里介绍网络通信最常用的两种策略和历史的IPC机制管道： 套接字(socket), 远程程序调用（RPC）和管道。

##### 3.6.1　套接字 93

套接字为通信的端点。通过网络通信的每对进程需要使用一对套接字，每个进程表示一个端点。
 每个套接字由一个IP地址和一个端口组成。通常，套接字采用客户端——服务器的架构。服务器通过监听指定端口，来等待客户请求。服务器收到请求后，接收来到套接字的连接，从而完成套接字的连接。（此细节和三次握手相关过程参考和计算机网络有关的参考书）。



![img](https:////upload-images.jianshu.io/upload_images/4793176-61ca6d044ea364e8.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### 3.6.2　远程过程调用 96

RPC 与IPC想对应，是目前业界许多互联网公司常用的通信方式。目前业界用的比较多rpc框架的有：应用级的服务框架：阿里的 Dubbo/Dubbox、Google gRPC、Spring Boot/Spring Cloud。

和IPC不同的是，RPC通信交换的消息具有明确的结构，而不仅仅是数据包。消息传递到RPC服务，RPC服务监听远程系统的端口号；消息包含用于指定，执行的函数的一个标识符和传给函数的参数。函数按照要求来进行执行，而所有的结构会通过另一种消息传递给请求方。
 rpc的通信过程：
 RPC语义上允许客户端调用位于远程主机的过程，就如同调用本地的过程一样。它一般通过客户端提供的**存根**(stub), 当用户调用远程过程是，RPC系统调用适当的存根，并且传递远程过程参数，都有存根定位服务器的端口，并且封装参数并且对rpc的内部结构进行序列化并进行打包。然后像服务器发送一个消息。
 服务器根据类似于存根接收到这个消息，并且调用定义好的Rpc协议格式，对封装的结构解包并进行反序列化，得到客户端的消息。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a7fd4e9aecf08697.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### 3.6.3　管道 98

管道允许两个进程进行通信，早期是Unix系统最早使用的一种IPC通信机制。现在基本不怎么用了。
 这里做最基本的管道设计考虑。

+ 管道允许单向通信还是双向通信。
+ 如果允许双向通信，他是半双工的还是全双工的。
+ 通信进程之间是否应该有一定的关系（父子进程）
+ 管道通信是否只能在单机进行，是否可以在网络上用？

根据管道的类型分为：普通管道和命名管道

##### 普通管道

回答上面的问题。

+ 普通管道是单向的，要双向通信，就要两个管道。
+ 普通管道通信是在父子进程间进行的。所以肯定不能进行网络通信。

![img](https:////upload-images.jianshu.io/upload_images/4793176-007ae206c0bad0b3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

pipe.png

##### 命名管道（FIFO）

+ 命名管道可以是双向的。但是具体在实现的时候一般都是半双工的，如果要实现双向通信，需要两个FIFO。
+ 不需要是父子关系。
+ 只能在一台机器上使用，网络通信还是选择套接字。



### 第4章　线程 112

#### 4.1　概述 112

每个线程是CPU使用的基本单元；他包括线程ID,程序计数器，寄存器和堆栈。它与统一进程的其他线程共享代码段，数据段和其他操作系统资源，如打开文件和信号。
 每个传统的进程只有一个控制线程。如果一个进程拥有多个控制线程，那么它能同时执行多个任务。下图说明了传统单线程和多线程进程的差异。



![img](https:////upload-images.jianshu.io/upload_images/4793176-7ad999eb2f23779e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### ==4.1.1　动机 112==

##### ==4.1.2　优点 113==

#### ==4.2　多核编程 114==

##### ==4.2.1　编程挑战 115==

##### ==4.2.2　并行类型 115==

#### 4.3　多线程模型 116

在线程运行的过程中，一般分为**用户线程** 和**内核线程**。
 用户线程位于内核之上，它的管理无需内核支持。
 内核线程由操作系统来直接支持和管理。几乎所有的现代操作系统都是支持内核线程的。
 在实现的方式上，用户线程和内核线程存在某种关系。一般分为三种：一对一模型，多对一模型，多对多模型。

##### 4.3.1　多对一模型 116

多对一模型映射多个用户线程到一个内核线程。线程管理是由用户控件的线程库来完成的，因此效率更高。但是，一个线程执行阻塞系统调用，那么整个进程将会被阻塞。在着，在任意时间只能有一个线程可以访问内核。万一内核线程也在同步等待其他事件的发生，那么此时，进程的其他线程也就得不到执行了。

![img](https:////upload-images.jianshu.io/upload_images/4793176-4ab8fb92fe949963.png?imageMogr2/auto-orient/strip|imageView2/2/w/1012/format/webp)

##### 4.3.2　一对一模型 116

一对一模型： 每个用户线程映射到一个线程。这个模型，在一个线程执行系统调用的时候，能够允许另一个线程继续执行。它也允许多个线程并行运行在多核处理器上。这个模型的缺点就是：创建一个用户线程就要创建一个内核线程，内核线程的创建开销影响程序的性能。所以在这个模型中，系统对支持的线程的数量是有限制的。

![img](https:////upload-images.jianshu.io/upload_images/4793176-efb25e58f5562742.png?imageMogr2/auto-orient/strip|imageView2/2/w/1004/format/webp)

##### 4.3.3　多对多模型 116

多对多模型多路复用多个用户线程到同样数量或者数量更小的内核线程上。这种模型结合了前两种的方式。没有了前两种的缺点。
 开发人员可以创建任意多的用户线程，并且相应内核线程能在多处理上进行并发执行。而且当一个用户执行系统调用的时候，内核可以调度另外的内核线程来执行其他线程的操作。

![img](https:////upload-images.jianshu.io/upload_images/4793176-1656115e84f2db98.png?imageMogr2/auto-orient/strip|imageView2/2/w/1118/format/webp)

#### 4.4　线程库 117

线程库为程序员提供了创建和管理线程的API。
 在展开对线程的API的整理之前，首先了解两个概念：**异步线程**和**同步线程**。

异步线程：一旦父线程创建了一个子线程，父线程继续自身的执行，不用管子线程何时终止，这个父子线程会并发执行，独立运行。由于各自独立运行，他们通常会有很少的数据共享。

同步线程：如果父线程创建一个子线程或者多个子线程后，那么在回复执行之前，它需要等待所有子线程的终止，就出现了同步线程。由父线程创建的子线程并发执行，但是父线程没有办法继续工作。一旦有线程终止，它就会与父线程连接。所有子线程完成后，父线程才继续工作，这种方式叫做 ***分叉-连接策略***。

##### 4.4.1　Pthreads 118

Pthreads库是Posix 标准定义的线程创建与同步API。它是线程的行为规范，而不是实现。大多数的操作系统都实现了这个线程规范。



```cpp
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int sum; // 这个数据是被线程共享的数据
void *runner(void* param) {
    printf("son thread is here.\n");
    int i = 0;
    int upper = atoi((char*)param);
    sum = 0;
    for (i = 0; i <= upper; ++i) {
        sum += i;
    }
    pthread_exit(0);
}

int main(int argc, char* argv[]) {
    pthread_t tid;
    pthread_attr_t attr;  // 设置线程属性

    if (argc != 2) {
        fprintf(stderr, "usage: a.cout <integer value> \n");
        return -1;
    }
    if (atoi(argv[1]) < 0) {
        fprintf(stderr, "%d must be >=0\n", atoi(argv[1]));
        return -1;
    }

    // 获取默认的线程属性
    pthread_attr_init(&attr);

    // 创建线程
    pthread_create(&tid, &attr, runner, argv[1]);

    // 等待线程退出
    pthread_join(tid, NULL);
    printf(" parent thread is here.\n");

    printf(" sum = %d\n", sum);
    return 0;
}
```

根据上面的代码来大概讲解下创建线程的API。

pthread_t tid; 声明了线程的线程标识符。每个线程都有一个唯一的线程标识符。
 每个线程都有一组属性，包括堆栈大小和调度信息。pthread_attr_t attr; 表示线程属性。
 通过调用pthread_attr_init(&attr) 可以设置这些属性。由于没有任何属性，这里为默认属性。
 pthread_create(); 可以创建一个单独的线程。除了传递线程的标识符和属性外，还要传递一个函数名，这里为runnner。以便线程从哪个函数开始执行，最后一个需要传递命令行参数。
 这里程序里有两个线程。一个为main的主线程 ,另外一个线程从runner()开始执行。根据上面的分叉-连接策略:主线程通过pthread_join()来等待runner()完成。 runner() 线程在pthread_exit()之后就会终止。

##### ==4.4.2　Windows线程 119==

##### ==4.4.3　Java线程 121==

#### 4.5　隐式多线程 122

##### 4.5.1　线程池 123

为了说明线程池，先来看一个多线程的web服务器。
 在web服务中，每当服务器收到一个请求时，他就会创建一个单独的线程来处理请求。虽然创建一个线程的代价比创建一个进程的代价要小，但是任然存在问题。

+ 线程在创建需要的时间是多少，处理完这个请求后，线程还是会被注销。
+ 如果允许所有并发请求都通过新线程来处理，那么系统的资源和内存需要受到限制，因为不可能无限制的创建新的线程。
     解决方法就是线程池。

**线程池**的主要思想就是：在进程开始的时候就创建一定量的线程。并加载到线程池中等待工作。当服务器受到请求是，它会唤醒池内的一个线程去处理请求。一旦线程完成了服务，它在回到池中等待再次被唤醒。如果池内没有可用线程，服务器会等待，知道有新的空线程为止。

##### ==4.5.2　OpenMP 124==

##### ==4.5.3　大中央调度 125==

##### ==4.5.4　其他方法 125==

#### 4.6　多线程问题 125

##### 4.6.1　系统调用fork()和exec() 125

##### 系统调用fork() 和exec()

如果程序内的某个线程调用了fork()，那么新的进程是复制所有线程呢，还是复制只调用fork()的线程？
 Unix 两种都支持，但是具体的实现还是根据是否要调用exec()来决定。
 如果分支后直接执行exec()，那么没有必要全部复制。因为exec()会把整个进程换掉。如果分支后不调用exec(),新进程应该重复所有进程。

##### 4.6.2　信号处理 126

**Unix信号**用于通知某个进程特定的事件已经发生。比如，I/O就绪，或者用户键盘输入了 ctrl+c等。
 这里在了解下**同步信号**和**异步信号**。
 和以前的概念一样，同步表示一直在等待信号的处理。异步表示发完信号就可以执行其他操作。
 一般进程街道信号后，可以有两种处理方法：

+ 缺省的信号处理程序。（操作系统定义的默认处理）
+ 用户自定义的信号处理程序。（比如异常捕捉等）。

这里就会产生一个问题。

1. 信号发给所有的使用的线程。（不同进程的线程）
2. 信号发给某个进程内所有线程。
3. 信号发给某个进程内的某些线程。
4. 规定一个特性线程只处理，接收外部的所有信号。

Unix 传递信号的系统调用为：
 kill(pid_t pid, int signal);
 发送给某个进程的 signal信号。
 pthread_kill(pthread_t tid, int signal);
 发给指定的线程signal信号。

##### 4.6.3　线程撤销 127

线程撤销是在线程完成之前终止线程。
 例子： 用户按下网页浏览器上的按钮，以停止进一步的加载网页。通常加载网页可能是多个线程，每个图像可能都是一个单独的线程在加载，当用户点下停止按钮，所有的网页加载线程都要被撤销。

需要撤销的线程称为**目标线程**。目标线程的终止一般分为

+ **异步撤销**： 一个线程立即终止目标线程。
+ **延迟撤销**： 目标线程需要检查它自身是否可以终止，比如它有依赖其他线程的终止，自己才能终止。

所以问题来了：
 当系统为进程分配资源的时候，分配到了已撤销的线程怎么办？ 比如 线程退出了，它的子线程和他在并行执行，但是资源分配到了父线程。还有就是已经撤销的线程正在更新和其他线程共享的数据，撤销会有问题。
 操作系统收回撤销线程的系统资源，不会收回所有的资源。所以在最后线程撤销的时候，最后有可能资源泄露。

pthread_cancel() 可以发起线程额撤销。这个是Pthread线程库提供的API。但是它并不能解决上面的问题。

##### ==4.6.4　线程本地存储 128==

##### 4.6.5　调度程序激活 128

多线程编程最后的一个问题就是线程库与内核之间的通信。
 许多系统在实现多对多的模型是，都在用户和内核线程之间加了一个中间数据结构** 轻量级进程 LWP(LightWeight process)**。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3aeb316ee3294881.png?imageMogr2/auto-orient/strip|imageView2/2/w/948/format/webp)

用户线程库与内核之间的一种通信方案称为**调度器激活**。

内核提供一组虚拟处理器LWP给应用程序，而应用程序可以调度用户线程到任意一个虚拟处理器上。此外，内核应将有关特事件通知应用程序。这个步骤 称为 **回调**。它由线程库通过**回调处理程序**来处理。
 当一个应用程序的线程要阻塞时，它会触发一个回调的事件。内核向应用程序发出一个回调，通知它有一个线程将会阻塞并标识特定线程。然后分配一个新的虚拟处理器给应用线程。应用程序在这个新的虚拟处理器上进行回调处理程序，保存它的阻塞状态，并释放阻塞线程运行的虚拟处理器。接着，回调处理函数调度一个新的线程到虚拟处理器上。当阻塞线程等待的事件发生时，内核向线程库在发出另一个回调，通知线程库，之前等待的事件发生了。该回调也需要虚拟处理器，内核可能分配一个新的虚拟处理器，或者抢占一个用户线程再次执行会回调处理程序。

#### ==4.7　操作系统例子 129==

##### ==4.7.1　Windows线程 129==

##### ==4.7.2　Linux线程 130==

### 第5章　进程同步 138

#### 5.1　背景 138

在之前的学习中，了解了进程能与系统内其他执行进程相互影响。协作进程之间或能直接共享逻辑地址空间（代码和数据），或能通过文件或者消息来共享数据。前一种是通过线程来实现的。

这里以进程之间最常见的同步问题，**生产者-消费者**（也叫**有界缓冲区**）问题来开始讨论吧。
 问题描述：假设有一个固定大小的缓冲区，生产者生产数据写入，消费者取出数据进行消费。当缓冲区满的时候，生产者必须等待；当缓冲区为空时，消费者必须等待。

对于这个问题，我们进行深入分析，首先有一个必须共享的缓冲区。所以先进行下面的定义。



```cpp
   typedef struct {
     ...
   } Item;
   #define BUFFER_SIZE 10
   Item  buffer[BUFFER_SIZE];
   int count = 0;    // 缓冲区中的数据量
   int in = 0;     // 用来标识生产者存放下一个数据位置
   int out = 0;    // 用来标识消费者取出下一个数据位置
```

我们假设缓冲区的数据为BUFFER_SIZE，则我们可以得到这样的伪代码:



```swift
// producer
while (true) {
    A = producer();
    // 缓冲区满,等待
    while (BUFFER_SIZE == count) ;
    buffer[in] = A;
    in = (in + 1) % BUFFER_SIZE;
    count++;
}

// cosumer
while (true) {
    // 缓冲区空,等待
    while (0 == count) ;
    B = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    count--;
} 
```

虽然以上代码在各自的程序中各自正确，但是在并发执行时，可能会存在问题，因为count 这个数据是两个程序共享的。
 "count++" 可以按照机器语言分解：

```cpp
    regisetr1 = count;
    register1 = register + 1 
    count = register;
```

相应的"count--" 可以分解为

```swift
    register2 = count ; 
    register2 =  register2 - 1;
    count = register2;
```

在CPU交替执行的时候，会产生三种结果。

![img](https:////upload-images.jianshu.io/upload_images/4793176-5a72df660444d13c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

producer_cosumer.png

这样的并发执行，我们得到count = 4; 如果T4，T5交换顺序，那么将得到6；

像这种，多个进程并发访问和操作同一数据并且执行结果与特定顺序有关，称为**竞争条件**。

#### 5.2　临界区问题 140

**临界区**： 假设某个系统有n个进程{P0, P1, P2 .... Pn-1} 每个进程有一段代码，进程在执行的过程中，可能修改公共变量，更新一个表，写一个文件。这段代码叫做临界区。

当有进程在临界区执行的时候，其他进程不允许在它们的临界区内执行。一般的临界区，我们会根据他们的结构分为**进入区（entry section）**，**退出区(exit section)**，**临界区(ctitical section)** 和 **剩余区(remainder section)**。如图所示：

![img](https:////upload-images.jianshu.io/upload_images/4793176-1424359eeb7da5f5.png?imageMogr2/auto-orient/strip|imageView2/2/w/1090/format/webp)

ctitical_section临界区结构.png

临界区问题的解决方案需要满足三个要求：

1. **互斥**：如果进程Pi在其临界区内执行，那么其他进程不能再其临界区执行。
2. **进步**： 如果没有进程在其临界区，并且有进程需要进入临界区，那么只有那些不在剩余区内执行的进程可以参选，以便确定谁能下次进入临界区，而且这种选择不能无限推迟。
3. **有限等待**： 从一个进程作出进入临界区的请求直到这个请求允许为止，其他进程进入临界区的次数具有上限。

#### 5.3　Peterson解决方案 141

针对临界区问题，PeterSon给出了自己的算法，只要适用于两个线程交错执行临界区和剩余区。
 假设有两个线程，P0和P1，这里只有两个线程，所以这里用i，j来表示，如果线程Pi表示一个线程，那么Pj表示另一个线程，两个线程共享数据



```cpp
int turn;
bool flags[2];
```

变量turn表示那个线程程可以进入到临界区，如果turn == 0;表示进程P0可以进入缓冲区。
 数组flags表示那个线程可以进入临界区。判断一个线程能否进入缓冲区需要 其他线程不在临界区。比如Pi线程需要判断，Pj在不在临界区内。他需要两个条件来判断 flags[j] 和turn == j; 所以可以得到这样的伪码结构：



![img](https:////upload-images.jianshu.io/upload_images/4793176-e36328a841cb89a7.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

peterson.png

为了进入临界区，进程Pi 首先设置flags[i] = true;表示第i个线程准备进入临界区。
 turn == j; 这个是一个在执行过程中，根据操作系统调度的顺序，来选择的线程的一个值。因为假设P0和P1两个线程同时执行到了这里，并且都对flags值进行了设置，那么根据操作系统的调度，有可能两个同时被执行了，虽然这里会有不同的结果，但是最后的 turn 只能有一个，要么为0，要么为1 。（根据语义，我们可以看出，这个值其实可以 可以用turn == i，我读到这里的一个疑问，但是请看下面的一个条件判断。）

while (flags[j] && turn == j ) ;  这个判断主要是看另外一个线程有没有在临界区，如果在临界区，那么忙等待它退出；(上一步的j不能被替换，因为他会使 这个循环直接退出)。
 flags[i] = false; 当退出临界区的时候，设置这个值，会使另外一个忙等待的线程跳出循环，进入到临界区。



```cpp
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>


// 线程同步之临界区问题
// PeterSon算法

// 共享count数据
int count = 0;

// turn表示哪个进程可以进入临界区，
// turn == i; 表示第i个线程允许在临界区内执行。
int turn = 0;
// 数组flags 表示哪个进程准备进入临界区。
// flags[i] 为true, 表示第i个线程准备进入临界区。
bool flags[2];

void* process0(void* arg) {
    while (true) {
        // 第0个线程准备进入临界区
        flags[0] = true;
        // 如果两个线程同时进去，那么turn 会几乎在同时设置为0和1;
        // 但是只有一个赋值语句可以成功，因为一个会被另外一个覆盖。
        // 变量turn 表示最后会去哪一个。
        // 这里turn 不能设置为0，因为下面的while的第二个条件直接跑走了。
        // 默认应该是进行忙等待的
        turn = 1;

        while (flags[1] && turn == 1);
        // 临界区代码
        {
            count++;
            sleep(1);
            printf("process 0 in 临界区. count=%d\n", count);
        }
        //
        flags[0] = false;
    }
}

void* process1(void* arg) {
    while (true) {
        // 第1个线程准备进入临界区
        flags[1] = true;
        // 如果两个线程同时进去，那么turn 会几乎在同时设置为0和1;
        // 但是只有一个赋值语句可以成功，因为一个会被另外一个覆盖。
        // 变量turn 表示最后会去哪一个。
        turn = 0;

        while (flags[0] && turn == 0);
        // 临界区代码
        {
            count++;
            sleep(1);
            printf("process 1 in 临界区. count=%d\n", count);
        }
        //
        flags[1] = false;
    }
}

int main() {

    pthread_t  t0, t1;
    flags[0] = flags[1] = false;

    int err;
    turn = 0;
    err = pthread_create(&t0, NULL, process0, NULL);

    if (err != 0)
        exit(-1);

    err = pthread_create(&t1, NULL, process1, NULL);

    if (err != 0)
        exit(-1);

    pthread_join(t0, NULL);
    pthread_join(t1, NULL);
    return 0;
}
```

#### 5.4　硬件同步 142

对于单处理器环境，临界区问题可简单的加以解决：在修改共享变量的时候只要**禁止中断**。这样就能保证当前指令正确的执行，且不会被抢占。
 然而，在多处理器环境，多处理器的中断禁止会很耗时，因为消息要传递到所有的处理器。消息传递会延迟进入临界区，并且降低多核的效率。另外，如果系统时钟是终端来更新的，那么问题就更复杂了。

因此，许多的现在系统提供特殊的硬件指令，用于检测和修改字的内容，或者用于原子的交换两个字（作为不可终端的指令）。它们通过这些特殊的指令，相对简单的解决临界区问题。

这里仅以test_and_set()指令为例来说明：



![img](https:////upload-images.jianshu.io/upload_images/4793176-c51305d90c7cfa9d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)



test_and_set()的指令是原子的。因此，不论CPU如何并发的执行，在指令上，test_and_set是不能被打断的。因此可以把临界区问题这样实现

![img](https:////upload-images.jianshu.io/upload_images/4793176-43532e5efd4b72ad.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

#### 5.5　互斥锁 144

上面的方法是基于硬件实现的，在临界区问题上，应用程序设计人员能用到最简单的工具就是**互斥锁（mutex lock)**。
 利用互斥锁的临界区问题的结构为：

![img](https:////upload-images.jianshu.io/upload_images/4793176-8c7916c9eec2eef4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

mutex_lock.png



这里可以看到，一个进入临界区时应该得到锁；在退出时释放锁。
 在这个结构中，我们给出acquire() 和release()的定义：



```cpp
  // available 表示锁是否可用
  acquire() {
      // 如果临界区不可用，进行忙等待。
      while (!available) ; 
      available = false;
  }
  
  release() {
      available = true;
  }
```

这里实现的一个缺点是：他需要忙等待。一般在实现时，互斥锁都是采用硬件来实现的。

这里介绍的这个方法处于忙等待。 一般把 在访问临界区时，不断地要通过忙等待判断锁是否可用的互斥锁称为 **自旋锁**（spin lock）

自旋锁与互斥锁有点类似，但是自旋锁不会引起调用者阻塞，如果自旋锁已经被别的执行单元保持，调用者会一直循环检查该自旋锁的保持者是否已经释放了锁，所以才叫自旋。自旋锁是忙等待，互斥锁是挂起阻塞。

自旋锁的有点：自旋锁一般持有的代码段耗时比较短，当进程获取到自旋锁后，不用进行上下文的切换。
 自旋锁一般用于多核系统中，自旋锁在一个处理器上"旋转"，其他线程在其他核上执行命令。

#### 5.6　信号量 144

##### 5.6.1　信号量的使用 145

信号量是线程同步中另一种方法。
 通常信号量是一个整数变量。信号量一般由**两个操作wait()** 和**signal()**，*wait()*操作称为**P**操作(荷兰语：proberen，测试) ，*signal()*称为**V**操作(荷兰语: verhogen,增加)。

信号量一般根据使用情况分为**计数信号量**和**二进制信号量**。

二进制信号量可以当做互斥锁来用。
 计数信号量可以用于控制访问具有多个实例的某种资源。
 P操作：当一个线程程执行操作wait()并且发现信号量值不为正的时候，他必须等待，这里的等待不是忙等待，而是阻塞自己，把自己放入到和信号量有关的队列中。
 V操作：当阻塞的线程等到其他线程执行了signal()操作后，线程会被wakeup(),将它从I/O队列加入到就绪对列中。

##### 5.6.2　信号量的实现 145

忙等待的信号量的实现是：



```bash
wait(S) {
    while (s <= 0) 
    ;   // busy wait
    S--;
} 
signal(S) {
     S++;
}
```

阻塞的信号量的实现为：



```csharp
    typedef struct {
        int value;
        struct  process * list; // 进程的队列
    } semaphore;

    wait (semaphore& s) {
       s.value -- ;
       // 如果value 的值小于它们的使用量，把进程加入到队列中进行挂起
       if (s.value < 0) {
             /* add this process to s.list */
             block();
       }
    }

    signal(semaphore & s) {
        s.value++；
        // 如果s->value的绝对值 表示挂起的队列的长度，如果挂起队列有值，就唤醒一个进程
        if (s->value <= 0) {
            remove a process from s.list;
            wakeup();
        }
   }
```

##### 5.6.3　死锁与饥饿 147

虽然上面的的进程队列，可以避免忙等待的情况，但是具有等待队列的信号量实现可能导致： 两个进程或者多个进程，无限等待一个事件，而该事件只有由等待的进程之一来产生，就会出现**死锁**。
 看下死锁的情况：
 假设有一个系统，它有两个进程，P0和P1，每个访问共享信号的S和Q，这两个的初始值都为1。

![img](https:////upload-images.jianshu.io/upload_images/4793176-cb3ef21e4b44515f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

deallock.png



这里就会看到，P0进程在等待P1进程的S,Q, P1进程在等待进程的P0的Q,S； 由于执行顺序的不确定，就会导致死锁。

除此之外，如果进程**无限等待**信号量而得不到执行，那么我们称这类问题为 **饥饿**。即信号量有关的信号有关的队列按照后进先出的顺序来增加和删除，第一个信号量将一直得不到响应。

##### 5.6.4　优先级的反转 147

假设有三个进程，L，M和H，他们的优先级顺序为L< M < H。假定进程H需要资源R，而R被进程L访问。通常进程H将等待L用完资源R，但是在假设M在这个过程中进入了可运行状态，从而抢占了L，从而导致了进程优先级低的M影响了H的执行，这种问题就是**优先级**翻转。
 在解决这类问题的时候，采用了**优先级继承协议**。
 比如：上面的例子中，优先级继承协议将允许有关进程L临时继承进程的H的优先级，从而防止M抢占执行。当进程L用完资源R后，它将放弃继承的进程H的优先级，从而采用原来的优先级。

#### 5.7　经典同步问题 148

##### 5.7.1　有界缓冲问题 148

问题描述：一组生产者进程和一组消费者进程共享一个初始为空大小为n的缓冲区，只有缓冲区没满时，生产者才能给缓冲区投放信息，否则必须等待；只有缓冲区不空时，消费者才能继续取出消息，否则也必须等待。由于缓冲区是临界资源，他只允许一个进程投放资源或者一个进程取出资源。

我们先用最基本的方法来实现下这个有界缓冲区的问题。
 我们假设缓冲区的数据为BUFFER_SIZE，则我们在不考虑执行并发的情况下，可以实现这样的伪代码:



```cpp
   typedef struct {
     ...
   } Item;
   #define BUFFER_SIZE 10
   Item  buffer[BUFFER_SIZE];
   int count = 0;    // 缓冲区中的数据量
   int in = 0;     // 用来标识生产者存放下一个数据位置
   int out = 0;    // 用来标识消费者取出下一个数据位置
```



```swift
// producer
while (true) {
    A = producer();
    // 缓冲区满,等待
    while (BUFFER_SIZE == count) ;
    buffer[in] = A;
    in = (in + 1) % BUFFER_SIZE;
    count++;
}
// cosumer
while (true) {
    // 缓冲区空,等待
    while (0 == count) ;
    B = buffer[out];
    out = (out + 1) % BUFFER_SIZE;
    count--;
} 
```

首先我们定义了，这样的一个伪代码的算法。

接下来我们考虑下加下信号量的实现方法。

首先，缓冲区是临界资源，那么不论是生产者还是消费者访问临界资源的时候都必须是互斥的访问。所以，对于访问临界资源必须有个互斥信号量———mutex，其初始值为1，表示可以访问。

可以先把临界区加上这个限制。



```csharp
// 生产者临界区
wait(mutex)；
      buffer[in] = A; 
      in = (in + 1) % BUFFER_SIZE;                                                         
      count++; 
signal(mutex);

// 消费者临界区

wait(mutex)
    B = buffer[out];   
    out = (out + 1) % BUFFER_SIZE;                                                       
    count--;   
signal(mutex)
```

对于临界资源的访问不分这个生产者还是消费者，谁访问都一样，都是一个进程访问临界资源的时候其他进程得等待。

接下来我们来用信号量来替换上面的忙等待部分。
 生产者与消费者是互相合作的关系，我们说，为完成某种任务而建立的多个进程，这些进程因为要在某些位置上协调他们的工作次序而等待。
 比如：A进程要工作必须等待B进程的一个结果，如果仅仅是A进程单方面的需要B进程的一个结果，那这张制约关系就是单方向的（此处的单方向和下面双方向是我个人的理解而用的词汇），如果同时B进程的工作也需要A进程工作的结果，那么这就是双方向的互相制约了。
 而生产者-消费者问题里的同步关系我认为是双方向的，原因如下：生产者要生产的前提是缓冲区没满，而缓冲区没满是消费者运行后的结果，同样消费者要运行的前提是缓冲区不空，而缓冲区不空是生产者不断生成的结果。所以，按本人的理解就是双方向制约的关系。
 也许有人好奇为什么要搞这么细，原因很简答，在指定同步关系的信号量的时候一个制约就是一个信号量，本题的同步关系需要两个信号量。一个是消费者通知生产者是否可以生产的“有空位”信号量——empty，一个是生产者通知消费者要消费的“有信息”信号量——full。
 (这一部分讲解摘自：[https://blog.csdn.net/m0_38041038/article/details/80958714](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fm0_38041038%2Farticle%2Fdetails%2F80958714)）

所以我们看到的具体实现逻辑为两个信号量相互制约控制。



```csharp
semaphore mutex=1;//互斥信号量
semaphore empty=n;//代表的是临界区的空位
semaphore full=0;//代表的是临界区的数据。空位+数据=n
producer(){ //生产者
    while(1){
        produce an item in nextep;
        p(empty);(想要什么p一下)      //获取空的缓冲区单元，有n个单元，每p一下。empty--一次
        p(muetx);                    //进入区。也就是进入临界区，互斥的访问。
        add nextep to buffer;        //临界区， 将数据装入缓冲区
        v(mutex);                    //退出区。
        v(full);(提供什么v一下)       //缓冲区有了数据了，没生产一个数据 full++一次。
    }
}
consumer(){ //消费者
    while(1){
        p(full);//获取满的缓冲单元
        p(muetx);//进入区
        remove an item from buffer;//临界区，取出缓冲区里的数据
        v(muetx);//退出区
        v(empty);//获取缓冲单元里的数据，产生一个空的缓冲单元。
        consume the item;//消费了数据
    }
}
```

下面有两种方法实现了生产者消费者问题

**POSIX信号量实现**



```cpp
#include <unistd.h>
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>
#include <semaphore.h>

#define BUFFER_SIZE  10

typedef struct {
  int value;
}Item;

Item buffer[BUFFER_SIZE] = {0};
int count = 0;
int in = 0;
int out = 0;

sem_t mutex;
sem_t blank;
sem_t data;

void* producer(void*) {
  srand(time(NULL));
  while(true) {
    Item item;
    item.value = rand() % 100;
    sem_wait(&blank);
    sem_wait(&mutex);
    count++;
    buffer[in] = item;
    printf("Produce write a item: %d to Buffer[%d].\n", item.value, in);
    in = (in + 1) % BUFFER_SIZE;
    sem_post(&mutex);
    sem_post(&data);
    sleep(1);
  }
}

void* cosumer(void*) {
  while (true) {
    Item item;
    sem_wait(&data);
    sem_wait(&mutex);
    count--;
    item.value = buffer[out].value;
    printf("cosumer read %d from buffer[%d].\n", item.value, out);
    out = (out + 1) % BUFFER_SIZE;
    sem_post(&mutex);
    sem_post(&blank);
    sleep(1);
  }
}

int main(int argc, char* argv[]) {
  sem_init(&mutex, 0, 1);
  sem_init(&blank, 0, BUFFER_SIZE);
  sem_init(&data, 0, 0);

  pthread_t pro[5];
  pthread_t cosu[10];

  for (int i = 0; i < 5; ++i) {
    pthread_create(&pro[i], NULL, producer, NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_create(&cosu[i], NULL, cosumer, NULL);
  }

  for (int i = 0; i < 5; ++i) {
    pthread_join(pro[i], NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_join(cosu[i], NULL);
  }

  sem_destroy(&mutex);
  sem_destroy(&blank);
  sem_destroy(&data);


  return 0;
}
```

**使用互斥量-条件变量实现**

```cpp
#include <unistd.h>
#include <stdio.h>
#include <pthread.h>
#include <stdlib.h>


#define BUFFER_SIZE  10

typedef struct {
  int value;
}Item;

Item buffer[BUFFER_SIZE] = {0};
int count = 0;
int in = 0;
int out = 0;

pthread_mutex_t mutex;
pthread_cond_t  not_empty;
pthread_cond_t  not_full;

void* producer(void*) {
  // 生产者，生产数据
  srand(time(NULL));
  while (true) {
    Item item;
    item.value = rand() % 100;

    pthread_mutex_lock(&mutex);
    // 这里的count必须在锁里面
    if (count == BUFFER_SIZE) {
      printf("buffer is full, producer wait for buffer has position.\n");
      pthread_cond_wait(&not_full, &mutex);
    }
    count++;
    printf("Produce write a item: %d to Buffer[%d].\n", item.value, in);
    buffer[in] = item;
    in = (in + 1) % BUFFER_SIZE;
    pthread_cond_signal(&not_empty);
    pthread_mutex_unlock(&mutex);
    sleep(1);
  }
}

void* cosumer(void*) {
  while(true) {
    Item item;
    pthread_mutex_lock(&mutex);
    if (count == 0) {
      printf("Buffer is empty, cosumer wait for buffer not empty.\n");
      pthread_cond_wait(&not_empty, &mutex);
    }
    count--;
    item.value = buffer[out].value;
    printf("cosumer read %d from buffer[%d].\n", item.value, out);
    out = (out + 1) % BUFFER_SIZE;
    pthread_cond_signal(&not_full);
    pthread_mutex_unlock(&mutex);
    sleep(1);
  }
}


int main(int argc, char* argv[]) {
  pthread_mutex_init(&mutex, NULL);
  pthread_cond_init(&not_empty, NULL);
  pthread_cond_init(&not_full, NULL);

  pthread_t pro[5];
  pthread_t cosu[10];

  for (int i = 0; i < 5; ++i) {
    pthread_create(&pro[i], NULL, producer, NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_create(&cosu[i], NULL, cosumer, NULL);
  }

  for (int i = 0; i < 5; ++i) {
    pthread_join(pro[i], NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_join(cosu[i], NULL);
  }

  pthread_mutex_destroy(&mutex);
  pthread_cond_destroy(&not_empty);
  pthread_cond_destroy(&not_full);

  return 0;
}
```

##### 5.7.2　读者-作者问题 149

问题描述：
 假设一个数据库为多个并发线程所共享。有的线程只读，有的线程需要写数据库。因此要求：①允许多个读者可以同时对文件执行读操作；②只允许一个写者往文件中写信息；③任一写者在完成写操作之前不允许其他读者或写者工作；④写者执行写操作前，应让已有的读者和写者全部退出。
 这个问题有两个变种：
 读者优先（第一读者-作者): 要求读者不应该保持等待，除非作者已经获得了权限使用对象。
 写者优先（第二读者-作者）: 要求一旦作者就绪，就应该尽快的执行，其他读者应该进行等待。
 两种问题都会带来了**饥饿**。
 第一种写者饥饿；第二种读者饥饿。

**读者优先**：

+ 关系分析。
     由题目分析读者和写者是互斥的，写者和写者也是互斥的，而读者和读者不存在互斥问题。

+ 整理思路。
     两个进程，即读者和写者。写者是比较简单的，它和任何进程互斥，它写入的数据部分应该属于临界区。用互斥信号量的P操作、V操作即可解决。

    读者的问题比较复杂，它必须实现与写者互斥的同时还要实现与其他读者的同步，因此，仅仅简单的一对P操作、V操作是无法解决的。
     那么，在这里用到了一个计数器，用它来判断当前是否有读者读文件。当有读者的时候写者是无法写文件的，此时读者会一直占用文件，当没有读者的时候写者才可以写文件。同时这里不同读者对计数器的访问也应该是互斥的。

+ 信号量设置。
     首先设置信号量count为计数器，用来记录当前读者数量，初值为0;
     设置mutex为互斥信号量，用于保护更新count变量时的互斥；
     设置互斥信号量rw用于保证读者和写者的互斥访问

这部分的讲解参考：([https://blog.csdn.net/u014253011/article/details/82744241](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fu014253011%2Farticle%2Fdetails%2F82744241)
 )



```swift
int count=0;  //用于记录当前的读者数量
semaphore mutex=1;  //用于保护更新count变量时的互斥
semaphore rw=1;  //用于保证读者和写者互斥地访问文件
writer () {  //写者进程
    while (1){
        P(rw); // 互斥访问共享文件
        Writing;  //写入
        V(rw) ;  //释放共享文件
    }
}

读者：作为读者，当你去拿一个资源时，你首先要看read_count是多少，如果是0，表示这个资源有可能被写进程占用，先用写锁将其锁住，避免写进程修改进来。当读完了，需要看，自己自己读完以后，是否是count 是0， 因为前面对 写进程进行了加锁，如果是0，需要释放让写进程可以拿到这个数据，

reader () {  // 读者进程
    while(1){
        P (mutex) ;  //互斥访问count变量
        if (count==0)  //当第一个读进程读共享文件时
            P(rw);  //阻止写进程写
        count++;  //读者计数器加1
        V (mutex) ;  //释放互斥变量count
        reading;  //读取
        P (mutex) ;  //互斥访问count变量
        count--; //读者计数器减1
        if (count==0)  //当最后一个读进程读完共享文件
            V(rw) ;  //允许写进程写
        V (mutex) ;  //释放互斥变量 count
    }
}
```

**写者优先**

在读者在运行时，有写请求进来，应该禁止后面的所有的读请求，而写请求进行到排队中，直到写请求执行完了，再次执行下一个读请求。
 为此，增加一个信号量并且在上面的程序中 writer()和reader()函数中各增加一对PV操作，就可以得到写进程优先的解决程序。



```swift
int count = 0;  //用于记录当前的读者数量
semaphore mutex = 1;  //用于保护更新count变量时的互斥
semaphore rw=1;  //用于保证读者和写者互斥地访问文件
semaphore w=1;  //用于实现“写优先”
 
writer(){
    while(1){
        P(w);  //在无写进程请求时进入
        P(rw);  //互斥访问共享文件
        writing;  //写入
        V(rw);  // 释放共享文件
        V(w) ;  //恢复对共享支件的访问
    }
}
 
reader () {  //读者进程
    while (1){
        P (w) ;  // 在无写进程请求时进入
        P (mutex);  // 互斥访问count变量
 
        if (count==0)  //当第一个读进程读共享文件时
            P(rw);  //阻止写进程写
 
        count++;  //读者计数器加1
        V (mutex) ;  //释放互斥变量count
        V(w);  //恢复对共享文件的访问
        reading;  //读取
        P (mutex) ; //互斥访问count变量
        count--;  //读者计数器减1
 
        if (count==0)  //当最后一个读进程读完共享文件
            V(rw);  //允许写进程写
 
        V (mutex);  //释放互斥变量count
    }
}
```

下面用两种方法解决下面的读写问题：

**信号量解决**

```cpp
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <semaphore.h>

int data = 1;
int count = 0;
sem_t rw_lock;
//  读者优先的情况下只需要两个信号量，但是会产生写线程饥饿。
//  写者优先需要三个信号量
// sem_t r_lock;
sem_t mutex;

// 读者优先
void* reader(void* arg) {
  while (true) {
    //  写者优先时需要打开
    //  sem_wait(&r_lock);
    sem_wait(&mutex);
    if (count == 0) {
      sem_wait(&rw_lock);
    }
    count++;
    printf("Reader has %d readers\n", count);
    sem_post(&mutex);
    printf("Reader data:%d\n", data);
    sleep(1);
    sem_wait(&mutex);
    count--;
    if (count == 0) {
      sem_post(&rw_lock);
    }
    sem_post(&mutex);
    sleep(5);
//  写者优先时,需要打开
//    sem_post(&r_lock);
  }
}

void* writer(void* arg) {
  while (true) {
    sem_wait(&rw_lock);
    int tmp = data;
    data = rand()% 100;
    printf("writer modify data from %d to %d.\n", tmp, data);
    sem_post(&rw_lock);
    sleep(1);
  }
}

int main(int argc, char* argv[]) {
  sem_init(&rw_lock, 0, 1);
  // 写者优先时需要打开
  //sem_init(&r_lock, 0, 1);
  sem_init(&mutex, 0, 1);

  srand(time(NULL));
  data = rand()% 100;

  pthread_t p_reader[10];
  pthread_t p_writer[3];
  for (int i = 0; i < 3; ++i) {
    pthread_create(&p_writer[i], NULL, writer, NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_create(&p_reader[i], NULL, reader, NULL);
  }

   for (int i = 0; i < 3; ++i) {
    pthread_join(p_writer[i], NULL);
  }

  for (int i = 0; i < 10; ++i) {
    pthread_join(p_reader[i], NULL);
  }

  sem_destroy(&mutex);
  sem_destroy(&rw_lock);
// 写者优先时,打开
//  sem_destroy(&r_lock);

  return 0;
}
```

**读写锁解决**

***LINUX 读写锁是写者优先\***



```cpp
#include<stdio.h>
#include<pthread.h>
#include<string.h>
#include<stdlib.h>
#include <unistd.h>

pthread_rwlock_t rwlock;

int good = 0;

void *writer(void *argv) {
  while(true) {
    // 加上写锁
    pthread_rwlock_wrlock(&rwlock);
    good = rand() % 1000;
    printf("%u write:%d\n",(unsigned int)pthread_self() % 100,good);
    pthread_rwlock_unlock(&rwlock);
    sleep(rand()%2);
  }
  return NULL;
}

void *reader(void *argv) {
  while(true) {
    // 加上读锁
    pthread_rwlock_rdlock(&rwlock);
    printf("%u read:%d\n",(unsigned int)pthread_self() % 100,good);
    pthread_rwlock_unlock(&rwlock);
    sleep(rand()%2);
  }
  return NULL;
}

int main() {
  pthread_t pt[8];
  int i;
  // 初始化读写锁
  pthread_rwlock_init(&rwlock,NULL);
  // 创建三个写线程
  for(i=0;i < 3;i++) {
    pthread_create(&pt[i],NULL,writer,NULL);
  }

  // 5个读线程
  for(;i<8;i++)
    pthread_create(&pt[i],NULL,reader,NULL);

  for(i=0;i<8;i++)
    pthread_join(pt[i],NULL);
  pthread_rwlock_destroy(&rwlock);
  return 0;
}
```

##### 5.7.3　哲学家就餐问题 150

问题描述
 由Dijkstra提出并解决的哲学家就餐问题是典型的同步问题。该问题描述的是五个哲学家共用一张圆桌，分别坐在周围的五张椅子上，在圆桌上有五个碗和五只筷子，他们的生活方式是交替的进行思考和进餐。平时，一个哲学家进行思考，饥饿时便试图取用其左右最靠近他的筷子，只有在他拿到两只筷子时才能进餐。进餐完毕，放下筷子继续思考。

用信号量解法：
 每只筷子都用一个信号量来表示，一个哲学家通过执行wait()试图获取左边和右边的筷子，它会通过signal()释放筷子。因此：



```bash
semaphore  chopstick[5] = {1,1,1,1,1}

// 第i个哲学家的伪码表示为：
while (true) {
     wait(chopstick[i])；
     wait(chopstick[(i+1)%5]);
       // eating
      signal(chopstick[(i+1)%5]);
      signal(chopstick[i]);
}
```

虽然这一解决方案保证两个邻居不能同时进食，但是它可能导致死锁，因此还是应被拒绝的。假若所有 5 个哲学家同时饥饿并拿起左边的筷子。所有筷子的信号量现在均为 0。当每个哲学家试图拿右边的筷子时，他会被永远推迟。

死锁问题有多种可能的补救措施：

+ 允许最多 4 个哲学家同时坐在桌子上。
+ 只有一个哲学家的两根筷子都可用时，他才能拿起它们（他必须在临界区内拿起两根 辕子)。
+ 使用非对称解决方案。即单号的哲学家先拿起左边的筷子，接着右边的筷子；而双 号的哲学家先拿起右边的筷子，接着左边的筷子。

**用互斥量实现哲学家就餐问题**

```cpp
#include<sys/types.h>
#include<unistd.h>
#include<stdlib.h>
#include<stdio.h>
#include<pthread.h>
#include<semaphore.h>
#include<time.h>
#define N 5     //哲学家数量

#define LEFT(i)    (i+N-1)%N  //左手边哲学家编号
#define RIGHT(i)   (i+1)%N    //右手边哲家编号

#define HUNGRY    0     //饥饿
#define THINKING  1     //思考
#define EATING    2     //吃饭

#define U_SECOND 1000000   //1秒对应的微秒数
pthread_mutex_t mutex;     //互斥量

int state[N];  //记录每个哲学家状态
//每个哲学家的思考时间，吃饭时间，思考开始时间，吃饭开始时间
clock_t thinking_time[N], eating_time[N], start_eating_time[N],
        start_thinking_time[N];
// 线程函数
void *thread_function(void *arg);

int main(int argc,char* argv[]) {
  pthread_mutex_init(&mutex, NULL);

  pthread_t a,b,c,d,e;
  //为每一个哲学家开启一个线程，传递哲学家编号
  const char* ch0 = "0";
  const char* ch1 = "1";
  const char* ch2 = "2";
  const char* ch3 = "3";
  const char* ch4 = "4";
  pthread_create(&a,NULL,thread_function, (void*)ch0);
  pthread_create(&b,NULL,thread_function, (void*)ch1);
  pthread_create(&c,NULL,thread_function, (void*)ch2);
  pthread_create(&d,NULL,thread_function, (void*)ch3);
  pthread_create(&e,NULL,thread_function, (void*)ch4);

  //初始化随机数种子
  srand((unsigned int)(time(NULL)));

  while(true) {
    ;
  }
}

void* thread_function(void*arg) {
  char *a = (char *)arg;
  int num = a[0] -'0';
  //根据传递参数获取哲学家编号
  int rand_time;
  while(1) {
    //关键代码加锁
    pthread_mutex_lock(&mutex);
    //如果该哲学家处于饥饿  并且左右两位哲学家都没有在吃饭 就拿起叉子吃饭
    if(state[num] == HUNGRY &&
       state[LEFT(num)] != EATING &&
       state[RIGHT(num)] != EATING) {
      state[num] = EATING;
      //记录开始吃饭时间
      start_eating_time[num] = clock();
      //随机生成吃饭时间
      eating_time[num] = (rand() % 5) * U_SECOND;
      //输出状态
      printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
      printf("%d is eating\n",num);
    } else if(state[num] == EATING) {
      //吃饭时间已到 ，开始思考
      if(clock() - start_eating_time[num] >= eating_time[num]) {
        state[num] = THINKING;
        printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
        printf("%d is thinking\n",num);
        //记录开始思考时间
        start_thinking_time[num] = clock();
        //随机生成思考时间
        thinking_time[num] = (rand() % 10 + 10) * U_SECOND;
      }
    } else if(state[num] == THINKING) {
      //思考一定时间后，哲学家饿了，需要吃饭
      if(clock() - start_thinking_time[num] >= thinking_time[num]) {
        state[num] = HUNGRY;
        printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
        printf("%d is hungry\n",num);
      }
    }
    pthread_mutex_unlock(&mutex);
  }
}
```

#### 5.8　管程 151

##### 5.8.1　使用方法 152

利用信号量可以同一种方便有效的进程同步机制，但是他们产生的错误也是难以检测到的，具体例子请参考下一节要讲的《线程同步经典问题系列》。
 为了解决这类问题，操作系统义工了管程的概念。
 管程其实是一种语法定义的数据和函数的集合。就类似于一个函数，但是它可以保证在管程的这段结构代码里，每次只有一个进程在管程内处于活动状态。

![img](https:////upload-images.jianshu.io/upload_images/4793176-c55d53e1a40bb1f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

monitor管程.png

而且管程在进行同步时，是下面的结构，一次只有一个管程在代码段中执行，其他都在入口队列中排队。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3a3d157e241c4418.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

管程的执行模式.png

进入管程时的互斥是由编译器负责，但通常是用一个互斥量的方式来进行的。虽然管程提供了一种实现互斥的方法，但是，当进程无法继续运行的是被阻塞。比如：在生产者-消费者中，我们怎么让生产者在缓冲区满的时候阻塞呢？
 解决的办法就是，一个新的同步机制**条件变量**；
 条件变量一般只有两个操作：wait()和signal();
 当一个管程无法继续运行时，它会在某个条件变量上执行wait();该操作使得调用线程自身阻塞直到其他管程执行signal()，并且还将另外一个以前在等待管程之外的线程调入管程。

**条件变量**

一般条件变量和互斥量是一起使用的。
 这种模式用于让一个线程锁住一个互斥量，然后当它不能获得某个结果的时候，挂起并等待一个条件变量。

比如生产者消费者中，假设生产者缓冲区满了，需要阻塞，那么,
 可以使用条件变量将其阻塞。

```php
pthread_mutex_lock(&mutex);
while( bufffer != 0) pthread_cond_wait(cond, mutex);
// buffer[n] = A;
pthread_cond_signal(cond);
pthread_mutex_unlock();
```

条件变量的wait操作一般干了三件事：
 1、给互斥锁解锁
 2、把调用线程投入睡眠，直到另外某个线程就本条件调用signal。
 3、然后，在返回前重新给互斥锁上锁（没有获得锁时一直阻塞在这里）

##### 5.8.2　哲学家就餐问题的管程解决方案 153

问题描述
 由Dijkstra提出并解决的哲学家就餐问题是典型的同步问题。该问题描述的是五个哲学家共用一张圆桌，分别坐在周围的五张椅子上，在圆桌上有五个碗和五只筷子，他们的生活方式是交替的进行思考和进餐。平时，一个哲学家进行思考，饥饿时便试图取用其左右最靠近他的筷子，只有在他拿到两只筷子时才能进餐。进餐完毕，放下筷子继续思考。

用信号量解法：
 每只筷子都用一个信号量来表示，一个哲学家通过执行wait()试图获取左边和右边的筷子，它会通过signal()释放筷子。因此：



```bash
semaphore  chopstick[5] = {1,1,1,1,1}

// 第i个哲学家的伪码表示为：
while (true) {
     wait(chopstick[i])；
     wait(chopstick[(i+1)%5]);
       // eating
      signal(chopstick[(i+1)%5]);
      signal(chopstick[i]);
}
```

虽然这一解决方案保证两个邻居不能同时进食，但是它可能导致死锁，因此还是应被拒绝的。假若所有 5 个哲学家同时饥饿并拿起左边的筷子。所有筷子的信号量现在均为 0。当每个哲学家试图拿右边的筷子时，他会被永远推迟。

死锁问题有多种可能的补救措施：

+ 允许最多 4 个哲学家同时坐在桌子上。
+ 只有一个哲学家的两根筷子都可用时，他才能拿起它们（他必须在临界区内拿起两根 辕子)。
+ 使用非对称解决方案。即单号的哲学家先拿起左边的筷子，接着右边的筷子；而双 号的哲学家先拿起右边的筷子，接着左边的筷子。

**用互斥量实现哲学家就餐问题**

```cpp
#include<sys/types.h>
#include<unistd.h>
#include<stdlib.h>
#include<stdio.h>
#include<pthread.h>
#include<semaphore.h>
#include<time.h>
#define N 5     //哲学家数量

#define LEFT(i)    (i+N-1)%N  //左手边哲学家编号
#define RIGHT(i)   (i+1)%N    //右手边哲家编号

#define HUNGRY    0     //饥饿
#define THINKING  1     //思考
#define EATING    2     //吃饭

#define U_SECOND 1000000   //1秒对应的微秒数
pthread_mutex_t mutex;     //互斥量

int state[N];  //记录每个哲学家状态
//每个哲学家的思考时间，吃饭时间，思考开始时间，吃饭开始时间
clock_t thinking_time[N], eating_time[N], start_eating_time[N],
        start_thinking_time[N];
// 线程函数
void *thread_function(void *arg);

int main(int argc,char* argv[]) {
  pthread_mutex_init(&mutex, NULL);

  pthread_t a,b,c,d,e;
  //为每一个哲学家开启一个线程，传递哲学家编号
  const char* ch0 = "0";
  const char* ch1 = "1";
  const char* ch2 = "2";
  const char* ch3 = "3";
  const char* ch4 = "4";
  pthread_create(&a,NULL,thread_function, (void*)ch0);
  pthread_create(&b,NULL,thread_function, (void*)ch1);
  pthread_create(&c,NULL,thread_function, (void*)ch2);
  pthread_create(&d,NULL,thread_function, (void*)ch3);
  pthread_create(&e,NULL,thread_function, (void*)ch4);

  //初始化随机数种子
  srand((unsigned int)(time(NULL)));

  while(true) {
    ;
  }
}

void* thread_function(void*arg) {
  char *a = (char *)arg;
  int num = a[0] -'0';
  //根据传递参数获取哲学家编号
  int rand_time;
  while(1) {
    //关键代码加锁
    pthread_mutex_lock(&mutex);
    //如果该哲学家处于饥饿  并且左右两位哲学家都没有在吃饭 就拿起叉子吃饭
    if(state[num] == HUNGRY &&
       state[LEFT(num)] != EATING &&
       state[RIGHT(num)] != EATING) {
      state[num] = EATING;
      //记录开始吃饭时间
      start_eating_time[num] = clock();
      //随机生成吃饭时间
      eating_time[num] = (rand() % 5) * U_SECOND;
      //输出状态
      printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
      printf("%d is eating\n",num);
    } else if(state[num] == EATING) {
      //吃饭时间已到 ，开始思考
      if(clock() - start_eating_time[num] >= eating_time[num]) {
        state[num] = THINKING;
        printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
        printf("%d is thinking\n",num);
        //记录开始思考时间
        start_thinking_time[num] = clock();
        //随机生成思考时间
        thinking_time[num] = (rand() % 10 + 10) * U_SECOND;
      }
    } else if(state[num] == THINKING) {
      //思考一定时间后，哲学家饿了，需要吃饭
      if(clock() - start_thinking_time[num] >= thinking_time[num]) {
        state[num] = HUNGRY;
        printf("state: %d %d %d %d %d\n",state[0],state[1],state[2],state[3],state[4]);
        printf("%d is hungry\n",num);
      }
    }
    pthread_mutex_unlock(&mutex);
  }
}
```

##### ==5.8.3　采用信号量的管程实现 154==

##### ==5.8.4　管程内的进程重启 155==

#### ==5.9同步==

##### ==5.9.1　Windows同步 156==

##### ==5.9.2　Linux同步 157==

##### ==5.9.3　Solaris同步 158==

##### ==5.9.4　Pthreads同步 159==

#### ==5.10　替代方法 160==

##### ==5.10.1　事务内存 161==

##### ==5.10.2　OpenMP 162==

##### ==5.10.3　函数式编程语言 162==

### ==第5.5章　死锁 163==

前面提到了死锁。这里对死锁进行几个概念性的总结。
 如果在一个系统中一下四个同时满足，那么就有可能引起死锁：

+ 互斥。 必须至少有一个资源只能允许一个线程使用。
+ 占用并等待。一个线程应该占有一个资源并且的等待其他资源。
+ 非抢占。 资源不能被抢占。
+ 循环等待。 有一组等待进程(P0,P1,....Pn), P0等待的资源P1占用，P1的资源P2占用...., Pn等待的资源P0占用，形成一个死循环。

##### ==5.11.1　系统模型 163==

##### ==5.11.2　死锁特征 164==

##### ==5.11.3　死锁处理方法 167==

##### ==5.12　小结 168==

### 第6章　CPU调度 180

#### 6.1　基本概念 180

根据之前进程一章中的介绍，进程的执行一般会分为两个执行周期：**CPU执行**和**I/O等待**。

![img](https:////upload-images.jianshu.io/upload_images/4793176-924ab730ed571bdf.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### ==6.1.1　CPU-I/O执行周期 180==

##### ==6.1.2　CPU调度程序 181==

##### 6.1.3　抢占调度 181

**抢占调度**：在进程执行的过程中，CPU被操作系统中断而执行其他进程的调度，称为抢占调度。
 	**非抢占调度**：一个进程分配到CPU 后，该进程会一直占用CPU，直到它终止或者切换到等待状态。

##### ==6.1.4　调度程序 182==

#### ==6.2　调度准则 182==

#### 6.3　调度算法 183

##### 6.3.1　先到先服务调度 183

最简单的调度算法：可以采用FIFO队列实现。

##### 6.3.2　最短作业优先调度 184

当CPU变为空闲时，它会执行占用CPU最短的进程。

SJF 调度可以是抢占的，也可以是非抢占的。

##### 6.3.3　优先级调度 186

每个进程都有优先级关系，而具有最高优先级的进程会分配到CPU。具有相同优先级的可以按照FCFS的算法执行。
 通常优先级的的分配根据进程的特点来自己定义的。
 优先级调度会产生 无穷阻塞或者 饥饿， 会使优先级低的进程得不到长时间执行。解决方法是老化，当一个进程15分钟没执行，那么它的优先级上升。

##### 6.3.4　轮转调度187

将一个较小单位定义为一个时间片，时间片的大小一般为10~100ms。就绪队列作为循环队列。CPU调度程序循环整个队列，为每个进程分配不超过一个时间片的CPU。

##### 6.3.5　多级队列调度 189

将进程分为不同的级别，比如，前台进程和后台进程。这两种类型的进程的响应时间要求不同，进而调度算法也不同。
 **多级队列调度**是将就绪队列分为多个单独的队列。根据进程属性。然后处于不同队列的调度算法不同。

##### ==6.3.6　多级反馈队列调度 190==

#### 6.4　线程调度 191

##### 6.4.1　竞争范围 191

由线程库调度的线程，他只会和它所在进程之间的线程进行竞争，这叫做**进程竞争范围**。
 由内核进行调度的线程，它会和系统中所有线程进行竞争，这种叫做**系统竞争范围**

##### 6.4.2　Pthreads调度 191

Pthreads 库的API 实现了两种竞争范围的接口：

+ PTHREAD_SCOPE_PROCESS: 采用进程竞争范围。
+ PTHREAD_SCOPE_SYSTEM: 采用系统竞争范围。

##### 6.5　多处理器调度 193

对于多处理系统，CPU 调度的一种方法是让一个处理器 处理所有的调度决定、I/O处理以及其他活动，其他的处理器只执行用户代码。这种称谓**非对称多处理**。这种调度比较简单，因为不存在多个CPU共享数据。
 还有一种方法是**对称多处理（SMP）**。即每个处理器自我调度，所有进程都可能处于一个共同的就绪队列，或者每个处理器有自己私有的就绪队列。这时就需要保证两个处理器不会同时选择同一个进程。

处理器亲和性： 当一个进程运行在一个特定的处理器上时，它会进行缓存数据。但是当一个进程在下次执行的时候，切换到了其他的处理器时，这个缓存就无效了。所以大多数SMP系统都会保证进程从一个处理器移到另外一个处理器。这就是处理器的亲和性。

##### ==6.5.1　多处理器调度的方法 193==

##### 6.5.2　处理器亲和性 193

SMP系统上，重要的就是保持所有的处理器的负载平衡。否则就会出现，一个或者多个处理器空闲，而其他处理器处于高负载状态。

负责均衡通常有两种办法：**推迁移和拉迁移**。推迁移指的是一个特定的任务周期性的检查每个处理器的负载，如果发现不平衡，那么通过将进程从超负载处理器推送到空闲的处理器上。对应的，空闲的处理器从忙的处理器上拉取一个任务，就是拉迁移。Linux调度程序实现了这两种技术。

##### 6.5.3　负载平衡 194

SMP系统上，重要的就是保持所有的处理器的负载平衡。否则就会出现，一个或者多个处理器空闲，而其他处理器处于高负载状态。

负责均衡通常有两种办法：**推迁移和拉迁移**。推迁移指的是一个特定的任务周期性的检查每个处理器的负载，如果发现不平衡，那么通过将进程从超负载处理器推送到空闲的处理器上。对应的，空闲的处理器从忙的处理器上拉取一个任务，就是拉迁移。Linux调度程序实现了这两种技术。

##### ==6.5.4　多核处理器 194==

#### ==6.6　实时CPU调度 196==

##### ==6.6.1　最小化延迟 196==

##### ==6.6.2　优先级调度 197==

##### ==6.6.3　单调速率调度 198==

##### ==6.6.4　最早截止期限优先调度 199==

##### ==6.6.5　比例分享调度 200==

##### ==6.6.6　POSIX实时调度 200==

#### ==6.7　操作系统例子 202==

##### ==6.7.1　例子：Linux调度 202==

##### ==6.7.2　例子：Windows调度 204==

##### ==6.7.3　例子：Solaris调度 206==

#### ==6.8　算法评估 207==

##### ==6.8.1　确定性模型 208==

##### ==6.8.2　排队模型 209==

##### ==6.8.3　仿真 209==

##### 



## 第三部分　内存管理

### 第7章　内存 220

#### 7.1　背景 220

##### 7.1.1　基本硬件 220

内存是现在计算机运行的核心。CPU可以直接访问的通用存储只有***内存\***和处理器的***内置寄存器\***。机器指令可以用内存地址作为参数，但是磁盘地址不可以。

在访问速度上，寄存器的内容一般都可以在一个时钟周期解释并执行完。但是对于内存，可能需要多个时钟周期。所以为了访问速度上能更加快速，一般会有**高速缓存**。

在系统安全上，首先，用户的进程不能影响操作系统的执行; 在多用户系统上，还应该保护不同的进程之间不能互相影响。所以一般会有专门的硬件方式来进行保护。
 为了进程之间不会相互影响，首先，我们需要确保每个进程都有一个单独的内存空间。单独的进程内存空间可以保护进程不互相影响。
 为了分开内存空间，我们需要能够确定一个进程可以访问的合法地址的范围；***并且确保该进程只能访问这些合法地址。\*** 一般通过两个寄存器来实现， **基地址寄存器**和**界限地址寄存器。**
 如下图，如果基地址是300040，而界限寄存器为120900，那么程序可以合法访问到的地址为300040 ~ 420939的所包含的地址。

![img](https:////upload-images.jianshu.io/upload_images/4793176-b28434b6a47448b0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1152/format/webp)

##### 7.1.2　地址绑定 222

一般的，进程在执行时，会先从磁盘被调入内存，而且根据内存的管理，有可能在执行的时候，被换出到磁盘上。但是磁盘的内容会被调入到物理内存的哪个地方，这个就是后面要讨论的内存分配机制。

大多数系统允许用户进程放在屋里内存中的任意位置。也就是说，物理内存的地址是从000000开始的，但是用户进程的地址不必也是000000。
 实际上，在用户程序被执行前，会有很多的步骤，这些步骤中对地址会有不同的表示方法。

+ **编译**：在程序中，一般我们不用地址来表示，比如C语言都是按照符号来表示的，在编译的时候，它是一堆符号地址，编译器将这些符号地址绑定到**可重定向的**地址，比如：从本模块的第 14个字节开始。
+ **加载**：在上面编译后，生成了可重定向地址，那么在加载时，加载器或者链接器会把这些可重定向地址 加上自己在地址空间的基地址，从而形成绝对地址。
+ **执行**：进程在执行的时候，从地址空间中映射到计算机的物理地址上。具体映射方法需要先了解下逻辑地址空间和物理地址空间。

##### 7.1.3　逻辑地址空间与物理地址空间 222

在程序执行的时候，从程序中加载的地址，称为逻辑地址。它是一个照片，我可以用手指出哪里是这个人的眼睛，耳朵，鼻子，但是这只是在照片的位置。这个人本人的肉体才是真正存在的东西。物理地址就好比这个人的肉体，是一个实际存在的东西。

加载时的地址绑定方法生成一个相同的逻辑地址和物理地址。然而，执行时，逻辑地址和物理内存上的地址不一定是一样的。后面内存分配会讲到。

##### 7.1.4　动态加载 223

动态加载指的是一个进程只有在执行的时候才被加载，而不是一次性把所有的进程全部放进内存中。
 动态链接类似于动态加载。但是这里不是加载和是链接，会延迟到运行。动态链接这里链接的一般称为**动态链接库**。它是系统库，一般会驻留在内存中，供所有的用户程序使用。
 它的原理是：当程序中有动态链接，在二进制文件中，每个库程序的引用都会有个存根（stub)。存根是一小段代码，用来指出如何定位适当的内存驻留库程序，以及不在内存时，如何加载。

动态链接库的好处是，一个库的更新，所有使用它的程序都会自动使用新的版本。

##### ==7.1.5　动态链接与共享库 224==

#### 7.2　交换 224

##### 7.2.1　标准交换 224

最后一个基本概念是交换（swap）。进程可以暂时从内存交换到备份存储中，当再次执行的时候，再调回到内存中。

##### 标准交换

这种交换和概念定义的一样，一般备份存储为磁盘。
 这种交换的上下文切换时间相当高，假设用户进程大小为100MB,并且磁盘的传输速度为50M/s， 那么换人和换成分别需要2s,加起来可能需要4s。

交换也受到其他因素的约束，如果我们想要换出一个进程，那么确保该进程是完全处于空闲的。特别需要关注等待I/O。当需要换出一个进程以释放内存时，该进程可能正在等待I/O操作。
 然而如果I/O异步访问用户内存的I/O缓冲区，那么该进程就不能被换出。假定由于设备忙，I/O操作正在排队等待。如果我们需要换出P1进程而执行P2进程，那么I/O操作可能试图使用现在已经属于进程P2的内存。
 解决的办法有两种：

+ 不能换出等待处理I/O的进程
+ I/O操作的执行只能操作系统的缓冲池。
     只有在进程进入时，操作系统缓冲才能与进程之间进行数据迁移。但是这增加了开销。

##### 7.2.2　移动系统的交换 225

虽然用于PC和服务器的大多数操作系统都支持一些交换。但是移动设备通常不支持任何形式的交换。
 移动设备通常是闪存（U盘，内存卡）而不是更大空间的硬盘。所以这是不支持交换的一个原因之一。还有一些原因就是，闪存和内存之间的数据交换的吞吐量差。
 苹果的IOS系统，当空闲内存降低到一定的阈值时，不是采用交换，而是要求应用程序资源放弃分配的内存，只读数据可以从系统中删除，以后如有必要再从闪存中重新加载。它修改的数据不会被删除，然而， 操作系统可以终止任意的未能释放足够多内存的用户程序。
 Android系统不支持交换。而当没有足够内存的时候，系统可以终止任何进程。但是他在删除前会把应用程序的状态写入到闪存，以便后面快速启动。
 由于这些限制，所以移动的程序员应该小心分配内存，避免有资源泄露。

#### 7.3　连续内存分配 226

##### 7.3.1　内存保护 226

我们通常需要把多个进程同时放在内存中，因此我们需要进程内存分配。一般内存分为两个区域：一个用于驻留在操作系统中，另一个用于用户进程。操作系统可以放在高位置，也可以放在低内存，影响这一决定的通常为中断向量的位置。一般PC是放在低内存位置的。

内存分配最简单的分配方式就是:将内存分为多个**固定大小的分区**。每个分区可以包含一个进程，因此，多道程序设计的受限于分区数。使用这种多分区方法，那么当一个分区空闲时，可以从进程的就输入队列中选择一个进程，加载到空闲分区。虽然这种方案在现在的操作系统中已经不用，但是它的思想为后面很多内存分配提供了思想。

除此之外，还有一个**可变分区**方案，它的主要思想是：
 操作系统有一个表，用于记录哪些内存可用，哪些内存已经使用。
 开始，所有的内存都有可用于用户进程，因此可以看做是一个很大的内存。随着进程进入系统，操作系统根据所有进程的内存需要和现有的内存情况，决定哪些系统可分配内存。
 在任何时候，都有一个可用块大小的列表和一个输入队列。操作系统根据调度算法来对输入队列进行排序。内存不断地分配给进程，直到下一个进程的内存需求不能满足为止，这时没有足够大的可用块来加载进程。或者继续往下扫描，输入队列，看看有没有其他内存需求比较小的进程可以被满足。

##### 7.3.2　内存分配 227

通常，按上面的分配算法，可用的内存块分散在内存里的不同大小的**孔（这里既是一个一个小的内存块）**的集合。当进程需要内存时，系统为该进程查找足够大的空，如果孔太大，那么就分为两块：一块分配给内存，另一块还给孔集合。当进程终止时，他将释放内存，该内存将还给孔集合。
 如果新孔与其他孔相邻，那么将这个孔合并成一个大孔。这是系统继续检测，有没有符合要求的处于等待的进程。
 这种分配方法被称为**动态存储分配问题**。 这种方法在我们程序设计中有很多的例子，比如（C++STL的内存分配，memory cache）。

这种方法有许多变种，其中根据从一组可用的孔中选择一个空闲孔的最为常用的方法包括： **首次适应， 最优适应，最差适应**。

+ 首次适应：分配首个足够大的空。查找从头开始，也可以从上次首次适应结束的时候开始，寻找一个足够大的空闲孔，分配个进程。
+ 最优适应： 分配足够小的孔，只要可以满足进程的内存需求。需要查找整个列表，所以可能要求，列表按大小进程排序，以便更快速的找到最小满足的孔。
+ 最差适应： 分配最大的孔。这个和最优相对，也要查找整个列表。
     经过模拟显示：首次适应和最优适应的执行效果和空间利用方面都要优于最差适应。

##### 7.3.3　碎片 228

**外部碎片**：根据上面的三种算法，随着进程的加载到内存和从内存退出，空闲内存空间被分为小的片段。当总的可用内存纸盒可以满足请求但并不连续时，他不能分配给进程。这些 不连续的内存块就成了碎片了。
 **内部碎片**： 假设有一个1800字节的碎片，并采取上面的算法进行分配，进程需要1798 的内存。那么分配完了以后，就剩下两个字节，这两个字节也需要维护，但是维护的代价比他本身的价值要大的多，所以在分配的时候，把这两个字节也分配给进程，那么这两个字节就在进程内部形成了碎片。

在清理内存碎片的时候，我们有两种解决方案：

+ 把所有的进程的移到内存的一端，把孔移到另一端，但是这种代价太高。
+ 允许进程的逻辑地址空间不是连续的; 从而让代码和数据分离，进程在使用的时候,当物理内存可用，就允许进程分配内存, 然后根据不同的基地址寄存器和界限寄存器来控制和保护进程的内存。 也就是**分段和分页**技术。

#### 7.4　分段 228

##### 7.4.1　基本方法 229

![img](https:////upload-images.jianshu.io/upload_images/4793176-3597f196730e4221.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

segmentation.png

在编写代码的时候，程序员认为它是由主程序加上一组方法，过程或者函数的集合。他还包括这种数据结构：对象，数组，堆栈，变量。每个模块或者数据元素通过名字来引用。而不关心具体的内存位置。
 **分段**就是支持这种用户视图的内存管理方法。逻辑地址空间是由一组段构成。每个段都有名称和长度，比如，代码段，数据段，堆栈，堆等等。
 地址指定了段名称和段内偏移。所以段一般是由两个量来进行表示：

```xml
           <段号， 偏移>
```

通常，在编译用户程序的时候，编译器会自动生成：

+ 代码
+ 全局变量
+ 堆
+ 栈
+ 程序库

##### 7.4.2　分段硬件 229

虽然用户可以实现通过**二维地址**来引用程序内的对象，但是实际物理内存还是一维的字节数组。因此需要我们需要定义一个映射方法，把二维地址转换为一维地址。
 这个地址是通过**段表** 来实现的。段表的每个条目都有一个**段基地址**和**段界限**。段基地址包含该段在内存中的开始物理地址，而界限地址指定该段的长度。

![img](https:////upload-images.jianshu.io/upload_images/4793176-744f534ee5be6b4a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

段表.png

从图中，我们可以看到段表和每个段的分布情况。

再看看分段的硬件实现，每个逻辑地址都有两个部分来实现，***段号s\***和***偏移d\***。 当我们从CPU的指令中得到一个地址时，它有段号和偏移d，段号首先去段表中进行索引，获取到段基(base)地址，然后加上偏移d,然后和 界限地址进行比较，得到最终的物理内存地址。**（这里段表上两个元素实际上是基地址寄存器和界限寄存器组成的结构体数组）。**

![img](https:////upload-images.jianshu.io/upload_images/4793176-b7c9e585ac1c2c2c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

#### 7.5　分页 230

分段允许进程的物理地址空间可以分为多个段，从而地址可以是非连续的。然而分段还是避免不了有外部碎片的情况，因为在给每个段分配内存的时候，各个段的大小是不一样的。从而让整块物理内存还是会有缝隙。**分页**避免了这种情况。

#### 基本概念

##### 7.5.1　基本方法 231

实现分页的最基本方法涉及到将物理内存分为固定大小的块，称为**帧或者页帧(frame，从英文名可以看出来这是个框架)**；而将逻辑内存也分为同意大小的块，叫做**页或者页面（page, 从英文名可以看出来这是一页纸）**。

当需要执行一个进程时，它的页从文件系统或者磁盘中加载到内存的可用页帧中。磁盘也被划分为固定大小的块，它与单个内存帧（frame）或者分为多个内存帧的大小一样。

##### 7.5.2　硬件支持 233

分页的硬件支持和分段类似，由CPU解码后的指令地址分为两个部分：**页码（page number）和页偏移（page offset）。**
 页码作为页的索引。页表包含每页所有物理内存的基地址。这个基地址与页偏移的组合形成了物理内存地址。

![img](https:////upload-images.jianshu.io/upload_images/4793176-c2d89cb076baa6da.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

分页硬件.png

再看看页表的结构(**注意这里的地址用的是帧码，也就是frame的页号，而不是字节**)。

![img](https:////upload-images.jianshu.io/upload_images/4793176-b71cd268778df6e0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

页表.png



我们通过上面的学习，应该可以看到，分页本身是一种动态的重定向。每个逻辑地址由分页硬件绑定某一个物理地址。采用分页类似于一个基地址寄存器，每个基址代表一个一个内存帧。

当系统进程需要执行时，它将检验该进程的大小，进程的每页都需要一帧。因此一个进程需要n页，那么内存中至少应该有n帧。如果有，那么可分配给新进程。进程的第一页装入一个已分配的帧，帧码会在装入后写到进程的页表中，下一页分配给另一帧，然后帧码也会写到进程的页表中。

![img](https:////upload-images.jianshu.io/upload_images/4793176-85c19399b15a5e08.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

页分配过程.png

有了分页，程序员就可以将内存看做一整块来处理。内存的分配和管理交给操作系统。他可以认为他的程序在逻辑上是连续的，但是实际上，物理内存中他们是分布在不同的帧上的。在操作系统管理内存的分配时：它需要知道哪些帧已经分配，哪些帧空着，总共有多少帧。这些都在内核的叫做**帧表 （frame table）**的数据结构中。在帧表中每个条目对应一个帧，以表示该帧是空闲还是已占用；如果占用，是被那个进程占用。

##### 7.5.3　保护 236

###### 分页的效率讨论

+ 页表具体的结构是怎样的？
     每个操作系统都有自己保存页表的方法。有的系统为每个进程分配一个页表，然后通过指针保存在PCB中。当启动一个进程的时候，他应该首先加载一个用户寄存器，并通过保存的用户页表来定义正确的硬件页表值。
+ 页表的硬件实现时怎样的？
     页表的硬件实现有很多种，但是最简单的方法是通过一组专用的寄存器来实现。这些寄存器应用高速逻辑电路来构成，以高效的进行分页地址的转换。由于每次访问内存都要经过分页映射，因此效率是一个重要的考虑因素。CPU分派器在加载其他寄存器时，当然也需要加载这些寄存器，当然，这些页表寄存器的加载指令也是特权的。
+ 用寄存器来实现页表的问题？
     如果页表比较小的时候，用页表寄存器时效率是很高的。但是现在的计算机基本都允许页表很大，对这些机器，采用寄存器就不行了。因此，页表需要放在内存中，并将**页表基地址寄存器（PTBR）**指向页表，改变页表只需要操作这一个寄存器就可以了。
+ 采用页表基地址寄存器的效率？
     采用这种方法的问题是访问用户内存位置的所需时间，如果需要访问位置i， 那么应该首先利用 PTBR的值，再加上i的页码，作为偏移，来查找页表。这一任务需要内存访问。 根据所得的帧码，再加上页偏移，就得到了真是的物理地址。接着就可以访问内存内的所需位置。采用这种方案，访问一个字节需要两次内存访问（一次是页表，一次是字节）。这样内存的访问效率就减半了。

因为效率问题，我们的解决方案是采用专用的，小的，查找快速的高速硬件缓冲，它称为**转换表缓冲区（TLB）**。它是一个关键的高速内存。
 -TLB的工作原理
 TLB条目由两部分组成：键和值。当关联内存根据给定值查找时，它会同时与所有的键进行比较。那么就得到相应的值。搜索的特别快。现代TLB的查找硬件是指令流的一部分，基本不会考虑性能代价。
 TLB只包含了少量的页表条目。当CPU产生一个逻辑地址后，它的页码就送到TLB，如果他能找到这个页码，它的帧码也就立即可用，可用于访问内存。如果页码不在TLB中，那么就需要访问页表（访问内存）。取决于CPU，这可能由硬件自动处理或者通过系统的中断来处理。当得到帧码后，就可以用它来访问内存，同时还会把页码和页帧加到TLB中。

![img](https:////upload-images.jianshu.io/upload_images/4793176-72a3b4881ea3a9f9.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

TLB_hardware.png



有的TLB在每个TLB条目中还保存**地址空间标识符**，他唯一标识每个进程，并为进程提供地址空间的保护。
 TLB是一个硬件功能，我们不需要关心，但是了解他的功能和特性，有利于我们在开发时，进行系统的优化。

###### 分页的安全讨论

分页环境下的内存保护是通过与每个帧关联的保护位来实现的，通常这些 保护位会保存在页表中。

用一个位可以定义一个页是可读可写或只可读。每次内存引用都要通过页表，来查找正确的帧码。在计算物理地址的同时，还可以通过检查保护位来保护系统对内存的正确操作。

还有一个位通常与页表中的每一条项目相关联：**有效-无效位**。
 当该位是有效位时，该值表示相关的页在进程的逻辑空间中内，因此它是合法的页。
 当改为为无效位时，表示相关的页不在进程的逻辑地址空间内。
 通过有效--无效位，非法地址会被捕捉，然后操作对该地址进行允许和不允许对某页的访问。

![img](https:////upload-images.jianshu.io/upload_images/4793176-b8064e8c50da538f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

有效-无效位.png

对于上图：14位地址空间 (016383）的系统，假设有一个程序，它的有效地址空间是010468。如果页的大小是2k,那么他会有5个页表。然而，如果试图产生页表 6和页表7，那么操作系统就会根据有效-无效位的捕捉到非法操作。

##### 7.5.4　共享页 237

分页的内存机制，还有一个优先就是可以共享公共代码。
 对于一个分时系统，假设有一个支持40个用户的系统，每个用户都指向一个文本编辑器，每个文本编辑器包括150k的代码和50k的数据空间，那么久需要8000kb的内存来支持40个用户。
 但是，如果代码是**可重入代码**。则可以进行共享。

**可冲入代码**是不能自我修改的代码，他在执行期间不会改变。因此两个或者多个进程可以同时执行相同的代码。每个进程都有自己的寄存器和数据存储，以保证数据的正确性和安全。

![img](https:////upload-images.jianshu.io/upload_images/4793176-2eaca3f1ef92a764.png?imageMogr2/auto-orient/strip|imageView2/2/w/1114/format/webp)

分页的代码共享.png

所以在物理内存中只需要保存一个编辑器的副本，每个用户的页表映射到编辑器的同一个物理副本，但是数据页映射到不同的帧。因此支持40个用户，只需要一个编辑器副本，和40个50k的数据空间，所以只需要2150k,而不是8000k。

#### 7.6　页表结构 238

##### 7.6.1　分层分页 238

大多数现代计算机系统支持大逻辑地址空间（2^32 ~ 2^64）。这种情况下，页表本身可以非常大。
 例如：假如具有32位逻辑地址空间的一个计算机系统。如果系统的页大小为4KB(2^12)。那么页表可以多达100万的条目 （2^32/ 2^12)。假设某个项目有4字节。那么每个进程需要4MB的地址物理地址来存储页表本身。显然，我们并不想在内存中连续分配这么多页表。
 这个问题的一个简单的解决方法就是讲页表划分为更小的块。完成这种划分方法有很多种。

最简单的方法就是使用两层分页算法，就是将页表再分页，例如，再次假设一个系统，具有32位逻辑地址空间和4K大小的页。一个逻辑地址被分为20位的页码和12位的页偏移。
 因此要对20位的页表进行再分页，所以该页码可以分10位的页码和10位的偏移。这样一个逻辑地址就会分为如下表示。

![img](https:////upload-images.jianshu.io/upload_images/4793176-4b83cc792e40b501.png?imageMogr2/auto-orient/strip|imageView2/2/w/814/format/webp)

二级页码表示.png

其中p1表示的用来访问外部页表的索引，而p2是内部页表的页偏移。采用这种结构的地址转换方法。由于地址转换有外向内，所以这种也称为**向前映射页表**。

![img](https:////upload-images.jianshu.io/upload_images/4793176-4da4feb8e5b8aeea.png?imageMogr2/auto-orient/strip|imageView2/2/w/1124/format/webp)

向前映射页表.png

在这种分页结构的方案中，假设，系统是64位系统，那么当它的地址空间就有2^64， 当再以4KB作为地址的话，那么页表就会2^52个条目，那么就把页表进行细分，从而形成三级分层分页，四级分层分页等等。

为了装换每个逻辑地址，74位的系统需要7个级别的分页，如此多的内存访问时不可取的，从而分层分页在64位的系统并不是最优的。

##### 7.6.2　哈希页表 240

处理大于32位的地址空间的常用方法是**哈希页表**，采用虚拟页码作为哈希表值。哈希页表的每一个条目都包括一个链表，该链表的元素哈希到同意位置（这表示它们有了哈希冲突）。每个元素由三个字段组成：虚拟页码，映射的帧码，指向链表内下一个元素的指针。
 该算法的工作如下：虚拟地址的虚拟页码哈希到哈希表。用虚拟页码与链表内的第一个元素的第一个字段相比较。如果匹配，那么相应的帧码（第二个字段）就用来形成物理地址。如果不匹配，那么与链表内的后续节点的第一个字段进行比较。以查找匹配的页码。该方案如图：

![img](https:////upload-images.jianshu.io/upload_images/4793176-0346307137428bb0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

哈希页表.png

这里书上提到的虚拟页码可以只看作是页码。（之所以叫虚拟页码，是因为根据虚拟内存的概念，逻辑地址空间可以比物理地址大，所以多出来的部分被称为虚拟的，具体介绍会在下一章提到）。

已提出用于64位地址空间的这个方案的一个变体。
 此变体采用 **聚簇页表**类似于哈希页表。不过哈希表内的每个条目引用多个页而不是单个页。单个页表的条目可以映射到多个物理帧。聚簇页表对于 **稀疏**地址空间特别有用。这里引用的是不连续的并且散布在整个地址空间。

##### 7.6.3　倒置页表 240

通常，每个进程都有一个关联的页表。该进程所使用的每个页都在也表中有一项（或者每个虚拟页都有一项）。这种表示方法比较自然，因为进程是通过虚拟地址来引用页的。然后是操作系统将这些地址转换为物理内存地址。
 由于页表是按照虚拟地址排序的，操作系统可计算所对应条目在页表的位置，可以直接使用该值。这种方法缺点就是：当每个页表包含百万级的数目时。会有性能问题，而且需要大量的内存来保存页表信息。

解决的方法处理上面的两种方法外，还有一种就是**倒置页表**。
 这里先介绍一个IBM RT 的倒置页表的表示方法：



```xml
 <pid, 页码， 偏移>
```

对于每个真正的内存页或者帧，倒置页表只有一个条目。每个条目包含**保存在真正内存位置上的页的虚拟地址**，以及拥有**该页的进程信息**。具体的过程如图：

![img](https:////upload-images.jianshu.io/upload_images/4793176-b2f3eeac04224beb.png?imageMogr2/auto-orient/strip|imageView2/2/w/1174/format/webp)

倒置页表.png



这里的进程的信息就是以前提到的 空间地址标识符（ASID）。主要原因是由于一个倒置页表通常包含了多个不同的映射物理内存的地址空间。具体进程的每个逻辑页可映射相应的物理帧。

采用倒置页表的系统在实现共享内存的时候会有问题，因为共享内存的实现为：将多个地址空间映射到同一个物理地址。这种方法，不能用于倒置页表，因为每个物理页只有一个虚拟的页条目，一个物理页不能有多个共享的虚拟地址。

##### 7.6.4　Oracle SPARC Solaris 241

#### 7.7　例子：Intel 32位与64位体系结构 242

##### 7.7.1　IA-32架构 242

IA-32 系统的内存管理可以分为分段和分页两个部分，工作如下：CPU 生成逻辑地址，并交给分段单元，分段单元为每个逻辑地址生成 一个线性地址。 然后线性地址交给分页单元，以生成内存的物理地址。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a7f5d5558feec217.png?imageMogr2/auto-orient/strip|imageView2/2/w/1186/format/webp)

IA-32地址映射.png

###### IA-32分段

IA-32 架构允许一个段的大小最多可以达到4G， 每个进程最多有16K个段。进程的逻辑地址空间分为两部分。
 第一部分最多由8K段组成，这部分是单个进程私有；
 第二部分也是最多由8K段组成，这部分是所有进程共享。

第一部分保存在**局部描述符表（LTD）**中，第二部分保存在**全局描述符表(GDT)**中，他们的每个 条目都是8个字节，包括一个段的详细信息。比如段基地址和段界限。
 逻辑地址一般为二元数组（选择器，偏移），选择器是一个16位的数：

![img](https:////upload-images.jianshu.io/upload_images/4793176-a6328cd919282f73.png?imageMogr2/auto-orient/strip|imageView2/2/w/684/format/webp)

IA-32段选择器.png

其中s表示段号，g表示实在LTD中还是在GDT中， p表示保护信息。
 段的寻址过程为：



![img](https:////upload-images.jianshu.io/upload_images/4793176-f54febf1e2ae3f0a.png?imageMogr2/auto-orient/strip|imageView2/2/w/896/format/webp)

IA-32段寻址.png

###### IA-32 分页

IA-32架构的页可分为4K，或者4M 。采用4K的页，IA-32采用二级分页方法。其中的32位的寻址和表示请参照二级分页算法。

为了提高物理内存的使用率，IA-32 的页表可以被交换存在磁盘。因此，页目录的条目通过一个**有效位**，以表示该条目所指的页表实在内存还是在磁盘上。如果页表再磁盘上，则操作系统可通过其他31位来表示页表的磁盘位置。之后根据需要调入内存。

随着软件开发人员的逐步发现，32位架构的4GB内存限制，Inter通过**页地址扩展**，以便允许访问大于4GB的物理地址空间。

引入页地址扩展，主要是将两级的分页方案扩展到了三级方案， 后者的最后两位用于指向页目录指针表。



![img](https:////upload-images.jianshu.io/upload_images/4793176-53f84ddd84b982f2.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

页地址扩展.png

页地址扩展使得地址地址空间从32位增加到了36位。Linux和Mac OS X 都支持了这项技术。

##### 7.7.2　x86-64 244

X86-64 支持更大的逻辑和物理地址空间。支持64位的地址空间意味着可寻址的内存达到惊人的2^64字节。64位系统有能力访问那么多的内存，但是实际上，目前设计的地址远没有那么多。
 目前提供的x86-64 架构的机器最多采用四级分页，支持48位的虚拟地址。它的页面大小可以4KB，2MB，或者1G。

![img](https:////upload-images.jianshu.io/upload_images/4793176-20175a5d8d848fd3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

x86-64位寻址.png

#### 7.8　例子：ARM架构 245

虽然Intel的芯片占了大部分的市场，但是移动设备的架构一直采用的是32位ARM的架构。现在的iPhone 和iPad 都或得了ARM的授权。Android的智能手机也都是ARM的处理器。
 ARM支持的页面大小：

+ 4KB 或者16KB。

+ 1MB 或者16MB的页（称为段）。
     系统使用的分页取决于所引用的是页还是段。
     一级分页用于1MB和16MB的段。 二级分页用于4KB和16KB的页。
     ARM的MMU的地址转换如图：

    ![img](https:////upload-images.jianshu.io/upload_images/4793176-f4c8d94e589fd04c.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

    ARM地址转换.png

ARM架构还支持两级TLB（高速缓存）。在外部，有两个微TLB： 一个用于数据，另一个用于指令。微TLB也支持 （ASID）进程地址空间标识符。 在内部 有一个主 TLB。 地址转换从微TLB级开始。如果没有找到，那么再检查主TLB。如果还没找到，再通过页表进行硬件查找。





### 第8章　虚拟内存 251

#### 8.1　背景 251

**虚拟内存**：虚拟内存可以是一种逻辑上扩充物理内存的技术。它会把进程的逻辑地址空间进行扩展。（书中说的虚拟内存，可以看做是假象的逻辑内存，虽然在平常使用top 命令看到了虚拟内存表示一些具体的物理实体，但是而书上一直用虚拟内存这个词个人认为不是很好 ）。
 **虚拟地址空间**： 其实这里的虚拟地址空间 感觉还是可以用逻辑地址空间的概念来代替。它表示进程在内存中的逻辑地址范围。

#### 8.2　请求调页 253

##### 8.2.1　基本概念 253

我们在把进程加载到内存的过程中， 并不是把进程的所有执行代码都加载到内存，而是在需要执行的时候才加载进内存，这种技术叫**请求调页**，常常用于虚拟内存系统。
 对于请求页的逻辑内存，页面只有在程序执行期间被请求时才被加载。因此，从未访问的那些页从不加载到物理内存中。而这种交换的方式也被称为 **惰性交换**。

基本执行过程：当换入进程时，调页程序会猜测在该进程被再次换出之前，会用到哪些页。调页程序不是调入整个进程，而是把哪些要使用的页调入内存。从而减少交换时间，而且还能避免物理内存空间的浪费。
 具体实现：在使用这种方案的时候，需要一定的硬件支持。以区分内存的页面和磁盘的页面。通常在页面上会有一个保护位，它的最后一位就是用于提供这个功能的。这个位是**有效-无效位**。
 当这个位被置为有效位时， 相关的页面是合法的，并且在内存中。
 当这个位被置为无效位时，页面无效或者是有效但是在磁盘上。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3b35b4bb87639188.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

有效-无效位的页表.png



当进程在执行过程中的时候，如果访问内存驻留的页面时，会一切顺利。但是当访问到尚未调入的页面的时候，对，标记为无效的页面会产生**缺页错误**。分页硬件在通过页表转换地址时，会发现无效位被设置，从而陷入操作系统。
 一般缺页错误的处理很简单：

1. 检查这个进程的内部表，以确认该引用是有效的还是无效的内存访问。
2. 如果引用无效，那么终止进程。如果引用有效但是没有调入内存，那么现在就调入。
3. 找到一个空闲帧。
4. 调度一个磁盘操作，以将所需页面读到刚分配的帧。
5. 磁盘读取完成，修改进程内部表和页表。以指示该页现在处于内存中。
6. 重新启动被陷入中断的指令。该进程能访问所需要的页面，就好像原来他就在内存中一样。

![img](https:////upload-images.jianshu.io/upload_images/4793176-ebb65c6dc523c111.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

缺页中断处理.png

**交换空间**

在进程执行过程中，需要一个辅助设备，用于保存不在内存中的那么页面。这种外存通常为高速硬盘，称为**交换设备**，用于交换的这部分磁盘叫做**交换空间**。
 请求调页的中使用的交换空间的磁盘I/O 通常要快于文件系统的。交换空间的文件系统更快，因为它是按更大的块来分配的。并且不采用文件查找和间接分配方法。
 因此，系统可以在进程启动时，将整个文件映像复制到交换空间，然后从交换空间执行请求调页，从而获取到好的分页吞吐量。
 另一个选择是开始时在文件系统进行请求调页，但是在置换页面时将换出的页面写入交换空间。这保证了后续的调页都是从交换空间完成的。

##### ==8.2.2　请求调页的性能 256==

#### 8.3　写时复制 258

在父进程调用fork()创建子进程时后，我们曾说子进程应该创建一个父进程地址空间的副本，复制属于父进程的页面。然而考虑到了许多子进程在创建之后立马执行了exec()函数替换了，所以复制父进程的页面可能没有必要。因此我们采用了**写时复制技术**。他通过允许父进程和子进程最初共享相同的页面来进行工作。这些页面是共享页面，被标记为写时复制，这意味着任何进程写入共享页面，那么就创建一个共享页面的副本。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a3444c3a02ba3b10.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

进程1修改页面C之前.png

![img](https:////upload-images.jianshu.io/upload_images/4793176-278a5d774d34c1da.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

进程1修改页面C之后.png

可以看到使用写时复制技术，仅复制任何一个进程修改的页面，所有未修改的页面可以由父进程和子进程共享。
 还要注意，只有可以修改的页面才需要标记写时复制。不能修改的页面可以父子共享（比如代码段）。

在实现上，许多操作系统都为这类请求提供了一个空闲的**页面池（和个人认为和内存池差不多）**。当进程的堆栈或者堆要进行扩展时，或者有写时复制的请求时，通常分配这里的空闲页面。操作系统分配这些页面通常采用**按需填0**的技术。以清理原来的内存。
 Linux 提供了 fork()的变种，vfork()。vfork()的操作不同于写时复制的fork()。它会将父进程挂起，子进程使用父进程的地址空间。
 由于vfork()不会发生写时复制，所以他的修改的页面在父进程中是可以看到的。当子进程在创建后立即会被exec()的情况下，可以使用vfork()。因为他们有复制页面，可以高效的启动新进程。

#### 8.4　页面置换 259

在内存分配页面的时候，不仅用于保存程序页面，用于I/O 的缓冲也需要消耗大量的内存，这种使用会增加内存置换算法的压力。确定多少内存给用户程序，多少内存给I/O缓冲去是个很棘手的问题，有的系统给I/O缓冲分配了固定百分比的内存，有些系统则允许用户进程和I/O子系统进行竞争所有内存。

##### 8.4.1　基本页面置换 260

当用户进程在执行时，可能发生缺页错误。操作系统确定所需要页面的磁盘位置，但是却发现内存上没有空闲的帧。所有内存都在使用。这时，操作系统不能终止一个进程来释放，因此这样太不友好了。此时，它会选择交换出一个进程，以释放它在所有帧并降低多道程序。这种技术就是**页面置换**。

![img](https:////upload-images.jianshu.io/upload_images/4793176-57b98698191163f4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

需要页面置换的情况.png

在没有空闲帧的情况下，那么就要查找当前不在使用的一个帧，并释放它，它就是哪个**牺牲帧**，他被换出到交换空间，并修改它的页表。
 可以看到当没有空闲帧的情况下，需要两个页面传输（一个传入，一个传出）。这种情况世界加倍了缺页错误处理时间，并增加了有效的访问时间。
 为了减少置换的时间，系统提供了一个**修改位**。
 在这种方案中，每个页面或者帧都有一个修改位，两者的关联采用硬件。每当一个页面内的任何字节被写入时，它的页面修改位会由硬件来设置，以表示该页面被修改过。
 当要选择一个页面置换时，它会先看这个页面或者帧有没有被修改过，如果没有修改过就不用将它换出，直接进行换入，将它的空间覆盖。

为了实现请求调页，那么就要涉及两个重要问题，**帧分配算法和页面置换算法**。

##### 8.4.2　FIFO页面置换 262

在置换算法中，FIFO 是最简单的算法。
 FIFO页面置换算法为每个页面记录了调用的时间。当必须置换页面是，选择最旧的页面，而不需要记录调入页面的确切时间。可以创建一个FIFO队列来管理所有的内存页面。置换的每次都是队首。

![img](https:////upload-images.jianshu.io/upload_images/4793176-44dcd18b90568e56.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

FIFO置换算法.png

FIFO页面置换算法易于理解和编程，但是它的性能并不是总是十分理想的。

##### 8.4.3　最优页面置换 263

**最优页面置换**：指的是置换**最长时间不会使用**的页面。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a4f1eb155d6aeb4a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

最优页面置换.png

**注意看**： 第五个和第6个数之间的， 0，3，0，4。 这里0刚一执行就被换出去了，这个是和下面LRU算法的区别。
 这种方法会产生最低的可能缺页错误率。然而，最优置换算法难以实现，因为需要应用直到引用串的未来推算。

##### 8.4.4　LRU页面置换 263

如果最优算法不可行，那么最优算法的近似或许可以。FIFO和OPT算法的关键区别在于，一个是页面调入内存的时间，一个是页面将来可能使用的时间。两者都不是最可控的。那么我们使用了最近的过去作为将来的近似，那么可以置换**最长时间没有使用**的页。

**最近最少使用(LRU)**的算法：LRU 置换将每个页面与它的上次使用的时间关联起来。

**注意看两个概念： OPT的算法是最长时间不会使用，而LRU是最长时间没有使用。细细的思考，就会发现一个是未来时间，一个是过去时间。**
 如果还没看懂，就来看看两个图的比较。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a8653ad8cfce7a42.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

最优页面置换.png



![img](https:////upload-images.jianshu.io/upload_images/4793176-d89768a0a570982d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

LRU置换算法.png

看出来区别了吗？OPT算法的在第五次置换的时候把刚执行过的0页面换出去了。这个它虽然名字是最优置换，但是不一定就是最优的解法了。

LRU算法通常被人们认为是不错的策略，也是被很多系统用到的页面置换算法。

##### ==8.4.5　近似LRU页面置换 265==

##### ==8.4.6　基于计数的页面置换 266==

##### ==8.4.7　页面缓冲算法 267==

除了特定的页面置换算法外，还经常有其他的措施。比如在系统中，会保留一个空闲帧缓冲池。当出现缺页终端时，它会首先在缓冲池中读取空闲帧。这允许进程快速启动。

##### ==8.4.8　应用程序与页面置换 267==

#### ==8.5　帧分配 267==

##### ==8.5.1　帧的最小数 268==

##### ==8.5.2　分配算法 269==

##### ==8.5.3　全局分配与局部分配 269==

##### ==8.5.4　非均匀内存访问 270==

#### ==8.6　系统抖动 270==

##### ==8.6.1　系统抖动的原因 271==

##### ==8.6.2　工作集模型 272==

##### ==8.6.3　缺页错误频率 273==

##### ==8.6.4　结束语 274==

#### 8.7　内存映射文件 274

假设采用标准系统调用open(), read()， write()来顺序读取磁盘文件。每个文件的访问都需要系统调用和磁盘访问。或者采用虚拟内存技术，将文件I/O作为常规的内存访问。这种方法称为**内存映射**文件，它允许虚拟内存和文件进行逻辑关联。

##### 8.7.1　基本机制 274

实现文件的内存映射，是将每个磁盘块映射到一个或者多个内存页面。（**注意这里是内存页面，内存页面，内存！！！重要的事情说三遍**）

最初，文件访问按普通请求调页来进行，从而产生缺页错误。这样，**文件的一个页面或者多个页面大小的内容从文件系统读取到了物理内存。**以后，文件的读写就按常规的内存访问来处理。从而减少了磁盘的访问。

请注意：内存映射文件的的写入和对磁盘的写入**不是同步的**。有的系统定期检查文件的内存映射页是否被修改，以便选择是否更新物理磁盘文件。当关闭文件时，内存映射页的数据会写入到磁盘，并从进程虚拟内存中删除。

有些操作系统只有特定的系统调用才能提供内存映射，这个在共享内存的使用的时候是必须的。

![img](https:////upload-images.jianshu.io/upload_images/4793176-178b106f1a324d2b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

内存映射文件.png

多个进程允许并发的内存映射同一个文件，以便允许数据共享，任何一个进程的写入都会修改映射文件的数据，并且其他映射同一文件的进程都可以看到。
 可以看到上图中，每个共享进程的虚拟内存映射指向物理内存的同一页面，而该页面有磁盘块的复制。内存映射的数据还可以支持***写时复制\***功能。

##### 8.7.2　共享内存Windows API 275

##### 8.7.3　内存映射I/O 277

**内存映射I/O（Memory-mapped I/O MMIO）**: 在MMIO中，内存和I/O设备共享同一个地址空间。MMIO是应用得最为广泛的一种IO方法，它使用相同的地址总线来处理内存和I/O设备，I/O设备的内存和寄存器被映射到与之相关联的地址。当CPU访问某个内存地址时，它可能是物理内存，也可以是某个I/O设备的内存。
 因此，用于访问内存的CPU指令也可来访问I/O设备。每个I/O设备监视CPU的地址总线，一旦CPU访问分配给它的地址，它就做出响应，将数据总线连接到需要访问的设备硬件寄存器。为了容纳I/O设备，CPU必须预留给I/O一个地址区域，该地址区域不能给物理内存使用。

![img](https:////upload-images.jianshu.io/upload_images/4793176-9a884fadfed5ab14.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

内存映射I:O.png

#### 8.8　分配内核内存 278

##### 8.8.1　伙伴系统 278

在用户模式下运行进程请求额外的内存时（new, malloc）时，从内核维护的空闲页帧列表上分配页面（书上的这句话，我理解为，new和malloc的时候，在虚拟空间会在堆空间进行申请内存，但是映射到物理内存，需要重新分配物理帧，而这个帧是由内核维护的空闲帧列表上来的）。
 这个帧的列表是用来进行页面置换的，所以在它相邻的部分，不一定是来自于进程的同一个连续的虚拟内存，而是来自于物理内存的不同空闲页面。而且在用户申请单个字节的时候，他为了避免内存碎片，会分配整个帧给进程。

用于内核分配内存的空闲内存池列表，和内核给用户进程分配的页面的列表不一样的。

+ 内核需要为不同大小的数据结构请求内存。有的小于一页，因此，在内核使用分配内存的时候，他应该尽量小的最小化碎片浪费。
+ 用户模式进程分配的页面不必连续物理内存。

这部分是自己网上资料理解的（书上写的太烂了）：
 Linux内核内存管理的一项重要工作就是如何在频繁申请释放内存的情况下，避免碎片的产生。Linux采用伙伴系统解决外部碎片的问题，采用slab解决内部碎片的问题。
 在这里我们先讨论外部碎片问题。避免外部碎片的方法有两种：
 （1）是利用分页单元把一组非连续的空闲页框映射到连续的线性地址区间；
 （2）伙伴系统：是记录现存空闲连续页框块情况，以尽量避免为了满足对小块的请求而分割大的连续空闲块。

#### 伙伴系统（解决外部碎片）

使用场景：内核中很多时候要求分配连续页，为快速检测内存中的连续区域，内核采用了一种技术：伙伴系统。
 原理：系统中的空闲内存总是两两分组，每组中的两个内存块称作伙伴。伙伴的分配可以是彼此独立的。但如果两个小伙伴都是空闲的，内核将其合并为一个更大的内存块，作为下一层次上某个内存块的伙伴。
 具体先看下一个例子：



![img](https:////upload-images.jianshu.io/upload_images/4793176-d7378ee97812c662.png?imageMogr2/auto-orient/strip|imageView2/2/w/990/format/webp)

伙伴系统具体例子.png

这是一个最初由256KB的物理内存，内核需要请求21KB的内存。
 那么最初他将256KB的内存进行分割，变成两个128KB的内存块，AL和AR，这两个内存块称为**伙伴**。 随后他发现128KB也远大于21KB，于是他继续分割为两个64KB的内存块，发现64KB 也不是满足需求的最小的内存块，于是他继续分割为两个32KB的。32KB再往下就是16KB，就不满足需求了，所以32KB是它满足需求的最下的内存块了，所以他就分割出来的CL 或者CR 分配给需求方。
 当需求方用完了，需要进行归还，然后他把32KB的内存还回来，它的另一个伙伴如果没被占用，那么他们地址连续，就合并成一个64KB的内存块，以此类推，进行合并。

注意这里的所有的分割都是进行二分来分割，所有内存块的大小都是2的幂次方。
 他们的具体结构会保存在一个结构中。



```cpp
#ifndef CONFIG_FORCE_MAX_ZONEORDER
#define MAX_ORDER 11
#else
#define MAX_ORDER CONFIG_FORCE_MAX_ZONEORDER
#endif
#define MAX_ORDER_NR_PAGES (1 << (MAX_ORDER - 1))

  struct free_area {
    struct list_head    free_list[MIGRATE_TYPES];//空闲块双向链表
    unsigned long       nr_free;//空闲块的数目
  };
  struct zone{
       ....
       struct free_area    free_area[MAX_ORDER];  
       ....
  };
```

它的从上到下都是都保存一个链表，每一条链表的第一个页框的物理地址是该块大小的整数倍。例如，大小为 16个页框的块，其起始地址是 16 * 2^12 （2^12 = 4096，这是一个常规页的大小）的倍数。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3a5636a58efece78.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

伙伴系统结构.png

Linux2.6为每个管理区使用不同的伙伴系统，内核空间分为三种区，DMA，NORMAL，HIGHMEM，对于每一种区，都有对应的伙伴算法。

伙伴系统的缺点：由于分配的都是2的幂次方的内存，会有内部碎片。

##### 8.8.2　slab分配 279

slab有两种，一种是专用cache,如下分析：
 它的基本思想是将内核中经常使用的对象放到高速缓存中，并且由系统保持为初始的可利用状态。将内核中经常使用的对象（比如进程描述符，文件描述符），他们也是需要内存的，所以一开始就直接分配好，按照所占用的内存大小分门别类放在下文讲的 kmem_cache (其实就和伙伴一摸一样)，下次用到直接拿去用，用完在放回原位置就行了。（简单的说就是带有结构体的内存池）。

需要注意的是slab分配器只管理内核的常规地址空间（直接被映射到内核地址空间的ZONE_NORMAL和ZONE_DMA）。

图 1 给出了 slab 结构的高层组织结构。在最高层是 cache_chain，这是一个 slab 缓存的链接列表。可以用来查找最适合所需要的分配大小的缓存（遍历列表）。cache_chain 的每个元素都是一个 kmem_cache 结构的引用（称为一个 cache）。它定义了一个要管理的给定大小的对象池（固定大小，即分门别类）。（`kmem_cache == 对象池`）

![img](https:////upload-images.jianshu.io/upload_images/4793176-de15446c6746e091?imageMogr2/auto-orient/strip|imageView2/2/w/402/format/webp)

slab 结构



每个缓存都包含了一个 slabs 列表，这是一段连续的内存块（通常都是页面）。存在 3 种 slab：

+ slabs_full 完全分配的 slab
+ slabs_partial 部分分配的 slab
+ slabs_empty 空 slab，或者没有对象被分配

slab 列表中的每个 slab 都是一个连续的内存块（一个或多个连续页），它们被划分成一个个对象。这些对象是从特定缓存中进行分配和释放的基本元素。注意 slab 是 slab 分配器进行操作的最小分配单位，通常来说，每个 slab 被分配为多个对象。
 由于对象是从 slab 中进行分配和释放的，因此单个 slab 可以在 slab 列表之间进行移动。例如，当一个 slab 中的所有对象都被使用完时，就从 slabs_partial 列表中移动到 slabs_full 列表中。

slab分配器分配的优点：

+ 可以提供小块内存的分配支持
+ 不必每次申请释放都和伙伴系统打交道，提供了分配释放效率
+ 如果在slab缓存的话，其在CPU高速缓存的概率也会较高。
+ 伙伴系统的操作对系统的数据和指令高速缓存有影响，slab分配器降低了这种副作用
+ 伙伴系统分配的页地址都页的倍数，这对CPU的高速缓存的利用有负面影响，页首地址对齐在页面大小上使得如果每次都将数据存放到从伙伴系统分配的页开始的位置会使得高速缓存的有的行被过度使用，而有的行几乎从不被使用。slab分配器通过着色使得slab对象能够均匀的使用高速缓存，提高高速缓存的利用率

当然，slab分配器也存在缺点：

+ 对于微型嵌入式系统，它显得比较复杂，这是可以使用经过优化的slob分配器，它使用内存块链表，并使用最先适配算法
+ 对于具有大量内存的大型系统，仅仅建立slab分配器的数据结构就需要大量内存，这时候可以使用经过优化的slub分配器

#### ==8.9　其他注意事项 280==

##### ==8.9.1　预调页面 280==

##### ==8.9.2　页面大小 280==

##### ==8.9.3　TLB范围 281==

##### ==8.9.4　倒置页表 282==

##### ==8.9.5　程序结构 282==

##### ==8.9.6　I/O联锁与页面锁定 283==

#### ==8.10　操作系统例子 284==

##### ==8.10.1　Windows 284==

##### ==8.10.2　Solaris 285==

#### 8.11 个人理解

1.逻辑地址和物理地址？进程的地址空间。内存，虚拟内存和交换空间是怎么回事？ 为什么进程地址空间可以比物理内存大？
 个人理解这个这个问题就通俗的比喻下就是一座高楼，和图纸的比喻。（虽然这个在后面分页分段的映射上不能使用，但是这是一个区别物理和逻辑地址好的方法）。
 物理地址就是楼层的建好后的实体， 每个物理地址代表这个楼房的一个位置。
 逻辑地址就好比手里拿到的工程师拿到图纸，你也可以通过地址指出楼的不同位置，但是只是逻辑上的，而不是实际运行的。

进程的地址空间其实也类比下，实际的物理地址空间。比如32位的物理地址空间为0~4G这个范围内的空间。 逻辑地址当然也是纸面上的地址空间。每个进程有它们自己的图纸，也就有它们自己的地址空间。

内存就是物理内存，虚拟内存其实是一种技术。虚拟内存技术，使得在进程执行过程中，通过页面置换来实现多道程序并发运行的。很多书上讲到的虚拟内存，其实更贴切的应该成为逻辑内存。就是图纸上的需要的内存。
 linux上top命令显示的 VIRT结果其实就是进程“需要的”虚拟内存大小，包括进程使用的库、代码、数据，以及malloc、new分配的堆空间和分配的栈空间等。
 具体区别参考：[linux top 命令---VIRT,RES,SHR,虚拟内存和物理内存](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fcrazyhacking%2Farticle%2Fdetails%2F40788553)

交换空间指的是在磁盘上开辟的一个分区，这个分区用来在进程切换的过程中，当需要的指令不在物理内存，而物理内存都被占用时进行换进换出操作的。

最后一个问题，虚拟内存其实就是图纸。图纸可以画出超出楼层，比如来个地下室，但是物理内存是固定的，他不能说加就加。

1. 内存机制是用怎样的方法保证各个进程之间执行时不会影响？
     每个进程都有自己的图纸，也就是有自己的地址空间，每次在被调到内存的时候，
     1.早期的时候，它是有一个基地址寄存器和界限地址寄存器，他每次在被加载到内存的时候，都会保存好这两个寄存器，然后每次 访问的时候都会根据这两个寄存器的值来进行保护。
2. 后来有了分段和分页技术，他们都有类似与前面的寄存器的东西，存放在段表和页表中，每次访问的时候，都是通过这两个东西保证不会访问其他进程的内存。他们的值是不可以随便改的，只有操作系统有权限改。
3. 内存分配分段和分页机制？
     分段：在进程编译，链接，生产逻辑地址的时候，就把他们分别进行分段，分为代码段，数据段，堆栈，堆等等。然后在执行的时候把每一段的内容分别映射到物理内存上，并通过段表来进行记录，所以每个进程应该都有一个段表。其中包括每一段的基地址和界限地址。
     分页：将每个进程执行前，把逻辑地址空间进行分页，分成很多很多小的页面，同时把物理内存也根据页面的大小分为很多很多页框（帧 ）（再次强调注意在书中用词，逻辑地址空间的分出来的部分叫页面，物理内存中分出来的叫帧）。当进程中的页面加载到内存中后，通过页表进行记录。

4.虚拟内存怎么理解？
 答案参见1中

5.什么是写时复制？

这个主要用于在父进程创建子进程的时候，而子进程一般会直接exec()替换新的地址空间，所以没有必要在fork的时候进行复制父进程的地址空间，而是让父子共享父进程的地址空间。但是也有那种不进行exec的子进程，那么这种只有父子进程开始修改共享部分的时候，共享部分才会复制一个副本，达到父子进程互不影响的目的。
 一般的只有写的时候，才进行复制，而写是只有写的那一页进行复制，没写到的页面不会复制，这个是在页表中的一个单独的保护位来实现的。
 为了取消写时拷贝，父子同步共享一个数据时，一般可以使用vfork()代替。但vfork()主要还是用于 创建后直接exec()。

1. 页面置换算法？

    一般页面置换算法有：FIFO 算法，OPT(最优页面置换)算法，LRU（最近最少使用），基于计数的页面置换和 页面缓冲池算法

    主要说下OPT 和LRU的区别：

    注意看两个概念： OPT的算法是最长时间不会使用，而LRU是最长时间没有使用。细细的思考，就会发现一个是未来时间，一个是过去时间。

    如果还没看懂，就来看看两个图的比较。

    ![img](https:////upload-images.jianshu.io/upload_images/4793176-a8653ad8cfce7a42.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

    最优页面置换.png

![img](https:////upload-images.jianshu.io/upload_images/4793176-d89768a0a570982d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

LRU置换算法.png

看出来区别了吗？OPT算法的在第五次置换的时候把刚执行过的0页面换出去了。这个它虽然名字是最优置换，但是不一定就是最优的解法了。

7.共享内存的实现？
 其实共享内存主要用了两种技术手段。
 一种是内存映射技术，一种是绑定地址。
 共享内存首先是在自己的地址空间建立一个共享内存区域。然后通过映射的方法，绑定到物理内存。这个区域比其他的区域不同，它可以被其他的进程 通过映射的方式绑定，也就是说，多个进程的逻辑内存，可以绑定同一个物理内存地址。从而实现了进程间的数据共享。

1. 内存分配策略？
     主要两种策略，一种是伙伴系统，一种是slab分配器。
     伙伴系统是：每次把一块内存一分为2， 他们都是2的幂次方大小的内存块。两个相同的内存块互为伙伴。然后通过一个结构体数组进行连接，每个结构体后面都对应的一个链表，它们分别会把不同大小的内存块进行连接，从而以后每次需要内存块的时候，可以快速的查找到匹配的内存块。（但是伙伴系统会产生内部碎片）。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3a5636a58efece78.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

伙伴系统结构.png

slab分配：
 它类似于一个包含了一系列的不同结构体的内存池。
 在内核中，对于一些常用的数据结构PCB，文件描述符等，因为经常使用到，所以每次创建和释放都是很麻烦的，所以内核为它们准备了以后分好的内存，每次使用直接使用这个结构体，用完了以后再还给slab，这样就加快内存分配的效率，并且将内核中经常使用的这些对象池放到高速缓存中。

一般对于这种动态存储进行匹配的时候，可以使用**首次适应，最优适应**等适配方法。



## 第四部分　存储管理

### 第9章　大容量存储结构 298

#### 9.1　大容量存储结构概述 298

##### 9.1.1　硬盘 298

现代计算机系统最常用的外部大量存储就是磁盘或者硬盘。

![img](https:////upload-images.jianshu.io/upload_images/4793176-ca2934b68d709a72.png?imageMogr2/auto-orient/strip|imageView2/2/w/888/format/webp)

磁盘结构.png

磁盘的大致结构为图上所示。他包括一系列的盘片（platter），在盘片的两面都涂着磁质材料。通过在盘片上进行磁性记录可以保存信息。

读写磁头（read-write head）在一个盘片“飞行”从而可以读写数据。磁头是悬挂在磁臂(arm)上的，磁臂将所有磁头作为一个整体和进行一起移动。

每一个磁盘的盘片（platter）上逻辑的可以分为磁道（track）。磁道又可以分为扇区(sector)。同一磁道上的柱面形成了柱面(cylinder)。每个磁盘驱动器有数千个同心柱面，每个柱面又有数百个扇区。

一般判断一个磁盘的好坏是通过**传输速率**和**定位时间**。
 **传输速率**：在驱动器和计算机之间数据传输的速率。
 **定位时间**又分为**寻道时间**（移动磁臂到所有的柱面所需时间）和 **旋转延时**（旋转磁臂到所要的扇区所需要的时间）。

磁盘驱动器通过I/O总线的一组电缆连到计算机。在使用中有多种可用的总线，这就对应了多种 I/O接口。其中包括 **硬盘接口技术（ATA）** ，**串行接口（SATA）**，外部串行（eSATA）,通用 串口总线(USB)。

数据传输的总线由 **控制器**来进行。在计算机的那一头的叫做**主机控制器**， 在I/O驱动器的这一头为**磁盘控制器**。

I/O操作在执行的时候，计算机会下达一个命令到主机控制器，接着主机控制器通过消息将该命令下达给 磁盘控制器， 磁盘控制器开始操作磁盘驱动器工作， 磁头和磁臂开始工作。**磁盘控制器通常具有内置缓存，他会把数据传输到缓存上，而到主机的传输，则由内置缓存和主机控制器进行**。

##### ==9.1.2　固态磁盘 299==

SSD 具有与传统硬盘相同的特性，容量大，但是它会更可靠，他们有移动部件；而且更快，没有寻道时间和延迟。

##### ==9.1.3　磁带 299==



#### 9.2　磁盘结构 300

##### 9.3　磁盘连接 300

计算机访问磁盘有两种方式。一种方式是通过I/O端口，称为**主机连接存储**， 另外一种是分布式系统的远程主机（称为网络连接存储）。

##### 9.3.1　主机连接存储 300

主机连接存储：主机连接通过本地的端口来访问存储。这些端口使用多种技术。典型的台式机采用I/O 总线，一般**SATA的接口**更为常用。

##### 9.3.2　网络连接存储 301

网络连接存储：网络连接存储（NAS）设备是一种专用的存储结构，可以通过数据网络来进行远程访问。客户机通过远程过程调用（RPC）来访问连接存储。 RPC通过TCP或者UDP协议进行访问。

网络连接存储（NAS）设备是一种专用的存储结构，可以通过数据网络来进行远程访问。客户机通过远程过程调用（RPC）来访问连接存储。 RPC通过TCP或者UDP协议进行访问。

![img](https:////upload-images.jianshu.io/upload_images/4793176-3930b99f113a0684.png?imageMogr2/auto-orient/strip|imageView2/2/w/1186/format/webp)

网络连接存储.png

##### 9.3.3　存储区域网络 301

网络连接存储有一个缺点： 存储I/O操作消耗太多的网络带宽。从而增加网络的延迟。这在大型C/S 的模式下非常严重。
 存储区域网络(SAN)为专用网络，它是在网络协议上使用了一种新的存储协议。



![img](https:////upload-images.jianshu.io/upload_images/4793176-a0275fcbbf405f7b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

存储区域网络.png

它的优势是在于灵活。多个主机和多个存储阵列（storage array）之间可以连接到同一个SAN上，存储可以动态分配到主机。 也就是说当主机的磁盘不够时，他可以通过配置SAN 来进行动态的磁盘分配。

#### 9.4　磁盘调度 302

每当进程需要进程磁盘访问的时候，他就向操作系统发出一个系统调用。每个请求需要一些信息：

+ 这个操作是输入还是输出
+ 传输的磁盘地址是什么
+ 传输的内存是什么
+ 传输的扇区数是多少
     如果磁盘驱动器和控制器空闲，则立即执行请求，否则需要将这个请求添加到磁盘驱动器的请求队列中。

对于处于请求队列的所有I/O请求，操作系统在进行选取的时候，应该需要一种调度算法来保证I/O设备的效率最大化。那么如何来评估这个效率呢
 对于磁盘驱动器（就是开始那张图的全部零件加起来的装置）是根据两个量来进行评估的。**访问速度**和 **磁盘带宽**。
 根据上面的图再来回顾下两个概念：
 **寻道时间**：移动磁臂到所有的柱面所需时间。
 **旋转延时**：旋转磁臂到所要的扇区所需要的时间。
 **磁盘带宽**：传输自己的总数除以所需要的总时间。

![img](https:////upload-images.jianshu.io/upload_images/4793176-ca2934b68d709a72.png?imageMogr2/auto-orient/strip|imageView2/2/w/888/format/webp)

磁盘结构.png

在考虑三个因素的情况下，下面有不同的调度算法。

##### 9.4.1　先来先服务FCFS调度 302

和进程的CPU调度和 内存调度一样，先到的先服务。实现起来比较见到，直接通过一个FIFO的队列实现即可。

![img](https:////upload-images.jianshu.io/upload_images/4793176-18a8abbdafd5a8b7.png?imageMogr2/auto-orient/strip|imageView2/2/w/952/format/webp)

FCFS磁盘调度.png

##### 9.4.2　最短寻道时间优先SSTF调度 302

顾名思义：当在移动磁头到别处处理请求之前，处理高进当前磁头位置最近的请求。
 和CPU调度的最短作业优先（SJF）调度一样，它会导致磁盘请求的饥饿。

![img](https:////upload-images.jianshu.io/upload_images/4793176-076289f9e06bc26a.png?imageMogr2/auto-orient/strip|imageView2/2/w/948/format/webp)

最短寻道时间.png

##### 9.4.3　扫描SCAN调度 303

这种调度方法也叫电梯调度，这个也比较好理解， 磁臂从磁盘的一段开始，向另一端移动，在移过每个柱面时，处理请求。当到达磁盘的另一端时，磁头反方向移动，继续处理。



![img](https:////upload-images.jianshu.io/upload_images/4793176-2986cd748e7d2d4e.png?imageMogr2/auto-orient/strip|imageView2/2/w/924/format/webp)

电梯调度.png

##### 9.4.4　循环扫描C-SCAN调度 304

这个是电梯算法的一个变种，它会把向一个方向进行扫描，直到磁盘的另一端结束，然后立马回到磁盘的另外一端，再次进行扫描。

![img](https:////upload-images.jianshu.io/upload_images/4793176-c0322d3324e7541b.png?imageMogr2/auto-orient/strip|imageView2/2/w/980/format/webp)

C-SCAN.png

##### 9.4.5　LOOK调度 304

它是C-SCAN的变种，它在扫描的时候并不是扫描整个磁盘，而是在队列中的最小和最大之间。

![img](https:////upload-images.jianshu.io/upload_images/4793176-82b20d6fc4999679.png?imageMogr2/auto-orient/strip|imageView2/2/w/920/format/webp)

LOOK调度.png

##### 9.4.6　磁盘调度算法的选择 304

如何选择调度算法？
 SSTF是最常见的，但是对于磁盘负荷较大的系统，SCAN和C-SCAN会更好，因为他不会产生饥饿。当然对于任何调度算法，性能还在余于请求的数量和类型。除此之外，文件的分配方式也会影响磁盘的服务请求。后面来讲文件系统结构和方式。
 对于没有没有移动磁头的SSD来说，它通常可以用FCFS的策略。

#### 9.5　磁盘管理 305

##### 9.5.1　磁盘格式化 305

一个新的磁盘是一个空白盘：它只是一个磁性记录材料的盘子。在磁盘上可以存储数据之前，他必须分为扇区，以便磁盘控制器能够读写。这个过程叫做**低级格式化**或者**物理格式化**。
 低级格式化为每个扇区使用特殊的数据结构，填充磁盘。每个扇区的数据结构通常包括**头部，数据区域和尾部**。头部和尾部包含了一些磁盘控制器的使用信息。如扇区的纠错代码（Error-Correcting, ECC）。当控制器通过正常I/O写入一个扇区的数据时，ECC采用根据区域所有字节而计算的新值来进行更新。在读取一个扇区时，ECC会重新计算。并且与原来的值进行比较。如果存储的和计算的值不一样，表示扇区已经损坏，并且扇区可能已经损坏。

大多数磁盘在制作的时候已经进行了低级格式化。这种格式化也方便磁盘制造商进行测试磁盘，并且初始化逻辑块号到无损磁盘扇区的映射。

##### 9.5.2　引导块 306

为了开始运行计算机，如打开电源或者重启时，它必须有一个**初始化的程序**来运行。它初始化系统的所有部分，从CPU寄存器到设备控制器和内存，接着启动操作系统。
 对于大多数计算机，自举程序处于**只读存储器 （ROM）**。它是只读的数据，不会被计算机病毒影响，所以位于ROM的自举程序一般放在固定位置，它的功能就是在通电或者复位的时候，从磁盘上调入完整的** 引导程序**。 这个引导程序存储在磁盘上，通常称为启动块。

![img](https:////upload-images.jianshu.io/upload_images/4793176-12243cf932625cfb.png?imageMogr2/auto-orient/strip|imageView2/2/w/804/format/webp)

windows的磁盘引导.png



windows允许将磁盘分为多个分区，有一个分区是**引导分区（boot partition）**,其中包含了操作系统和设备程序。 一般的windows系统将引导代码存放在磁盘的第一个扇区，它为**主引导记录（MBR）**。
 windows的启动过程，通电后，首先加载并允许在系统ROM中的代码，这个代码会指示系统从MBR上加载引导代码。除了引导代码,MBR还包括 一个分区表和一个表示（标示那个分区为引导系统）。

##### 9.5.3　坏块 307

因为磁盘具有移动部件并且容错差（磁头在磁盘表面上方），容易出现故障。有时候，这种故障时彻底的，需要更换磁盘，并且从备份介质上将其内容恢复到新盘。更为常见的是， 一个或者多个扇区坏掉，而且大多数磁盘出厂时就有坏块。

复杂的磁盘在恢复坏块时，它的控制器维护磁盘内的坏块列表。这个列表在**出厂低级格式化**时初始化，并且在磁盘使用寿命内更新。低级格式化将一些块放在一边作为备用，操作系统看不到这些块。控制器可以采用备用块来逻辑地代替坏块。这种方案叫做**扇区备用**。
 一般的扇区处理：

+ 操作系统尝试读取逻辑块87
+ 控制器计算ECC,发现87为坏块。他向操作系统汇报这一情况。
+ 当操作系统下次启动时可以运行特殊命令，请求控制器转换成备用块替代坏块
+ 之后的访问87，就使用的是 备用块。

它的替代方案是：**扇区滑动**。 假定17号扇区变坏，并且第一个可用备用逻辑扇区在202之后，那么他就会进行滑动，类似于在有序数组的插入一个中间值，所有的扇区都往后移动一个扇区。直到扇区18写入扇区19。然后以后的17就映射到现在的18。

#### ==9.6　交换空间管理 308==

##### ==9.6.1　交换空间的使用 308==

##### ==9.6.2　交换空间位置 308==

##### ==9.6.3　交换空间管理例子 309==

#### 9.7　RAID结构 309

##### 9.7.1　通过冗余提高可靠性 310

**磁盘冗余阵列（Redundant Array of Independent Disk, RAID）** 是由多块硬盘通过RAID 控制器控制管理组成的一个更大容量的逻辑盘，在操作系统中识别为一个盘符。

**RAID**

什么是RAID？
 RAID :磁盘阵列
 1988年由加利福尼亚大学伯克利分校提出来的。
 多个磁盘合成一个“阵列”来提高更好的性能，冗余，或都提供。
 RAID 可以提高磁盘I/O能力，也可以提高磁盘的耐用性。
 RAID的实现方式：
  可以外接：通过扩展卡提供适配性
  内置式RAID:主板集成RIAD控制器，安装方便，目前主流的服务器都是内置式的。
  软件实现：通过os系统实现。

##### ==9.7.2　通过并行处理提高性能 311==

##### 9.7.3　RAID级别 311

RAID-0 ：RAID-0 是将多块硬盘捆绑成为一个大容量的逻辑磁盘，可以同时从多块硬盘读取数据，也可以同时往多块硬盘写入数据。磁盘I/O 是单块硬盘的多倍。在所有RAID模式下，同等硬盘下，RAID-0是最快的。但是RAID0 没有数据冗余能力，一个磁盘损坏，所有数据都丢失。
 特点：

+ 读写都得到提升
+ 可用空间大（n*min）
+ 没有容错能力，只要有一块硬盘损坏，数据就丢失了。
+ 需要2个或以上组成。

![img](https:////upload-images.jianshu.io/upload_images/4793176-de0552bbb7d901fb.png?imageMogr2/auto-orient/strip|imageView2/2/w/856/format/webp)

RAID_0.png

RAID-1 :
 RAID-1也被称为镜像，仅用于两块硬盘的情况下，同样的数据在两块硬盘上分别存储一份，两块硬盘中的数据完全相同。即使有一块硬盘出现问题， 也不会影响数据安全与中断系统运行。
 RAID-1 主要用于数据安全性比较高的环境，比如，数据库，电脑的系统盘等。RAID1不会提高性能。

特点：

+ 读性能提升，但写性能下降

+ 可用空间只有百分之50，相当于有一个备份盘。

+ 有冗余能力

+ 至少要2个或以上的硬盘

    ![img](https:////upload-images.jianshu.io/upload_images/4793176-4994e0573ebd76f5.png?imageMogr2/auto-orient/strip|imageView2/2/w/950/format/webp)

    RAID1.png

RAID-5 ：
 RAID 5至少需要三个硬盘，RAID5 不是对存储的数据备份，而是把数据和相对应的奇偶校验信息存储到组成RAID5 的各个磁盘上，并且就检验码信息和相对应的数据分别存储在不同的磁盘上。当RAID5 的任何一个磁盘数据反生损坏，可以利用剩下的数据和相应的奇偶校验码信息去恢复被损坏的数据。
 特点：

+ 读写性能提升
+ 可用空间是n-1
+ 有容错能力，但最多1块硬盘损坏。
+ 需要3块或以上的磁盘组成。

![img](https:////upload-images.jianshu.io/upload_images/4793176-210d8dfe398932fa.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

RAID5.png

RAID-6 ：
 RAID6 至少需要4个硬盘，与RAID5 相似，RAID6也是不对 存储数据进行备份，而是把数据和相对应的奇偶校验信息存储在RAID6的各个磁盘， 与RAID5不同的是，RAID6有两个校验盘，即使同时又两块硬盘出问题，它也可以利用剩下的数据和相应的奇偶信息恢复损坏的磁盘。
 特点：

+ 读写性能提升
+ 可用空间n-2
+ 有容错能力，允许最多2块硬盘损坏
+ 需要4块或以上组成

![img](https:////upload-images.jianshu.io/upload_images/4793176-680db8a4e0432ee4.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

RAID6.png

RAID-1+0 :
 RAID 10 仅用于硬盘的情况下，把4个硬盘分为两组，每组的两个互为RAID1。然后 在做RAID1。
 特点：

+ 读写性能提升
+ 可用空间n/2
+ 有容错能力，每组最多只能坏一块硬盘。
+ 需要4块或以上组成

![img](https:////upload-images.jianshu.io/upload_images/4793176-60f8abb0a12ef2c3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

RAID10.png

RAID-0+1 ：多块硬盘先实现RAID0,然后再组成RAID1
    相比RAID10,表面看没什么区别，但风险更大，使用一般不采用。



![img](https:////upload-images.jianshu.io/upload_images/4793176-4dd26c7975889558.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

RAID01.png

RAID-5+0 :多块硬盘先组成RAID5，每组的硬盘不少于3组，再组成RAID0

![img](https:////upload-images.jianshu.io/upload_images/4793176-a7df247310622745.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

RAID50.png

JBOD:将多块硬盘组成一个大的连续使用的空间，可以空间是sum（s1 s2...）。一般使用在对数据安全性要求不高的场合，家庭使用比较合适。

![img](https:////upload-images.jianshu.io/upload_images/4793176-cdd3c81a66f6b349.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

jobRAID.png

![img](https:////upload-images.jianshu.io/upload_images/4793176-7d417bcac32b602a.png?imageMogr2/auto-orient/strip|imageView2/2/w/1080/format/webp)

磁盘阵列对比表.png



##### ==9.7.4　RAID级别的选择 314==

##### ==9.7.5　扩展 315==

##### ==9.7.6　RAID的问题 315==

#### ==9.8　稳定存储实现 316==



### 第10章　文件系统接口 322

#### 10.1　文件概念 322

##### ==10.1.1　文件属性 322==

文件是操作系统对存储设备的物理属性加以抽象定义的逻辑存储单位。

##### ==10.1.2　文件操作 323==

文件包括文件的属性和操作：
 属性包括：名称，标识符，类型，位置，尺寸，权限，创建和修改时间，文件的所有者和使用者等等。
 文件的操作包括：创建文件，读文件，写文件，重新定位文件的读写位置，删除文件和截断文件。

上面的文件操作涉及搜索目录。为了方便文件操作的搜索更简单，许多系统在首次使用文件之间进行系统调用open()。 操作系统在**内核上**维护一个**打开文件表**用于维护所有打开文件的信息。当进行访问文件时，直接使用这个表的索引指定文件。
 当多个进程一起访问同一个文件时，操作系统采用两级文件表，进程维护一个文件表和操作系统维护一个进程表。单个进程的文件表执行整个系统的打开文件表。一旦有进程打开了一个文件表，系统表就会包含该文件的条目，当一个进程执行open(), 进程表和系统表都增加一个条目（加入系统表中没有这个文件时）。当系统表中有这个条目时，它会进行将**文件打开计数**加1。每次close() 将打开计数减1。当为0 时，可以删除。

##### ==10.1.3　文件类型 327==

##### ==10.1.4　文件结构 328==

文件类型也可用于指示文件的内部结构。源文件和目标文件都具有一定的结构，以便匹配读取他们程序的格式。

##### 10.1.5　内部文件结构 328

磁盘系统通常具有明确的块大小，这是扇区大小的决定的。所有的磁盘I/O按块为单位执行，而所有的块的大小相同。一般为512字节。
 要对一个文件的位置的偏移进行定位。

#### 10.2　访问方法 328

我们在访问文件信息的时候，一般根据文件的结构来进行访问。

##### 10.2.1　顺序访问 329

最简单的访问方式是顺序访问，文件信息按顺序加以处理，比如编辑器。
 有时需要在中间加入或者访问中间的，需要将当前的文件指针前移或者后移。

##### 10.2.2　直接访问 329

一般的文件由**固定长度的逻辑记录**组成。以按任意顺序的快速读取和写入记录。数据库一般就是这样的模式。
 作为一个例子：对于一个航班订票系统，将特定航班的所有信息存储在由航班编号标识的块中。我们访问的时候直接用航班编号定位到数据块。而不是后移文件指针进行访问。

##### 10.2.3　其他访问方法 330

#### 10.3　目录与磁盘的结构 331

##### ==10.3.1　存储结构 331==

##### ==10.3.2　目录概述 332==

##### ==10.3.3　单级目录 332==

##### ==10.3.4　两级目录 333==

##### ==10.3.5　树形目录 334==

##### ==10.3.6　无环图目录 335==

##### ==10.3.7　通用图目录 337==

##### 10.4　文件系统安装 338

正如文件在使用前必须要打开一样，在使用文件系统之前必须先安装（mount）。 一个新的文件系统，要被操作系统识别，必须先进行安装。
 为了说明文件系统的安装，如下图，其中三角形表示所感兴趣的目录子树，左边a图是一个现有的文件系统，b图是一个未安装的位于/device/dsk上的文件系统。这时，只有现有文件系统上的文件可以访问。



![img](https:////upload-images.jianshu.io/upload_images/4793176-8b48a01d4ff72a4f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1176/format/webp)

文件系统挂载前.png

将/device/dsk上的卷安装到/users后的文件系统后，就可以进行访问了。



![img](https:////upload-images.jianshu.io/upload_images/4793176-134f3ba6ab8a6517.png?imageMogr2/auto-orient/strip|imageView2/2/w/862/format/webp)

文件系统挂载后.png

#### 10.5　文件共享 339

##### ==10.5.1　多用户 339==

当操作系统支持多个用户时，多个用户的文件之间应该支持共享，和文件保护。这就是文件的权限问题。
 一般用户都采用文件的**所有者，所属组的概念**。

##### ==10.5.2　远程文件系统 340==

随着网络技术的发展，远程计算机之间进行通信很常见，所以在不同的计算机之间进行文件共享称为可能。
 一般文件有三种方法：

+ 通过ftp等程序在机器之间手动传输文件。
+ 通过**分布式文件系统（DFS）**，远程目录在本地可以直接访问。
+ www 万维网。
     除此之外，云计算也慢慢变为一种方式。

##### ==10.5.3　一致性语义 342==

##### ==10.6　保护 343==

##### ==10.6.1　访问类型 343==

##### ==10.6.2　访问控制 343==

##### ==10.6.3　其他保护方式 345==

##### 

### 第11章　文件系统实现 349

#### 11.1　文件系统结构 349

文件系统本身通过由许多不同层组成，每层的设计利用更低层的功能，创造新的功能。



![img](https:////upload-images.jianshu.io/upload_images/4793176-efad887001d6fefd.png?imageMogr2/auto-orient/strip|imageView2/2/w/656/format/webp)

文件系统结构.png

**I/O控制层(I/O control)**：包括设备驱动和中断处理程序，以在主内存和磁盘系统之间进行信息交流。
 **基本文件系统(basic file system)**: 本层需要发送通用命令给设备驱动层，并且还要管理**内存缓冲区**和**保存各种文件系统，目录和数据块的缓存**。
 **文件组织模块（file-organization module）**: 知道文件及其逻辑块以及物理块。由于知道所用的文件系统的类型和文件位置，文件组织可以将逻辑块地址转换成物理地址以供基本文件系统传输。
 **逻辑文件系统（logical file system）**: 管理元数据信息。元数据包括文件系统的所有结构，而不包括实际数据，逻辑文件系统管理目录结构，以便根据给定的文件名称为文件组织模块提供所需的信息。

#### 11.2　文件系统实现 350

##### 11.2.1　概述 351

稳健性系统的实现需要采用多个磁盘和内存的结构。虽然这些结构因操作系统和文件系统而异，但是还是有些通用原则的。
 在磁盘上，文件系统包括以下信息：

+ **引导控制块**：指示如何启动存储在哪里的操作系统，如果磁盘不包含操作系统，那么这部分为空，一般为启动分区的第一个扇区。 Unix上称为**引导块**， Windows系统称为**分区引导扇区**
+ **卷控制块**： 包括卷（分区）的详细信息，如分区的数量和块的大小，空闲块的数量和指针，空闲的文件控制块(FCB)和FCB指针。Unix称为**超级块**，Windows称为**主控文件表**。
+ 文件系统的目录结构和用于组织文件。Unix中 包含 文件名和相关的inode的号码。
+ 每个文件的FCB 包括该文件的许多信息。它有一个唯一的标识号。以便与目录条目相关联。

在内存中的信息：

+ 内存的**安装表**： 包含每个安装卷的信息。
+ 内存中的目录结构的缓存含有最近访问的目录的信息。
+ 整个系统的打开文件表。
+ 每个进程的打开文件表。
+ 缓冲区保存的文件系统的块。

**文件的创建过程**：
 为了创建新的文件，应用程序调用逻辑文件系统。逻辑文件系统直到目录结构的格式。它会分配一个FCB。然后将相应的目录读到内存，然后使用新的文件名和FCB进行更新，将它写会磁盘。

**文件的读写过程**：

+ 打开文件的实际操作：
     一旦文件被创建，他就可以进行I/O操作。不过他首先应该被打开，系统调用open()将文件名传递到逻辑文件系统。系统首先搜索整个系统的打开文件表，以便确定这个文件是否被其他进程使用。
     如果是，则在进程的打开文件表中创建一个条目，并让他指向现有的系统的打开文件表，进程的打开文件表的计数加1。
     如果没有，则根据给定的文件名来搜索目录结构。（部分的目录结构通常缓存在内存中，以加快目录操作。）找到文件后，它的FCB 会被复制到内存的整个系统的开发文件表中。该表不但存储FCB，还跟踪打开文件进程的数量。接下来，在整个进程的打开文件表中，会创建一个条目，指向整个系统打开文件条目的一个指针，以及其他域。这些域可能包含文件系统的当前位置的指针和打开文件的访问模式。随后，进程的打开文件表的计数加1。
+ open() 文件最终返回一个文件指针。以后所有的操作都用这个指针，UNIX 中称为**文件描述符**， windows 称为**文件句柄**。
+ 进程关闭文件时，它的单个进程的条目会被删除，并且系统的打开文件表中这个文件的打开计数会被减1。当文件计数为0时，他把文件的元数据写入此案。然后系统的打开文件表中删除此文件的条目。

##### 11.2.2　分区与安装 353

磁盘布局可以有多种，具体取决于操作系统。一个磁盘可以分成多个区，或者一个卷跨越多个磁盘的多个分区。后者就是RAID的形式。

分区可以使原始的，什么文件系统都没有，什么都没有的时候就是**原始磁盘**。UNIX的交换分区就是一个原始磁盘， 有些数据库也使用原始磁盘，并且格式数据，以满足他们的要求。

**引导的信息**可以存储在各自分区中，而且可以有自己的格式，不一定和要加载的文件系统的保存结构一样。因此在这个阶段，还没有加载操作系统，更不会有文件系统。因此引导信息通常是一系列的块，可作为映像加载到内存。映像一般的执行从预先定义的位置执行。这个引导的信息里面应该包含应该以怎样的结构来解析接下来的文件系统信息，从而找到内核，并进行执行。
 许多系统可以有多个引导程序，因为可以安装双系统。

**根分区**，包括操作系统内核其他文件系统，在启动时安装，其他卷可以在引导时自动安装以后安装。

##### 11.2.3　虚拟文件系统 353

现代操作系统必须同时支持多中类型的文件系统，但是操作系统如何才能实现将多个类型的文件系统集成到目录结构中呢？用户如何在多个文件系统中无缝的迁移？

在大多数的操作系统中，都支持了在不同文件类型可通过相同的类型来实现。它也包括网络文件系统类型。
 数据结构和程序用于隔离基本系统调用的功能和实现细节。因此，文件系统的实现由三个主要层组成。

+ 第一次为文件系统接口层。基于系统调用open, read, write 和close。
+ 虚拟文件系统层（VFS）。他提供两个功能：
    1. 通过定义一个清晰的VFS接口，将文件系统的通用操作和实现分开。VFS提供多个通用接口，允许透明的访问本地安装的不同类型的文件系统。
    2. 它提供了一种机制，唯一表示网络上的文件。VFS 基于称为**虚拟节点**的文件表示结构， 虚拟节点包含一个数字指示符，唯一表示网络上的一个文件。UNIX中采用inode。内核为每个活动节点保存一个vnode结构。

![img](https:////upload-images.jianshu.io/upload_images/4793176-bf531dffa3df494e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1178/format/webp)

虚拟文件系统.png

VFS根据文件系统的类型调用特定的操作以便处理本地请求。通过NFS协议程序来处理远程请求。

#### 11.3　目录实现 355

##### 11.3.1　线性列表 355

线性列表：不合适。频繁的搜索称为问题。

##### 11.3.2　哈希表 355

哈希表：哈希表根据文件名获取一个值，并返回线性列表内的一个元素指针。减少了目录搜索的时间。当发生冲突的时候可以使用链式哈希结构。

#### 11.4　分配方法 356

创建文件时，如何在空闲的磁盘上为一个新文件分配空间？
 一般由是那种方法： 连续，链接和索引。

##### 11.4.1　连续分配 356

连续分配表示每个文件在磁盘上占有一组连续的块。磁盘地址为一组线性结构。
 问题：在给每个分配的时候，容易在中间产生碎片。 还有无法估算一个文件有多大， 估算的很小，没有办法扩展。

##### 11.4.2　链接分配 357

 解决了连续分配的所有问题，每个文件都是磁盘块的链表。文件的磁盘块可能分布在磁盘的任何地方，通过链表来进行连接。
 问题：只能顺序的访问文件，要找到第i个块，必须从头开始。还有每次磁盘块的最后4个字节需要保存下一个磁盘的地址。而且如果链断了，就全部完了。

##### 11.4.3　索引分配 359

索引分配通过将所有指针放在一起，称为索引块。
 每个文件都有自己的索引块。这是一个磁盘块地址的数组。索引块的第一个条目指向文件的第i块。目录包含索索引块的地址。

![img](https:////upload-images.jianshu.io/upload_images/4793176-56bce5c2f7435260.png?imageMogr2/auto-orient/strip|imageView2/2/w/890/format/webp)

索引分配.png

##### ==11.4.4　性能 360==

#### 11.5　空闲空间管理 361

由于磁盘空间有限，如果可能，需要将删除文件的空间重新用于新文件。为了跟踪空闲磁盘空间。系统需要维护一个**空闲空间列表**。空闲空间列表记录了所有空闲磁盘空间。

##### 11.5.1　位向量 361

 空闲空间列表按 位图（bitmap）来实现。每个块用一个位来表示。块是空闲的，位为1；如果块是分配的，位为0。

##### 11.5.2　链表 362

空闲空间将所有的空闲磁盘块用链表连接起来。

##### 11.5.3　组 362

 空闲列表方法的一个改进是，在**第一个空闲块中存储n个空闲块的地址**。
 这些块的前n-1个确实为空。
 最后一块包含另外n个空闲块的地址。

##### ==11.5.4　计数 362==

##### ==11.5.5　空间图 363==

#### ==11.6　效率与性能 363==

##### ==11.6.1　效率 363==

##### ==11.6.2　性能 364==

#### ==11.7　恢复 366==

##### ==11.7.1　一致性检查 366==

##### ==11.7.2　基于日志的文件系统 366==

##### ==11.7.3　其他解决方法 367==

##### ==11.7.4　备份和恢复 368==

#### ==11.8　NFS 368==

##### ==11.8.1　概述 369==

##### ==11.8.2　安装协议 370==

##### ==11.8.3　NFS协议 370==

##### ==11.8.4　路径名称转换 371==

##### ==11.8.5　远程操作 372==

#### ==11.9　例子：WAFL文件系统 372==

#### 11.10文件系统答疑

1.文件系统怎样映射
 文件系统由多个模块组成，每一层都是对下一层设备的概念的屏蔽



![img](https:////upload-images.jianshu.io/upload_images/4793176-efad887001d6fefd.png?imageMogr2/auto-orient/strip|imageView2/2/w/656/format/webp)

文件系统结构.png

**I/O控制层**：是为了屏蔽各个厂商的设备差异，所以包含了设备驱动程序和一些终端处理程序，它们之间在使用的时候是通过I/O总线来进行操作的。
 **基本文件系统层**： 主要是内核系统使用，它需要维护内核缓冲区，目录和数据块加载到内存后的数据结构。 以及发送命令给I/O 控制层。
 **文件组织模块**：对于一个存储在设备上的文件，文件组织模块直到他们物理块是如何实现的，比如是链表还是索引，然后根据分配和存储规则对文件进行 逻辑块到物理块的映射。
 **逻辑文件系统**: 维护文件控制块，包括文件属性和权限信息，和目录结构。
 这些层共同组成文件系统。

1. 文件I/O 有哪些，文件的的I/O的具体过程。
     一般的文件有 open,  read , write 和seek 操作。
     open 打开文件时，文件系统首先搜索目录结构，找到对应的文件，然后把文件目录加载到内存， 然后会在内核文件表 加一个条目，表示打开了这个文件，并且进程的地址空间中的打开文件表中也加上一个条目，表示进程打开了这个文件。当文件被多个进程打开时，在进程的地址空间中加上一个打开的文件描述符，而内核的打开进程表中，只需要进行引用计数即可。
     read 读取文件数据，操作系统内核进行对I/O控制器执行read 命令请求。 控制器通过I/O端口的四个寄存器来实现读写。  控制器检查 状态寄存器的位，如果忙，就进行等待，如果不忙，就把状态寄存器置为忙，并把需要读取的数据读到数据输出寄存器上，然后一个字或者一个字节的读到内存中。后来有了DMA，就可以等待都写入到缓冲区，等待数据准备好了，就把DMA的数据 一块发给内存。
     write 和read 的操作类型。只不过相反。
     seek 表示移动当前位置，这个需要看文件的组织结构是什么样的，一般以扇区（一般为512字节）或者块进行分配，它会有链式结构和索引结构的分配方式，所以进行索引的时候，需要根据分配的方法进行索引。

1. RAID 矩阵是怎么回事？
     RAID 矩阵指的是 将将多个磁盘进行合并，从而组成可以提高更好的性能，冗余的数据存储。
     一般分为不同的级别。
     RAID0 ：需要两块盘，数据分布在不同的盘上，从而可以在读取或者写入的时候，并行进行，从而得到更好的性能。但是他不安全。一个磁盘损坏，导致这个文件都坏了
     RAID1：需要两块盘， 数据分布在两个盘上，两个进行互备。从而保证数据的安全。
     RAID5：RAID 5至少需要三个硬盘，RAID5 不是对存储的数据备份，而是把数据和相对应的奇偶校验信息存储到组成RAID5 的各个磁盘上，并且就检验码信息和相对应的数据分别存储在不同的磁盘上。当RAID5 的任何一个磁盘数据反生损坏，可以利用剩下的数据和相应的奇偶校验码信息去恢复被损坏的数据。
     RAID6 至少需要4个硬盘，与RAID5 相似，RAID6也是不对 存储数据进行备份，而是把数据和相对应的奇偶校验信息存储在RAID6的各个磁盘， 与RAID5不同的是，RAID6有两个校验盘，即使同时又两块硬盘出问题，它也可以利用剩下的数据和相应的奇偶信息恢复损坏的磁盘。
     RAID-1+0 :
     RAID 10 仅用于硬盘的情况下，把4个硬盘分为两组，每组的两个互为RAID1。然后 在做RAID1。
     RAID-0+1 ：多块硬盘先实现RAID0,然后再组成RAID1，相比RAID10,表面看没什么区别，但风险更大，使用一般不采用。

具体参照[《操作系统概念精要》之磁盘篇（二）-RAID结构](https://www.jianshu.com/p/c661690ba417)

1. unix 的文件系统是怎么构成的？

了解文件系统构成之前，先了解下Unix 下的三个概念。
 **superblock**：记录此 filesystem 的整体信息，包括inode/block的总量、使用量、剩余量， 以及文件系统的格式与相关信息等；
 **inode**：记录文件的属性，一个文件占用一个inode，同时记录此文件的数据所在的 block 号码；
 **block**：实际记录文件的内容，若文件太大时，会占用多个 block 。
 每个 inode 与 block 都有编号，而每个文件都会占用一个 inode ，inode 内则有文件数据放置的 block 号码。 因此，我们可以知道的是，如果能够找到文件的 inode 的话，那么自然就会知道这个文件所放置数据的 block 号码， 当然也就能够读出该文件的实际数据了。
 这样一来我们的磁盘就能够在短时间内读取出全部的数据， 读写的效能比较好啦。
 我们将 inode 与 block 区块用图解来说明一下，如下图所示，文件系统先格式化出 inode 与 block 的区块，假设某一个文件的属性与权限数据是放置到 inode 4 号(下图较小方格内)，而这个 inode 记录了文件数据的实际放置点为 2, 7, 13, 15 这四个 block 号码，此时我们的操作系统就能够据此来排列磁盘的阅读顺序，可以一口气将四个 block 内容读出来！ 那么数据的读取就如同下图中的箭头所指定的模样了。

![img](https:////upload-images.jianshu.io/upload_images/4793176-7d4d3afd9f63bb61.png?imageMogr2/auto-orient/strip|imageView2/2/w/772/format/webp)

inode-block结构.png



1. 如何理解I/O模式和I/O复用
     在 Linux 的缓存 I/O 机制中，操作系统会将 I/O 的数据缓存在文件系统的页缓存（ page cache ）中，也就是说，数据会先被拷贝到操作系统内核的缓冲区中，然后才会从操作系统内核的缓冲区拷贝到应用程序的地址空间。
     所以说，当一个read操作发生时，它会经历两个阶段：
2. 等待数据准备 (Waiting for the data to be ready)
3. 将数据从内核拷贝到进程中 (Copying the data from the kernel to the process)

正式因为这两个阶段，linux系统产生了下面五种网络模式的方案。

+ 阻塞 I/O（blocking IO）
+ 非阻塞 I/O（nonblocking IO）
+ I/O 多路复用（ IO multiplexing）
+ 信号驱动 I/O（ signal driven IO）
+ 异步 I/O（asynchronous IO）

**阻塞I/O**
 在linux中，默认情况下所有的socket都是blocking，一个典型的读操作流程大概是这样：

![img](https:////upload-images.jianshu.io/upload_images/4793176-6b1d2cb8cf6038f3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1156/format/webp)

阻塞I:O.png



当用户进程调用了recvfrom这个系统调用，kernel就开始了IO的第一个阶段：准备数据（对于网络IO来说，很多时候数据在一开始还没有到达。比如，还没有收到一个完整的UDP包。这个时候kernel就要等待足够的数据到来）。这个过程需要等待，也就是说数据被拷贝到操作系统内核的缓冲区中是需要一个过程的。而在用户进程这边，整个进程会被阻塞（当然，是进程自己选择的阻塞）。当kernel一直等到数据准备好了，它就会将数据从kernel中拷贝到用户内存，然后kernel返回结果，用户进程才解除block的状态，重新运行起来。



```undefined
所以，blocking IO的特点就是在IO执行的两个阶段都被block了。
```

** 非阻塞 I/O（nonblocking IO）**

linux下，可以通过设置socket使其变为non-blocking。当对一个non-blocking socket执行读操作时，流程是这个样子：

![img](https:////upload-images.jianshu.io/upload_images/4793176-badf27ab68183c95.png?imageMogr2/auto-orient/strip|imageView2/2/w/1060/format/webp)

非阻塞I:O模式.png

当用户进程发出read操作时，如果kernel中的数据还没有准备好，那么它并不会block用户进程，而是立刻返回一个error。从用户进程角度讲 ，它发起一个read操作后，并不需要等待，而是马上就得到了一个结果。用户进程判断结果是一个error时，它就知道数据还没有准备好，于是它可以再次发送read操作。一旦kernel中的数据准备好了，并且又再次收到了用户进程的system call，那么它马上就将数据拷贝到了用户内存，然后返回。



```undefined
所以，nonblocking IO的特点是用户进程需要不断的主动询问kernel数据好了没有。
```

**I/O 复用**
 IO multiplexing就是我们说的select，poll，epoll，有些地方也称这种IO方式为event driven IO。select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理就是select，poll，epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。

![img](https:////upload-images.jianshu.io/upload_images/4793176-ce12a228a89dd252.png?imageMogr2/auto-orient/strip|imageView2/2/w/1154/format/webp)

I:O 复用.png



当用户进程调用了select，那么整个进程会被block，而同时，kernel会“监视”所有select负责的socket，当任何一个socket中的数据准备好了，select就会返回。这个时候用户进程再调用read操作，将数据从kernel拷贝到用户进程。

这个图和blocking IO的图其实并没有太大的不同，事实上，还更差一些。因为这里需要使用两个system call (select 和 recvfrom)，而blocking IO只调用了一个system call (recvfrom)。但是，用select的优势在于它可以同时处理多个connection。

## 第12章　I/O系统 379

#### 12.1　概述 379

计算机设备的控制是操作系统的设计人员的主要关注之一。因为I/O设备的功能与速度差异很大。所以需要采用不同的方法来控制设备，这些方法构成了内核的I/O子系统。
 I/O设备技术呈现两个冲突趋势。一方面。软件和硬件的接口标准化日益增长。这个趋势有助于将改进和升级设备，从而可以集成到计算机中。另一方面，I/O设备的种类也日益增多。为了封装各种设备的细节与特点，操作系统内核采用设备驱动程序。
 **设备驱动程序**为I/O子系统提供了统一的设备访问接口。

![img](https:////upload-images.jianshu.io/upload_images/4793176-d36ad371f393afbd.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

内核I:O子系统结构.png

#### 12.2　I/O硬件 379

设备与计算机系统的通信，可以通过电缆或者空气来进行发送信息。设备与计算机的通信通过一个连接点或者端口，比如串行端口。连接这些端口电缆就是总线。

![img](https:////upload-images.jianshu.io/upload_images/4793176-a7ec80f7f909ef78.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

一个典型的PC 总线的结构.png

总线在计算机体系中应用广泛。上图是一个典型的PC总线结构，图中，PCI 总线将内存和各个设备进行连接。 其他的设备都需要连接到PCI 总线上。 扩展总线连接相对较慢的设备。如键盘和串口USB端口。4个磁盘通过小型计算机系统接口（SCSI）总线连接到SCSI控制器。

控制器是可以操作端口和总线或者设备的一组电子器件。串行端口控制器是计算机内的单个芯片，用于控制串口线路信号。 SCSI控制器一般是比较复杂，遵循SCSI协议。一般为单独的电路板。他包括，处理器，微代码和一些专业内存。

处理器如何对控制器发出命令和数据以便完成I/O传输？
 控制器是具有多个寄存器的，用于数据和控制信号。处理器通过读写这些寄存器的位模式与控制器通信。这种通信的一种方式是，通过使用特殊的I/O指令 针对I/O端口 地址传输一个字节或字。
 I/O指令出发总线线路，选择适当的设备，并将位移入或者移出设备寄存器。
 或者设备控制器支持内存映射I/O。在这种情况下，设备控制寄存器被映射到处理器的地址空间，处理器执行I/O 请求是通过标准数据传输指令读写映射到物理内存的设备控制器。

I/O端口通常由四个寄存器组成， 即状态，控制，数据输入，数据输出寄存器。
 数据输入寄存器： 被主机读出以便获取数据。
 数据输出寄存器：被主机写入以发送数据。
 状态寄存器：包含一些主机可以读取的位，例如当前命令是否完成，数据输入寄存器是否有数据可读，是否出现设备故障等。
 控制寄存器： 主机写入，以便启动命令或者改变设备模式。

##### 12.2.1　轮询 381

主机与控制器之间交互的完整协议可以很复杂，但基本的握手概念是比较简单的。握手概念可以通过例子来解释。
 假设采用2个位协调控制器和主机之间的生产者和消费者关系，控制器通过状态寄存器的忙来显示状态。控制器工作忙时就置忙， 而可以接收下个命令时就清忙位。主机通过命令 寄存器的    命令就绪位来表示意愿。当主机有命令需要控制器执行时，命令就绪位被置为1。

当主机需要通过端口来输出数据时，主机与控制器之间的握手的协调如下：

1. 主机重复读取忙位，直到该位清零。
2. 主机设置命令寄存的写位，并写出一个字节到数据输出寄存器。
3. 主机设置命令就绪位。
4. 当主控器注意到命令就绪位被设置，则设置忙位。
5. 控制器读取命令寄存器，并看到写命令。它从数据输出寄存器中读取一个字节，并向设备执行I/O操作。
6. 控制器清除命令就绪位，清除状态寄存器的故障位表示设备I/O成功，清除忙位表示完成。
     对于每个字节重复这个循环。在步骤1 中，主机处于忙等待，或者轮询。

##### 12.2.2　中断 382

基本的中断机制的工作原理是：CPU 硬件有一条线，称为 中断请求线（IRL）。CPU 在执行完每条指令的后，都会检查是否有中断请求，当CPU检测到控制器已在IRL上发出了一个信号时，CPU执行状态保留并且跳到内存固定位置的中断处理程序中。 中断处理程序确定中断原因，并执行必要处理，执行状态恢复。并且返回中断指令以便CPU 回到中断前的位置继续执行。

设备控制器通过中断请求线发送信号引起中断，CPU捕获中断，并分派到中断处理程序，中断处理程序通过处理设备来清除中断。

##### 12.2.3　直接内存访问（DMA） 385

对于执行大量传输的设备，例如磁盘驱动器，如果通过昂贵的CPU来按字节的方式发送数据到控制寄存器，则会增加CPU的负担。所以为了避免这种情况，将这一部分任务交给一个专用的处理器（DMA控制器）。
 在启动DMA 传输时，主机将DMA 命令块写到内存。该块包含传输来源地址，目标地址和传输的字节数。 CPU将这个命令块写入DMA控制器后，继续工作。 DMA 再继续直接操作内存总线，将地址放到总线，在没有主CPU的帮助下，进行传输。

DMA控制器与设备控制器之间的握手，通过 一组对称的 DMA 请求和DMA确认来完成的。当有数据需要传输时，设备控制器发送信号到 DMA 请求线路。这个信号使得DMA控制器占用内存总线，发送所需地址到内存地址总线，并发送信号到DMA确认线路。当设备控制器收到DMA确认信号时，他就传输数据到内存，并且清除DMA 请求信号。
 当DMA 控制器占用内存总线时，CPU被暂时阻止访问总线，但是仍然可以返回主缓存。虽然这种周期窃取 会减慢CPU的计算，但是将数据传输给工作交给DMA控制器通常能够改进总的系统性能。

![img](https:////upload-images.jianshu.io/upload_images/4793176-1660b35b9e9eba20.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

DMA数据传输.png

##### ==12.2.4　I/O硬件小结 386==

#### 12.3　应用程序I/O接口 386

![img](https:////upload-images.jianshu.io/upload_images/4793176-e7758f0668e5e065.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

内核I:O子系统结构.png

在操作系统对I/O设备封装的时候，因为各个设备的不同，所以会形成上面的那个I/O结构。接下面主要说明一些不同的设备的处理方法。

##### 12.3.1　块与字符设备 388

**块设备接口**为磁盘驱动器和其他基于块设备的访问。它提供了read(), write(), seek()等接口屏蔽了 底层设备的差异。
 键盘是**字符流设备的**。通常使用get()  和put() 接口来进行封装。其他设备还有键盘，鼠标，modem。

##### 12.3.2　网络设备 389

因为网络I/O的性能和寻址的特点不同于磁盘I/O，大多数操作系统提供的网络I/O接口不同于 read()-write()-seek()操作。 许多系统都使用socket接口。
 为了支持实现服务器，套接字还支持了select()函数。调用select可以得知，那个套接字已经有消息接收和处理。

##### 12.3.3　时钟与定时器 389

大多数计算机都有硬件时钟来定时器。以便实现三种功能

1. 获取当前时间
2. 获取经过时间
3. 设置定时器，以便在T时出发操作X
     测量经过的时间和触发操作的硬件称为**可编程间隔定时器**。他可以设置一段时间，然后触发中断；调度程序采用这种机制产生中断，以便抢占时间片用完的进程。
     I/O系统使用这种机制，定期刷新脏的缓存到磁盘，网络子系统中，定时取消由于网络拥塞或者故障而产生的太慢的操作。

##### 12.3.4　非阻塞与异步I/O 390

系统调用接口的另一方面设计选择阻塞I/O 和非阻塞I/O。当应用程序执行阻塞系统调用时，应用程序的执行就被挂起。应用程序会从操作系统的运行队列移到等待队列。等待系统调用完成后，再回到运行队列。
 有些用户级进程需要使用非阻塞I/O。比如，一个用户接口，用来接收键盘和鼠标的输入，同时处理数据并显示到屏幕。
 针对非阻塞系统，系统一般提供了异步系统调用。异步调用立即返回，无需等待I/O完成。应用程序继续执行代码，等将来I/O 完成了，通过设置应用程序地址空间的某个变量，或通过触发信号或软件中断，或者执行回调函数，来通知应用程序。

##### ==12.3.5　向量I/O 391==

#### 12.4　内核I/O子系统 391

内核提供与I/O相关的许多服务。包括调度，缓冲，假脱机，设备预留及错误处理。

##### 12.4.1　I/O调度 391

调度一组I/O请求意味着，确定好顺序，来执行它们。应用程序执行系统调用的顺序很少是最佳的。

操作系统开发人员通过为每个设备维护一个请求等待队列，来实现队列。当应用程序发出阻塞I/O的系统调用时，该请求被添加到相应的设备的队列。I/O调度程序重新安排队列顺序。以便提高总的效率和应用程序平均的响应时间。之前就有提到过磁盘的调度算法。

当内核支持异步I/O时，它必须能够同时跟踪许多I/O请求。为此，操作系统可能会将等待附加到设备状态表中。内核管理此表，其中每个条目对应每个I/O 设备。每个表条目表明设备的类型， 地址和状态。如果设备忙于一个请求，则请求的类型和其他参数都被保存在该设备的表条目中。

![img](https:////upload-images.jianshu.io/upload_images/4793176-1be535bac5e10491.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

设备状态表.png

##### 12.4.2　缓冲 392

调度I/O操作是I/O子系统提高计算机效率的一种方法。另外一种方法是通过缓冲，缓存和假脱机，使用内存或磁盘的存储空间。

**缓冲区**是一块内存区域，用于保存在两个设备之间或者设备和应用程序之间传输的数据。采用缓冲有三个理由：

1. 处理数据流的生产者和消费者之间的速度不匹配。例子：通过调制解调器接收一个文件，并保存在硬盘。调制解调器的速度比硬盘慢1000倍。这样创建一个缓冲区在内存中，以便累积从调制解调器出接受的字节。当整个缓冲区填满时，就可以通过一次磁盘操作写入磁盘。
2. 调节传输代销不一致的设备。这种不一致在计算机网络中特别常见，缓冲区大量用于消息的分段和重组。在发送端，一个大的消息分成若干个小的网络分组。这些网络分组通过网络传输，而接收端将他们放在重组缓冲区内，以便完成的源数据进行重组映射。
3. 支持应用程序I/O的复制语义。假设应用程序有一个数据缓冲区，它希望写到磁盘。它调用系统调用write，提供缓冲区的指针和表示所写字节数量的整数。在系统调用返回后，如果应用程序更改缓冲区的内容，那么会发生什么？采用复制语义，写到磁盘的的数据版本保证是系统调用时的版本，而与应用程序缓冲区的任何后续操作无关。
     系统调用write 返回到应用程序之前，复制应用程序缓冲区到系统内核缓冲区。磁盘写入通过内核缓冲区来执行。以便应用程序缓冲区的后续更改没有影响。

##### 12.4.3　缓存 393

缓冲和缓存的区别是： 缓冲可以保存数据项的唯一现有版本。而缓存只是提供了一个位于其他地方的数据项的更快存储版本。

缓存和缓冲的功能不同，但是有时一个内存区域可以用于两个目的。
 如为了保留复制语义和有效地调度磁盘I/O。 操作系统采用内存中缓冲区来保存磁盘数据。这些缓冲区也用作缓存，以便提高文件的I/O效率。这些文件可被多个程序共享，或者更快速的写入和重读。
 当内核收到文件I/O请求时，内核首先访问缓冲区缓存。以便查看文件区域是否已经在内存中可用。

##### 12.4.4　假脱机与设备预留 394

假脱机是保存设备输出的缓冲区，这些设备，如打印机，不能接受交叉的数据流。虽然打印机只能一次打印一个任务，但是多个应用程序可能希望并发打印输出，而不能让他们的输出混合在一起。操作系统拦截所有的打印数据。来解决这个问题。
 应用程序的输出先是假脱机到一个单独的磁盘文件。当应用程序完成打印后，假脱机系统排序相应的假脱机文件。以便输出到打印机。

##### ==12.4.5　错误处理 394==

##### ==12.4.6　I/O保护 394==

##### 12.4.7　内核数据结构 395

内核需要保存I/O组件的使用的状态信息。它通过各种内核数据结构来完成。内核使用许多类似的结构来跟踪网络连接，字符设备和其他的I/O操作。
 Unix提供了各种实体的文件系统访问，如用户文件，原始设备和进程的地址空间。虽然这些实体都支持read 操作， 但是语义不同。当读取用户文件时，内核首先需要检查缓冲区缓存，然后决定是否执行磁盘I/O。 当读取原始磁盘时，内核需要确保，请求大小是 磁盘扇区大小的倍数而且与扇区边界对齐。 当读取进程映像时， 内核秩序从内存读取数据。



![img](https:////upload-images.jianshu.io/upload_images/4793176-a16ced0ff0ec1e3d.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

unix I:O内核结构.png

##### ==12.4.8　电源管理 396==

##### ==12.4.9　内核I/O子系统小结 397==

#### ==12.5　I/O请求转成硬件操作 397==

操作系统如何将应用程序请求连到网络线路或者特定的磁盘扇区。
 从磁盘文件读文件。应用程序通过文件名称引用数据。对于磁盘，文件系统通过文件目录对文件名进行映射，从而得到文件的空间分配。
 但是如何建立文件名称到磁盘控制器的连接?
 MS-DOS  文件名称的第一部分， 在冒号之前表示特定硬件设备的字符串。 C： 是主硬盘的每个文件名称的第一部分。 C: 通过设备表映射到特定的端口地址。由于冒号分隔符，设备名称空间不同于文件系统的名称空间。
 Unix 通过常规文件系统的名称空间来表示设备名称。与具有冒号分隔符的MS-DOS系统不用， Unix没有 路径 没有明确的设备部分。为了解析路径，Unix 检查安装表内的名称，以查找最长的匹配前缀；安装表的相应条目给出了设备名称。

#### ==12.6　流 399==

#### ==12.7　性能 400==

#### ==12.8　小结 402==

