# blog_demo
## 技术小白的博客
1.lftp get命令下载时指定路径

#get -O 路径 文件名

2.Linux内核编译完成后。并没有使用新内核启动，使用老内核启动，但是系统出现找不到网卡，开启不了虚拟机等一系列问题，查询后发现系统加载模块变少了。可能是编译时将各模块间的依赖关系破坏掉了，，使用depmod -a分析所有可用的模块

3. 远程拷贝和发送文件或目录
拷贝
文件和目录
文件：scp root@10.0.2.68:/root/Desktop/文件 /root/Desktop/
目录：scp -r root@10.0.2.68:/root/Desktop/目录 /root/Desktop/
发送
文件和目录
文件：scp /root/Desktop/文件 root@10.0.2.68:/root/Desktop/
目录:scp -r /root/Desktop/目录 root@10.0.2.68:/root/Desktop/

4.lspci不能使用，安装记录下。
yum -y install pciutils

5.#ethtool -s eth0 speed 100 ------》将eth0网口速率变为100Mbps
错误：“Cannot advertise speed 100”时，
要在后面添加autoneg off 设置网口不自动协商
#ethtool -s eth0 speed 100 autoneg off

6./boot/grub/device.map
(hd0) /dev/hda
grub使用的设备名称和Linux的不太一样，这是一张映射表。
(hd0,2)对应/dev/hda3；(hd0,4)对应/dev/hda5

7.当系统启动出现问题卡住了，可以使用方向键查看详细信息,查看哪一步出现问题

8.当主引导扇区的DPT分区表被破坏后，重启系统会报error 22错误

9.Linux下动态获取IP地址
dhclient eth0                //启动dhcp客户端获取IP
dhclient -r eth0            //清除ip ，针对上行的操作
windows下动态获取ip地址
ipconfig /renew           //为网卡重新从DHCP服务器上面获取新的IP地址
ipconfig /release         // 释放当前网卡获取的IP地址

10. Linux 下yum安装wireshark后执行wireshark &后台运行时出现错误
[root@cm ~]# wireshark &
[1] 3490
[root@cm ~]# -bash: wireshark: command not found
问题原因：没有安装wireshark的图形化界面
yum search wireshark（搜索匹配特定字符的rpm包）
yum install wireshark-gnome.i386（wireshark的图形界面）

宾果!!!

11.Cisco Packet Tracer安装成功使用出现问题，重装无果，
原因真的很无语，这个软件只支持英文输入法，不支持中文输入法，改成英文输入法就ok啦

12.安装Cisco Packet Tracer出现依赖关系问题，需要安装如下安装包
 


13.vsftpd不支持目录软链接的解决办法
Linux支持将一部分文件系统挂载在其他位置情况 mount --bind
mount --bind /path/share/dir /path/ftp/dir
可以把需要共享的文件夹“/path/to/share/dir”挂载到FTP目录中的一个子目录上“/path/of/ftp/subdir”。
这个目录对于vsftpd而言是一个正常文件系统的目录，于是就可以被共享了。 当不需要共享目录时，直接umount即可，就使用而言，不比link麻烦多少。

14.通过md5算法来生成密码
[root@CentOS6 ~]# grub-md5-crypt #通过md5算法来生成密码；
Password:  #键入预要设置的密码；
Retype password: #确认密码；
$1$J99TE$c7VWrcDAfB1GrVqI5.E0L. #用md5算法生成的密码

15.yum clean all 出错




16.关闭防火墙
iptables -F
setenforce 0
配置DNS服务器主从服务器的同步问题时，开始时数据第一个更新成功了，但是当主DNS服务器更改数据区域文件时，从域名服务器上面的数据区域文件还是旧的数据，尝试许久，关闭防火墙后，世界明朗了。。。。。。。当然还有一种可能，要保持主从服务器的时间同步

17.主从服务器的时间同步问题
（可能也需要关闭防火墙）
1.首先主服务器要启用xinetd服务
service xinetd start
2.开启time-dgram和time-stream服务
chkconfig time-dgram on
chkconfig time-stream on
3. 从服务器上同步时间
rdate -s 192.168.100.1 ----主服务器的IP地址
可能报错拒绝连接要执行chkconfig time on
如果出现
[root@cmclient2 yum.repos.d]# chkconfig time on
在 time 服务中读取信息时出错：没有那个文件或目录
需要安装portmap
#yum install portmap

18.Linux root目录下怎么也找不到.ssh目录
可能是没有用root帐号ssh登陆过，登录一次以后就会自动生成了

19.修改语言编码
====
[root@lichao520 yum.repos.d]# locale
LANG=en_US.UTF-8
LC_CTYPE="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_PAPER="en_US.UTF-8"
LC_NAME="en_US.UTF-8"
LC_ADDRESS="en_US.UTF-8"
LC_TELEPHONE="en_US.UTF-8"
LC_MEASUREMENT="en_US.UTF-8"
LC_IDENTIFICATION="en_US.UTF-8"
LC_ALL=
[root@lichao520 yum.repos.d]#
===
修改语言编码，临时修改
[root@lichao520 yum.repos.d]# LANG=zh_CN.GB2312
[root@teacher ssh]# LANG=zh_CN.UTF-8

===
修改语言
[root@lichao520 yum.repos.d]# vim /etc/sysconfig/i18n
[root@lichao520 yum.repos.d]# cat /etc/sysconfig/i18n
LANG="zh_CN.UTF-8"
[root@lichao520 yum.repos.d]#
====
[root@lichao520 yum.repos.d]# vim /etc/sysconfig/i18n
[root@lichao520 yum.repos.d]# cat !$
cat /etc/sysconfig/i18n
LANG="en_US.UTF-8"
[root@lichao520 yum.repos.d]#
==./
!$ 代表上一条命令使用的路径

20.启动httpd服务出错，显示端口被占用
解决方法：找到占用80端口的进程，然后杀死该进程后重启httpd服务就ok啦



