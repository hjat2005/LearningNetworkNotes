# 影响性能的因素

1. CPU
2. 内存访问速度
3. 中断系统

# 工具利器
1. perf
2. top
3. vmstat
4. mpstat
5. pidstat
6. objdump
7. memtest
8. iperf
9. netperf
10. 查询网络相关信息的脚本

# 高性能编码方式


# 分析性能的基本方式



# 网络性能分析案例
## 网络驱动的收发基本流程
在开始描述网络的性能分析前，先简单讲一下网卡驱动程序的收发流程。

### 接收流程
Lemon的一个网口有16个硬件接收队列组成，每个队列深度为1024， 当网卡接收到数据时，通过多元组的RSS算法来决定分发到哪个硬件队列，并填充对应软件预先分配好的BD，一般每个控制器都会有可配置的中断聚合参数，也就是软件可以配置中断上报的频率，网卡中断聚合参数分两种，一种是中断超时，一种是中断个数（这两者的配置都是基于网卡的而不是队列的），只要其中一个条件满足后，硬件就会上报中断上报，上报中断后，先屏蔽相应的硬中断，启用软中断轮询解析BD内容，转换为协议栈识别的skb数据，向上层传递，轮询完成后，就打开硬中断，等待新的数据，如此反复循环。



### 发送流程


## UDP性能不达标问题定位
最刚开始解决的是多进程UDP性能达不到82599的标准，而且在自动化环境还发现10进程的性能还比5进程还低，甚至降到连1进程都达不到的水平，既然是XGE和82599、10线程和5线程性能的对比问题，那就测试在同等条件下这两组环境的各项对比数据，如下：

通过上述的对比数据，得出结论：无论是10进程、5进程还是3进程，XGE都是CPU核6的si占用率都100%，使用perf top命令查看热点函数，都是tx回收资源 而


tips：
期间为了加快测试速度，写了一个shell脚本进行快速
- 在跑相同进程数量的netperf，使用perf和top工具分析差异，有个明显的差异点，通过top信息看进行  top命令和82599做对比


Supposedly, you are working on Ubuntu 12.04 or newer version!

Firstly, to download estuary with following commands:

$mkdir -p ~/bin; sudo apt-get update; sudo apt-get upgrade -y; sudo apt-get install -y wget git
$wget -c http://download.open-estuary.org/AllDownloads/DownloadsEstuary/utils/repo -O ~/bin/repo
$chmod a+x ~/bin/repo; echo 'export PATH=~/bin:$PATH' >> ~/.bashrc; export PATH=~/bin:$PATH; mkdir -p ~/open-estuary; cd ~/open-estuary
$repo abandon master
$repo forall -c git reset --hard
$repo init -u "https://github.com/open-estuary/estuary.git" -b refs/tags/v2.3-rc1 --no-repo-verify --repo-url=git://android.git.linaro.org/tools/repo 
$false; while [ $? -ne 0 ]; do repo sync; done
Secondly, you can build the whole project with the default config file as following command:

$sudo ./estuary/build.sh --file=./estuary/estuarycfg.json --builddir=./workspace
or build specified platform and distribution such as:

$sudo ./estuary/build.sh -p QEMU -d Ubuntu
 To try more different platform or distributions based on estuary, please get help information about build.sh as follow:

 $./estuary/build.sh -h
 More detail user guide about this project, please refer to Estuary User Manual.

 If you just want to quickly try it with binaries, please refer to our binary Download Page to get the latest binaries and documentations for each corresponding boards.

 Accessing from China: ftp://117.78.41.188

 Accessing from outside-China: http://download.open-estuary.org/
