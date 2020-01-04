# linux_snmp_install

依赖包：  
yum -y install net-snmp net-snmp-libs net-snmp-utils libsensors3 lm_sensors
yum -y install *snmp*

启动及开机启动  
service snmpd start
chkconfig snmpd on

本地测试  
snmpwalk -v 2c -c public localhost sysName.0
snmptranslate -To | head

远程测试  
snmpwalk -v 2c -c public 1.1.1.1 sysName.0

/etc/snmp/snmpd.conf
增加一行：
view systemview included .1
去掉注释：
proc mountd
proc ntalkd 4
proc sendmail 10 1
exec echotest /bin/echo hello world
disk / 10000
load 12 14 14
service snmpd restart