21.源码编译安装的基本步骤
• 先取得源代码，解压，然后进入源代码目录
• ./configure 执行configure,建立makefile,这一步非常重要，configure程序会检测编译源代码需要的操作系统，库文件等等， 执行前最好看看目录下的README或者INSTALL文件，看看有什么需要注意的地方
• make clean 根据makefile中定义的clean操作执行一些清理工作
• make 根据makefile的内容进行编译
• make install 根据makefile中定义的install将编译生成的文件安装到相应目录

22.YUM的基本工作流程如下：
服务器端：在服务器上面存放了所有的RPM软件包，然后以相关的功能去分析每个RPM文件的依赖性关系，将这些数据记录成文件存放在服务器的某特定目录内。 
客户端：如果需要安装某个软件时，先下载服务器上面记录的依赖性关系文件(可通过WWW或FTP方式)，通过对服务器端下载的纪录数据进行分析，然后取得所有相关的软件，一次全部下载下来进行安装。
创建仓库索引文件的软件包：createrepo.noarch（未安装）

23.gurb
1、在grub中所有硬盘都识别为hd，不同的硬盘基于数字标识：如hd0（表示第一块硬盘）, hd1（第二块硬盘），同一个硬盘上的不同分区，也使用数字标识，如hd0,0（第一块硬盘上的第一个分区）；
    2、因为grub不能识别物理卷（PV），当然root不能放在逻辑卷（lv）上，所以root一般单独分区且一定为基本磁盘分区。 

24.通过md5算法来生成密码
[root@CentOS6 ~]# grub-md5-crypt #通过md5算法来生成密码； 
Password:  #键入预要设置的密码； 
Retype password: #确认密码； 
$1$J99TE$c7VWrcDAfB1GrVqI5.E0L. #用md5算法生成的密码

25.关于grub与mbr
一般，会将mbr功能的引导代码与mbr扇区混淆； 其实，grub是直接写进mbr硬盘的主引导记录中的，计算机BIOS 在启动时，按照预定的方式，将mbr内的代码加载至内存指定位置， 然后跳转至那里，mbr的代码就开始运行了！ 如果将grub写入mbr，bios就引导grub； 将winxp的引导代码写入mbr，bios就引导ntldr； 将dos的引导代码写入mbr，bios就引导dos； 总之，mbr是用来存放，由bios加载、运行的一小段代码； 代码的功能，由他们自己实现（如，在引导dos之前，先运行一个病毒， 这就是引导型病毒）； dos下执行grub.exe，其实，就是由dos执行grub.exe来完成bios加载引导代码的功能， 实现引导的； 简单地说，grub.exe 是dos下的可执行程序，由dos运行； grub是引导代码，由bios加载至内存（指定位置）开始执行； 他们最终实现的功能是一样的（都是将引导代码加载至内存指定位置，并运行）。 另外，mbr是独立于操作系统的，地位与分区表同级，所以，格式化任何分区内都影响不到他， 包括ghost备份，还原。

26.系统查看一个文件的过程：根据文件名查找inode号---》查找相应的文件的属性信息（块号）----》根据块号找到具体的数据块，调出具体的数据内容

27.软链接与硬链接的区别：
软链接：链接文件也有一个inode号。软链接的inode号中最后一行不是记录数据块的位置，而是记录所链接文件的绝对路径，然后找到被链接文件，根据被链接的文件名找到被链接文件的inode号，然后找到数据块的存放位置，然后根据块号找到数据，因此软链接可以跨分区（inode号是与分区相关 的）
硬链接：硬链接与源链接文件是同inode号，根据硬链接文件名直接找到源链接文件的inode号，找到数据存储的块号，然后找到数据，由于硬链接与源链接是同一inode号的，所以硬链接是不能跨越磁盘分区的，由于硬链接和源文件是同一inode号的，当文件被删除后，硬链接依然存在，但是源文件被删除后，由于软链接所对应的文件地的绝对路径不在了，如果在同一路径下建立一个同名文件。软链接依旧有效，但是对应的内容是新文件的内容
可以对目录作软链接但是不可以作硬链接

28.让ssh连接速度快一些，不用在进行dns进行查询了
[root@localhost ~]# vim /etc/ssh/sshd_config 
或者[root@localhost ~]# gedit /etc/ssh/sshd_config &

81 GSSAPIAuthentication no 第81行修改
122 UseDNS no 第122行修改
[root@localhost ~]# service sshd restart 刷新
[root@localhost ~]# 
Stopping sshd: [ OK ]
Starting sshd: [ OK ]
[root@localhost ~]#
===
ssh huitailang@172.16.20.252 远程登录172.16.20.252机器，使用huitailang这个用户
[root@localhost ~]# exit

29.wall 发送信息给所有登录系统的人
       mesg n 不接受消息

30.处理 /root/.vimrc 时发生错误
[root@mini ~]# vim passwd
处理 /root/.vimrc 时发生错误:
第 3 行:
E492: 不是编辑器的命令: sdfsdfsdfsdf
请按 ENTER 或其它命令继续
[root@mini ~]# vim .vimrc
处理 /root/.vimrc 时发生错误:
第 3 行:
E492: 不是编辑器的命令: sdfsdfsdfsdf
请按 ENTER 或其它命令继续
PS:以上出现的错误，是因为~/.vimrc文件内有错误的命令，导致每次调用vim程序时，该程序自动读取~/vimrc内的命令，而报错。
==========================

31.过滤出eth0设备的ip地址
[root@mini ~]# ifconfig eth0|head -2|tail -1|tr -s " "|cut -d " " -f3|cut -d: -f 2
172.16.0.155
[root@mini ~]# ifconfig eth0|grep "inet addr"|cut -d: -f2|cut -d " " -f1
172.16.0.155
以上操作为过滤出eth0设备的ip地址

32. su - 与su的区别
su -
#su - redhat     //当执行这个命令的时候表示切换到redhat用户，并且重新读取用户环境相关配置文件，具体的来说就是执行下用户家目录下.bash_profile和.bashrc文件，这个我们成为全切换
 
