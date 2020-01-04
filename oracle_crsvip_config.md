# oracle_crsvip_config

手工配置
<CRS_HOME>/bin # ./oifcfg setif -global eth0/172.30.2.0:public  
<CRS_HOME>/bin # ./oifcfg setif -global eth1/172.10.0.0:cluster_interconnect  
<CRS_HOME>/bin # ./oifcfg getif  
 eth0 172.16.63.0 global public  
 eth1 10.0.0.0 global cluster_interconnect

<CRS_HOME>/bin # ./oifcfg iflist
eth0 192.168.1.0
eth1 10.10.10.0  

```shell
改 vi vipca
    arch=`uname -m`
#   if [ "$arch" = "i686" -o "$arch" = "ia64" ]
#   then
#   LD_ASSUME_KERNEL=2.4.19
#   export LD_ASSUME_KERNEL
#   fi
上面五行加上注释后，再./vipca
```
