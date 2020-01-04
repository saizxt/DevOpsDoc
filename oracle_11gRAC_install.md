# oracle_11gRAC_install

1. 安装软件包
[root@lgh01 ~]# for i in binutils compat-gcc-34 compat-libstdc++-296 control-center \
gcc gcc-c++ glibc glibc-common glibc-devel libaio libgcc \
libstdc++ libstdc++-devel libXp make openmotif22 setarch \
compat-libstdc++-33 libaio-devel sysstat unixODBC unixODBC-devel
do
rpm -q $i &>/dev/null || F="$F $i"
done ;echo $F;unset F
compat-gcc-34 compat-libstdc++-296 libXp openmotif22 setarch compat-libstdc++-33 libaio-devel unixODBC unixODBC-develcompat-gcc-34 compat-libstdc++-296 gcc gcc-c++ glibc-devel libstdc++-devel libXp openmotif22 setarch compat-libstdc++-33 libaio-devel unixODBC unixODBC-devel
[root@lgh01 ~]# compat-gcc-34 compat-libstdc++-296 libXp openmotif22 setarch compat-libstdc++-33 libaio-devel unixODBC unixODBC-devel
[root@lgh01 ~]# yum -y install compat-gcc-34 compat-libstdc++-296 libXp openmotif22 setarch compat-libstdc++-33 libaio-devel unixODBC unixODBC-devel

2. 修改主机hosts信息
172.21.0.141    lgh01
172.21.0.142    lgh02
172.21.0.143    lgh01-vip
172.21.0.144    lgh02-vip
172.21.0.145    lgh-scan
10.21.0.141    lgh01-priv
10.21.0.142    lgh02-priv

3. 调整内核参数 （具体根据系统内存的配置调整）
kernel.shmmax = 4398046511104
kernel.shmall = 1073741824
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
net.ipv4.ip_local_port_range = 9000 65500
net.core.rmem_default = 262144
net.core.rmem_max = 4194304
net.core.wmem_default = 262144
net.core.wmem_max = 1048576
fs.aio-max-nr = 1048576
fs.file-max = 6815744
/sbin/sysctl -p

4. 修改两节点的ORACLE用户限制
vi /etc/security/limits.conf 末尾添加：
oracle soft nofile 2047
oracle hard nofile 65536
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft stack 10240
oracle hard stack 32768

5. 修改两节点的/etc/pam.d/login未尾加
session required /lib64/security/pam_limits.so

6. 关闭两节点的防火墙和selinux
service iptables stop
chkconfig iptables off
setenforce 0
getenforce
vi /etc/selinux/config 确保以下内容
SELINUX=disabled

7. 两节点都停用ntp服务
[root@ ~]#
service ntpd stop
chkconfig ntpd off
mv /etc/ntp.conf /etc/ntp.conf.bak
rm -rf /etc/ntp.conf

8. 两节点建立必要的组和用户
[root@ ~]#
groupadd -g 501 oinstall
groupadd -g 502 dba
useradd -u 501 -g oinstall -G dba oracle
passwd oracle

9. 两节点建立oracle安装目录
[root@ ~]#
mkdir /u01/oracle/db -p
mkdir /u01/grid    -p
chown -R oracle:oinstall /u01

10. 两节点设置oracle用户的环境变量
su - oracle
vi .bash_profile 末尾添加：
export ORACLE_BASE=/u01/oracle
export ORACLE_HOME=/u01/oracle/db
export ORACLE_SID=orcl1或orcl2
export PATH=$PATH:$ORACLE_HOME/bin:/u01/grid/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
注：:/u01/grid目录用于存放grid Infrastructure的内容
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib; export CLASSPATH
[oracle@rac1 ~]$ exit

11. 配置FOR RAC 的SSH
在主节点 RAC1 上以 oracle 用户身份生成用户的公匙和私匙
su - oracle
$ mkdir ~/.ssh
$ ssh-keygen -t rsa
$ ssh-keygen -t dsa
在副节点 RAC2 上执行相同的操作，确保通信无阻
su - oracle
$ mkdir ~/.ssh
$ ssh-keygen -t rsa
$ ssh-keygen -t dsa
在主节点 RAC1 上 oracle 用户执行以下操作
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
ssh rac2 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
ssh rac2 cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
scp ~/.ssh/authorized_keys rac2:~/.ssh/

12. 配置共享磁盘FOR RAC   15398091122
fdisk /dev/sdb
[root@rac1 ~]# partprobe
vi /etc/udev/rules.d/60-raw.rules
ACTION=="add", KERNEL=="sdb1", RUN+="/bin/raw /dev/raw/raw1 %N"
ACTION=="add", KERNEL=="sdc1", RUN+="/bin/raw /dev/raw/raw2 %N"
ACTION=="add", KERNEL=="sdd1", RUN+="/bin/raw /dev/raw/raw3 %N"
ACTION=="add", KERNEL=="sde1", RUN+="/bin/raw /dev/raw/raw4 %N"
ACTION=="add", KERNEL=="sdd1", RUN+="/bin/raw /dev/raw/raw5 %N"
ACTION=="add", KERNEL=="sde1", RUN+="/bin/raw /dev/raw/raw6 %N"
ACTION=="add", KERNEL=="sdf1", RUN+="/bin/raw /dev/raw/raw7 %N"
ACTION=="add", KERNEL=="sdg1", RUN+="/bin/raw /dev/raw/raw8 %N"
ACTION=="add", KERNEL=="sdh1", RUN+="/bin/raw /dev/raw/raw9 %N"
start_udev
ls /dev/raw -l
 vi /etc/rc.local  
