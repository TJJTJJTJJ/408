## 操作系统

### 1.3.2 中断与异常

中断机制 -- 操作系统介入 -- CPU由用户态转化为核心态 -- 多道程序并发

1. 用户态-->核心态: 只能通过中断
2. 核心态-->用户态: 执行特权指令, 修改状态位

中断分类: 内中断(来自CPU内部), 外中断(来自CPU外部)

## 2.1 进程和线程

进程--多道程序技术--进程实体的PCB
PCB是进程存在的唯一标志
进程是动态的=PCB(操作系统的数据)+程序段+数据段
进程实体是静态的
进程的组织: 链接方式(执行指针, 就绪队列指针,阻塞队列指针), 索引方式
进程是资源分配, 接受调度的独立单位
进程的特征: 动态性, 并发性, 独立性, 异步性, 结构性

### 进程的状态与转换

进程的三种状态: 运行态(只有一个), 就绪态, 阻塞态, 创建态, 终止态

## 2.1 进程与线程

### 进程状态的转换

创建态--就绪态（其他）--运行态（处理机，其他）--阻塞态（）

运行态--阻塞态：主动行为，系统调度
阻塞态--就绪态：被动行为
就绪态--运行态：进程被调度
运行态--就绪态：时间片或者更高级的进程

### 进程控制

进程控制：原语
**原语**：关中断指令和开中断指令
修改 PCB 的内容，修改队列，分配/回收资源
进程创建：创建原语，用户登录，作业调度，提供服务，应用请求
进程终止：终止原语，正常结束，异常结束，外界干预
进程的阻塞和唤醒：阻塞原语，唤醒原语，成对出现
进程的切换：切断原语

## 2.2 处理机调度

高响应比优先算法:
非抢占式
等待时间/服务时间

上面三个方法只适合早期的批处理系统

下面的方法更适合交互式系统

时间片轮转调度算法(RR)
分时操作系统, 时钟中断, 响应快

优先级调度算法
实时操作系统, 抢占式/非抢占式

多级反馈队列调度算法

### 进程通信

**共享存储，消息传递，管道通信**。

共享存储：
共享空间（互斥）：基于数据结构，基于存储区

管道通信：
管道：共享文件，即内存中固定大小的缓冲区
半双工通信
互斥
字符流，管道只能写满或者读空
数据一旦读出，就会被抛弃

消息传递：
以格式化的消息为单位
通过 发送消息/接受消息 两个原语实现
例如报文
消息缓冲队列
直接通信方式，间接通信方式

### 线程、多线程模型

线程：进程同时处理很多事情
线程是程序执行流的最小单位
进程是除CPU之外的系统资源的分配单位
进程是资源分配的基本单位
线程是处理机调度的基本单位

线程的实现方式：
用户级线程(线程库)：对用户不透明，对内核透明
内核级线程(核心态)：对内核不透明
内核级线程是处理机分配的单位

多线程模型：
多对一模型，一对一模型，多对多模型

## 2.2 处理机调度

### 2.2.1 处理机调度的基础知识

调度：
进程个数>处理机个数

调度的三个层次：高级调度，中级调度，低级调度
高级调度：作业调度，面向作业，外存与内存之间的调度，建立PCB
中级调度：内存调度，面向进程，虚拟存储技术，挂起态，决定哪个处于挂起态的进程重新调入内存，挂起态-->阻塞态
低级调度：进程调度，就绪态-->运行态

挂起：就绪挂起，阻塞挂起

### 2.2.2 进程调度的时机切换与过程调度方式

进程调度的时机：
进程主动放弃或者被动放弃
不能的情况：处理中断的情况，进程在操作系统内核程序临界区，原子操作
临界区：访问临界资源的代码

进程调度的方式：
非剥夺调度方式，剥夺调度方式

进程的切换与过程：
保存原进程的数据，恢复新进程的数据

### 2.2.3 调度算法的评价指标

CPU利用率，系统吞吐量，周转时间，等待时间，响应时间

### 2.2.4 调度算法

先来先到，短作业优先，高响应优先，
作业调度，进程调度
抢占式，非抢占式
饥饿

先来先到(FCFS)：
作业调度(后备队列)，进程调度(就绪队列)
非抢占式
等待时间更久的优先
对长作业有利，对短作业不利
不会导致饥饿

短作业优先(SJF):
运行时间为标准
作业调度，进程调度
非抢占式和抢占式(最短剩余时间优先)
平均等待时间，平均周转时间很短

优先级调度算法
操作系统更偏好 I/O 型进程 （非计算型进程）
饥饿

