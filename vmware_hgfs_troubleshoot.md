# vmware_hgfs_troubleshoot

1. services.sh restart
2. yum -y install *headers  
3. ./vmware-config-tools.pl
4. lsmod |grep vm  
vmci
vmhgfs
vmware_balloon
5. services.sh restart