su
#su  redhat      //执行这个命令的时候系统不读取以上两个文件，所以我们一般称它为半切换，这样切换过去之后，redhat用户使用的依旧是此前用户的环境配置信息

33.tar包和rpm包素材网址
Tarball 封包：
.tar.gz 和 .tar.bz2 格式居多
软件素材参考：http://sourceforge.net 找源码的
RPM软件包
软件素材参考：http://rpmfind.net

一些其他的素材网站:
mirrors.163.com

httpd.apache.org httpd官网
http://nginx.org nginx 官网
找rpm包的
http://rpm.phone.net
http://rpmfind.net
http://rpmfusion.org

34.特殊权限命令  SUID与SGID
SUID：当一个设置了SUID位的可执行文件被执行时，该文件以所有者的身份运行，也就是无论谁来执行这个文件，它都拥有该所有者的特权，可以任意使用拥有者所能使用的全部系统资源。
SGID：当一个设置了SGID位的可执行文件被执行时，该文件将具有所属组的特权，任意存取整个组的所能使用的所有系统资源；若一个目录设置了SGID，则所有被复制到这个目录下的文件，其所属的组都会重设为和这个组一样，除非在复制文件时加上-p选项，才能保留原来所属的群组设置


35.如何修改Linux终端下tab键空几格
命令提示符下按tab是用来补全命令的,不跳格
如果是vi /vim ,编辑~/.vimrc ,没有这个文件的话，可以新建
set tabstop=? 这里?换成数字，如4，则跳4格

36.网卡配置文件 /etc/udev/rules.d/70-persistent-net.rules



37.录制终端会话
#script -t 2>timing.log -a output.session
Script started, file is output.session      ----开始录入
....
exit                                                             -----退出
#scriptreplay timing.log output.session --播放

38.使命令不需要输入绝对路径直接执行的方法
1.更改PATH环境变量
vi .bashrc  将命令路径加入PATH环境变量
2.作别名
vim .bashrc  alias  别名='命令绝对路径'

39.建立http yum源
从网站安装
先从网站下载安装repo文件
然后复制在/etc/yum.repos.d/下，保证这个目录下只有一个repo文件，然后进入这个repo文件更改路径（每一个都要改）

然后执行 yum clean all
yum makecache

40 .parted 磁盘分区命令
parted命令
格式：parted 磁盘或分区
GPT是一种新型磁盘模式，与我们常用的MBR磁盘相比更稳定，自纠错能力更强，一块磁盘上主分区数量不受（4个的）限制，支持大于2T的总容量及大于2T的分区（几乎没有上限，最大支持到128个分区，分区大小支持到256TB）。
对GPT磁盘分区表进行操作，我们要使用功能强大的parted命令。

例：常用的parted命令
# parted /dev/sdb
GNU Parted 1.8.1
Using /dev/sdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) mklabel gpt
将MBR磁盘格式化为GPT
(parted) mkpart primary 0 100
划分一个起始位置为0大小为100M的主分区
(parted) mkpart primary 100 200
划分一个起始位置为100M大小为100M的主分区
(parted) mkpart primary 0 -1
划分所有空间到一个分区
(parted) print

41.mysql恢复系统表的方法
使用基于GTID的主从复制的时候，出现--server-id的错误
初始化的时候删除过ibdata1文件，虽然初始化之后又重新生成了文件，但是还有五个系统表没有生成，需要重新使用系统带有的备份文件还原
1.，先删除五个系统表
drop table if exists innodb_index_stats;
drop table if exists innodb_table_stats;
drop table if exists slave_master_info;
drop table if exists slave_relay_log_info;
drop table if exists slave_worker_info;
2.再删除五个系统表的元数据和表定义文件
#cd /data/mysqldb/mysql
rm -rf innodb_index_stats*;
rm -rf innodb_table_stats*;
rm -rf slave_master_info*;
rm -rf slave_relay_log_info*;
rm -rf slave_worker_info*;
3.还原系统表
#mysql -uroot -p
#use mysql
#source /usr/local/mysql/share/mysql_system_tables.sql
还原之后记得还得重启数据库才可以生效
service mysqld restart

42.编译好php之后动态编译加载模块过程
1.先在php解压后的安装包下的ext目录下查看有没有需要加载的模块./例如gettext模块
linux php扩展安装gettext模块
[root@3350 lnampsoft]# cd php-5.3.6
[root@3350 php-5.3.6]# cd ext/
[root@3350 ext]# cd gettext/
[root@3350 gettext]# pwd
/usr/local/src/lnampsoft/php-5.3.6/ext/gettext
[root@3350 gettext]# /usr/local/webserver/php/bin/phpize 
Configuring for:
PHP Api Version:         20090626
Zend Module Api No:      20090626
Zend Extension Api No:   220090626
[root@3350 gettext]# ./configure --with-php-config=/usr/local/webserver/php/bin/php-config --with-gettext
[root@3350 gettext]# make && make install
//将这个下面的模块放到php.ini的默认目录/usr/local/webserver/php/lib/php/extensions
[root@3350 etc]# vim php.ini  添加如下内容
extension=gettext.so
[root@3350 etc]# /etc/init.d/httpd restart

43.如何 修改Linux系统时间？

（1）date -s 05/10/2009或者date -s 10:01:02
（2）使用ntpdate pool.ntp.org让他自动同步标准时间


（3）rdate -s IP  前提是此IP地址所在主机开启了服务

44.Linux手工释放内存
echo 3 > /proc/sys/vm/drop_caches


45.awk同时指定多个分隔符
 awk 'BEGIN{ FS=":|/";}{print $5}' filename  同时指定：和/为分隔符
实验结果
[root@cmserver rh]# cat a
111:3434/j3434/3434:34545
[root@cmserver rh]# awk 'BEGIN{ FS=":|/";}{print $5}' a
34545

