# 一个小时掌握所有Linux核心命令(2020.02.28更新)


## bash相关操作
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228090851592.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
###  数据流重导向
#### 功能
将命令在终端的标准输出 标准出错 重定向到别的地方比如文件中
或者读取文件的内容来取代标准输入

#### 输入语法

```powershell
1. 标准输入 (stdin) ：代码为 0 ，使用 < 或 << ； 一般和cat配合使用将一个文件的内容输出到另外一个文件
2. 标准输出 (stdout)：代码为 1 ，使用 > 或 >> ； 将命令的标准输出重定向到另外一个位置 比如文件中
3.  标准错误输出(stderr)：代码为 2 ，使用 2> 或 2>> 将命令的标注出错重定向到另外一个位置比如文件中
```
#### 实例

```powershell
实例1:
将命令"ll / "的标准输出存到 ~/rootfile这个文件中  (也可以用1>)
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件 则这个文件的内容将会被全部替换
[dmtsai@study ~]$ ll / > ~/rootfile 
```

```powershell
实例2:
将命令"ll / "的标准输出存到 ~/rootfile这个文件中(也可以用1>>)
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件 则会将标准输出的内容添加到这个文件原有内容的末尾
[dmtsai@study ~]$ ll / >> ~/rootfile 
```

```powershell
实例3:
将命令"ll / "的标准错误存到 ~/rootfile这个文件中 
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件 则这个文件的内容将会被全部替换
[dmtsai@study ~]$ ll / 2> ~/rootfile 
```

```powershell
实例4:
将命令"ll / "的标准错误存到 ~/rootfile这个文件中 
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件  则会将标准错误的内容添加到这个文件原有内容的末尾
[dmtsai@study ~]$ ll / 2>>~/rootfile 
```


```powershell
实例5:
将命令"ll / "的标准输出存到 stdoutput.txt       将标准错误输出到stderr.txt 
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件 则这个文件的内容将会被全部替换
[dmtsai@study ~]$ ll / 1>stdoutput.txt 2>~/stderr.txt 
```

```powershell
实例6:
将命令"ll / "的标准输出和标准错误都存到 stdoutput.txt       
如果原本没有这个文件 则这个文件将被创建 
如果原本有这个文件  则这个文件的内容将会被全部替换
[dmtsai@study ~]$ ll / 1>stdoutput.txt  2>&1  正确写法
[dmtsai@study ~]$ ll / 1>stdoutput.txt  2>~/stdoutput.txt   错误写法 会导致文件中两种输出是乱序的
```

```powershell
实例7:
将命令"ll / "的标准输出存到 stdoutput.txt        标准错误存到垃圾桶(/dev/null)
  标准错误不会显示 标准输出存到文件stdoutput.txt中
[dmtsai@study ~]$ ll /   1>~/stdoutput.txt 2>/dev/null 
```

```powershell
实例8:
利用 cat 指令来建立一个文件的简单流程
[dmtsai@study ~]$ cat > catfile
testing 
cat file test
<==这里按下 [ctrl]+d 来离开
```

```powershell
实例9:
用 标准输入 取代键盘的输入以建立新文件的简单流程
也就是将/.bashrc的内容传到catfile里面 (<是覆盖 <<是补充到文件末尾)
[dmtsai@study ~]$ cat > catfile < ~/.bashrc
[dmtsai@study ~]$ ll catfile ~/.bashrc
-rw-r--r--. 1 dmtsai dmtsai 231 Mar 6 06:06 /home/dmtsai/.bashrc
-rw-rw-r--. 1 dmtsai dmtsai 231 Jul 9 18:58 catfile
# 注意看，这两个文件的大小会一模一样！几乎像是使用 cp 来复制一般！
```

```powershell
实例10:
键盘的输入以建立新文件 以指定的字符作为结束符
[dmtsai@study ~]$ cat > catfile << "eof"
> This is a test.
> OK now stop
> > eof <==输入这关键词，立刻就结束而不需要输入 [ctrl]+d

[dmtsai@study ~]$ cat catfile
This is a test.
OK now stop <==只有这两行，不会存在关键词那一行
```

