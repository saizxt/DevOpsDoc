# linux_gfs_install_lgh

```shell
虚拟FENCE中的DOMAIN 是指本机机器名
yum -y install cman rgmanager system-config-cluster gfs* lvm2-cluster* kmod-gfs*
或(用luci客户端来管理配置)
yum -y install ricci system-config-cluster gfs* lvm2-cluster* kmod-gfs*
====================================================================
INFO: task blocked for more than 120 seconds.解决办法
tast umount.gfs2:8767 blocked for more than 120 seconds.解决方法
简单讲就是设置在文件 /etc/sysctl.conf中加入 “vm.dirty_ratio=10” 。
====================================================================
[root@sybase02 ~]# rpm -qa |grep gfs
gfs2-utils-0.1.53-1.el5
kmod-gfs-xen-0.1.31-3.el5
kmod-gfs-0.1.31-3.el5
gfs-utils-0.1.18-1.el5
kmod-gfs-PAE-0.1.31-3.el5
====================================================================
rpm -ivh lvm2-cluster-2.02.40-7.el5.i386.rpm
yum -y install gfs*
chkconfig clvmd on

export

fdisk -l
fdisk /dev/sdb (n\p\1\回车\回车\t\8e\p  用8e 好像不能umout掉，n\p\1\回车\回车。。。好像才能umount ?)
partprobe /dev/sdb
pvcreate /dev/sdb1  (如不行，加-ff参数)  
（pvcreate /dev/sdb ）
vgcreate vg0 /dev/sdb1
vgdisplay
lvcreate -L 500G -I64 -n lv0 vg0  (i是有两块盘)
    ( lvcreate -l 76798 -I64 -n myfs01 vg0 或 lvcreate -l +76798 -I64 -n myfs01 vg0 )
service clvmd restart
vgchange -ay vg0 (如果只有一边激活的话，需要手工激活)
lvscan (两边看一下是否可用)
gfs_mkfs -p lock_dlm -t gh_cluster:mysqlfs -j 3 /dev/vg0/mysqlfs

mkfs.gfs2 -t gh_002:ghfs -p lock_dlm -j 3 /dev/vg0/ghgfs  
=======
clusvcadm -r udbcluster (切换测试)
mkfs.gfs2 -t lgh:mygfs -p lock_dlm -j 3 lvmdevice  
mkfs.gfs2 -t gh56:orafs56 -p lock_dlm -j 3 /dev/vg5602/orafs5602 (集群名:lv名,lv名可随意)
mkfs.gfs -t lgh_cluster:sybasefs -p lock_dlm -j 3 /dev/vg0/lghlv  (LINUX 5.3/5.4 X86_64 版要用这个）
mount /dev/vg0/lghgfs01 /u01
mount /dev/vg0/mysqlfs /mysql
chkconfig cman on
chkconfig rgmanager on
chkconfig gfs2 on
===========
clustat -i 1
lvs lvdisplay vgscan lvscan vgdisplay -v vgdisplay vg0
====================
mount -o lockproto=lock_nolock /dev/vg0/lghgfs01 /u01   (集群坏了，以单机挂载)
mount -o lockproto=lock_nolock /dev/vg0/lv0 /u01    (号百)
ifconfig eth0:1 172.30.0.240 netmask 255.255.255.0       (号百)
===============================

ora_udbcls    (集群名)
mkfs.gfs -t ora_udbcls:lv0 -p lock_dlm -j 3 /dev/vg0/lv0
```
