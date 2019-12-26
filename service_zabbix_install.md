# service_zabbix_install

## install

```shell
#!/bin/bash
lname=`hostname -s`
yum install gcc mysql-devel net-snmp-devel curl curl-devel libxml2 libxml2-devel -y
echo "172.22.4.1 zabbix" >> /etc/hosts
cd /opt
wget -c http://172.30.1.230:10008/soft/zabbix-3.0.1.tar.gz
tar xvf zabbix-3.0.1.tar.gz
cd zabbix-3.0.1
groupadd zabbix
useradd -g zabbix zabbix
ln -s /usr/local/lib/libiconv.so.2 /usr/lib/libiconv.so.2
/sbin/ldconfig
./configure --prefix=/usr/local/zabbix --enable-agent --with-mysql --enable-ipv6 --with-net-snmp --with-libcurl --with-libxml2
#./configure --prefix=/usr/local/zabbix --enable-server --enable-agent --with-mysql --enable-ipv6 --with-net-snmp --with-libcurl --with-libxml2
make && make install
rm -f /usr/local/sbin/zabbix_agent /usr/local/sbin/zabbix_agentd  
rm -f /usr/local/bin/zabbix_get /usr/local/bin/zabbix_sender  
ln -s /usr/local/zabbix/sbin/* /usr/local/sbin/
ln -s /usr/local/zabbix/bin/* /usr/local/bin/
sed -i "s%# Hostname=%Hostname=$lname%" /usr/local/zabbix/etc/zabbix_agentd.conf
chkconfig zabbix_agentd on
/etc/init.d/zabbix_agentd start
```

## zabbix v3.0安装部署

关于zabbix及相关服务软件版本：
Linux：centos 6.6
nginx：1.9.15
MySQL：5.5.49
PHP：5.5.35  

一、安装nginx：
安装依赖包：
yum -y install gcc gcc-c++ autoconf automake zlib zlib-devel openssl openssl-devel pcre* make gd-devel libjpeg-devel libpng-devel libxml2-devel bzip2-devel libcurl-devel
创建用户：
useradd nginx -s /sbin/nologin -M
下载nginx软件包并进入到目录中：
wget <http://nginx.org/download/nginx-1.9.15.tar.gz> && tar xvf nginx-1.9.15.tar.gz && cd nginx-1.9.15
 编译：
./configure --prefix=/usr/local/product/nginx1.9.14 --user=www --group=www --with-http_ssl_module --with-http_v2_module --with-http_stub_status_module --with-pcre
make && make install
ln -s /usr/local/product/nginx1.9.14 /usr/local/nginx    ==>创建软链接
参数解释：
--with-http_stub_status_module：支持nginx状态查询
--with-http_ssl_module：支持https
--with-http_spdy_module：支持google的spdy,想了解请百度spdy,这个必须有ssl的支持
--with-pcre：为了支持rewrite重写功能，必须制定pcre

二、安装PHP
下载PHP安装包：
wget <http://cn2.php.net/get/php-5.5.35.tar.gz/from/this/mirror>  
解压并编译：
mv mirror php-5.5.35.tar.gz &&  
tar xvf php-5.5.35.tar.gz && cd php-5.5.35
./configure --prefix=/usr/local/product/php-5.5.35 --with-config-file-path=/usr/local/product/php-5.5.35/etc --with-bz2 --with-curl --enable-ftp --enable-sockets --disable-ipv6 --with-gd --with-jpeg-dir=/usr/local --with-png-dir=/usr/local --with-freetype-dir=/usr/local --enable-gd-native-ttf --with-iconv-dir=/usr/local --enable-mbstring --enable-calendar --with-gettext --with-libxml-dir=/usr/local --with-zlib --with-pdo-mysql=mysqlnd --with-mysqli=mysqlnd --with-mysql=mysqlnd --enable-dom --enable-xml --enable-fpm --with-libdir=lib64 --enable-bcmath
make && make install
ln -s /usr/local/product/php-5.5.35 /usr/local/php
cp php.ini-production /usr/local/php/etc/php.ini
cd /usr/local/php/etc/
cp php-fpm.conf.default php-fpm.conf