### 多个命令同时执行
#### 功能
某些情况下，很多指令我想要一次输入去执行，而不想要分次执行时
#### 输入语法

```powershell
方法一:用分号连接 命令之间没有关联
sync; sync; shutdown -h now 
```
```powershell
方法二:用&&连接
cmd1 && cmd2
1. 若 cmd1 执行完毕且正确执行($?=0)，则开始执行 cmd2。
2. 若 cmd1 执行完毕且为错误 ($?≠0)，则 cmd2 不执行。
```

```powershell
方法三:用||连接
cmd1 || cmd2
3. 若 cmd1 执行完毕且正确执行($?=0)，则 cmd2 不执行。
4. 2. 若 cmd1 执行完毕且为错误 ($?≠0)，则开始执行 cmd
```

### 命令的多行显示
#### 功能
如果命令太长可以用多行显示
#### 输入
用\符号分割
#### 实例

```powershell
[dmtsai@study ~]$ cp /var/spool/mail/root /etc/crontab \
> /etc/fstab /root
上面这个指令用途是将三个文件复制到 /root 这个目录下而已。
```


### 命令的通配符
#### 功能
对命令进行模糊匹配
#### 输入
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022808294494.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
#### 实例

```powershell
范例一:找出 /etc/ 底下以 cron 为开头的档名
[dmtsai@study ~]$ ll -d /etc/cron*
<==加上 -d 是为了仅显示目录而已

范例二:找出 /etc/ 底下文件名『刚好是五个字母』的文件名
[dmtsai@study ~]$ ll -d /etc/?????

范例三:找出 /etc/ 底下文件名含有数字的文件名
[dmtsai@study ~]$ ll -d /etc/*[0-9]*
<==记得中括号左右两边均需 *

范例四:找出 /etc/ 底下,档名开头非为小写字母的文件名:
[dmtsai@study ~]$ ll -d /etc/[^a-z]*
<==注意中括号左边没有 *

范例五:将范例四找到的文件复制到 /tmp/upper 中
[dmtsai@study ~]$ mkdir /tmp/upper; cp -a /etc/[^a-z]* /tmp/upper
```

### 命令的定时执行
#### 功能
即crontab命令 ,设定某个命令的周期性执行

#### 输入
首先要介绍一下 crond，因为 crontab 命令需要 crond服务支持。
cron 是 Linux 下用来周期地执行某种任务或等待处理某些事件的一个守护进程，
和 Windows 中的计划任务有些类似。 所以要先打开cron服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020022808430139.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
crontab
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228084345660.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228084357853.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228084449900.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
### 命令的执行历史
#### 功能
即history命令 ,获取你在终端输入的命令历史

```powershell
[dmtsai@study ~]$ history [n]
[dmtsai@study ~]$ history [-c]
 n :数字,意思是『要列出最近输入的 n条命令 』
-c :将目前的 shell 中的所有 history 内容全部消除
```

### 命令的别名
#### 功能
即 给命令指定一个别名 主要用在命令比较长的时候

```powershell
 alias lm='ls -al | more' 比如这个把ls -al | more重命名为lm
```
```powershell
unalias lm  撤销刚才的重命名
```


### 命令的解释
#### 功能
即获取命令的manpage   用man命令 






## 变量相关命令
### 变量的声明赋值
#### 功能
相当于是声明一个变量 同时给变量赋值 
#### 输入
声明方法1:  通过 赋值符号 =
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228123044705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
 声明方法2: 通过read
 
用read以后 变量赋值是通过**终端等待用户的键盘输入** 来执行
```powershell
[dmtsai@study ~]$ read [-pt] variable
选项与参数：
-p ：后面可以接提示字符！
-t ：后面可以接等待的『秒数！』
```

#### 实例
```powershell
变量的普通赋值形式
[dmtsai@study ~]$ CHENWEIJIE=0820
```


```powershell
让用户由键盘输入一内容，将该内容变成名为 atest 的变量
[dmtsai@study ~]$ read ates
This is a test <==此时光标会等待你输入！请输入左侧文字看看
[dmtsai@study ~]$ echo ${atest}
This is a test <==你刚刚输入的数据已经变成一个变量内
```

