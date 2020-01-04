# oracle_11g_install

1. 上传解压database
2. 修改机器名network/hosts
3. 安装依赖包：
yum -y install gcc gcc-c++ libaio-devel compat-libstdc++-33 elfutils-libelf-devel libstdc++ pdksh
如果没有pdksh自己网上找
4. 修改内核参数
vi /etc/sysctl.conf  
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

5. create user  
groupadd -g 501 oinstall
groupadd -g 502 dba
useradd -u 501 -g oinstall -G dba oracle
passwd oracle

6. vi /etc/security/limits.conf
oracle          soft     nproc           16384
oracle          hard     nproc           16384
oracle          soft     nofile          65536
oracle          hard     nofile          65536

7. vi /etc/pam.d/login  
session    required     /lib/security/pam_limits.so

8. vi /etc/selinux/config  
SELINUX=disabled

9. service iptables stop
chkconfig iptables off
setenforce 0
getenforce

10. mkdir /u01/oracle/db -p
chown -R oracle:oinstall /u01

11. su - oracle
vi .bash_profile
oracle environmnet
export ORACLE_BASE=/u01/oracle
export ORACLE_HOME=/u01/oracle/db
export ORACLE_SID=oracledb
export PATH=$PATH:$ORACLE_HOME/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
export NLS_LANG=AMERICAN_AMERICA.AL32UTF8
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
. .bash_profile
vipca
dbca
库名必须和sid相同！
