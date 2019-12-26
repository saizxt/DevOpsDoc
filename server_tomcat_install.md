# server_tomcat_install

```shell
mkdir /usr/local/java/
cd /opt
tar -zxvf jdk-7u45-linux-x64.tar.gz -C /usr/local/java/
```

```shell
vim /etc/profile
#set java evironment
JAVA_HOME=/usr/local/java/jdk1.7.0_45
CLASSPATH=.:$JAVA_HOME/lib.tools.jar:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin
export JAVA_HOME CLASSPATH PATH
```

```shell
source /etc/profile
javac
```

```shell
==JDK_test============================================================
java -version
==tomcat_install======================================================
tar -C
rpm —-prefix=
rpm --relocate /usr/bin=
==tomcat_test=========================================================
ps axuwf|grep java
netstat -lnpt
==tomcat_login========================================================
../conf/tomcat-users.xml  
```