```powershell
让用户由键盘输入一内容, 会有提示的字符串提示你输入 , 然后将该内容变成名为 name 的变量
kylechen@kyle:~$ read -p"请输入你的姓名:" -t 5 name
请输入你的姓名:
kylechen@kyle:~$ echo $name
chenweijie

```

### 变量的查看转换
#### 功能
查看或转化指定的变量 
#### 输入

```powershell
echo 查看指定单个变量的值 可以用用**echo PATH** 或者 **echo ${PATH}** 格式
env  查看所有环境变量
set 查看所有变量(包括用户设定的变量和环境变量,其他bash接口变量)
export  变量名=变量值 将自定义变量转化成环境变量

关于环境变量:
1.  环境变量是用来定义系统运行环境的一些参数，比如每个用户不同的家目录（HOME）、邮件存放位置（MAIL）等。
2.   环境变量所有终端下所有程序都可直接取用的  可以理解为 环境变量=全局变量  自定义变量=局部变量
3.   可以通过export将自定义变量设定为当前终端下的环境变量 但是也就是只能当前终端下的父子进程可以取用 其他终端不行
 4.  可以通过自定义变量的export语句添加到个人目录下的.bashrc文件中 这样当前用户的所有程序都可以使用这个环境变量
5.   可以通过将自定义变量的export语句添加到/etc/profile文件下 这样所有用户的所有程序都可以使用这个环境变量
```

#### 实例

```powershell
输出自定义变量myname

[dmtsai@study ~]$ echo ${myname}
 <==这里并没有任何数据～因为这个变量尚未被设定！是空的[
 dmtsai@study ~]$ myname=VBird
 [dmtsai@study ~]$ echo ${myname}
 VBird <==出现了！因为这个变量已经被设定
```

```powershell
输出变量PATH
[dmtsai@study ~]$ echo  $PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/b
```
```powershell
输出变量PATH
[dmtsai@study ~]$ echo  ${PATH}
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/dmtsai/.local/bin:/home/dmtsai/b
```

```powershell
列出目前的 shell 环境下的所有环境变量与其内容。
[dmtsai@study ~]$ env
```

```powershell
列出目前的 shell 环境下的所有变量。
[dmtsai@study ~]$ set
```

```powershell
将自定义的变量CHENWEIJIE转换成环境变量
export CHENWEIJIE=0820 
```



### 变量的类型指定

#### 功能
变量声明的时候指定类型
#### 输入

```powershell
$ declare [-aixr] variable

选项与参数：
-a ：将后面名为 variable 的变量定义成为数组 (array) 类型
-i ：将后面名为 variable 的变量定义成为整数数字 (integer) 类型
-x ：用法与 export 一样，就是将后面的 variable 变成环境变量；
-r ：将变量设定成为 readonly 类型，该变量不可被更改内容，也不能 unset
```

#### 实例

```powershell
范例一：让变量 sum 进行 100+300+50 的加总结果
[dmtsai@study ~]$ sum=100+300+5
[dmtsai@study ~]$ echo ${sum}100+300+50 <==咦！怎么没有帮我计算加总？因为这是文字型态的变量属性啊！[dmtsai@study ~]$ declare -i sum=100+300+50
[dmtsai@study ~]$ echo ${sum}
450 
```

```powershell
范例二：将 sum 变成环境变量
[dmtsai@study ~]$ declare -x sum
[dmtsai@study ~]$ export | grep sum
declare -ix sum="450" <==果然出现了！包括有 i 与 x 的宣告！

范例三：让 sum 变成只读属性，不可更动！[dmtsai@study ~]$ declare -r sum
[dmtsai@study ~]$ sum=tesgtin-bash: sum: readonly variable <==老天爷～不能改这个变数了！

范例四：让 sum 变成非环境变量的自定义变量吧
[dmtsai@study ~]$ declare +x sum <== 将 - 变成 + 可以进行『取消』动作

[dmtsai@study ~]$ declare -p sum <== -p 可以单独列出变量的类型
declare -ir sum="450" <== 看吧！只剩下 i, r 的类型，不具有 x 
```


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



