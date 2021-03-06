系统监控
===
### PS 进程监控命令
选项 | 说明
:---:|:---
a | 显示终端上的所有进程，包括其他用户的进程。
u | 以用户为主的格式来显示程序状况。
x | 显示所有程序，不以终端来区分。
-f | 以完整格式显示
-e | 显示所有进程。
o | 其后指定要输出的列，如user，pid等，多个列之间用逗号分隔。
-p | 后面跟着一组pid的列表，用逗号分隔，该命令将只是输出这些pid的相关数据。

- `ps aux`
- `ps -eo user,pid,%cpu,%mem,start,time,command | head -n 4`
- `ps -ef`
- `ps -fp 1`

### nice renice 改变进程优先级
`nice [-n <优先等级>][执行指令]`，其中优先等级的范围从`-20-19`，其中-20最高，19最低，只有系统管理者可以设置负数的等级。

`renice`命令主要用于为已经执行的进程重新设定nice值
- `-g` 使用程序群组名称，修改所有隶属于该程序群组的程序的优先权。
- `-p` 改变该程序的优先权等级，此参数为预设值。
- `-u` 指定用户名称，修改所有隶属于该用户的程序的优先权。

- `nice -n 19 sleep 100 &` 
- `renice -n 5 -u stephen`

### lsof 列出当前系统打开文件的工具
lsof(list opened files)，其重要功能为列举系统中已经被打开的文件，如果没有指定任何选项或参数，lsof则列出所有活动进程打开的所有文件。
选项 | 说明
:---:|:---
`-a` |  该选项会使后面选项选出的结果列表进行and操作。
`-c` | command_prefix 显示以command_prefix开头的进程打开的文件。
`-p` | PID  显示指定PID已打开文件的信息
`+d` | directory  从文件夹directory来搜寻(不考虑子目录)，列出该目录下打开的文件信息。
`+D` | directory  从文件夹directory来搜寻(考虑子目录)，列出该目录下打开的文件信息。
`-d` | num_of_fd  以File Descriptor的信息进行匹配，可使用3-10，表示范围，3,10表示某些值。
`-u` | user 显示某用户的已经打开的文件，其中user可以使用正则表达式。
`-i` |  监听指定的协议、端口、主机等的网络信息，格式为：[proto][@host|addr][:svc_list|port_list]

- `lsof /dev/null | head -n 5` #查看打开/dev/null文件的进程。
- `lsof -i:22` #查看打开22端口的进程
- `lsof -c init` #查看init进程打开的文件
- `lsof -p 1` #查看pid为1的进程(init)打开的文件，其输出结果等同于上面的命令，他们都是init。
- `lsof -u root` #查看owner为root的进程打开的文件。
- `lsof -u ^root` #查看owner不为root的进程打开的文件。
- `lsof -i tcp@192.168.220.134:22` #查看打开协议为tcp，ip为192.168.220.134，端口为22的进程。
- `lsof +d /root` #查看打开/root文件夹，但不考虑目录搜寻
- `lsof +D /root` #查看打开/root文件夹以及其子目录搜寻
- `lsof -d 0-3` #查看打开FD(0-3)文件的所有进程
- `lsof +d .` #-a选项会将+d选项和-c选项的选择结果进行and操作，并输出合并后的结果。
- `lsof -a -c bash +d .`
最后需要额外说明的是，如果在文件名的末尾存在(delete)，则说明该文件已经被删除，只是还存留在cache中。

### pgrep/pkill 进程查找/杀掉命令
选项 | 说明
:---:|:---
-d | 定义多个进程之间的分隔符, 如果不定义则使用换行符。
-n | 表示如果该程序有多个进程正在运行，则仅查找最新的，即最后启动的。
-o | 表示如果该程序有多个进程正在运行，则仅查找最老的，即最先启动的。
-G | 其后跟着一组group id，该命令在搜索时，仅考虑group列表中的进程。
-u | 其后跟着一组有效用户ID(effetive user id)，该命令在搜索时，仅考虑该effective user列表中的进程。
-U | 其后跟着一组实际用户ID(real user id)，该命令在搜索时，仅考虑该real user列表中的进程。
-x | 表示进程的名字必须完全匹配, 以上的选项均可以部分匹配。
-l | 将不仅打印pid,也打印进程名。
-f | 一般与-l合用, 将打印进程的参数。

