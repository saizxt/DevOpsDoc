# oracle_10gRAC_install

1. 新数仓1： Ndwrac1  192.168.108.51(52)  root/Super#yyptdbkr1   ecp/ECP#yyptdbkr1  oracle/ECP#yyptdbkr1
新数仓2： Ndwrac2  192.168.108.53(54)  root/Super#yyptdbkr2   ecp/ECP#yyptdbkr2  oracle/ECP#yyptdbkr2
yum -y install binutils compat-libstdc++ elfutils-libelf elfutils-libelf-devel gcc gcc-c++ glibc glibc-common glibc-devel libaio libaio-devel libgcc libstdc++
yum -y install libstdc++-devel make sysstat unixODBC unixODBC-devel libXp rsh-* openmotif2*

2. vi  /etc/hosts  
127.0.0.1       localhost.localdomain   localhost
172.30.2.141  db03
172.30.2.142  db04
172.30.2.154  db03_vip
172.30.2.155  db04_vip
172.10.0.174  db03_priv
172.10.0.175  db04_priv

3. vi /etc/sysctl.conf //内存根据实际情况设置
kernel.shmall = 2097152
kernel.shmmax = 4294967295
kernel.shmmni = 4096
kernel.sem = 250 32000 100 128
fs.file-max = 65536
net.ipv4.ip_local_port_range = 1024 65000
net.core.rmem_default=262144
net.core.rmem_max=262144
net.core.wmem_default=262144
net.core.wmem_max=262144
/sbin/sysctl -p

4. 修改参数
vi /etc/security/limits.conf  
soft    nproc   2047
hard    nproc   16384
soft    nofile  1024
hard    nofile  65536  
soft nproc 16384
hard nproc 16384
soft nofile 65536
hard nofile 65536

5. vi /etc/pam.d/login  
session    required     /lib/security/pam_limits.so

6. vi /etc/selinux/config  
SELINUX=disabled  

7. vi /etc/modprobe.conf  
options hangcheck-timer hangcheck_tick=30 hangcheck_margin=180

8. create user  
groupadd -g 501 oinstall
groupadd -g 502 dba
useradd -u 501 -g oinstall -G dba oracle
passwd oracle
usermod -g group loginname      强行设置某个用户所在组
usermod -G groups loginname     把某个用户改为 group(s)  
usermod -a -G groups loginname  把用户添加进入某个组(s）
groupadd oper
useradd -g oinstall -G dba oracle
passwd oracle  

9. create directories  
mkdir -p /u01/crs/oracle/product/10.2.0/crs
mkdir -p /u01/app/oracle/product/10.2.0/db_1
mkdir -p /u01/oradata
chown -R oracle:oinstall /u01  

10. vi oracle .base_profile  
Oracle Settings
TMP=/tmp; export TMP
TMPDIR=$TMP; export TMPDIR
ORACLE_BASE=/u01/app/oracle;  
export ORACLE_BASE
ORACLE_HOME=$ORACLE_BASE/product/10.2.0/db_1;  
export ORACLE_HOME
ORACLE_CRS_HOME=/u01/crs/oracle/product/10.2.0/crs  
export ORACLE_CRS_HOME
ORACLE_SID=rac1;  
export ORACLE_SID
ORACLE_TERM=xterm;  
export ORACLE_TERM
PATH=/usr/sbin:$PATH;  
export PATH
PATH=$ORACLE_HOME/bin:$ORACLE_CRS_HOME/bin:$PATH;  
export PATH
export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK
LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib;  
export LD_LIBRARY_PATH
CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib;  
export CLASSPATH
if [ $USER = "oracle" ]; then
  if [ $SHELL = "/bin/ksh" ]; then
    ulimit -p 16384
    ulimit -n 65536
  else
    ulimit -u 16384 -n 65536
  fi
fi

11. show disk  
cd /dev
ls sd*
sda  sda1  sda2  sdb  sdc  sdd  sde  sdf  

12. fdisk /dev/sdb    after show again
cd /dev
ls sd*
sda  sda1  sda2  sdb  sdb1  sdc  sdc1  sdd  sdd1  sde  sde1  sdf  sdf1  

13. vi /etc/udev/rules.d/60-raw.rules
ACTION=="add", KERNEL=="sdb1", RUN+="/bin/raw /dev/raw/raw1 %N"
ACTION=="add", KERNEL=="sdb2", RUN+="/bin/raw /dev/raw/raw2 %N"
ACTION=="add", KERNEL=="sdb3", RUN+="/bin/raw /dev/raw/raw3 %N"
ACTION=="add", KERNEL=="sdb4", RUN+="/bin/raw /dev/raw/raw4 %N"
KERNEL=="raw[1-4]", OWNER="oracle", GROUP="oinstall", MODE="640"
start_udev
ls /dev/raw -l

14. RUN:
ln -s /dev/raw/raw1  /u01/oradatacd  

15. vi /etc/rc.local  
chown oracle:oinstall /dev/raw/raw1
chown oracle:oinstall /dev/raw/raw2
chown oracle:oinstall /dev/raw/raw3
chown oracle:oinstall /dev/raw/raw4
chmod 600 /dev/raw/raw1
chmod 600 /dev/raw/raw2
chmod 600 /dev/raw/raw3
chmod 600 /dev/raw/raw4

16. RUN  
chown -R oracle:oinstall /dev/raw
chown -R oracle:oinstall /u01
chmod 600 /dev/raw/raw1
chmod 600 /dev/raw/raw2
chmod 600 /dev/raw/raw3
chmod 600 /dev/raw/raw4
chmod 600 /dev/raw/raw5
chmod 600 /dev/raw/raw6
chmod 600 /dev/raw/raw7
chmod 600 /dev/raw/raw8
chmod 600 /dev/raw/raw9

17. RUN
ping -c 3 rac1
ping -c 3 rac1-priv
ping -c 3 rac2
ping -c 3 rac2-priv

18. using the runcluvfy.sh check  (root)  
19. on RAC1 run  
./runInstaller
方法2:安装时忽略系统检查
./runInstaller -ignoreSysPrereqs
vi /etc/yum.repos.d/base_install.repo  
[base_install]
enabled=1
name=rrr
baseurl=file:///mnt/Server
gpgcheck=0
/1/clusterware/cluvfy/runcluvfy.sh stage -pre crsinst -n db03,db04 -verbose
ssh db03 date
ssh db04 date
ssh db03_priv date
ssh db04_priv date
hosts.equiv
============n /u01/crs/oracle/product/10.2.0/crs/root.sh   (root:rac1,rac2)  
cd /u01/crs/oracle/product/10.2.0/crs
./root.sh
(在rac2运行完 root.sh后，最后有：***/oracle/crs/oracle/product/10/crs/jdk/jre//bin/java: error while loading shared libraries:libpthread.so.0: cannot open shared object file报错
解决方法：root用户
./oifcfg setif -global eth0/10.1.2.0:public  
./oifcfg setif -global eth1/172.16.0.0:cluster_interconnect  
./oifcfg getif  
./oifcfg iflist
改 vi vipca
arch=`uname -m`
if [ "$arch" = "i686" -o "$arch" = "ia64" ]
then
LD_ASSUME_KERNEL=2.4.19
export LD_ASSUME_KERNEL
fi
再运行root用户运行 ./vipca就可以解决不能运行VIPCA的问题）

20. Run vipca as root on RAC2
cd /u01/crs/oracle/product/10.2.0/crs/bin
./vipca  

21. 配置FOR RAC 的SSH
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
ssh db03 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
ssh db03 cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
scp ~/.ssh/authorized_keys db03:~/.ssh/authorized_keys  