## 进程相关命令

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


### ulimit 
#### 功能
限制用户使用系统的某些资源 包括可以开启的文件数量， 可以使用的 CPU 时间，可以使用的内存总量等等。
#### 输入
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200228132814523.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
配额可以使unlimited 这样就设定为无限制





## 磁盘相关命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200226215614331.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200226215628466.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)

### du
#### 功能
显示指定的目录或文件所占用的磁盘空间。
#### 输入语法

```powershell

du [参数][目录路径或文件路径]

常用参数:
-a 显示指定目录下所有单独的文件大小 后面不加文件或者目录路径则表示本目录
-b 以byte为单位显示
-c 除了显示单独的目录或者文件的大小外，同时也显示所有目录或文件的总和。
-h 以K，M，G为单位显示，提高信息的可读性。
如果不加参数则显示当前目录下总的大小
```

#### 实例

```powershell
kylechen@kyle:~/test$ du
20	.

```


```powershell
ylechen@kyle:~/test$ du -a
16	./test.jpg
0	./readme.txt
20	.

```

```powershell
kylechen@kyle:~/test$ du -ab
14491	./test.jpg
0	./readme.txt
18587	.

```

```powershell
kylechen@kyle:~/test$ du -abc
14491	./test.jpg
0	./readme.txt
18587	.
18587	总用量


```
### df
#### 功能
显示目前在Linux系统上的文件系统的磁盘使用情况统计。
#### 输入语法

```powershell
df [参数]
```

```powershell
df  不加参数 显示整个磁盘空间中文件系统的使用情况
df -h 以人类友好格式显示文件系统硬盘空间使用情况 这样就会在显示合适的单位 比如 K M G
df -T 显示文件系统类型
df -t ext4 仅列出某些文件系统 比如这里是限制只列出ext4系统的文件系统
df -x ext4 排除指定类型的文件系统 比如这里是列出了除 ext4 类型以外的全部文件系统
```

#### 输出信息

```powershell
Filesystem – Linux 系统中的文件系统
1K-blocks – 文件系统的大小，用 1K 大小的块来表示。
Used – 以 1K 大小的块所表示的已使用数量。
Available – 以 1K 大小的块所表示的可用空间的数量。
Use% – 文件系统中已使用的百分比。
Mounted on – 已挂载的文件系统的挂载点。
```

#### 实例