- `pgrep sleep` #查找进程名为sleep的进程，同时输出所有找到的pid
- `pgrep -d: sleep` #查找进程名为sleep的进程pid，如果存在多个，他们之间使用:分隔，而不是换行符分隔。
- `pgrep -n sleep` #查找进程名为sleep的进程pid，如果存在多个，这里只是输出最后启动的那一个。
- `pgrep -o  sleep` #查找进程名为sleep的进程pid，如果存在多个，这里只是输出最先启动的那一个。
- `pgrep -G root,stephen sleep` #查找进程名为sleep，同时这个正在运行的进程的组为root和stephen。
- `pgrep -u root,oracle sleep` #查找有效用户ID为root和oracle，进程名为sleep的进程。
- `pgrep -U root,oracle sleep` #查找实际用户ID为root和oracle，进程名为sleep的进程。
- `pgrep -x sleep` #查找进程名为sleep的进程，注意这里找到的进程名必须和参数中的完全匹配。
- `pgrep -x sle` #-x不支持部分匹配，sleep进程将不会被查出，因此下面的命令没有结果。
- `pgrep -l sleep` #查找进程名为sleep的进程，同时输出所有找到的pid和进程名。    
- `pgrep -lf sleep` #查找进程名为sleep的进程，同时输出所有找到的pid、进程名和启动时的参数。
- `pgrep -f sleep -d, | xargs ps -fp` #查找进程名为sleep的进程，同时以逗号为分隔符输出他们的pid，在将结果传给ps命令，-f表示显示完整格式，-p显示pid列表，ps将只是输出该列表内的进程数据。

### watch Linux的实时监测命令
watch可以同时运行多个命令，命令间用分号分隔。

- `watch -d -n 1 'who'`   #每隔一秒执行一次who命令，以监视服务器当前用户登录的状况
- `watch -d -n 1 'df -h; ls -l'` #监控磁盘的使用状况，以及当前目录下文件的变化状况，包括文件的新增、删除和文件修改日期的更新等。

### free 查看当前系统内存使用状况
选项 | 说明
:---:|:---
-b | 以字节为单位显示数据。
-k | 以千字节(KB)为单位显示数据(缺省值)。
-m | 以兆(MB)为单位显示数据。
-s delay | 该选项将使free持续不断的刷新，每次刷新之间的间隔为delay指定的秒数，如果含有小数点，将精确到毫秒，如0.5为500毫秒，1为一秒。

- `total` 总计物理内存的大小。
- `used`  已使用的内存数量。
- `free`  可用的内存数量。
- `Shared`  多个进程共享的内存总额。
- `Buffers/cached`  磁盘缓存的大小。

### mpstat CPU的实时监控工具
该命令主要用于报告当前系统中所有CPU的实时运行状况。

- `mpstat 2 5`  #该命令将每隔2秒输出一次CPU的当前运行状况信息，一共输出5次，如果没有第二个数字参数，mpstat将每隔两秒执行一次，直到按CTRL+C退出。
  - `%user` 在internal时间段里，用户态的CPU时间(%)，不包含nice值为负进程  (usr/total)*100
  - `%nice` 在internal时间段里，nice值为负进程的CPU时间(%)   (nice/total)*100
  - `%sys`  在internal时间段里，内核时间(%)       (system/total)*100
  - `%iowait` 在internal时间段里，硬盘IO等待时间(%) (iowait/total)*100
  - `%irq`  在internal时间段里，硬中断时间(%)     (irq/total)*100
  - `%soft` 在internal时间段里，软中断时间(%)     (softirq/total)*100
  - `%idle` 在internal时间段里，CPU除去等待磁盘IO操作外的因为任何原因而空闲的时间闲置时间(%) (idle/total)*100
- `mpstat -P ALL 2 3`  #-P ALL表示打印所有CPU的数据，这里也可以打印指定编号的CPU数据，如-P 0(CPU的编号是0开始的)

### vmstat 虚拟内存的实时监控工具
vmstat命令用来获得UNIX系统有关进程、虚存、页面交换空间及CPU活动的信息。这些信息反映了系统的负载情况。

