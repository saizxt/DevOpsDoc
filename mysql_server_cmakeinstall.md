# mysql_server_cmakeinstall

1. 解压介质包
tar zxvf cmake-2.8.4.tar.gz
tar zxvf mysql-5.5.11.tar.gz

2. 新建用户，修改用户密码
useradd mysql
passwd mysql

3. 编译并安装插件
cd cmake-2.8.4
./bootstrap && gmake && gmake install

4. 最后输出结果为零，则成功。否则失败，查找原因。
echo $?

5. 编译并安装mysql数据库(GBK)=====要UTF8
cd mysql-5.5.11/
cmake -DCMAKE_INSTALL_PREFIX=/home/mysql \
-DDEFAULT_CHARSET=gbk \
-DDEFAULT_COLLATION=gbk_chinese_ci \
-DWITH_EXTRA_CHARSETS:STRING=all \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1

6. 编译并安装mysql数据库(UTF8)
cmake -DCMAKE_INSTALL_PREFIX=/home/mysql \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_EXTRA_CHARSETS:STRING=all \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1  
echo $?
make && make install
echo $?

7. 创建一个连接
ln -s /home/mysql/lib/libmysqlclient.so.18 /usr/lib/libmysqlclient.so.18
进入support-files/ 目录，拷贝文件到对应目录。
cd support-files/
cp my-large.cnf /etc/my.cnf
cp mysql.server /etc/init.d/mysqld

8. 编辑my.cnf 文件，在文件中找到[mysqld] 在下一行添加

```shell
vi /etc/my.cnf
[mysqld]
bulk_insert_buffer_size = 32M
max_heap_table_size = 32M
query_cache_limit = 10M
skip-name-resolve
最好放在innodb 后面
max-connections=1000
innodb_buffer_pool_size=3G
key_buffer_size=512M
#innodb_flush_method=O_DIRECT
innodb_file_per_table
innodb_log_buffer_size=4M
innodb_file_format=Barracuda
innodb_change_buffering=all/
innodb_thread_concurrency=9
innodb_io_capacity=400
innodb_buffer_pool_instances=3
#innodb_fast_shutdown=0
innodb_purge_threads=1
innodb_read_io_threads = 8
innodb_write_io_threads = 8
innodb_flush_log_at_trx_commit = 1
#如报max_allowed_packet = 1M  太小的话  max_allowed_packet = 4M  
```

创建mysql环境变量

```shell
#/home/mysql/scripts/mysql_install_db \
--defaults-file=/etc/my.cnf \
--basedir=/home/mysql \
--datadir=/home/mysql/data \
--user=mysql
#修改/mysql 目录写的文件属性
#chown -R mysql:mysql /home/mysql
#给mysqld 付权限
#chmod +x /etc/init.d/mysqld
#修改mysqld 文件，找到basedir= datadir=添加对应的目录
#vi /etc/init.d/mysqld  
basedir=/home/mysql
datadir=/home/mysql/data
#chkconfig --add mysqld
#chkconfig --level 235 mysqld on
#启动mysql数据库
#service mysqld start
#更改用户，登录mysql数据库。给数据库添加用户，和对应的权限。
# su - mysql
$ mysql -uroot
mysql>  grant all privileges on *.* to ggs@localhost identified by 'ggs';
mysql>  grant all privileges on *.* to ggs@'%' identified by 'ggs';
##例：
mysql>  grant all privileges on *.* to zfjggeter@localhost identified by 'ctsi123';
mysql>  grant all privileges on *.* to zfjggeter@'%' identified by 'ctsi123';
mysql> flush privileges;  (#生效新加用户权限)
# su - mysql
mysql –uzfjggeter  -pctsi123
$ mysql>create database NBZFJGMYDB
##查看mysql字符集
show variables like '%char%'
```