```powershell
kylechen@kyle:~/test$ df
文件系统                        1K-块      已用     可用 已用% 挂载点
udev                          1936656         0  1936656    0% /dev
tmpfs                          392016      2068   389948    1% /run
/dev/mapper/ubuntu--vg-root 243559804 181980468 49137532   79% /
tmpfs                         1960060    311900  1648160   16% /dev/shm
tmpfs                            5120         4     5116    1% /run/lock
tmpfs                         1960060         0  1960060    0% /sys/fs/cgroup
/dev/loop0                       1024      1024        0  100% /snap/gnome-logs/81
/dev/loop1                     127360    127360        0  100% /snap/vscode/93
/dev/loop2                       4352      4352        0  100% /snap/gnome-calculator/544
/dev/loop3                     144128    144128        0  100% /snap/gnome-3-26-1604/98
/dev/loop4                       1024      1024        0  100% /snap/gnome-logs/73
/dev/loop5                        256       256        0  100% /snap/gtk2-common-themes/9
/dev/loop6                     126976    126976        0  100% /snap/vscode/89
/dev/loop7                     164096    164096        0  100% /snap/gnome-3-28-1804/116
/dev/loop8                      93568     93568        0  100% /snap/core/8689
/dev/loop9                      56064     56064        0  100% /snap/core18/1668
/dev/loop11                     56064     56064        0  100% /snap/core18/1650
/dev/loop10                      4352      4352        0  100% /snap/gnome-calculator/536
/dev/loop12                     15232     15232        0  100% /snap/remmina/3995
/dev/loop13                    267008    267008        0  100% /snap/kde-frameworks-5-core18/32
/dev/loop14                       256       256        0  100% /snap/gtk2-common-themes/5
/dev/loop15                     15232     15232        0  100% /snap/remmina/3968
/dev/loop16                     15104     15104        0  100% /snap/gnome-characters/399
/dev/sda1                      523248      2284   520964    1% /boot/efi
/dev/loop17                    160512    160512        0  100% /snap/gnome-3-28-1804/110
/dev/loop18                    127872    127872        0  100% /snap/vscode/87
/dev/loop19                     15104     15104        0  100% /snap/gnome-characters/375
/dev/loop20                     98688     98688        0  100% /snap/typora-alanzanattadev/2
/dev/loop21                      3840      3840        0  100% /snap/gnome-system-monitor/123
/dev/loop22                     98176     98176        0  100% /snap/kdenlive/22
/dev/loop23                     98304     98304        0  100% /snap/kdenlive/13
/dev/loop24                     46080     46080        0  100% /snap/gtk-common-themes/1440
/dev/loop25                    144128    144128        0  100% /snap/gnome-3-26-1604/97
/dev/loop26                     45312     45312        0  100% /snap/gtk-common-themes/1353
/dev/loop27                    259584    259584        0  100% /snap/electronic-wechat/7
/dev/loop28                     93568     93568        0  100% /snap/core/8592
/dev/loop29                      3840      3840        0  100% /snap/gnome-system-monitor/127
tmpfs                          392012        16   391996    1% /run/user/121
tmpfs                          392012        36   391976    1% /run/user/1000

```


```powershell
kylechen@kyle:~/test$ df -h
文件系统                     容量  已用  可用 已用% 挂载点
udev                         1.9G     0  1.9G    0% /dev
tmpfs                        383M  2.1M  381M    1% /run
/dev/mapper/ubuntu--vg-root  233G  174G   47G   79% /
tmpfs                        1.9G  307M  1.6G   17% /dev/shm
tmpfs                        5.0M  4.0K  5.0M    1% /run/lock
tmpfs                        1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/loop0                   1.0M  1.0M     0  100% /snap/gnome-logs/81
/dev/loop1                   125M  125M     0  100% /snap/vscode/93
/dev/loop2                   4.3M  4.3M     0  100% /snap/gnome-calculator/544
/dev/loop3                   141M  141M     0  100% /snap/gnome-3-26-1604/98
/dev/loop4                   1.0M  1.0M     0  100% /snap/gnome-logs/73
/dev/loop5                   256K  256K     0  100% /snap/gtk2-common-themes/9
/dev/loop6                   124M  124M     0  100% /snap/vscode/89
/dev/loop7                   161M  161M     0  100% /snap/gnome-3-28-1804/116
/dev/loop8                    92M   92M     0  100% /snap/core/8689
/dev/loop9                    55M   55M     0  100% /snap/core18/1668
/dev/loop11                   55M   55M     0  100% /snap/core18/1650
/dev/loop10                  4.3M  4.3M     0  100% /snap/gnome-calculator/536
/dev/loop12                   15M   15M     0  100% /snap/remmina/3995
/dev/loop13                  261M  261M     0  100% /snap/kde-frameworks-5-core18/32
/dev/loop14                  256K  256K     0  100% /snap/gtk2-common-themes/5
/dev/loop15                   15M   15M     0  100% /snap/remmina/3968
/dev/loop16                   15M   15M     0  100% /snap/gnome-characters/399
/dev/sda1                    511M  2.3M  509M    1% /boot/efi
/dev/loop17                  157M  157M     0  100% /snap/gnome-3-28-1804/110
/dev/loop18                  125M  125M     0  100% /snap/vscode/87
/dev/loop19                   15M   15M     0  100% /snap/gnome-characters/375
/dev/loop20                   97M   97M     0  100% /snap/typora-alanzanattadev/2
/dev/loop21                  3.8M  3.8M     0  100% /snap/gnome-system-monitor/123
/dev/loop22                   96M   96M     0  100% /snap/kdenlive/22
/dev/loop23                   96M   96M     0  100% /snap/kdenlive/13
/dev/loop24                   45M   45M     0  100% /snap/gtk-common-themes/1440
/dev/loop25                  141M  141M     0  100% /snap/gnome-3-26-1604/97
/dev/loop26                   45M   45M     0  100% /snap/gtk-common-themes/1353
/dev/loop27                  254M  254M     0  100% /snap/electronic-wechat/7
/dev/loop28                   92M   92M     0  100% /snap/core/8592
/dev/loop29                  3.8M  3.8M     0  100% /snap/gnome-system-monitor/127
tmpfs                        383M   16K  383M    1% /run/user/121
tmpfs                        383M   36K  383M    1% /run/user/1000

```