修改php.ini参数：（zabbix环境需要修改的参数）
max_execution_time = 300  
memory_limit = 128M  
post_max_size = 16M  
upload_max_filesize = 2M  
max_input_time = 300  
date.timezone = PRC  

三、安装MySQL
添加mysql用户，创建mysql的数据目录：
groupadd mysql
mkdir -pv /data/mysql
useradd -r -g mysql -d /data/mysql -s /sbin/nologin mysql
chown -R mysql.mysql /data/mysql
安装cmake及依赖：
yum install cmake gcc* ncurses-devel -y  
下载MySQL安装包：
wget <http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.49.tar.gz>
编译安装MySQL：
tar -xvf mysql-5.5.49.tar.gz && cd mysql-5.5.49
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/product/mysql5.5.49 -DDEFAULT_CHARSET=utf8 -DENABLED_LOCAL_INFILE=1 -DMYSQL_DATADIR=/data/mysql -DWITH_EXTRA_CHARSETS=all -DWITH_READLINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DMYSQL_TCP_PORT=3306 -DDEFAULT_COLLATION=utf8_general_ci
make && make install
ln -s /usr/local/product/mysql5.5.49 /usr/local/mysql
chown -R mysql.mysql /usr/local/mysql
拷贝mysql的配置文件：
cd /usr/local/mysql/support-files/
cp my-medium.cnf /data/mysql/my.cnf
cp mysql.server /etc/init.d/mysqld
chmod +x /etc/init.d/mysqld
初始化MySQL：
cd /usr/local/mysql/scripts
./mysql_install_db --user=mysql --basedir=/usr/local/mysql/ --datadir=/data/mysql/
修改MySQL配置文件my.cnf中数据目录：
datadir=/data/mysql/
启动MySQL：
[root@zabbix ~]# /etc/init.d/mysqld start
Starting MySQL... SUCCESS  
登录数据库，创建zabbix数据库及用户名和密码：
mysql> create database zabbix default charset utf8;
Query OK, 1 row affected (0.00 sec)
mysql> grant all privileges on zabbix.* to zabbix@'localhost' identified by 'zabbix';
Query OK, 0 rows affected (0.03 sec)

mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
mysql> show databases;
[root@zabbix ~]# mysql
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/lib/mysql/mysql.sock' (2)
解决办法[root@zabbix ~]# ln -s /tmp/mysql.sock /var/lib/mysql/
为数据库的root创建密码：
[root@zabbix zabbix-3.0.3]# mysqladmin -uroot password  "zabbix"
四、安装zabbix server：
安装zabbix：
[root@zabbix ~]# tar zxf zabbix-3.0.1.tar.gz && cd zabbix-3.0.1
编译zabbix：
./configure --prefix=/usr/local/zabbix-3.0.1/ --enable-server --enable-agent --with-mysql --with-net-snmp --with-libcurl --with-libxml2
make && make install
故障：
checking for mysql_config... no
configure: error: MySQL library not found
解决：yum install mysql-devel -y
故障：
checking for net-snmp-config... no
configure: error: Invalid Net-SNMP directory - unable to find net-snmp-config
解决：yum install net-snmp-devel -y
创建zabbix用户：
[root@zabbix zabbix-3.0.3]# groupadd zabbix
[root@zabbix zabbix-3.0.3]# useradd zabbix -s /sbin/nologin -M -g
zabbix server需要导入3个sql文件：
[root@zabbix zabbix-3.0.3]# mysql -uroot -pzabbix zabbix < database/mysql/schema.sql  
[root@zabbix zabbix-3.0.3]# mysql -uroot -pzabbix zabbix < database/mysql/images.sql  
[root@zabbix zabbix-3.0.3]# mysql -uroot -pzabbix zabbix < database/mysql/data.sql  
[root@zabbix zabbix-3.0.3]# pwd
/root/zabbix-3.0.3
五、zabbix管理网站配置（nginx）：
创建项目目录：
[root@zabbix zabbix-3.0.3]# mkdir /data/web/zabbix.lifec.com -p
[root@zabbix zabbix-3.0.3]# mkdir /data/logs/zabbix -p
 将前端文件拷贝到项目目录下：
