# server_tomcat_move

```shell
login
ssh 172.30.1.129
echo $JAVA_HOME
/usr/local/java/jdk1.7.0_45
tar -zcvf 7108.tar.gz /service/tomcat-toepage7108/ --exclude=/service/tomcat-toepage7108/webapps/awifi_toe_strategy/media --exclude=/service/tomcat-toepage7108/logs/localhost_access_log.* --exclude=/service/tomcat-toepage7108/logs/awifi_toe.log.*
scp 7108.tar.gz root@172.22.4.17:/server/
root@awifi
mount -t nsp
```