vmstat 可以用来确定一个系统的工作是受限于CPU还是受限于内存：如果CPU的sy和us值相加的百分比接近100%，或者运行队列(r)中等待的进程数总是不等于0，且经常大于4，同时id也经常小于40，则该系统受限于CPU；如果bi、bo的值总是不等于0，则该系统受限于内存。

`vmstat 1 3`  #这是vmstat最为常用的方式，其含义为每隔1秒输出一条，一共输出3条后程序退出。
procs  -----------memory----------   ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd      free      buff   cache   si   so     bi    bo     in   cs  us  sy id  wa st
 0  0        0 531760  67284 231212  108  0     0  260   111  148  1   5 86   8  0
 0  0        0 531752  67284 231212    0    0     0     0     33   57   0   1 99   0  0
 0  0        0 531752  67284 231212    0    0     0     0     40   73   0   0 100 0  0

`vmstat 1`   #其含义为每隔1秒输出一条，直到按CTRL+C后退出。

- 有关进程的信息有：(procs)
  - `r` 在就绪状态等待的进程数。
  - `b` 在等待状态等待的进程数。    
- 有关内存的信息有：(memory)
  - `swpd`  正在使用的swap大小，单位为KB。
  - `free`    空闲的内存空间。
  - `buff`    已使用的buff大小，对块设备的读写进行缓冲。
  - `cache` 已使用的cache大小，文件系统的cache。
- 有关页面交换空间的信息有：(swap)
  - `si`  交换内存使用，由磁盘调入内存。
  - `so` 交换内存使用，由内存调入磁盘。  
- 有关IO块设备的信息有：(io)
  - `bi`  从块设备读入的数据总量(读磁盘) (KB/s)
  - `bo` 写入到块设备的数据总理(写磁盘) (KB/s)   
- 有关故障的信息有：(system)
  - `in` 在指定时间内的每秒中断次数。
  - `sy` 在指定时间内每秒系统调用次数。
  - `cs` 在指定时间内每秒上下文切换的次数。   
- 有关CPU的信息有：(cpu)
  - `us`  在指定时间间隔内CPU在用户态的利用率。
  - `sy`  在指定时间间隔内CPU在核心态的利用率。
  - `id`  在指定时间间隔内CPU空闲时间比。
  - `wa` 在指定时间间隔内CPU因为等待I/O而空闲的时间比。   

### iostat 设备IO负载的实时监控工具
iostat主要用于监控系统设备的IO负载情况，iostat首次运行时显示自系统启动开始的各项统计信息，之后运行iostat将显示自上次运行该命令以后的统计信息。用户可以通过指定统计的次数和时间来获得所需的统计信息。

- `iostat -d 1 3`    #仅显示设备的IO负载，其中每隔1秒刷新并输出结果一次，输出3次后iostat退出。
- `iostat -d 1`  #和上面的命令一样，也是每隔1秒刷新并输出一次，但是该命令将一直输出，直到按CTRL+C退出。
- `iostat -dx 1 3` #iostat还有一个比较常用的选项-x，该选项将用于显示和io相关的扩展数据。
- `iostat -dx sda 1 3`   #指定监控的设备名称为sda，该命令的输出结果和上面命令完全相同。

列名 | 说明
:---:|:---
Blk_read/s | 每秒块(扇区)读取的数量。
Blk_wrtn/s | 每秒块(扇区)写入的数量。
Blk_read | 总共块(扇区)读取的数量。
Blk_wrtn | 总共块(扇区)写入的数量。
rrqm/s | 队列中每秒钟合并的读请求数量
wrqm/s | 队列中每秒钟合并的写请求数量
r/s | 每秒钟完成的读请求数量
w/s | 每秒钟完成的写请求数量
rsec/s | 每秒钟读取的扇区数量
wsec/s | 每秒钟写入的扇区数量
avgrq-sz | 平均请求扇区的大小
avgqu-sz | 平均请求队列的长度。队列长度越短越好。
await | 平均每次请求的等待时间。这个时间包括了队列时间和服务时间，也就是说，一般情况下，await大于svctm，它们的差值越小，则说明队列时间越短，反之差值越大，队列时间越长，说明系统出了问题。
util | 设备的利用率。如果它接近100%，通常说明设备能力趋于饱和。

