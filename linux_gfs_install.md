# linux_gfs_install

```shell  
安装包：
cman rgmanager system-config-cluster gfs* lvm2-cluster* kmod-gfs*
====================================================================
INFO: task blocked for more than 120 seconds.解决办法
tast umount.gfs2:8767 blocked for more than 120 seconds.解决方法
简单讲就是设置在文件 /etc/sysctl.conf中加入 “vm.dirty_ratio=10” 。
====================================================================
hosts:
172.22.0.55 udb01
172.22.0.56 udb02
172.22.0.57 vip

chkconfig clvmd on
export DISPLAY
集群名：zxt_udb
Cluster：
 Cluster Nodes:
  udb01
  udb02
 Fence Devices:
  udbfen
 Managed Resources:
  Failover Domains:
   udbdom
  Resources:
   IP Address:172.22.0.57
  Services:
   Service udbcluster
fdisk -l
fdisk /dev/sdb
partprobe /dev/sdb
pvcreate /dev/sdb1
vgcreate vg0 /dev/sdb1
vgdisplay
lvcreate -L 500G -I64 -n lv0 vg0
service clvmd restart
lvscan
vgchange -ay vg0 #上一步未激活时需要激活
[root@udb02 vg0]# mkfs.gfs2 -t zxt_udb:udbfen -p lock_dlm -j 3 /dev/vg0/lv0
This will destroy any data on /dev/vg0/lv0.
Are you sure you want to proceed? [y/n] y
Device:                    /dev/vg0/lv0
Blocksize:                 4096
Device Size                499.99 GB (131069952 blocks)
Filesystem Size:           499.99 GB (131069951 blocks)
Journals:                  3
Resource Groups:           2000
Locking Protocol:          "lock_dlm"
Lock Table:                "zxt_udb:udbfen"
UUID:                      39EB0CEF-4F5B-0739-B24B-1CE680D59E23
chkconfig cman on
chkconfig rgmanager on
chkconfig gfs2 on
--------------------------------------------------------
mkfs.gfs2 -t zxt_udb:udbfs001 -p lock_dlm -j 3 /dev/vg0/lv0  (LINUX 5.3/5.4 X86_64 版要用这个）
mount /dev/vg0/lv0 /u01
chkconfig cman on
chkconfig rgmanager on
chkconfig gfs2 on
```