多级反馈队列调度算法
用于进程调度
抢占式

### 2.3.6 管程

信号量机制存在问题 (PV 操作)
进程对共享资源的互斥同步访问
生产者消费者模型

管程：
定义共享数据结构
定义访问共享数据的的入口
只有特定入口才可以访问共享数据
管程每次只能开放一个入口，并且只能让一个进程或线程进入(互斥性由编译器负责实现)
可以设置条件变量及等待/唤醒操作解决同步问题

## 2.3 进程同步 进程互斥

### 2.3.1 介绍

进程的异步性：各并发执行的进程以各自独立的、不可预知的速度向前推进
进程的同步性：协调进程之间的工作次序 （有时候进程2的某个代码需要进程1的某个代码执行后才能执行）
进程互斥：共享资源
临界/互斥资源：一个时间段内只允许一个进程访问的资源
互斥访问过程：进入区-->临界区-->退出区-->剩余区
临界资源互斥访问原则：空闲让进，忙则等待，有限等待，让权等待

### 2.3.2 进程互斥的软件实现方法

单标志法, 双标志先检查, 双标志后检查, Peterson算法

单标志法
每个进程进入临界区的权限只能被另一个进程赋予

双标志先检查法
用布尔型数组中的各个元素用来标记各进程想进入临界区的意愿
检查和上锁无法一气呵成

双标志后检查法

Peterson算法
双标志后检查法+标志位的孔融让梨

### 2.3.2 进程互斥的硬件实现方法

中断屏蔽方法，TestAndSet 指令，Swap 指令

中断屏蔽方法：
开/关中断
关中断表示当前进程不可被中断
不适用多处理机，需要内核态

TestAndSet 指令
TSL指令
适用于多处理机，不满足让权等待

Swap 指令
适用于多处理机，不满足让权等待

### 2.3.3 信号量机制

整型信号量， 记录型信号量

目的：让权等待

用户进程通过操作系统提供的一对原语对信号量进行操作
信号量：是个变量，表示系统中某种资源的数量，分为整型信号量和记录型信号量
一对原语：wait(S) signal(S)，可以写成 P(S), V(S)

整型信号量
操作：初始化， P操作， V操作
wait 进入区， signal 退出区
有点类似先检查后上锁
会发生忙等

记录型信号量
wait， signal 系统资源的申请与释放
记录资源数和等待队列
```python
int value;
struct process *L;
```
可以实现让权等待
wait：block使进程从运行态变成阻塞态，先value--
signal：wakeup使进程从阻塞态变成就绪态，先value++

### 2.3.4 用信号量机制实现进程互斥、进程同步、前驱关系

信号量机制实现进程互斥：
互斥信号量 mutex 
初始化为1
临界区之前用P，临界区之后用V
PV操作，P 申请 V 释放

信号量机制实现进程同步：
初始化为0
进程同步：保证一前一后的执行顺序
前操作之后用V进行释放，后操作之前用P进行阻塞

信号量机制实现前驱关系
为每一对前驱关系设置一个同步变量
前操作之后用V，后操作之前用P

对一类系统资源的申请与释放
初始值为资源的数量

### 2.3.6 生产者消费者问题

生产者、消费者共享一个初始为空、大小为n的缓冲区
只有缓冲区没满时，生产者才能把产品放入缓冲区，否则必须等待：进程同步
只有缓冲区不空时，消费者才能从中取出产品，否则必须等待：进程同步
缓冲区是临界资源，各进程必须互斥访问：进程互斥
PV操作

生产者每次消耗（P）一个空闲缓冲区，并生成（V）一个产品
消费者每次要消耗（P）一个产品，并释放（V）一个空闲缓冲区


semaphore mutex = 1 // 互斥信号量，实现对缓冲区的互斥访问
semaphore empty = n // 同步信号量，表示空闲缓冲区的数量，用于实现生产者等待消费者
semaphore full = 0  // 同步信号量，表示缓冲区中产品的数量，用于实现消费者等待生产者

```C++
producer(){
    while(1){
        生成一个产品;
        P(empty); # 消耗一个空闲缓冲区
        P(mutex)
        把产品放入缓冲区;
        V(mutex)
        V(full)  # 增加产品
    }
}

consumer(){
    while(1){
        P(full) # 消耗一个产品
        P(mutex)
        从缓冲区取出一个产品;
        V(mutex)
        V(empty) # 增加一个空闲缓冲区
        使用产品
    }
}
```
实现互斥的P操作一定要在实现同步的P操作之后
V操作可以互相交换