chown oracle:oinstall /dev/raw/raw1
chown oracle:oinstall /dev/raw/raw2
chown oracle:oinstall /dev/raw/raw3
chown oracle:oinstall /dev/raw/raw4
chown oracle:oinstall /dev/raw/raw5
chown oracle:oinstall /dev/raw/raw6
chown oracle:oinstall /dev/raw/raw7
chown oracle:oinstall /dev/raw/raw8
chown oracle:oinstall /dev/raw/raw9
chmod 600 /dev/raw/raw1
chmod 600 /dev/raw/raw2
chmod 600 /dev/raw/raw3
chmod 600 /dev/raw/raw4
绑定裸设备
raw /dev/raw/raw< N> /dev/< blockdev>
删除裸设备
raw /dev/raw/raw< N> 0 0
如用raw /dev/raw/raw1 0 0 删除裸设备/dev/raw/raw1
sleep 2
chmod 660 /dev/raw/raw*
[root@Ndwrac1 mapper]# ll /dev/mapper/
vi /etc/udev/rules.d/60-raw.rules
ACTION=="add", KERNEL=="dm-3", RUN+="/bin/raw /dev/raw/raw1 %N"  
ACTION=="add", KERNEL=="dm-4", RUN+="/bin/raw /dev/raw/raw2 %N"  
ACTION=="add", KERNEL=="dm-5", RUN+="/bin/raw /dev/raw/raw3 %N"  
ACTION=="add", KERNEL=="dm-6", RUN+="/bin/raw /dev/raw/raw4 %N"  
ACTION=="add", KERNEL=="dm-7", RUN+="/bin/raw /dev/raw/raw5 %N"  
ACTION=="add", KERNEL=="dm-8", RUN+="/bin/raw /dev/raw/raw6 %N"  

13. 创建软链接
[root@lgh01 ~]# cd /lib64/
[root@lgh01 lib64]# ln -s libcap.so.2.16 libcap.so.1

14. 创建ASM磁盘组，用asmca 来创建
=====GRID可能会遇到的问题
安装需要的其他软件
1: 两节点都要执行：
[root@rac1 ~]#
rpm -ivh pdksh-5.2.14-36.el5.x86_64.rpm
rpm -ivh rlwrap-0.36-1.8.x86_64.rpm
rpm -ivh ttfonts-zh_CN-2.14-6.noarch.rpm --nodeps
解决linux6上不存在libcap.so.1的问题
运行Grid Infrastructure脚本时会用到libcap.so.1，如果找不到该文件就会报错退出安装，而linux6上只有libcap.so.2，不存在libcap.so.1。
发现libcap.so.2是个链接，于是创建一个libcap.so.1的链接即可解决此问题。
[root@ ~]#
cd /lib64/
ln -s libcap.so.2.16 libcap.so.1
[root@rac1 ~]# cd /lib64/
[root@rac1 lib64]# ln -s libcap.so.2.16 libcap.so.1
[root@rac2 ~]# cd /lib64/
[root@rac2 lib64]# ln -s libcap.so.2.16 libcap.so.1
在配scan名称 和 HOSTS的SCAN名称一致
3：gsd结尾的为兼容9i的服务，可以不用启动，ora.oc4j也可以不用启动。
如果想启动oc4j服务，执行如下内容：
[oracle@rac1 ~]$ srvctl enable oc4j
[oracle@rac1 ~]$ srvctl start oc4j
[oracle@rac1 ~]$ crs_stat -t | grep oc4j
ora.oc4j ora.oc4j.type ONLINE ONLINE rac2
[oracle@rac2 ~]$ crs_stat -t | grep oc4j
ora.oc4j ora.oc4j.type ONLINE ONLINE rac2
4：当运行 /u01/grid/root.sh 失败后，
/u01/grid/crs/install/roothas.pl -deconfig -force
5：第二个root脚本运行：
该脚本与linux6有兼容性问题，解决方法为：运行此脚本到显示Adding daemon to inittab时，
马上使用root用户在另一个窗口不停地执行/bin/dd if=/var/tmp/.oracle/npohasd of=/dev/null bs=1024 count=1，
这条dd命令执行后，会一直停在那里，当Grid Infrastructure安装完后，才可以用ctrl+c退出。
注意：所有的节点运行这个root脚本时都要执行这条dd命令。
5:安装桌面软件包
yum grouplist
yum -y groupinstall  Desktop
6:安装VNCSERVER
yum -y install tigervnc tigervnc-server
6：禁止11g RAC 自启动
srvctl disable database -d db_name
禁止10g Clusterware在系统重启后自动启动：
$ /etc/init.d/init.crs disable
在系统正常启动后:让Clusterware在系统重启后自动启动:
$ /etc/init.d/init.crs enable
 可以用以下方动启动crs:
$/etc/init.d/init.cssd start
$/etc/init.d/init.crs start
rac1-> ./init.crs start
Startup will be queued to init within 90 seconds.
rac1-> ./init.cssd start
Startup will be queued to init within 90 seconds.
或:
$crsctl start crs
检查crs状态:
$crsctl check crs
rac1-> crsctl check crs
CSS appears healthy
CRS appears healthy
EVM appears healthy
rac2-> crsctl check crs
CSS appears healthy
CRS appears healthy
EVM appears healthy
启动数据库
$srvctl start nodeapps -n rac1
$srvctl start nodeapps -n rac2
$srvctl start asm -n rac1
$srvctl start asm -n rac2
$srvctl start instance -d racdb -i racdb1
$srvctl start instance -d racdb -i racdb2