```powershell
kylechen@kyle:~/test$ df -T
文件系统                    类型         1K-块      已用     可用 已用% 挂载点
udev                        devtmpfs   1936656         0  1936656    0% /dev
tmpfs                       tmpfs       392016      2068   389948    1% /run
/dev/mapper/ubuntu--vg-root ext4     243559804 181980572 49137428   79% /
tmpfs                       tmpfs      1960060    302084  1657976   16% /dev/shm
tmpfs                       tmpfs         5120         4     5116    1% /run/lock
tmpfs                       tmpfs      1960060         0  1960060    0% /sys/fs/cgroup
/dev/loop0                  squashfs      1024      1024        0  100% /snap/gnome-logs/81
/dev/loop1                  squashfs    127360    127360        0  100% /snap/vscode/93
/dev/loop2                  squashfs      4352      4352        0  100% /snap/gnome-calculator/544
/dev/loop3                  squashfs    144128    144128        0  100% /snap/gnome-3-26-1604/98
/dev/loop4                  squashfs      1024      1024        0  100% /snap/gnome-logs/73
/dev/loop5                  squashfs       256       256        0  100% /snap/gtk2-common-themes/9
/dev/loop6                  squashfs    126976    126976        0  100% /snap/vscode/89
/dev/loop7                  squashfs    164096    164096        0  100% /snap/gnome-3-28-1804/116
/dev/loop8                  squashfs     93568     93568        0  100% /snap/core/8689
/dev/loop9                  squashfs     56064     56064        0  100% /snap/core18/1668
/dev/loop11                 squashfs     56064     56064        0  100% /snap/core18/1650
/dev/loop10                 squashfs      4352      4352        0  100% /snap/gnome-calculator/536
/dev/loop12                 squashfs     15232     15232        0  100% /snap/remmina/3995
/dev/loop13                 squashfs    267008    267008        0  100% /snap/kde-frameworks-5-core18/32
/dev/loop14                 squashfs       256       256        0  100% /snap/gtk2-common-themes/5
/dev/loop15                 squashfs     15232     15232        0  100% /snap/remmina/3968
/dev/loop16                 squashfs     15104     15104        0  100% /snap/gnome-characters/399
/dev/sda1                   vfat        523248      2284   520964    1% /boot/efi
/dev/loop17                 squashfs    160512    160512        0  100% /snap/gnome-3-28-1804/110
/dev/loop18                 squashfs    127872    127872        0  100% /snap/vscode/87
/dev/loop19                 squashfs     15104     15104        0  100% /snap/gnome-characters/375
/dev/loop20                 squashfs     98688     98688        0  100% /snap/typora-alanzanattadev/2
/dev/loop21                 squashfs      3840      3840        0  100% /snap/gnome-system-monitor/123
/dev/loop22                 squashfs     98176     98176        0  100% /snap/kdenlive/22
/dev/loop23                 squashfs     98304     98304        0  100% /snap/kdenlive/13
/dev/loop24                 squashfs     46080     46080        0  100% /snap/gtk-common-themes/1440
/dev/loop25                 squashfs    144128    144128        0  100% /snap/gnome-3-26-1604/97
/dev/loop26                 squashfs     45312     45312        0  100% /snap/gtk-common-themes/1353
/dev/loop27                 squashfs    259584    259584        0  100% /snap/electronic-wechat/7
/dev/loop28                 squashfs     93568     93568        0  100% /snap/core/8592
/dev/loop29                 squashfs      3840      3840        0  100% /snap/gnome-system-monitor/127
tmpfs                       tmpfs       392012        16   391996    1% /run/user/121
tmpfs                       tmpfs       392012        36   391976    1% /run/user/1000

```

