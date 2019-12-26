# linux_firewall_config

```shell
echo > /etc/sysconfig/iptables
vim /etc/sysconfig/iptables
```

```shell
#Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT DROP [0:0]
:FORWARD DROP [0:0]
:OUTPUT ACCEPT [0:0]
####################################################################
####default#########################################################
##linked and echo
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
##accept ssh port
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
##accept ping
-A INPUT -p icmp -j ACCEPT
##accept loopback
-A INPUT -i lo -j ACCEPT
####################################################################
####service#########################################################
##accept mail smtp server
#-A INPUT -m tcp -p tcp --dport 110 -j ACCEPT
-A INPUT -m tcp -p tcp --dport 25 -j ACCEPT
##accept nfs server
-A INPUT -m tcp -p tcp --dport 111 -j ACCEPT
##accept ftp server
#-A INPUT -m tcp -p tcp --dport 21 -j ACCEPT
#-A INPUT -m tcp -p tcp --dport 20 -j ACCEPT
##accept DNS server
#-A INPUT -m tcp -p tcp --dport 53 -j ACCEPT
##accept web server
#-A INPUT -m tcp -p tcp --dport 80 -j ACCEPT
##accept MySQL server
#-A INPUT -m tcp -p tcp --dport 3306 -j ACCEPT
##accept MySQL-Proxy server
#-A INPUT -m tcp -p tcp --dport 3307 -j ACCEPT
##accept Redis server
#-A INPUT -m tcp -p tcp --dport 6379 -j ACCEPT
##accept Tomcat server
#-A INPUT -m tcp -p tcp --dport 7115 -j ACCEPT
#-A INPUT -m tcp -p tcp --dport 17115 -j ACCEPT
#-A INPUT -m tcp -p tcp --dport 27115 -j ACCEPT
####################################################################
##refused###########################################################
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```

```shell
service cups stop
chkconfig cups off
service iptables restart
iptables -L -n
```