46.使用二进制日志恢复数据库
1.使用pos号
mysqlbinlog --start-position="120" --stop-position="232" mysql-bin.000001  | mysql -uroot –p 开始恢复
2.使用时间点
 mysqlbinlog –start-datetime=”2016-08-13 21:57:55” –stop-datetime=”2016-08-13 21:58:07” mysql-bin.000001 | mysql –uroot –p   开始恢复数据

47.shell终端查看mysql slave状态
mysql -u -p  -h127.0.0.1 -P3306 -e "show status;" 

48.记一次心塞的排错
买了腾讯云的主机，配置好web服务后，浏览器一直不能打开web服务的首页，发现telnet 公网ip 80端口也不能连接上，但是云主机的80端口是打开的，多方面排错无果后，突然发现telent 内网ip 80连接上了，，，，，意思就是以公网Ip不能连接80端口，最后想起来买主机的时候选择的是只打开22号端口。。。。。。。。将安全组改为全部端口打开就瞬间连接成功了，，，，排错就是这样的心塞啊

49.编辑vim配置文件，使编写python代码更加方便
编辑~/.vimrc
set autoindent
set smartindent
set expandtab
set tabstop=4 
set shiftwidth=4
set softtabstop=4
“”
50.在配置虚拟机的时候，在将虚拟机的网卡连接方式从nat改为桥接模式的时候，重启网卡时出现问题
Restarting network (via systemctl):Job for network.service faild because the control process exited with error code. see "systemctl status network.service" and journalctl -xe" for details
上网搜索了一下，尝试将网卡配置文件备份，删除，重写，第一次启动并没有成功，，，但是过了几分钟之后，重启成功了。。。。。。。I don't know why  摊手~~~~

51.Public key for mysql....rpm is not installed
解决方案：
此时导入rpm的签名信息即可，用root登录，执行下面命令（我的linux版本是centos 5.1）
[root@freeman opt]# rpm --import/etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-5
如果是其它版本，例如redhat，执行以下命令
rpm --import/etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
OK，问题解决

52.PHP Parse error:  syntax error, unexpected '[' in /var/www/html/zabbix/index.php on line 29
安装zabbix时候访问web页面出现问题，，原因是因为php版本太低，要升级为5.6版本

53. zabbix下载地址
http://jaist.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/

54.转换时间戳格式
时间转换成数值格式： date -d "2014-09-16" +%s
数值转换为时间格式：date -d @1402243200 "+%Y-%m-%d"

55.打开windows的服务
CMD下敲入：services.msc

弹出windows所有本地服务


56.shell脚本中可以使用printf来定义输出结果格式
printf "%d$num"

57.Windows下安装mysql出错
记得将mysql安装在全英文或者数字目录下，不能安装在中文目录下，否则最后一步安装的时候会出现my-template.ini could not be processed and written to XXX\my.ini.Error code-1
ps：mysql5.0 Windows版本在班级百度云有安装包

