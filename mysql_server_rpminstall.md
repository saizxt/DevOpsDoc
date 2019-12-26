# mysql_server_rpminstall

```shell
yum remove mysql
yum remove mysql-libs
rpm -ivh MySQL-server
rpm -ivh MySQL-devel
rpm -ivh MySQL-client
================================================T0bmyG5k6UcssI4y
#cp /usr/share/mysql/my-default.cnf /etc/my.cnf
datadir=/data/mysql
socket=/data/mysql/mysql.sock
character-set-server=utf8
#/usr/bin/mysql_install_db
cat /root/.mysql_secret
mysql -uroot –p******
mysql>SET PASSWORD = PASSWORD('123456');
================================================
mysql>GRANT ALL PRIVILEGES  
ON *.*  
TO 'root'@'%'
WITH GRANT OPTION;
mysql>FLUSH PRIVILEGES;  
================================================
/var/lib/mysql/   #数据库目录
/usr/share/mysql  #配置文件目录
/usr/bin          #相关命令目录
/etc/init.d/mysql #启动脚本
================================================
changeMySQL datafile
================================================
mkdir /data
mysqladmin -u root -p shutdown
mv /var/lib/mysql　/data/
mkdir /var/lib/mysql
ln -s /data/mysql/ /var/lib/mysql
================================================
vi my.cnf
[mysqld]
port=3306
datadir=/data/mysql
socket=/data/mysql/mysql.sock
================================================
vi /etc/init.d/mysql
datadir=/data/mysql  
```
