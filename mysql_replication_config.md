# mysql_replication_config

```shell
[master]
vi /etc/my.cnf
log_bin=master_bin
server_id=1
#binlog_do_db=
binlog_ignore_db=information_schema,mysql,performance_schema
========================================================================
create user repl;
grant replication slave on *.* to 'repl'@'172.22.4.%' identified by 'replication';
flush privileges;
========================================================================
service mysql restart
#lock-backup-copy
MYSQL>FLUSH TABLES WITH READ LOCK;
$cd /mysql
$tar -cvf data.tar data
========================================================================
show master status\G
$File
$Position
========================================================================
========================================================================
[slave]
vi /etc/my.cnf
server-id=2
========================================================================
#restart&&backup
========================================================================
stop slave;
reset slave;
change master to master_host='172.22.4.5',
master_port=3306,
master_user='repl',
master_password='replication',
master_log_file='master_bin.000002',
master_log_pos=120;
start slave;

show slave status\G
show processlist;
```

## 本地镜像系统操作 (主库)

1.修改/etc/my.cnf ,把server-id = 1 放开。如果注释掉就操作，没有注释掉就不用操作
$vi /etc/my.cnf
server-id       = 1
2.登录mysql，添加用户，用来同步数据。命令中用户名为rep、密码rep .IP根据需要修改,10.2.20.30为目标端IP(镜像的备机）；
$mysql -uroot
MYSQL>GRANT REPLICATION SLAVE ON *.* TO rep@10.2.20.30 IDENTIFIED BY 'rep'
3.给同步用户付权限。IP根据需要修改，10.2.20.30为目标端IP；
MYSQL>GRANT FILE,SELECT,REPLICATION SLAVE ON *.* TO rep@10.2.20.30 IDENTIFIED BY 'rep';
4. 锁住日志，不要关闭该终端。否则锁住失效；
MYSQL>FLUSH TABLES WITH READ LOCK;
5. 查看主数据标志。记下File: 和 Position:  两个对应的值；
mysql>show master status\G;
*************************** 1. row ***************************
            File: mysql-bin.000021
        Position: 1102
    Binlog_Do_DB:
Binlog_Ignore_DB:
1 row in set (0.00 sec)
ERROR:
No query specified
6.数据库初始化准备；（另外起窗口操作）
$cd /mysql
$tar -cvf data.tar data
7. 将打包的data.tar 文件拷贝到目标数据库/mysql 下。
$sftp>cd /mysql
$sftp>put data.tar
8. 解锁（在刚才锁表的界面操作）
MYSQL>unlock tables;

## 数据中心服务器上操作 （备机）

1. 登录远程服务器，解压data.tar ，覆盖MYSQL数据库data目录；
$mv data data_bak
$tar -xvf data.tar
2. 修改/etc/my.cnf ,把server-id = 2 放开。如果注释掉就操作，没有注释掉就不用操作。增加需要同步的数据。例子是同步的GGS、CDC两个库；
$vi /etc/my.cnf
server-id = 2
将1注释掉，将2放开
3. 登录目标数据库mysql，停止同步进程；
MYSQL>stop slave ;
4. 在备数据库上增加同步数据库标志点和登录主数据库的用户名等 (IP 为主库机器IP）。执行语句；

```sql
MYSQL>change master to master_host='172.16.155.10',
master_user='rep',
master_password='rep',
master_log_file='mysql-bin.000009’, //与第5条  File: mysql-bin.000009中的数值一致。
master_log_pos=105140;   //与第5条  Position: 105140中的数值一致。
eg：(在备机上操作，以下IP为主库机器的IP）
change master to master_host='10.2.20.29', master_user='rep',master_password='rep', master_log_file='mysql-bin.000021',master_log_pos=811;
```

5.断开后重新连接数据库时间
MYSQL>CHANGE MASTER TO  MASTER_CONNECT_RETRY=60;
6.启动同步
MYSQL>start slave;
7.查看状态
MYSQL>show slave status\G;
8.配置完成以后重启数据库服务器
用root用户

```shell
# service mysqld restart
```

注意点：主库的serverid=1
备份库的server=2
show variables like 'server_id'; 查看数据库的server-id
set global server_id=2;   修改server-id