### pidstat 当前运行进程的实时监控工具
pidstat主要用于监控全部或指定进程占用系统资源的情况，如CPU，内存、设备IO、任务切换、线程等。

选项 | 说明
:---:|:---
-l | 显示该进程和CPU相关的信息(command列中可以显示命令的完整路径名和命令的参数)。
-d | 显示该进程和设备IO相关的信息。
-r | 显示该进程和内存相关的信息。
-w | 显示该进程和任务时间片切换相关的信息。
-t | 显示在该进程内正在运行的线程相关的信息。
-p | 后面紧跟着带监控的进程id或ALL(表示所有进程)，如不指定该选项，将监控当前系统正在运行的所有进程。

- `pidstat -p 1 2 3 -l` #监控pid为1(init)的进程的CPU资源使用情况，其中每隔2秒刷新并输出一次，3次后程序退出。
  - %`usr` 该进程在用户态的CPU使用率。
  - %`system` 该进程在内核态(系统级)的CPU使用率。
  - %`CPU` 该进程的总CPU使用率，如果在SMP环境下，该值将除以CPU的数量，以表示每CPU的数据。
  - `CPU` 该进程所依附的CPU编号(0表示第一个CPU)。
- `pidstat -p 1 2 3 -d` #监控pid为1(init)的进程的设备IO资源负载情况，其中每隔2秒刷新并输出一次，3次后程序退出。    
  - `kB_rd/s`   该进程每秒的字节读取数量(KB)。
  - `kB_wr/s`   该进程每秒的字节写出数量(KB)。
  - `kB_ccwr/s` 该进程每秒取消磁盘写入的数量(KB)。
- `pidstat -p 1 2 3 -r` #监控pid为1(init)的进程的内存使用情况，其中每隔2秒刷新并输出一次，3次后程序退出。
  - `%MEM`  该进程的内存使用百分比。
- `pidstat -p 1 2 3 -w` #监控pid为1(init)的进程任务切换情况，其中每隔2秒刷新并输出一次，3次后程序退出。
  - `cswch/s`    每秒任务主动(自愿的)切换上下文的次数。主动切换是指当某一任务处于阻塞等待时，将主动让出自己的CPU资源。
  - `nvcswch/s` 每秒任务被动(不自愿的)切换上下文的次数。被动切换是指CPU分配给某一任务的时间片已经用完，因此将强迫该进程让出CPU的执行权。
- `pidstat -p 1 2 3 -tr` #监控pid为1(init)的进程及其内部线程的内存(r选项)使用情况，其中每隔2秒刷新并输出一次，3次后程序退出。需要说明的是，如果-t选项后面不加任何其他选项，缺省监控的为CPU资源。结果中黄色高亮的部分表示进程和其内部线程是树状结构的显示方式。
  - `TGID` 线程组ID。
  - `TID` 线程ID。  

以上监控不同资源的选项可以同时存在，这样就将在一次输出中输出多种资源的使用情况，
如：`pidstat -p 1 -dr`

### df 报告磁盘空间使用状况
`df -h`

### du 评估磁盘的使用状况
选项 | 说明
:---:|:---
-a | 包括了所有的文件，而不只是目录。
-b | 以字节为计算单位。
-k | 以千字节(KB)为计算单位。
-m | 以兆字节(MB)为计算单位。
-h | 使输出的信息更易于阅读。
-s | 只显示工作目录所占总空间。
--exclude=PATTERN | 排除掉符合样式的文件,Pattern就是普通的Shell样式，？表示任何一个字符，*表示任意多个字符。
--max-depth=N | 从当前目录算起，目录深度大于N的子目录将不被计算，该选项不能和s选项同时存在。 

- `du --max-depth=1 -h` #仅显示子一级目录的信息。
- `du -sh ./*`   #获取当前目录下所有子目录所占用的磁盘空间大小。
- `du --exclude=Te* -sh ./*`   #在当前目录下，排除目录名模式为Te*的子目录(./Test)，输出其他子目录占用的磁盘空间大小。