```powershell
kylechen@kyle:~/test$ df -t ext4
文件系统                        1K-块      已用     可用 已用% 挂载点
/dev/mapper/ubuntu--vg-root 243559804 181980664 49137336   79% /

```

```powershell
kylechen@kyle:~/test$ df -x ext4
文件系统         1K-块   已用    可用 已用% 挂载点
udev           1936656      0 1936656    0% /dev
tmpfs           392016   2068  389948    1% /run
tmpfs          1960060 299520 1660540   16% /dev/shm
tmpfs             5120      4    5116    1% /run/lock
tmpfs          1960060      0 1960060    0% /sys/fs/cgroup
/dev/loop0        1024   1024       0  100% /snap/gnome-logs/81
/dev/loop1      127360 127360       0  100% /snap/vscode/93
/dev/loop2        4352   4352       0  100% /snap/gnome-calculator/544
/dev/loop3      144128 144128       0  100% /snap/gnome-3-26-1604/98
/dev/loop4        1024   1024       0  100% /snap/gnome-logs/73
/dev/loop5         256    256       0  100% /snap/gtk2-common-themes/9
/dev/loop6      126976 126976       0  100% /snap/vscode/89
/dev/loop7      164096 164096       0  100% /snap/gnome-3-28-1804/116
/dev/loop8       93568  93568       0  100% /snap/core/8689
/dev/loop9       56064  56064       0  100% /snap/core18/1668
/dev/loop11      56064  56064       0  100% /snap/core18/1650
/dev/loop10       4352   4352       0  100% /snap/gnome-calculator/536
/dev/loop12      15232  15232       0  100% /snap/remmina/3995
/dev/loop13     267008 267008       0  100% /snap/kde-frameworks-5-core18/32
/dev/loop14        256    256       0  100% /snap/gtk2-common-themes/5
/dev/loop15      15232  15232       0  100% /snap/remmina/3968
/dev/loop16      15104  15104       0  100% /snap/gnome-characters/399
/dev/sda1       523248   2284  520964    1% /boot/efi
/dev/loop17     160512 160512       0  100% /snap/gnome-3-28-1804/110
/dev/loop18     127872 127872       0  100% /snap/vscode/87
/dev/loop19      15104  15104       0  100% /snap/gnome-characters/375
/dev/loop20      98688  98688       0  100% /snap/typora-alanzanattadev/2
/dev/loop21       3840   3840       0  100% /snap/gnome-system-monitor/123
/dev/loop22      98176  98176       0  100% /snap/kdenlive/22
/dev/loop23      98304  98304       0  100% /snap/kdenlive/13
/dev/loop24      46080  46080       0  100% /snap/gtk-common-themes/1440
/dev/loop25     144128 144128       0  100% /snap/gnome-3-26-1604/97
/dev/loop26      45312  45312       0  100% /snap/gtk-common-themes/1353
/dev/loop27     259584 259584       0  100% /snap/electronic-wechat/7
/dev/loop28      93568  93568       0  100% /snap/core/8592
/dev/loop29       3840   3840       0  100% /snap/gnome-system-monitor/127
tmpfs           392012     16  391996    1% /run/user/121
tmpfs           392012     36  391976    1% /run/user/1000

```