[root@zabbix zabbix-3.0.3]# cp -rp frontends/php/* /data/web/zabbix.lifec.com/
编辑nginx虚拟主机：
[root@zabbix conf]# mkdir extra
[root@zabbix conf]# cd extra/
[root@zabbix extra]# vim zabbix.conf
server {
listen 8027;
server_name zabbix.lifec.com;
access_log /data/logs/zabbix/zabbix.lifec.com.access.log main;
index index.html index.php index.html;
root /data/web/zabbix.lifec.com;
location /{
       try_files $uri $uri/ /index.php?$args;
}
location ~ ^(.+.php)(.*)$ {
       fastcgi_split_path_info ^(.+.php)(.*)$;
       include fastcgi.conf;
       fastcgi_pass 127.0.0.1:9000;
       fastcgi_index index.php;
       fastcgi_param PATH_INFO $fastcgi_path_info;
}
}
编辑nginx.conf配置文件：  

```shell
[root@zabbix conf]# cat nginx.conf
user  nginx;
worker_processes  1;
#error_log  logs/error.log warning;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
pid        logs/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    #keepalive_timeout  0;
    keepalive_timeout  65;
    #gzip  on;
    include extra/*.conf;
}  
```

编辑zabbix_server.conf文件：
  [root@zabbix etc]# pwd
  /usr/local/zabbix-3.0.2/etc
LogFile=/tmp/zabbix_server.log
PidFile=/tmp/zabbix_server.pid
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix

六、启动服务
[root@zabbix conf]# /usr/local/nginx/sbin/nginx
[root@zabbix conf]# /usr/local/php/sbin/php-fpm
[root@zabbix conf]# /usr/local/zabbix-3.0.3/sbin/zabbix_server
如果启动的时候报错：
  [root@zabbix ~]# /usr/local/zabbix-3.0.2/sbin/zabbix_server
  /usr/local/zabbix-3.0.2/sbin/zabbix_server: error while loading shared libraries: libmysqlclient.so.18: cannot open shared object file: No such file or directory
  [root@zabbix ~]# ln -s /usr/local/mysql/lib/libmysqlclient.so.18 /usr/lib64/
添加/etc/hosts文件：
192.168.119.140 zabbix.lifec.com
[root@zabbix conf]# netstat -lntup
Active Internet connections (only servers)
Proto Recv-Q Local Address    PID/Program name  
tcp        0 0.0.0.0:22      029/sshd  
tcp        0 0.0.0.0:8027    3730/nginx  
tcp        0 0.0.0.0:10051   3743/zabbix_server  
tcp        0 127.0.0.1:9000  3736/php-fpm  
tcp        0 0.0.0.0:3306    24922/mysqld  
tcp        0 :::22           1029/sshd  
udp       0 0.0.0.0:68       880/dhclient  
将服务加入开机自启动：
[root@zabbix ~]# echo "/usr/local/nginx/sbin/nginx" >>/etc/rc.local  
[root@zabbix ~]# echo "/usr/local/php/sbin/php-fpm" >>/etc/rc.local  
[root@zabbix ~]# echo "/etc/init.d/mysqld start" >>/etc/rc.local
[root@zabbix ~]# echo "/usr/local/zabbix-3.0.3/sbin/zabbix_server" >>/etc/rc.local

七、web端配置zabbix  

```shell
vim /data/web/zabbix.lifec.com/include/locales.inc.php
#'zh_CN' => ['name' => _('Chinese (zh_CN)'), 'display' => false],
'zh_CN' => ['name' => _('Chinese (zh_CN)'), 'display' => true],
将选择的字体上传到Linux服务器的zabbix的fonts目录：/data/web/zabbix.lifec.com/fonts
vim /data/web/zabbix.lifec.com/include/defines.inc.php
define('ZBX_GRAPH_FONT_NAME', 'DejaVuSans'); // font file name
define('ZBX_GRAPH_FONT_NAME', 'simsun'); // font file name        ==>此行为新增行；
define('ZBX_FONT_NAME', 'DejaVuSans');
define('ZBX_FONT_NAME', 'simsun');       ==>此行为新增行；  
```
