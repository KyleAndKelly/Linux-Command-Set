# 后端开发必须掌握的Linux命令汇总(更新中)

## 性能检测相关命令
![image](https://img-blog.csdnimg.cn/2020022521482651.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
### mpstat
#### 功能:

```
显示CPU的状态信息
这些信息存放在/proc/stat文件中。

在多CPUs系统里，

其不但能查看所有CPU的平均状况信息，

而且能够查看特定CPU的信息。 
```

#### 输入语法:

```
mpstat(选项)(参数)

选项

-A ： 此选项等效于# mpstat -I ALL -u -P ALL

-I {SUM | CPU | ALL} ： 报告中断统计信息。 使用SUM关键字，mpstat命令报告每个处理器的中断总数。使用CPU关键字，显示CPU或CPU每秒接收的每个中断的数量。ALL关键字等效于指定上面的所有关键字，因此显示所有中断统计信息。

-P {cpu [，...] | ON | ALL} ： 指示要报告统计信息的处理器编号。cpu是处理器号。注意，处理器0是第一个处理器。 ON关键字表示将为每个在线处理器报告统计信息，而ALL关键字指示要为所有处理器报告统计信息。

参数

间隔时间：每次报告的间隔时间（秒）； 
次数：显示报告的次数。
```

#### 输出信息:

```
user 在internal时间段里，用户态的CPU时间（%），不包含 nice值为负 进程 (usr/total)*100  
nice 在internal时间段里，nice值为负进程的CPU时间（%）   (nice/total)*100  
system 在internal时间段里，内核态的CPU时间（%）   (system/total)*100
iowait 在internal时间段里，硬盘IO等待时间（%） (iowait/total)*100
irq 在internal时间段里，硬中断时间（%）      (irq/total)*100
soft 在internal时间段里，软中断时间（%）    (softirq/total)*100
idle 在internal时间段里，CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间（%）(idle/total)*100


```

#### 实例:
##### mpstat
显示开机到现在以来cpu的平均状态信息


```
Linux 4.15.0-88-generic (kyle) 	2020年02月25日 	_x86_64_	(4 CPU)

20时12分09秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
20时12分09秒  all   27.16    0.55    9.85    0.34    0.00    0.43    0.00    0.00    0.00   61.66

```

##### mpstat -P ALL 2 3
每隔2秒显示一次 所有cpu的状态信息
一共产生3个间隔的信息
最后给出这3次间隔的平均信息


```
kylechen@kyle:~$ mpstat -P ALL 2 3
Linux 4.15.0-88-generic (kyle) 	2020年02月25日 	_x86_64_	(4 CPU)

20时12分43秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
20时12分45秒  all    9.96    0.00    3.57    0.00    0.00    0.12    0.00    0.00    0.00   86.35
20时12分45秒    0    8.96    0.00    2.99    0.00    0.00    0.00    0.00    0.00    0.00   88.06
20时12分45秒    1    8.87    0.00    3.45    0.49    0.00    0.49    0.00    0.00    0.00   86.70
20时12分45秒    2    9.76    0.00    2.44    0.00    0.00    0.00    0.00    0.00    0.00   87.80
20时12分45秒    3   12.32    0.00    4.43    0.00    0.00    0.00    0.00    0.00    0.00   83.25

20时12分45秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
20时12分47秒  all   13.43    0.00    4.68    0.00    0.00    0.12    0.00    0.00    0.00   81.77
20时12分47秒    0   16.59    0.00    5.69    0.00    0.00    0.47    0.00    0.00    0.00   77.25
20时12分47秒    1   12.08    0.00    3.86    0.00    0.00    0.00    0.00    0.00    0.00   84.06
20时12分47秒    2   12.80    0.00    4.74    0.00    0.00    0.00    0.00    0.00    0.00   82.46
20时12分47秒    3   12.44    0.00    5.26    0.00    0.00    0.00    0.00    0.00    0.00   82.30

20时12分47秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
20时12分49秒  all    8.44    0.00    2.02    0.00    0.00    0.13    0.00    0.00    0.00   89.42
20时12分49秒    0    7.69    0.00    1.54    0.00    0.00    0.51    0.00    0.00    0.00   90.26
20时12分49秒    1    6.47    0.00    2.99    0.00    0.00    0.50    0.00    0.00    0.00   90.05
20时12分49秒    2   10.66    0.00    1.02    0.00    0.00    0.00    0.00    0.00    0.00   88.32
20时12分49秒    3    8.46    0.00    2.49    0.00    0.00    0.00    0.00    0.00    0.00   89.05

平均时间:  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
平均时间:  all   10.65    0.00    3.44    0.00    0.00    0.12    0.00    0.00    0.00   85.78
平均时间:    0   11.20    0.00    3.46    0.00    0.00    0.33    0.00    0.00    0.00   85.01
平均时间:    1    9.17    0.00    3.44    0.16    0.00    0.33    0.00    0.00    0.00   86.91
平均时间:    2   11.09    0.00    2.77    0.00    0.00    0.00    0.00    0.00    0.00   86.13
平均时间:    3   11.09    0.00    4.08    0.00    0.00    0.00    0.00    0.00    0.00   84.83

```


##### mpstat -P ALL -I SUM
查看cpu中断的统计(各个cpu分开显示)

```
inux 4.15.0-88-generic (kyle) 	2020年02月25日 	_x86_64_	(4 CPU)

20时13分24秒  CPU    intr/s
20时13分24秒  all   2057.27
20时13分24秒    0    473.14
20时13分24秒    1    591.17
20时13分24秒    2    687.38
20时13分24秒    3    493.70

```

##### mpstat -I SUM
查看cpu中断的统计(各个cpu合并显示)


```
kylechen@kyle:~$  mpstat -I SUM
Linux 4.15.0-88-generic (kyle) 	2020年02月25日 	_x86_64_	(4 CPU)

20时13分47秒  CPU    intr/s
20时13分47秒  all   2036.32

```


### free

#### 功能
free指令会显示内存的使用情况，
包括实体内存，
虚拟的交换文件内存，
共享内存区段，
以及系统核心使用的缓冲区等。
#### 输入语法

```
free [-bkmotV][-s <间隔秒数>]
参数说明：

-b 　以Byte为单位显示内存使用情况。
-k 　以KB为单位显示内存使用情况。
-m 　以MB为单位显示内存使用情况。
-h 　以合适的单位显示内存使用情况，最大为三位数，自动计算对应的单位值。单位如下：

B = bytes
K = kilos
M = megas
G = gigas
T = teras
-t 　显示内存总和列。
-V 　显示版本信息。
-o 　不显示缓冲区调节列。
-s<间隔秒数>持续观察内存使用状况。

```

#### 输出信息

```

total 列显示系统总的可用物理内存和交换空间大小。
used 列显示已经被使用的物理内存和交换空间。
free 列显示还有多少物理内存和交换空间可用使用。
shared 列显示被共享使用的物理内存大小。(进程间的共享内存)
buff/cache 列显示被   磁盘缓存 使用的物理内存大小。(buffer表示写缓冲   cache表示读缓冲)
available 列显示还可以被应用程序使用的物理内存大小

```

```
注意点
1.当应用程序需要内存时，如果没有足够的 free 内存可以用
，内核就会从 buffer 和 cache 中回收内存来满足应用程序的请
求。所以从应用程序的角度来说，available  = free + buffer + cache。
请注意，这只是一个很理想的计算方式，实际中的数据往往有较大的误差。

2.swap space 是磁盘上的一块区域，可以是一个分区，也可以是一个文件
。所以具体的实现可以是 swap 分区也可以是 swap 文件
。当系统物理内存吃紧时，Linux 会将内存中不常访问的数据保存到 swap 上，
这样系统就有更多的物理内存为各个进程服务，而当系统需要访问 swap 上存储的内容时
，再将 swap 上的数据加载到内存中，这就是常说的换出和换入。
交换空间可以在一定程度上缓解内存不足的情况，
但是它需要读写磁盘数据，所以性能不是很高。
```

#### 实例


```
free 不指定单位显示内存使用情况

              总计         已用        空闲      共享    缓冲/缓存    可用
内存：     3920116     2680416      179976      596300     1059724      408108
交换：      999420      519916      479504


```

```
free -h以合适的单位显示内存使用情况
kylechen@kyle:~$ free -h
              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.6G        178M        573M        1.0G        401M
交换：        975M        507M        468M


```


```
free -th以合适的单位显示内存使用情 况,同时显示总量列
kylechen@kyle:~$ free -th
              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        200M        570M        1.0G        424M
交换：        975M        507M        468M
总量：        4.7G        3.0G        668M


```
```
free -h -s 1  以1秒为间隔显示内存使用情况

kylechen@kyle:~$ free -h -s 1
              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        198M        569M        1.0G        421M
交换：        975M        507M        468M

              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        196M        571M        1.0G        419M
交换：        975M        507M        468M

              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        195M        571M        1.0G        419M
交换：        975M        507M        468M

              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        196M        571M        1.0G        420M
交换：        975M        507M        468M

              总计         已用        空闲      共享    缓冲/缓存    可用
内存：        3.7G        2.5G        199M        568M        1.0G        423M
交换：        975M        507M        468M

```


### top

#### 功能

```
top是系统管理员最重要的工具之一。被广泛用于监视服务器的负载。
它可以动态的查看系统当前正在运行的进程情况 , 内存使用情况, cpu使用情况
也就是说上面mpstate 和free能实现的功能 它都内在的包含了
```

#### 输入语法

很简单 直接输入top

#### 输出信息


```

任务: 298 total,   1 running, 246 sleeping,   0 stopped,   0 zombie
%Cpu(s):  5.7 us,  1.6 sy,  0.0 ni, 92.6 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  3920116 total,   301948 free,  2514912 used,  1103256 buff/cache
KiB Swap:   999420 total,   480272 free,   519148 used.   618380 avail Mem 

进程id USER      PR  NI    VIRT    RES    SHR �  %CPU %MEM     TIME+ COMMAND     
 1590 kylechen  20   0 3974220 265424  72628 S   7.3  6.8   5:33.19 gnome-shell 
 1446 kylechen  20   0  521960  38412  19292 S   6.3  1.0   3:28.27 Xorg        
 3008 kylechen  20   0  780520  50972  32452 S   4.6  1.3   0:17.32 gnome-term+ 
 2779 kylechen  20   0 1201196 318344  79024 S   1.3  8.1  11:48.81 chrome      
 1955 kylechen  20   0 1505112 399200 243564 S   1.0 10.2   5:32.38 chrome      
    1 root      20   0  225908   4064   1584 S   0.7  0.1   0:08.15 systemd     
 1036 gdm       20   0 3492528  89676  62056 S   0.7  2.3   0:11.70 gnome-shell 
 1993 kylechen  20   0  556488  55780  24440 S   0.7  1.4   0:34.27 chrome      
    8 root      20   0       0      0      0 I   0.3  0.0   0:08.76 rcu_sched   
  983 mysql     20   0 1358648      0      0 S   0.3  0.0   0:03.85 mysqld      
 1571 kylechen  20   0  220792    156      0 S   0.3  0.0   0:03.21 at-spi2-re+ 
 2192 kylechen  20   0  486420   5828      0 S   0.3  0.1   0:01.88 sogou-qimp+ 
 5330 kylechen  20   0 2621992 450080  80948 S   0.3 11.5   1:13.24 XMind       
 5708 root      20   0       0      0      0 I   0.3  0.0   0:01.65 kworker/u8+ 
 6974 kylechen  20   0  880400 185648 107616 S   0.3  4.7   0:45.80 chrome      
 7245 root      20   0       0      0      0 I   0.3  0.0   0:00.22 kworker/2:0 
 7760 kylechen  20   0   51360   4004   3332 R   0.3  0.1   0:00.19 top    
```

输出信息如上所示 一大坨 很繁琐 咱们一行一行来分析

**第一行表示** 当前的进程状态 总共有298个进程  1个处理runing   246个处于sleeping  0个处于stopped状态

```
任务: 298 total,   1 running, 246 sleeping,   0 stopped
```
**第二行表示** 当前的cpu状态  这里显示的状态参数其实和mpstat那里说的是一样的 具体可以看上面的mpstat命令讲解

```
%Cpu(s):  5.7 us,  1.6 sy,  0.0 ni, 92.6 id,  0.1 wa,  0.0 hi,  
```
**第三四行表示 **当前的内存状态  这里显示的状态参数其实和free那里说的是一样的 具体可以看上面的free命令讲解

```
KiB Mem :  3920116 total,   301948 free,  2514912 used,  1103256 buff/cache
KiB Swap:   999420 total,   480272 free,   519148 used.   6183800.1 wa,  0.0 hi,  
```

**后面几行**其实 表示的就是每个进程具体占用的系统资源 会随着时间动态变化

**PID**

进程ID，进程的唯一标识符

**USER**

进程所有者的实际用户名。

**PR**

进程的调度优先级。这个字段的一些值是’rt’。这意味这这些进程运行在实时态。

**NI**

进程的nice值（优先级）。越小的值意味着越高的优先级。

**VIRT**

进程使用的虚拟内存。

**RES**

驻留内存大小。驻留内存是任务使用的非交换物理内存大小。

**SHR**

SHR是进程使用的共享内存。

**S**

这个是进程的状态。它有以下不同的值:

D – 不可中断的睡眠态。

R – 运行态

S – 睡眠态

T – 被跟踪或已停止

Z – 僵尸态

 I - 空闲状态（idle）


%CPU

自从上一次更新时到现在任务所使用的CPU时间百分比。

**%MEM**

进程使用的可用物理内存百分比。

**TIME+**

任务启动后到现在所使用的全部CPU时间，精确到百分之一秒。

**COMMAND**

运行进程所使用的命令。

还有许多在默认情况下不会显示的输出，它们可以显示进程的页错误、有效组和组ID和其他更多的信息。



```
进程id USER      PR  NI    VIRT    RES    SHR �  %CPU %MEM     TIME+ COMMAND     
 1590 kylechen  20   0 3974220 265424  72628 S   7.3  6.8   5:33.19 gnome-shell 
 1446 kylechen  20   0  521960  38412  19292 S   6.3  1.0   3:28.27 Xorg        
 3008 kylechen  20   0  780520  50972  32452 S   4.6  1.3   0:17.32 gnome-term+ 
 2779 kylechen  20   0 1201196 318344  79024 S   1.3  8.1  11:48.81 chrome      
 .......
 
```
#### 实例


```
实例1:top

top - 21:25:55 up  1:27,  1 user,  load average: 1.51, 1.87, 1.49
任务: 305 total,   2 running, 250 sleeping,   0 stopped,   0 zombie
%Cpu(s): 22.7 us,  1.8 sy,  0.0 ni, 75.1 id,  0.1 wa,  0.0 hi,  0.3 si,  0.0 st
KiB Mem :  3920116 total,   121880 free,  2859016 used,   939220 buff/cache
KiB Swap:   999420 total,   333328 free,   666092 used.   182088 avail Mem 

PID  USER      PR  NI    VIRT    RES    SHR � %CPU %MEM     TIME+ COMMAND      
 2779 kylechen  20   0 1191.7m 314.5m  59.2m S 17.1  8.2  20:45.14 chrome       
 1590 kylechen  20   0 3881.1m 242.1m  70.3m R  4.8  6.3   7:19.67 gnome-shell  
 1446 kylechen  20   0  444.6m  31.1m  11.8m S  1.3  0.8   4:42.48 Xorg         
 8711 kylechen  20   0  622.0m  17.4m   5.6m S  0.7  0.5   0:02.43 gnome-termi+ 
 1955 kylechen  20   0 1458.6m 348.2m 189.5m S  0.5  9.1   7:31.10 chrome       
 1989 kylechen  20   0  653.0m 112.1m  47.2m S  0.5  2.9   7:06.33 chrome       
 8736 kylechen  20   0   50.2m   1.5m   0.8m R  0.2  0.0   0:05.51 top          
 1036 gdm       20   0 3410.7m  82.2m  55.3m S  0.1  2.1   0:15.10 gnome-shell  
 1484 kylechen  20   0  442.8m  48.0m   8.9m S  0.1  1.3   0:40.19 fcitx        
 1993 kylechen  20   0  551.4m  48.9m  17.8m S  0.1  1.3   0:45.63 chrome       
 3299 kylechen  20   0  633.0m  20.9m   5.1m S  0.1  0.5   0:17.90 chrome       
    1 root      20   0  220.6m   3.0m   0.6m S  0.0  0.1   0:10.02 systemd      
    2 root      20   0    0.0m   0.0m   0.0m S  0.0  0.0   0:00.00 kthreadd     
    4 root       0 -20    0.0m   0.0m   0.0m I  0.0  0.0   0:00.00 kworker/0:0H 
    6 root       0 -20    0.0m   0.0m   0.0m I  0.0  0.0   0:00.00 mm_percpu_wq 
    7 root      20   0    0.0m   0.0m   0.0m S  0.0  0.0   0:00.22 ksoftirqd/0  
    8 root      20   0    0.0m   0.0m   0.0m I  0.0  0.0   0:11.86 rcu_sched  
```


```

实例2: 交互式命令

top是一个交互式的命令
所以用top调出动态显示的进程状态以后 
在界面上继续用键盘输入指令 
会继续在界面上执行对应的操作


交互命令1:  回车/空格

top命令默认在一个特定间隔(3秒)后刷新显示。
要手动刷新，用户可以输入回车或者空格。

交互命令2:   B
一些重要信息会以加粗字体显示。
这个命令可以切换粗体显示。


交互命令3:  d 或s
当按下’d’或’s’时，你
将被提示输入一个值（以秒为单位），
它会以设置的值作为刷新间隔。
如果你这里输入了1，top将会每秒刷新。

交互命令4 :‘R’
切换反向/常规排序。

交互命令5 :‘V’
切换树视图。

交互命令6:  ‘k’
top命令中最重要的一个命令之一。
用于发送信号给任务（通常是结束任务）。


交互命令7:  ‘e’
切换显示的单位
依次以k ->m->g->t->p单位选择
```



﻿## 进程相关命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200225233143947.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
### ps
#### 功能



ps 用来查看进程状态
不过这种查看是静态的 也就是只会显示 你输入命令那一刻的进程状态
不会像top那样是动态变化的
ps命令支持三种使用的语法格式：


UNIX 风格，选项可以组合在一起，并且选项前必须有“-”连字符
BSD 风格，选项可以组合在一起，但是选项前不能有“-”连字符
GNU 风格的长选项，选项前有两个“-”连字符
这几种风格可以混用，但是可能会发生冲突。

#### 输入语法
ps[参数]

```cpp
参数如下
ps 没有属性参数的时候显示的是同一终端terminal下所有的进程
ps T  显示同一个终端terminal下的所有进程 输出信息更丰富了一些
ps a 显示同一控制终端tty下的所有进程  结果按照进程id来排序 输出的关键属性有 进程状态  进程控制终端
ps c  显示进程的名称 不显示路径 
ps -A 显示所有用户的所有进程  包括没有控制终端的进程 结果按照进程id排序 输出的关键属性有进程的控制终端 和ps -aux 相比 它输出的信息没有那么全面 比如 没有cpu和mem列
ps -e 等于“ps -A”
ps f  显示同一个控制终端tty下的进程  同时用树状结构的显示程序间的关系 
ps -a  显示所有用户进程 不包括没有控制终端的进程 结果按照进程id排序
ps -u  显示本用户下所有进程 不包括没有控制终端的进程  结果按照进程id排序 而且显示的进程信息很全面
ps  -u [用户名] 显示指定用户名下的所有进程     不包括没有控制终端的进程  结果按照进程id排序 而且显示的进程信息很全面 比如 ps -u root
ps -x  显示本用户所有进程 包括没有控制终端的进程 结果按照进程id排序

ps -au 显示当前用户下所有的进程  不包括没有控制终端的进程
ps -ax 显示所有用户的进程 包括没有控制终端的进程 输出信息里面没有USER用户列
ps -ux 显示当前用户下所有进程     包括没有控制终端的进程
ps -aux  显示所有用户的所有进程 包括没有控制终端的进程
ps -aux --sort -pcpu  显示所有的进程 并且按照cpu使用率排序
ps -aux --sort -pmem  显示所有的进程 并且按照cpu使用率排序
ps -auxf 显示所有用户的所有进程 包括没有控制终端的进程 同时以树形结构显示进程间的关系
ps -e  f  (注意e和f中间有空格)显示所有进程  包括没有控制终端的进程   同时会用树状结构的显示程序间的关系 









```


#### 输出信息

```powershell


USER - 运行该过程的用户
PID 就是这个程序的 ID 
PPID 则是其上级父程序的ID
%CPU- 进程 cpu 利用率。
%MEM - 进程驻留集大小占计算机物理内存的百分比。
VSZ  - 进程的虚拟内存大小 KiB。
RSS- 进程正在使用的物理内存的大小。
PRI 这个是 Priority (优先执行序) 的缩写
NI 这个是 Nice 值
ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 "-"
TTY 登入者的终端机位置
TIME 使用掉的 CPU 时间。
CMD 所下达的指令为何
STAT 代表这个程序的状态 
			ps工具标识进程的5种状态码: 
			D 不可中断 
			R 运行 
			S 中断 
			T 停止
			Z 僵死 
			在上面这些状态吗后面还会有下面这些后缀
			< 优先级高的进程
			N 优先级较低的进程
			L 有些页被锁进内存；
			s 进程的领导者（在它之下有子进程）；
			l 多进程的（使用 CLONE_THREAD, 类似 NPTL pthreads）；
			+ 位于后台的进程组；


```

####  实例

```powershell
实例1
kylechen@kyle:~$ ps
  PID TTY          TIME CMD
17796 pts/0    00:00:00 bash
24667 pts/0    00:00:00 ps

```

```powershell
实例2
kylechen@kyle:~$ ps -T
  PID  SPID TTY          TIME CMD
17796 17796 pts/0    00:00:00 bash
24673 24673 pts/0    00:00:00 ps

```

```powershell
实例3
kylechen@kyle:~$ ps a
  PID TTY      STAT   TIME COMMAND
 1444 tty2     Ssl+   0:00 /usr/lib/gdm3/gdm-x-session --run-script env GNOME_SH
 1446 tty2     Sl+   19:57 /usr/lib/xorg/Xorg vt2 -displayfd 3 -auth /run/user/1
 1457 tty2     Sl+    0:00 /usr/lib/gnome-session/gnome-session-binary --session
 1590 tty2     Sl+   31:32 /usr/bin/gnome-shell
 1628 tty2     Sl     0:00 ibus-daemon --xim --panel disable
 1632 tty2     Sl     0:00 /usr/lib/ibus/ibus-dconf
 1636 tty2     Sl     0:00 /usr/lib/ibus/ibus-x11 --kill-daemon
 1708 tty2     Sl+    0:01 /usr/lib/gnome-settings-daemon/gsd-power
 1710 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-print-notification
 1711 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-rfkill
 1714 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-screensaver-proxy
 1717 tty2     Sl+    0:03 /usr/lib/gnome-settings-daemon/gsd-sharing
 1720 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-smartcard
后面的篇幅太长 ...略掉
```

```powershell
实例4
kylechen@kyle:~$ ps a
  PID TTY      STAT   TIME COMMAND
 1444 tty2     Ssl+   0:00 /usr/lib/gdm3/gdm-x-session --run-script env GNOME_SH
 1446 tty2     Sl+   19:57 /usr/lib/xorg/Xorg vt2 -displayfd 3 -auth /run/user/1
 1457 tty2     Sl+    0:00 /usr/lib/gnome-session/gnome-session-binary --session
 1590 tty2     Sl+   31:32 /usr/bin/gnome-shell
 1628 tty2     Sl     0:00 ibus-daemon --xim --panel disable
 1632 tty2     Sl     0:00 /usr/lib/ibus/ibus-dconf
 1636 tty2     Sl     0:00 /usr/lib/ibus/ibus-x11 --kill-daemon
 1708 tty2     Sl+    0:01 /usr/lib/gnome-settings-daemon/gsd-power
 1710 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-print-notification
 1711 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-rfkill
 1714 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-screensaver-proxy
 1717 tty2     Sl+    0:03 /usr/lib/gnome-settings-daemon/gsd-sharing
 1720 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-smartcard
后面的篇幅太长 ...略掉
```


```powershell
实例5
kylechen@kyle:~$ ps a
  PID TTY      STAT   TIME COMMAND
 1444 tty2     Ssl+   0:00 /usr/lib/gdm3/gdm-x-session --run-script env GNOME_SH
 1446 tty2     Sl+   19:57 /usr/lib/xorg/Xorg vt2 -displayfd 3 -auth /run/user/1
 1457 tty2     Sl+    0:00 /usr/lib/gnome-session/gnome-session-binary --session
 1590 tty2     Sl+   31:32 /usr/bin/gnome-shell
 1628 tty2     Sl     0:00 ibus-daemon --xim --panel disable
 1632 tty2     Sl     0:00 /usr/lib/ibus/ibus-dconf
 1636 tty2     Sl     0:00 /usr/lib/ibus/ibus-x11 --kill-daemon
 1708 tty2     Sl+    0:01 /usr/lib/gnome-settings-daemon/gsd-power
 1710 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-print-notification
 1711 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-rfkill
 1714 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-screensaver-proxy
 1717 tty2     Sl+    0:03 /usr/lib/gnome-settings-daemon/gsd-sharing
 1720 tty2     Sl+    0:00 /usr/lib/gnome-settings-daemon/gsd-smartcard
后面的篇幅太长 ...略掉
```


```powershell
实例6
kylechen@kyle:~$ ps c
  PID TTY      STAT   TIME COMMAND
 1444 tty2     Ssl+   0:00 gdm-x-session
 1446 tty2     Sl+   20:05 Xorg
 1457 tty2     Sl+    0:00 gnome-session-b
 1590 tty2     Sl+   31:46 gnome-shell
 1628 tty2     Sl     0:00 ibus-daemon
 1632 tty2     Sl     0:00 ibus-dconf
 1636 tty2     Sl     0:00 ibus-x11
 1708 tty2     Sl+    0:01 gsd-power
 1710 tty2     Sl+    0:00 gsd-print-notif
 1711 tty2     Sl+    0:00 gsd-rfkill
 1714 tty2     Sl+    0:00 gsd-screensaver
 1717 tty2     Sl+    0:03 gsd-sharing
 1720 tty2     Sl+    0:00 gsd-smartcard
 1725 tty2     Sl+    0:00 gsd-xsettings
 1728 tty2     Sl+    0:00 gsd-wacom
 1735 tty2     Sl+    0:00 gsd-sound
 1746 tty2     Sl+    0:00 gsd-a11y-settin
 1747 tty2     Sl+    0:00 gsd-clipboard
 1751 tty2     Sl+    0:04 gsd-color
 1754 tty2     Sl+    0:00 gsd-datetime
 1755 tty2     Sl+    0:02 gsd-housekeepin
 1756 tty2     Sl+    0:00 gsd-keyboard
 1759 tty2     Sl+    0:01 gsd-media-keys
 1763 tty2     Sl+    0:00 gsd-mouse
 1787 tty2     Sl+    0:00 gsd-printer
 1807 tty2     Sl+    0:00 gsd-disk-utilit
 1955 tty2     SLl+  38:57 chrome
后面的篇幅太长 ...略掉
```


```powershell
kylechen@kyle:~$ ps -aux --sort -pmem
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
kylechen  5330  0.4  9.6 2675660 377160 tty2   Sl+  2月25   4:39 /opt/XMind ZEN/XMind --type=renderer --no-sandbox --primordial-pipe-token=AE1FEBFB388740D8B17A50EFC55AA15A 
kylechen  1955  3.7  9.5 1592304 373052 tty2   SLl+ 2月25  39:07 /opt/google/chrome/chrome
kylechen 14282  4.0  7.9 1240340 312420 tty2   Sl+  2月25  35:06 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=131391
kylechen 19242  0.4  6.9 1008672 273388 tty2   Sl+  00:25   3:11 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=1313919
kylechen 24091  3.2  5.3 916448 209380 tty2    Sl+  12:50   0:38 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=1313919
kylechen  1590  3.1  5.3 3998928 208124 tty2   Sl+  2月25  32:04 /usr/bin/gnome-shell
kylechen 23859  0.6  5.3 899524 207904 tty2    Sl+  12:46   0:08 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=1313919
kylechen 19254  0.1  4.9 866404 194272 tty2    Sl+  00:25   0:50 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=1313919
kylechen  2439  0.0  3.5 1497400 140056 tty2   SLl+ 2月25   0:20 /usr/bin/gnome-software --gapplication-service
kylechen 19681  1.3  2.9 640148 114188 tty2    Sl+  00:32  10:14 /proc/self/exe --type=gpu-process --field-trial-handle=13139199688567382976,8847439337573611818,131072 --gpu
kylechen 19231  0.1  2.8 835928 110572 tty2    Sl+  00:25   0:59 /opt/google/chrome/chrome --type=renderer --disable-webrtc-apm-in-audio-service --field-trial-handle=1313919
kylechen  5263  0.2  2.1 2234672 84160 tty2    Sl+  2月25   2:06 /opt/XMind ZEN/XMind
后面篇幅过长 略掉
```

```powershell
kylechen@kyle:~$ ps -e f
  PID TTY      STAT   TIME COMMAND
    2 ?        S      0:00 [kthreadd]
    4 ?        I<     0:00  \_ [kworker/0:0H]
    6 ?        I<     0:00  \_ [mm_percpu_wq]
    7 ?        S      0:00  \_ [ksoftirqd/0]
    8 ?        I      0:54  \_ [rcu_sched]
    9 ?        I      0:00  \_ [rcu_bh]
   10 ?        S      0:00  \_ [migration/0]
   11 ?        S      0:00  \_ [watchdog/0]
   12 ?        S      0:00  \_ [cpuhp/0]
   13 ?        S      0:00  \_ [cpuhp/1]
   14 ?        S      0:00  \_ [watchdog/1]
   15 ?        S      0:00  \_ [migration/1]
   16 ?        S      0:02  \_ [ksoftirqd/1]
   18 ?        I<     0:00  \_ [kworker/1:0H]
   19 ?        S      0:00  \_ [cpuhp/2]
   20 ?        S      0:00  \_ [watchdog/2]
   21 ?        S      0:00  \_ [migration/2]
   22 ?        S      0:01  \_ [ksoftirqd/2]
   24 ?        I<     0:00  \_ [kworker/2:0H]
   25 ?        S      0:00  \_ [cpuhp/3]
   26 ?        S      0:00  \_ [watchdog/3]
   27 ?        S      0:00  \_ [migration/3]
   28 ?        S      0:00  \_ [ksoftirqd/3]
   30 ?        I<     0:00  \_ [kworker/3:0H]
   31 ?        S      0:00  \_ [kdevtmpfs]
   32 ?        I<     0:00  \_ [netns]
   33 ?        S      0:00  \_ [rcu_tasks_kthre]
   34 ?        S      0:00  \_ [kauditd]
后面略掉
```

### pstree
#### 功能
清晰明了的用树形图显示所有进程的层次关系
#### 输入语法
pstree
#### 输出信息

```powershell
systemd─┬─Main───4*[{Main}]
        ├─ModemManager───2*[{ModemManager}]
        ├─NetworkManager─┬─dhclient
        │                └─2*[{NetworkManager}]
        ├─accounts-daemon───2*[{accounts-daemon}]
        ├─acpid
        ├─avahi-daemon───avahi-daemon
        ├─bluetoothd
        ├─boltd───2*[{boltd}]
        ├─chrome─┬─2*[cat]
        │        ├─chrome─┬─chrome─┬─14*[chrome───10*[{chrome}]]
        │        │        │        ├─chrome───19*[{chrome}]
        │        │        │        ├─chrome───11*[{chrome}]
        │        │        │        └─chrome───7*[{chrome}]
        │        │        └─nacl_helper
        │        ├─chrome───8*[{chrome}]
        │        ├─chrome───6*[{chrome}]
        │        ├─chrome───7*[{chrome}]
        │        └─32*[{chrome}]
        ├─colord───2*[{colord}]
        ├─cron
        ├─cups-browsed───2*[{cups-browsed}]
        ├─cupsd───dbus
        ├─2*[dbus-daemon]
        ├─fcitx───{fcitx}
        ├─fcitx-dbus-watc
        ├─fwupd───4*[{fwupd}]
        ├─gdm3─┬─gdm-session-wor─┬─gdm-x-session─┬─Xorg───3*[{Xorg}]
        │      │                 │               ├─gnome-session-b─┬─gnome-shell─┬─XMind─┬─XMind─┬─XMind───13*[{XMind}]
        │      │                 │               │                 │             │       │       └─XMind───14*[{XMind}]
        │      │                 │               │                 │             │       ├─XMind───4*[{XMind}]
        │      │                 │               │                 │             │       └─41*[{XMind}]
        │      │                 │               │                 │             ├─ibus-daemon─┬─ibus-dconf───3*[{ibus-dconf}]
        │      │                 │               │                 │             │             ├─ibus-engine-lib───3*[{ibus-engine-lib}]
        │      │                 │               │                 │             │             ├─ibus-engine-sim───2*[{ibus-engine-sim}]
        │      │                 │               │                 │             │             └─2*[{ibus-daemon}]
        │      │                 │               │                 │             └─13*[{gnome-shell}]
        │      │                 │               │                 ├─gnome-software───3*[{gnome-software}]
        │      │                 │               │                 ├─gsd-a11y-settin───3*[{gsd-a11y-settin}]
        │      │                 │               │                 ├─gsd-clipboard───2*[{gsd-clipboard}]
        │      │                 │               │                 ├─gsd-color───3*[{gsd-color}]
        │      │                 │               │                 ├─gsd-datetime───3*[{gsd-datetime}]
        │      │                 │               │                 ├─gsd-disk-utilit───2*[{gsd-disk-utilit}]
        │      │                 │               │                 ├─gsd-housekeepin───3*[{gsd-housekeepin}]
        │      │                 │               │                 ├─gsd-keyboard───3*[{gsd-keyboard}]
        │      │                 │               │                 ├─gsd-media-keys───4*[{gsd-media-keys}]
        │      │                 │               │                 ├─gsd-mouse───3*[{gsd-mouse}]
        │      │                 │               │                 ├─gsd-power───4*[{gsd-power}]
        │      │                 │               │                 ├─gsd-print-notif───2*[{gsd-print-notif}]
        │      │                 │               │                 ├─gsd-rfkill───2*[{gsd-rfkill}]
        │      │                 │               │                 ├─gsd-screensaver───2*[{gsd-screensaver}]
        │      │                 │               │                 ├─gsd-sharing───3*[{gsd-sharing}]
        │      │                 │               │                 ├─gsd-smartcard───4*[{gsd-smartcard}]
        │      │                 │               │                 ├─gsd-sound───3*[{gsd-sound}]
        │      │                 │               │                 ├─gsd-wacom───2*[{gsd-wacom}]
        │      │                 │               │                 ├─gsd-xsettings───3*[{gsd-xsettings}]
        │      │                 │               │                 ├─ssh-agent
        │      │                 │               │                 ├─update-notifier───3*[{update-notifier}]
        │      │                 │               │                 └─3*[{gnome-session-b}]
        │      │                 │               └─2*[{gdm-x-session}]
        │      │                 └─2*[{gdm-session-wor}]
        │      └─2*[{gdm3}]
        ├─gnome-keyring-d───3*[{gnome-keyring-d}]
        ├─gsd-printer───2*[{gsd-printer}]
        ├─ibus-x11───2*[{ibus-x11}]
        ├─irqbalance───{irqbalance}
        ├─2*[kerneloops]
        ├─login───bash
        ├─lvmetad
        ├─mysqld───26*[{mysqld}]
        ├─networkd-dispat───{networkd-dispat}
        ├─packagekitd───2*[{packagekitd}]
        ├─polkitd───2*[{polkitd}]
        ├─pulseaudio───2*[{pulseaudio}]
        ├─rsyslogd───3*[{rsyslogd}]
        ├─rtkit-daemon───2*[{rtkit-daemon}]
        ├─snapd───21*[{snapd}]
        ├─sogou-qimpanel───10*[{sogou-qimpanel}]
        ├─sogou-qimpanel-
        ├─sshd
        ├─systemd─┬─(sd-pam)
        │         ├─at-spi-bus-laun─┬─dbus-daemon
        │         │                 └─3*[{at-spi-bus-laun}]
        │         ├─at-spi2-registr───2*[{at-spi2-registr}]
        │         ├─dbus-daemon
        │         ├─dconf-service───2*[{dconf-service}]
        │         ├─evolution-addre─┬─evolution-addre───5*[{evolution-addre}]
        │         │                 └─4*[{evolution-addre}]
        │         ├─evolution-calen─┬─evolution-calen───8*[{evolution-calen}]
        │         │                 └─4*[{evolution-calen}]
        │         ├─evolution-sourc───3*[{evolution-sourc}]
        │         ├─gconfd-2
        │         ├─gnome-shell-cal───5*[{gnome-shell-cal}]
        │         ├─gnome-terminal-─┬─bash───pstree
        │         │                 └─3*[{gnome-terminal-}]
        │         ├─goa-daemon───3*[{goa-daemon}]
        │         ├─goa-identity-se───3*[{goa-identity-se}]
        │         ├─gvfs-afc-volume───3*[{gvfs-afc-volume}]
        │         ├─gvfs-goa-volume───2*[{gvfs-goa-volume}]
        │         ├─gvfs-gphoto2-vo───2*[{gvfs-gphoto2-vo}]
        │         ├─gvfs-mtp-volume───2*[{gvfs-mtp-volume}]
        │         ├─gvfs-udisks2-vo───2*[{gvfs-udisks2-vo}]
        │         ├─gvfsd─┬─gvfsd-dnssd───2*[{gvfsd-dnssd}]
        │         │       ├─gvfsd-network───3*[{gvfsd-network}]
        │         │       ├─gvfsd-smb-brows───3*[{gvfsd-smb-brows}]
        │         │       ├─gvfsd-trash───2*[{gvfsd-trash}]
        │         │       └─2*[{gvfsd}]
        │         ├─gvfsd-fuse───5*[{gvfsd-fuse}]
        │         ├─gvfsd-metadata───2*[{gvfsd-metadata}]
        │         ├─ibus-portal───2*[{ibus-portal}]
        │         ├─xdg-document-po───5*[{xdg-document-po}]
        │         └─xdg-permission-───2*[{xdg-permission-}]
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-resolve
        ├─systemd-timesyn───{systemd-timesyn}
        ├─systemd-udevd
        ├─thermald───{thermald}
        ├─udisksd───4*[{udisksd}]
        ├─unattended-upgr───{unattended-upgr}
        ├─upowerd───2*[{upowerd}]
        ├─whoopsie───2*[{whoopsie}]
        └─wpa_supplicant

```


### kill
#### 功能
发送信号到指定进程

#### 输入语法

```powershell
有两种方式
1.kill [命令参数] 
2.kill [信号参数] [指定进程]
```

```powershell
kill [命令参数] 

-l  后面加信号名称 显示指定信号对应的编号 如果后面不加信号名称 则显示所有信号对应编号
-u 后面加用户名称 表示杀死指定用户的所用进程 即给指定用户的所有进程发送SIGTERM信号
```

```powershell
kill [信号参数] [指定进程]

信号参数--就是你要给指定进程发送的信号值 可以是名称的形式比如-SIGKILL ,-SIGHUP 也可以是编号的形式 比如-9 ,-1 分别对应SIGKILL ,SIGHUP.
指定进程--就是你想要给其发送信号的进程pid 可以通过一些手段比如ps/pstree/top等获取


注意点:

1.也可以不指定信号参数 这样默认发送的就是编号为15的SIGTERM信号

2.当用kill向这些进程发送信号时，必须是这些进程的主人。
如果试图给一个没有权限或者不存在的进程发送信号，就会得到一个错误信息。

3.只有第9种信号(SIGKILL)才可以无条件终止进程，其他信号进程都有权利忽略。
 下面是常用的信号：
SIGHUP    1    终端断线
SIGINT     2    中断（同 Ctrl + C）
SIGQUIT    3    退出（同 Ctrl + \）
SIGTERM   15    终止
SIGKILL    9    强制终止
SIGCONT   18    继续（与STOP相反， fg/bg命令）
SIGSTOP    19    暂停（同 Ctrl + Z）

4.init进程是不可杀的
 ```

####  实例

```powershell
实例1 : 显示所有信号及其编号
kylechen@kyle:~$ kill -l
 1) SIGHUP	 2) SIGINT	 3) SIGQUIT	 4) SIGILL	 5) SIGTRAP
 6) SIGABRT	 7) SIGBUS	 8) SIGFPE	 9) SIGKILL	10) SIGUSR1
11) SIGSEGV	12) SIGUSR2	13) SIGPIPE	14) SIGALRM	15) SIGTERM
16) SIGSTKFLT	17) SIGCHLD	18) SIGCONT	19) SIGSTOP	20) SIGTSTP
21) SIGTTIN	22) SIGTTOU	23) SIGURG	24) SIGXCPU	25) SIGXFSZ
26) SIGVTALRM	27) SIGPROF	28) SIGWINCH	29) SIGIO	30) SIGPWR
31) SIGSYS	34) SIGRTMIN	35) SIGRTMIN+1	36) SIGRTMIN+2	37) SIGRTMIN+3
38) SIGRTMIN+4	39) SIGRTMIN+5	40) SIGRTMIN+6	41) SIGRTMIN+7	42) SIGRTMIN+8
43) SIGRTMIN+9	44) SIGRTMIN+10	45) SIGRTMIN+11	46) SIGRTMIN+12	47) SIGRTMIN+13
48) SIGRTMIN+14	49) SIGRTMIN+15	50) SIGRTMAX-14	51) SIGRTMAX-13	52) SIGRTMAX-12
53) SIGRTMAX-11	54) SIGRTMAX-10	55) SIGRTMAX-9	56) SIGRTMAX-8	57) SIGRTMAX-7
58) SIGRTMAX-6	59) SIGRTMAX-5	60) SIGRTMAX-4	61) SIGRTMAX-3	62) SIGRTMAX-2
63) SIGRTMAX-1	64) SIGRTMAX	
```

```powershell
实例2 :  显示SIGHIP的编号
kylechen@kyle:~$ kill -l SIGHUP
1

```

```powershell
实例3: 给用户kylechen的所有进程发送默认信号SIGTERM
kylechen@kyle:~$ kill -u kylechen
```

```powershell
实例4  给进程26467发送默认信号 SIGTERM
kylechen@kyle:~$ kill 26467

```
```powershell
实例5  给进程26467发送编号为9的信号 SIGKILL
kylechen@kyle:~$ kill  -9 26467

```

```powershell
实例5 给进程26467发送名称为SIGHUP的信号 
kylechen@kyle:~$ kill  -SIGHUP 26467

```



