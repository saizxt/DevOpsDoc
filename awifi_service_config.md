# awifi_service_config

```shell
##tar
tar zcvf **.tar.gz / --exclude=
tar zcvf tomcat-toeadmin7104_20160811.tar.gz tomcat-toeadmin7104/ --exclude=tomcat-toeadmin7104/awifi_toe_admin/media --exclude=tomcat-toeadmin7104/logs

##mysql##
#login_mysql_master
sql>create database awifi_toe_city;
sql>create table ();
sql>greant insert,delete,update,select,create,drop,alter on awifi_toe_city.* to Toec@% identified by 'Toec#ER$';
##nginx##
vim /usr/local/openresty/config/toecity.conf
upstream city_server_pool {
    server 172.22.4.11:7104;
    server 172.22.4.12:7104;
    }

    server {
            listen              80;
            server_name         city.51awifi.com;
            access_log          /service/log/city_access_80.log  main;
            location / {
                proxy_cache off;
                expires off;
                proxy_pass http://city_server_pool;
                proxy_set_header Host $host:80;
                proxy_set_header X-Forwarded-For $remote_addr;
                proxy_set_header X-Forwarded-Port $remote_port;
                }

            }

##iptables##

##mount##
mount -t nfs  192.168.11.225:/service/nfs/awifi2b/media /service/zzz/awifiadmin/media -o nolock
mount -t nfs  192.168.11.225:/service/nfs/toe /service/tomcat-toeadmin-7005/awifi_toe_admin/media -o nolock
172.30.1.190:/service/nfs/toe /service/tomcat-toeadmin7104/awifi_toe_admin/media

##hosts##
echo '' >> /etc/hosts
##tomcat##
tar -zxvf jdk-7u45-linux-x64.tar.gz
mkdir /service/
mkdir /usr/local/java/
mv /opt/jdk1.7.0_45/ /usr/local/java/jdk1.7.0_45/
echo '#set java evironment' >> /etc/profile
echo 'JAVA_HOME=/usr/local/java/jdk1.7.0_45' >> /etc/profile
echo 'CLASSPATH=.:$JAVA_HOME/lib.tools.jar:$JAVA_HOME/lib:$JAVA_HOME/jre/lib' >> /etc/profile
echo 'PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin' >> /etc/profile
echo 'export JAVA_HOME CLASSPATH PATH' >> /etc/profile
source /etc/profile
echo $JAVA_HOME
javac
##test##
curl  
```
