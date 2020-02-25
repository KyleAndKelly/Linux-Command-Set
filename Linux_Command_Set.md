# Linu常x命令汇总(更新中)

## 性能检测相关命令
![image.png](http://note.youdao.com/yws/res/5087/WEBRESOURCE231988a339d18122117a0fd29d8c5a44)
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

![image.png](http://note.youdao.com/yws/res/5128/WEBRESOURCE5e6b80b7d2f55371a4c8c0d578bd0229)
##### mpstat -P ALL 2 3
每隔2秒显示一次 所有cpu的状态信息
一共产生3个间隔的信息
最后给出这3次间隔的平均信息

![image.png](http://note.youdao.com/yws/res/5130/WEBRESOURCE163a1853a4005c71630ed6dd2b5c78f8)

##### mpstat -P ALL -I SUM
查看cpu中断的统计(各个cpu分开显示)
![image.png](http://note.youdao.com/yws/res/5132/WEBRESOURCE5446ca67282e319931562d6860718e49)
##### mpstat -I SUM
查看cpu中断的统计(各个cpu合并显示)

![image.png](http://note.youdao.com/yws/res/5134/WEBRESOURCE62242d0b2ebf96cbe731919cebea9ebc
)

### top
### free