## 网络相关命令
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200226230336229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
![](https://img-blog.csdnimg.cn/20200226230348236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200226230404327.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3ZqaGdoamdoag==,size_16,color_FFFFFF,t_70)
### netstat
#### 功能
显示网络状态。
#### 输入

```powershell

netstat [参数]

-a-显示所有socket
-l 显示所有处于监听状态的socket
-t 显示所有使用tcp协议的socket
-u-显示UDP传输协议的连线状况。
-s 显示所有网络协议的统计信息。
```

#### 输出

```powershell
1. 列出所有端口 (包括监听和未监听的)
列出所有端口:     netstat -a
列出所有tcp端口:  netstat -at
列出所有udp端口:  netstat -au
2. 列出所有处于监听状态的 Sockets
只显示监听端口:          netstat -l
只列出所有监听tcp端口:   netstat -lt
只列出所有监听udp端口:   netstat -lu
只列出所有监听UNIX端口:  netstat -lx
3. 显示每个协议的统计信息
显示所有端口的统计信息 netstat -s
```


### ifconfig
#### 功能
用于显示或设置网络设备
#### 输入

```powershell

语法
ifconfig [网络设备][down up ][add<地址>][del<地址>][<hw<网络设备类型><硬件地址>][mtu<字节>][netmask<子网掩码>][IP地址]
参数说明：

add<地址> 设置网络设备IPv6的IP地址。
del<地址> 删除网络设备IPv6的IP地址。
down 关闭指定的网络设备。
up 启动指定的网络设备。
<hw<网络设备类型><硬件地址> 设置网络设备的类型与硬件MAC地址。

mtu<字节> 设置网络设备的MTU。
netmask<子网掩码> 设置网络设备的子网掩码。
[IP地址] 指定网络设备的IP地址。

```


#### 实例

```powershell
显示网络设备信息

# ifconfig        
eth0   Link encap:Ethernet HWaddr 00:50:56:0A:0B:0C 
     inet addr:192.168.0.3 Bcast:192.168.0.255 Mask:255.255.255.0
     inet6 addr: fe80::250:56ff:fe0a:b0c/64 Scope:Link
     UP BROADCAST RUNNING MULTICAST MTU:1500 Metric:1
     RX packets:172220 errors:0 dropped:0 overruns:0 frame:0
     TX packets:132379 errors:0 dropped:0 overruns:0 carrier:0
     collisions:0 txqueuelen:1000 
     RX bytes:87101880 (83.0 MiB) TX bytes:41576123 (39.6 MiB)
     Interrupt:185 Base address:0x2024 

lo    Link encap:Local Loopback 
     inet addr:127.0.0.1 Mask:255.0.0.0
     inet6 addr: ::1/128 Scope:Host
     UP LOOPBACK RUNNING MTU:16436 Metric:1
     RX packets:2022 errors:0 dropped:0 overruns:0 frame:0
     TX packets:2022 errors:0 dropped:0 overruns:0 carrier:0
     collisions:0 txqueuelen:0 
     RX bytes:2459063 (2.3 MiB) TX bytes:2459063 (2.3 MiB)
启动关闭指定网卡

# ifconfig eth0 down
# ifconfig eth0 up
为网卡配置和删除IPv6地址

# ifconfig eth0 add 33ffe:3240:800:1005::2/ 64 //为网卡诶之IPv6地址

# ifconfig eth0 del 33ffe:3240:800:1005::2/ 64 //为网卡删除IPv6地址
用ifconfig修改MAC地址

# ifconfig eth0 down //关闭网卡
# ifconfig eth0 hw ether 00:AA:BB:CC:DD:EE //修改MAC地址
# ifconfig eth0 up //启动网卡
# ifconfig eth1 hw ether 00:1D:1C:1D:1E //关闭网卡并修改MAC地址 
# ifconfig eth1 up //启动网卡
配置IP地址

# ifconfig eth0 192.168.1.56 
//给eth0网卡配置IP地址
# ifconfig eth0 192.168.1.56 netmask 255.255.255.0 
// 给eth0网卡配置IP地址,并加上子掩码
# ifconfig eth0 192.168.1.56 netmask 255.255.255.0 broadcast 192.168.1.255
// 给eth0网卡配置IP地址,加上子掩码,加上个广播地址
启用和关闭ARP协议

# ifconfig eth0 arp  //开启
# ifconfig eth0 -arp  //关闭
设置最大传输单元

# ifconfig eth0 mtu 1500 
//设置能通过的最大数据包大小为 1500 bytes
```