58.shell中输出一个指定范围内的随机数
rnumber=$(((RANDOM%(max-min+divisibleBy))/divisibleBy*divisbleBy+min)）
rnumber:输出的随机数
max:指定范围的最大值
min:指定范围的最小值
divisbleBy:指定范围内的间隔，例如是3的倍数，divisbleBy就是3

59,awk同时指定多个分隔符
cat /etc/passwd|awk 'BEGIN{FS=":|/"}{print $1,$7}'      //同时指定冒号和斜杠作为分隔符

60.pip install 时出现问题
/usr/bin/ld: cannot find -lncurses
collect2: ld 返回 1
首先到usr/lib/目录下寻找libncurses开头的文件
1.如果没有那就是缺少库文件
解决方法：
$ sudo yum search ncurses-
有这样一个结果：
[root@i-FBC8F232 vhost]# yum search ncurses-
Loaded plugins: fastestmirror, security
Loading mirror speeds from cached hostfile
========================== N/S Matched: ncurses- ===========================
ncurses-base.x86_64 : Descriptions of common terminals
ncurses-devel.i686 : Development files for the ncurses library
ncurses-devel.x86_64 : Development files for the ncurses library
ncurses-libs.i686 : Ncurses libraries
ncurses-libs.x86_64 : Ncurses libraries
ncurses-static.x86_64 : Static libraries for the ncurses library
ncurses-term.x86_64 : Terminal descriptions
#yum install ncurses* -y 

2.如果有这个文件，那么就不缺少库，只是文件类型的问题
查看此库应该不是以so结尾的，输入下面的代码，创建连接即可
sudo ln -rf libncurses(*****) libncurses.so

61.mysql数据库乱码 问题

如图，mysql数据库的编码为latin1而不是utf8，可能会导致部分数据访问乱码问题set
先修改部分编码格式

还有部分修改不成功，需要修改mysql配置文件 /etc/my.cnf   ---要确定是这个配置文件，可以先
find / -name /etc/my.cnf  查看是否在默认的配置文件顺序中是否还有其他的my.cnf配置文件，先关闭mysql数据库 service mysqld stop ，然后修改如下

[mysql.server]
user=mysql
basedir=/var/lib
default-character-set=utf8
init_connect='SET NAMES utf8'

[mysqld_safe]
err-log=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
[client]
default-character-set=utf8

[mysqld]
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
default-character-set=utf8
红色代码为添加部分,保存修改后启动mysql数据库，查看编码格式如下

手动修改方法：
1、
如何修改数据库成utf8的： 
mysql>alter database abc character set utf8; 
修改表默认用utf8： 
mysql> alter table type character set utf8; 
修改字段用utf8： 
mysql> alter table type modify type_name varchar(50) CHARACTER SET utf8; 

62,[python sqlite3]ImportError : No module named _sqlite3
【解决办法】
需要安装 sqlite-devel库，再重新编译安装Python
yum search sqlite-devel
yum install sqlite-devel.x86_64

63. pip install readline出错
gcc: readline/libreadline.a: No such file or directory
gcc: readline/libhistory.a: No such file or directory

解决：
只需要
yum -y install readline-devel
yum -y install patch

64，将整齐的excel表格转换为逗号分割的纯文本
另存为CSV（以逗号分隔），文件名后缀为txt


65.Received value [(Not all processes could be identified, non-owned process in....)   ---zabbix监控项错误
zabbix使用自己的脚本添加自定义key失败，出现错误。发现错误是脚本里面使用了netstat -p命令，默认脚本使用zabbix用户启动的，zabbix用户并没有netstat命令的使用权限，所以提取脚本的时候显示为空，从网上搜来的资料：
场景：因为使用了netstat -p参数。
权限问题，zabbix_agentd是zabbix用户启动的，默认不能执行netstat -p等命令，导致从服务器取到的自动发现脚本为空
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
解决方法 ：
chmod +s /bin/netstat
chmod +s 是什么意思
为了方便普通用户执行一些特权命令，SUID/SGID程序允许普通用户以root身份暂时执行该程序，并在执行结束后再
或者在netstat命令前面添加sudo命令，也是可行的

66.grep查找与一个变量值匹配的行
在写脚本的时候发现在命令行下
cat filename|grep $var这条命令执行是没有问题的，但是在写入脚本的时候读取不到内容最后在网上查找了一下资料发现了原因：
文本是从windows下传输过来的，与Linux的文本格式不符，将文本改为unicode和utf-8格式都读取不到内容（grep读取不到变量的值，echo可以。。。。）在vim下设置了一下文件格式后可以正常查询了，
# vim filename
:set ff=unix
:set nobomb
:wq!
备注：unicode格式改完后vim进去后会是比较乱码，但是cat查看还是 正常的显示
          utf-8格式显示内容依旧正常、

67.Python连接mysql数据库，安装MySQL-python包
在Python代码
conn = MySQLdb.Connect(host='localhost', user='root', passwd='root', db='python') 中加一个属性：
 改为：
conn = MySQLdb.Connect(host='localhost', user='root', passwd='root', db='python',charset='utf8')
charset是要跟你数据库的编码一样，如果是数据库是gb2312 ,则写charset='gb2312'。

下面贴一下常用的函数：

然后,这个连接对象也提供了对事务操作的支持,标准的方法
commit() 提交
rollback() 回滚

cursor用来执行命令的方法:
callproc(self, procname, args):用来执行存储过程,接收的参数为存储过程名和参数列表,返回值为受影响的行数
execute(self, query, args):执行单条sql语句,接收的参数为sql语句本身和使用的参数列表,返回值为受影响的行数
executemany(self, query, args):执行单挑sql语句,但是重复执行参数列表里的参数,返回值为受影响的行数
nextset(self):移动到下一个结果集

cursor用来接收返回值的方法:
fetchall(self):接收全部的返回结果行.
fetchmany(self, size=None):接收size条返回结果行.如果size的值大于返回的结果行的数量,则会返回cursor.arraysize条数据.
fetchone(self):返回一条结果行.
scroll(self, value, mode='relative'):移动指针到某一行.如果mode='relative',则表示从当前所在行移动value条,如果 mode='absolute',则表示从结果集的第一行移动value条.

68.pip install 安装出错
 SNIMissingWarning
/usr/lib/python2.6/site-packages/pip-9.0.1-py2.6.egg/pip/_vendor/requests/packages/urllib3/util/ssl_.py:122: InsecurePlatformWarning: A true UANSSLContext object is not available. This prevents urllib3 from configuring SSL appropriately and may cause certain SSL connections to fail. You can upgrade to a newer version of Python to solve this. For more information, see https://urllib3.readthedocs.io/en/latest/security.html#insecureplatformwarning.

这个错误第一感觉是openssl没有安装。但是使用本地yum安装完成之后还是有问题，最后使用网络yum源重新安装了openssl之后。pip install就成功了，根据上面提示到https://urllib3.readthedocs.io/en/latest/security.html#insecureplatformwarning查看之后，说要 pip install urllib3

69.一些windows下的python模块下载地址，.whl格式，下载下来 pip install 文件名安装就可以了
http://www.lfd.uci.edu/~gohlke/pythonlibs/

70.python import module出错，当执行python程序显示找不到模块的时候，
第一反应是不是脚本的名称和模块名称冲突了，第二检查。sys.path的路径中能不能找到该模块
import sys
print sys.path


python模块的搜索路径

模块的搜索路径都放在了sys.path列表中，如果缺省的sys.path中没有含有自己的模块或包的路径，可以动态的加入（sys.path.apend）即可。下面是sys.path在Windows平台下的添加规则。
1、sys.path第一个路径往往是主模块所在的目录。在交互环境下添加一个空项，它对应当前目录。
2、如果PYTHONPATH环境变量存在，sys.path会加载此变量指定的目录。
3、我们尝试找到Python Home，如果设置了PYTHONHOME环境变量，我们认为这就是Python Home，否则，我们使用python.exe所在目录找到lib/os.py去推断Python Home。
如果我们确实找到了Python Home，则相关的子目录（Lib、plat-win、lib-tk等）将以Python Home为基础加入到sys.path，并导入（执行）lib/site.py，将site-specific目录及其下的包加入。
如果我们没有找到Python Home，则把注册表Software/Python/PythonCore/2.5/PythonPath的项加入sys.path（HKLM和 HKCU合并后加入），但相关的子目录不会自动添加的。
4、如果我们没有找到Python Home，并且没有PYTHONPATH环境变量，并且不能在注册表中找到PythonPath，那么缺省相对路径将加入（如：./Lib;./plat-win等）。

71.删除一个目录的软链接文件时，文件名后面一定不能加/ ,否则会将源目录文件删除掉，不加/则删除的是链接文件，建议删除用rm不加 -rf的参数，如果提示是删除目录则不能继续删除，否则会将源目录文件删除
，如果提示的是删除链接文件，则正确，选择继续删除

72，wget获取网站指定目录下的所有文件
wget -c -r -np -k -L -p www.xxx.org/pub/path/
在下载时。有用到外部域名的图片或连接。如果需要同时下载就要用-H参数。
wget -np -nH -r --span-hosts www.xxx.org/pub/path

73，下载软件包，制作本地yum源
先将系统镜像作为本地yum源，然后将下载下来的软件包（注意要下载好所有的依赖包，可以结合wget下载原网络yum源下的所有软件包）拷贝到本地yum源下Packages文件夹下，然后使用createrepo工具生成依赖关系
#cd /myyum/     ####一定要进入 /myyum目录（本地yum源目录）
#createrepo /myyum/       ####/myyum/为yum源根目录
74，虚拟机选择以U盘启动
可以先在虚拟机添加一个硬盘，选择u盘，然后开机启动选择刚刚添加的那块硬盘启动

75，yum安装httpd服务，启动后怎么都访问不了，显示permission denied和403 forbidden
最后才发现是防火墙和Selinux 的原因

https://www.google.com/patents/CN104375867A?cl=zh

76.安装redis报错，显示tcl版本不对，yum安装tcl解决

77. 编译安装setuptools报错
下载setuptools的源码包，使用python安装 python setup.py install报错  ，显示没有zlib模块
解决方法
#yum install zlib zlib-devel -y
然后再重新编译python ,进入python的源码包，（python是源码编译安装的）
#make  && make install 
然后重新安装setuptools就可以了

78.关闭uwsgi服务
killall -s INT /usr/local/python2.7/bin/uwsgi


79.windows共享文件到Linux主机
今天在使用CentOS release 5.2 (Final)，mount其它服务器的文件目录时
# mount -t smbfs -o username="administrator",password="" //192.168.1.100/cp /mnt/ntfs
提示出错：
mount: unknown filesystem type 'smbfs'
查资料后，说smbfs改为cifs了，所以要用下面的方法：
# mount -t cifs -o username="administrator",password="" //192.168.1.101/cp /mnt/ntfs

成功！！

80.#同步系统时间

ntpdate cn.pool.ntp.org

hwclock --systohc

echo -e "0 1 * * * root/usr/sbin/ntpdate cn.pool.ntp.org > /dev/null"  >>/etc/crontab

service crond restart

81.cobbler安装报错
[root@server1 CentOS-6.8-x86_64]# cobbler import --path=/var/www/html/os/CentOS-6.8-x86_64/ --name=CentOS-6.8 --arch=x86_64
No signature matched in /var/www/cobbler/ks_mirror/CentOS-6.8-x86_64
!!! TASK FAILED !!!
原因是cobbler不识别你指定的镜像，最后才发现是自己重启过机器，之前的挂载不在了，指定的目录是个空目录

82. ping报错： From 183.59.14.250 icmp_seq=236 Packet filtered
防火墙禁止了icmp包

83. centos liveCD liveDVD netinstall minimal DVD1 DVD2 版本区别
LiveCD 和 LiveDVD 是可以直接光盘运行系统，但不能安装，两者差别在于容量大小，dvd包含的软件要多一些。
netinstall 是用于网络安装和系统救援的镜像文件。
minimal 这个镜像文件用于安装一个非常基本的 CentOS系统,包含了一些基本所需的最小安装包。
DVD 镜像文件包含了完整的发布版，可以用于安装完整的 CentOS 系统。
CentOS-6.3-x86_64-bin-DVD1.iso  完整的系统
CentOS-6.3-x86_64-bin-DVD2.iso   完整的安装包
 
84.  cobbler的snippet文件理解 

#set http_server = $getVar("http_server","")
#set distro = $getVar("distro_name","")
#set mac = $getVar("system_name","")
#if 'suse' not in $distro.lower()
wget "http://$http_server/cobbler_wrap/record/process/mac=$mac&type=software&starttime=1&status=1" -O /dev/null
wget "http://$http_server/cobbler_wrap/record/process/mac=$mac&type=yum&starttime=1&status=1" -O /dev/null
#else
      <script>
        <chrooted config:type="boolean">true</chrooted>
        <interpreter>shell</interpreter>
        <filename>software_install_start</filename>
        <source>
<![CDATA[
wget "http://$http_server/cobbler_wrap/record/process/mac=$mac&type=software&starttime=1&status=1" -O /dev/null
wget "http://$http_server/cobbler_wrap/record/process/mac=$mac&type=yum&starttime=1&status=1" -O /dev/null
]]>
        </source>
      </script>
#end if
注意“#”是同样有效的，是cheetah的语法格式
#set   设置一个临时变量，文件使用完，变量失效
#if     if循环
#end if  结束if循环
<![CDATA[......]]  CDATA部分中的所有内容都会被xml解析器忽略
也可以直接写成普通的脚本，cobbler会将脚本直接写到ks文件中执行

85. 使用虚拟机做cobbler实验的时候由于装机实验次数太多，磁盘进行多次擦写操作，造成磁盘损坏，导致之后每一次的装机（只有虚拟机文件在之前的那个磁盘分区下）装机都会显示磁盘损坏的信息，如    ？？？？？这个问题原因还有待解决


86.shell中字符实现大小写转换
1.用tr
UPPERCASE=$(echo $VARIABLE | tr '[a-z]' '[A-Z]')   (把VARIABLE的小写转换成大写)
LOWERCASE=$(echo $VARIABLE | tr '[A-Z]' '[a-z]')   (把VARIABLE的大写转换成小写)
2, 用typeset
 typeset -u VARIABLE  (把VARIABLE的小写转换成大写)
 typeset -l  VARIABLE  (把VARIABLE的大写转换成小写) 
 例如：typeset -u VARIABLE
           VARIABLE="True"
            echo $VARIABLE
            输出为TRUE

87. # %pre部分脚本（系统安装前执行）系统在解析 ks.cfg 文件之后立即运行，而且必须以 %pre 命令开头。注意，你在 %pre 部分可以访问网络；然而，名称服务（name service）在此时还没有被配置，因此只有 IP 地址才能奏效。

88.vim删除空格，回车，换行符
:%s/\s//g     删除空格
:%s/\r//g     删除DOS文件中的回车符 ‘^M’
:%s/\r/\r/g   将^M转换为真正的换行符
:%s/\n//g     删除换行符

89.导入cobbler镜像的时候发现存放cobbler根目录的磁盘空间不够用，需要将cobbler目录迁移到其他足够大的分区下，然后建立软链接访问
/var/www/cobbler 目录必须具有足够容纳 Linux安装文件的空间。如果空间不够，可以对/var/www/cobbler目录进行移动，建软链接来修改文件存储位置。
例如：
# ln -s /home/cobbler /var/www

90.wget下载设置断点续传，限制速度（有时候要从外地机房下载镜像，通过专线，上面可能有业务，下载速度过快会影响到业务，需要将下载速度限制在2M以内）
1.断点续传，只需要添加-c 参数即可

wget -c http://mirrors.163.com/ubuntu-releases/9.10/ubuntu-9.10-desktop-amd64.iso
2.限速下载，只需要添加–limit-rate=300k合理参数即可

wget -c --limit-rate=300k http://mirrors.163.com/ubuntu-releases/9.10/ubuntu-9.10-desktop-amd64.iso 

scp限速 -l参数表示限速，单位是字节
scp -l 20000
3，递归下载某网站的某目录下的所有内容
      wget -c -r -np ftp://name:pw@server/dir/ 实现从先前虚拟主机ftp上转移某目录下所有文件到新VPS上。
所用参数 -c 断点续传（备注：使用断点续传要求服务器支持断点续传），-r 递归下载（目录下的所有文件,包括子目录），-np 递归下载不搜索上层目录

91,一次性下载所有依赖的包
yumdownloader --resolve --destdir /root/mypackages/ httpd
92.下次再需要离线安装软件的时候，先建一个最小化的相同版本的虚拟机，使用yumdownloader下载下来所有的依赖包，没有的包可以去官方yum源找，或者epel源下载，

93.selinux的三种模式
•enforcing：强制模式，代表 SELinux 运作中，且已经正确的开始限制 domain/type 了；
•permissive：宽容模式：代表 SELinux 运作中，不过仅会有警告讯息并不会实际限制 domain/type 的存取。这种模式可以运来作为 SELinux 的 debug 之用；
•disabled：关闭，SELinux 并没有实际运作。
关闭SELinux的方法：
修改/etc/selinux/config文件中的SELINUX="" 为 disabled ，然后重启。
如果不想重启系统，使用命令setenforce 0
注：
setenforce 1 设置SELinux 成为enforcing模式
setenforce 0 设置SELinux 成为permissive模式

94.# service cobblerd status
cobblerd 已死，但是 subsys 被锁    
虚拟机异常关闭之后，重启之后启动cobbler失败，查看cobbler状态显示 cobblerd 已死，但是 subsys 被锁  ，也找不到这个进程，查看/var/log/cobbler/cobbler.log
Sat Aug 26 05:43:59 2017 - INFO | mkdir: /var/www/cobbler/rendered
Sat Aug 26 05:43:59 2017 - INFO | Exception occured: <type 'exceptions.OSError'>
Sat Aug 26 05:43:59 2017 - INFO | Exception value: [Errno 2] No such file or directory: '/var/www/cobbler/rendered'
Sat Aug 26 05:43:59 2017 - INFO | Exception Info:
  File "/usr/lib/python2.6/site-packages/cobbler/utils.py", line 1301, in mkdir
    return os.makedirs(path,mode)
   File "/usr/lib64/python2.6/os.py", line 157, in makedirs
    mkdir(name, mode)
显示创建 /var/www/cobbler/rendered失败。查看这个/var/www/cobbler这个文件发现不存在，才想起来之前cobbler的根目录内容是挂载在另一个磁盘分区的，开机之后，系统没有自动挂载分区，找不到这个文件，重新挂载磁盘分区，之后，启动成功，，再次证明有问题看日志，就能解决大部分的问题

95.cobbler自动装机，修改centos7的内核参数，更改网卡为eth0
cobbler profile edit --name=CentOS-7.1-x86_64 --kopts='net.ifnames=0 biosdevname=0'

96.当 GPT 分区的盘在老的只支持 mbr 的工具下查看时，也会读到分区信息，只不过看到的是只有一个分区，这是为了防止用户误认为盘是空盘。当大于 2TB 盘安装安装 CentOS 7 时报错
Your BIOS-based system needs a special partition to boot from a GPT disk label. To continue, please create a 1MiB 'biosboot' type partition
对于这个问题，解决办法是在 kickstart 文件中添加
part biosboot --fstype=biosboot --size=1
备注：之前安装了windows系统之后再重装centos系统可能也会出现相同报错，可以做相同处理

97.ipmitool设置远程主机pxe网卡启动以及重启远程机器
'ipmitool -I lanplus -H {0} -U {1} -P {2} chassis bootdev pxe'
'ipmitool -I lanplus -H {0} -U {1} -P {2} chassis power reset'

98.VMvare安装fedora失败，原因VMware不支持ubuntu和fedora最新版的安装，升级到最新版本也不支持
看支持fedora22及以下

99.fedora22安装http后无法访问，一开始就是查找防火墙的问题，按照之前redhat的命令
#systemctl stop iptables.service
#setenforce 0
还是不能访问，上网谷歌后发现fedora22有一个防火墙的命令，设置http和https通过，重新加载防火墙规则之后，就可以访问了
#firewall-cmd  --permanent --add-service=http
#firewall-cmd --permanent --add-service=https
#firewall-cmd --reload

100.将查询结果作为临时表查询
select IPMI_IP,count(IP) from (select * from automsg where id >537) as tmp group by IPMI_IP;

101.一个python脚本，本来都运行好好的，然后写了几行代码，而且也都确保每行都对齐了，但是运行的时候，却出现语法错误： 
IndentationError: unindent does not match any outer indentation level
使用sublime显示所有字符之后发现，第一行是制表符，而剩下行前面是空格，导致看着是对齐的，其实字符不一致
设置sublime text3显示所有空白字符
路径：Preferences->Settings-User 增加一项：
"draw_white_space": "all",

102. Linux修改php.ini文件之后重新加载配置文件
PHP的一般默认安装目录是：
/usr/local/php/
我们用php-fpm来进行重新加载配置文件（如php.ini）：
/usr/local/php/sbin/php-fpm reload
注：/usr/local/php/sbin/php-fpm还有其他参数，包括：start|stop|quit|restart|reload|logrotate。
使用PHP-FPM来控制PHP-CGI的FastCGI进程
　　/usr/local/php/sbin/php-fpm {start|stop|quit|restart|reload|logrotate}
　　--start 启动php的fastcgi进程
　　--stop 强制终止php的fastcgi进程
　　--quit 平滑终止php的fastcgi进程
　　--restart 重启php的fastcgi进程
　　--reload 重新平滑加载php的php.ini
　　--logrotate 重新启用log文件
提示：很多时候（老版本、没有PHP-FPM）直接重启http服务器即可，比如重启apache、IIS、nginx即可

103：mysql报以下错的解决方法ERROR 1030 (HY000): Got error 28 from storage engine
出现此问题的原因：临时空间不够，无法执行此SQL语句解决方法：将tmpdir指向一个硬盘空间很大的目录即可

104.django使用json.loads时出现TypeError：expected string or buffer
dumps接口的功能能将python中的dict转换为json字符串
loads接口的功能是将python的json字符串转化为python中的dict

105.cobbler dhcp分配地址异常
测试自动装机时在cobbler上面配置dhcp，dhcp怎么都获取不到，抓包，发现有dhcp请求但是没有应答信息，排查了许久，最后将之前注释掉的dhcp段的配置加上之后，dhcp获取正常了

106.对包含多个字典的列表按照字典的某一个键值排序
比如
data=[{'name':'minchen1','num':'1'},{'name':'minchen2','num':'3'},{'name':'minchen3','num':'2'}]
降序
print (sorted(data,key=lambda i:i['num'],reverse=True))
[{'name': 'minchen2', 'num': '3'}, {'name': 'minchen3', 'num': '2'}, {'name': 'minchen1', 'num': '1'}]
升序
print (sorted(data,key=lambda i:i['num'],reverse=False))
[{'name': 'minchen1', 'num': '1'}, {'name': 'minchen3', 'num': '2'}, {'name': 'minchen2', 'num': '3'}]

107.解决UnicodeEncodeError: 'ascii' codec can't encode characters in position问题
今天把一个列表转换成字符串输出的时候出现了UnicodeEncodeError: 'ascii' codec can't encode characters in position 32-34: ordinal not in range(128)问题
解决方法1：
在开头加上
import sys
reload(sys)
sys.setdefaultencoding( "utf-8" )

解决方法2：
使用cmd运行python程序，能正常显示结果

108.dell 机器ipmitool命令查看网卡信息
[root@IIM_SERVER ~]# ipmitool -I lanplus -H 10.5.181.100 -U root  -P calvin delloem mac list
System LOMs
NIC Number    MAC Address        Status
2        80:18:44:e7:33:42    Enabled
3        80:18:44:e7:33:43    Enabled
0        80:18:44:e7:33:40    Enabled
1        80:18:44:e7:33:41    Enabled
iDRAC8 MAC Address 50:9a:4c:65:fd:6f

109.将windows格式的文档vi打开异常，执行脚本显示格式错误，因为vi打开默认打开为dos文件，要转换为unix格式文件
 在Windows系统下编辑的文件，换行符回车的格式为'\r\n'，在linux系统下，回车的格式为'\n'，在Windows下编辑的文本文件在上传至linux服务器时，回车'\r\n'就显示成^M+'\n'。
　　在Windows环境下，用ultraedit或者notepad plus都有相应的选项可以将Windows下的文本格式文件转换成unix格式文件。比如：notepad plus中“编辑”—>“档案格式转换”—>“转换为UNIX格式”。
　　在Linux下面，一般有三种方式来转换文件：
　　1.使用dos2unix工具
　　一般的分发版本中都带有这个小工具（如果没有可以根据下面的连接去下载），使用起来很方便: 
　　$ dos2unix myfile.txt 
　　该命令会去掉行尾的^M。
　　2.用vi修改文件
如果文件是在windows环境下创建并编辑的，文件中所有的换行符都是'\r\n'，vi会在打开文件时识别出该文件是dos格式，此时不会显示^M，在命令行模式下输入:set fileformat=unix，然后保存即可。
如果文件中的换行符有些是为'\r\n'，有些是'\n'，在vi显示文件时，为'\r\n'将会显示为^M然后换行。这种情况可以直接用vi的替换功能。
":%s/^M//g"   替换所有的^M
":%s/^M$//g"   替换行尾的^M
":%s/^M/[ctrl-v]+[enter]/g"  将^M替换成回车
":%s/^M/\r/g"   将^M替换成回车
在命令中，^M的输入方式是Ctrl+v，Ctrl+m，是一个字符，不是两个字符。（^I 制表符也是如此。）
替换后，如果去看那些内容，你会发现还没替换掉，但是如果你:x 保存退出后，再次用vi打开就发现他们已经完全被替换掉了。
　　如果碰到有提示：E486: Pattern not found: ^M，单文件中实际存在^M，比如用"cat -v"或"cat -A"查看时，这种情况应该是因为文本中每行都是'\r\n'结尾，vi自动用dos模式打开，这样就看不到^M，这时候直接用:set fileformat=unix就可以了。
　　3.用sed命令修改
　　$ sed -e 's/^M/\n/g' myfile.txt
　　^M = Ctrl+v,Ctrl+m


