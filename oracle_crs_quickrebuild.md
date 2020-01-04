# oracle_crs_quickrebuild

```shell
Quick steps to rebuild the CRS
Root user to perform. the following script
on node1
------------
cd $ORA_CRS_HOME/install
./rootdelete.sh
on node2
------------
cd $ORA_CRS_HOME/install
./rootdelete.sh
on node1 only
--------------
cd $ORA_CRS_HOME/install
./rootdeinstall.sh
cd $ORA_CRS_HOME
./root.sh
on node2
-----------
cd $ORA_CRS_HOME
./root.sh
use oracle user  
use netca to delete and add the listener to nodeapps on both nodes
rac1-> srvctl add asm -n rac1 -i +ASM1 -o /u01/app/oracle/product/10.2.0/db_1
rac1-> srvctl add asm -n rac2 -i +ASM2 -o /u01/app/oracle/product/10.2.0/db_1
rac1-> srvctl start asm -n rac1
rac1-> srvctl start asm -n rac2
if ora-27550 then set the parameter of cluster_interconnects on both ASM instances
rac1-> srvctl add database -d orcl -o /u01/app/oracle/product/10.2.0/db_1
rac1-> srvctl add instance -d orcl -i orcl1 -n rac1
rac1-> srvctl add instance -d orcl -i orcl2 -n rac2
rac1-> srvctl modify instance -d orcl -i orcl1 -s +ASM1
rac1-> srvctl modify instance -d orcl -i orcl2 -s +ASM2
rac1-> srvctl start database -d orcl
rac1-> srvctl add service -d orcl -s icm_taf -r orcl1,orcl2 -P BASIC
rac1-> srvctl start service -d orcl -s icm_taf
============oracle 11g rac ==========
node1、node2
/u01/app/11.2.0/grid/crs/install/rootcrs.pl
```
