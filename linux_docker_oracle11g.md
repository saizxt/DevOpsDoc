# linux_docker_oracle11g_install

VMware Workstation:9.0
操作系统:centos6.5
[root@dockerServer Desktop]# uname -a
Linux dockerServer 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
docker版本:
[root@dockerServer Desktop]# docker version
Client version: 1.2.0
Client API version: 1.14
Go version (client): go1.3.3
Git commit (client): fa7b24f/1.2.0
OS/Arch (client): linux/amd64
Server version: 1.2.0
Server API version: 1.14
Go version (server): go1.3.3
Git commit (server): fa7b24f/1.2.0
该例子前提dokcer已安装。基础镜像已生成。前面章节有介绍。

## 第一步:查看images

[root@dockerServer Desktop]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
centos6.5           oracle              08962d48775f        15 hours ago        417.5 MB

## 第二步:启动容器

[root@dockerServer Desktop]# docker run -d -P centos6.5:oracle
da88acdb6106b887e756eb93530fe962d20220cfdf16f89219d4e4bd4e971a78

## 第三步:查看容器

[root@dockerServer Desktop]# docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                   NAMES
da88acdb6106        centos6.5:oracle    "/bin/sh -c '/usr/sb   3 seconds ago       Up 1 seconds        0.0.0.0:49153->22/tcp   agitated_curie

## 第四步:ssh登录

[root@dockerServer Desktop]# ssh root@127.0.0.1 -p 49153
The authenticity of host '[127.0.0.1]:49153 ([127.0.0.1]:49153)' can't be established.
RSA key fingerprint is 9c:b5:88:5c:d7:99:71:03:2c:e7:39:e9:eb:fb:14:fb.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[127.0.0.1]:49153' (RSA) to the list of known hosts.
root@127.0.0.1's password:
Last login: Tue Nov  4 00:56:57 2014 from 172.17.42.1

## 第五步:安装gcc包和glibc包

-bash-4.1# yum -y install gcc*
-bash-4.1# yum -y install glibc*

## 第六步:查看主机名或者修改主机名

-bash-4.1# cat /etc/sysconfig/network
NETWORKING=yes
HOSTNAME=oracle11gServer
注:network可能不存在，创建即可。内容如上。

## 第七步:修改host文件

```shell
-bash-4.1# cat /etc/hosts
# 172.17.0.2    da88acdb6106
127.0.0.1       oracle11gServer
::1     localhost ip6-localhost ip6-loopback
# fe00::0       ip6-localnet
# ff00::0       ip6-mcastprefix
# ff02::1       ip6-allnodes
# ff02::2       ip6-allrouters
注:127.0.0.1对应的主机名，是network文件中配置的。
```

## 第八步:创建oracle用户和dba组

-bash-4.1# groupadd dba
-bash-4.1# useradd -g dba oracle
-bash-4.1# passwd oracle
Changing password for user oracle.
New password:
BAD PASSWORD: it is based on a dictionary word
BAD PASSWORD: is too simple
Retype new password:
passwd: all authentication tokens updated successfully.

## 第九步:创建oracle目录和设置权限

-bash-4.1# mkdir -p /u01/app/oracle
-bash-4.1# mkdir -p /u01/app/product/11.2.0/db_1
-bash-4.1# chown -R oracle:dba /u01
-bash-4.1# chmod -R 775 /u01

## 第十步:修改oracle用户的.bash_profile文件

```shell
-bash-4.1# cat /home/oracle/.bash_profile
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/product/11.2.0/db_1
export ORACLE_SID=orcl
export PATH=$PATH:$ORACLE_HOME/bin
```

## 第十一步:上传oracle11g_x86_64安装文件，并解压

-bash-4.1# unzip linux.x64_11gR2_database_1of2.zip -d ./oracle
-bash-4.1# unzip linux.x64_11gR2_database_2of2.zip -d ./oracle
注：一定要下载正确的oracle软件版本。否则后面安装不会成功。本例中操作系统是64位，所以下载的oracle11g也应该是64位。

## 第十二步:解压的目录

-bash-4.1# ls
linux.x64_11gR2_database_1of2.zip  linux.x64_11gR2_database_2of2.zip  oracle

## 第十三步:复制rsp文件到/home/oracle目录下

-bash-4.1# cp /usr/software/oracle/database/response/* /home/oracle/

## 第十四步:查看/home/oracle目录下的rsp文件

-bash-4.1# ls
db_install.rsp  dbca.rsp  netca.rsp

## 第十五步:修改db_install.rsp文件内容如下

```shell
-bash-4.1# cat db_install.rsp
####################################################################
## Copyright(c) Oracle Corporation 1998,2008. All rights reserved.##
##                                                                ##
## Specify values for the variables listed below to customize     ##
## your installation.                                             ##
##                                                                ##
## Each variable is associated with a comment. The comment        ##
## can help to populate the variables with the appropriate        ##
## values.                                                        ##
##                                                                ##
## IMPORTANT NOTE: This file contains plain text passwords and    ##
## should be secured to have read permission only by oracle user  ##
## or db administrator who owns this installation.                ##
##                                                                ##
####################################################################
#------------------------------------------------------------------------------
# Do not change the following system generated value.
#------------------------------------------------------------------------------
oracle.install.responseFileVersion=/oracle/install/rspfmt_dbinstall_response_schema_v11_2_0
#------------------------------------------------------------------------------
# Specify the installation option.
# It can be one of the following:
# 1. INSTALL_DB_SWONLY
# 2. INSTALL_DB_AND_CONFIG
# 3. UPGRADE_DB
#-------------------------------------------------------------------------------
oracle.install.option=INSTALL_DB_SWONLY
#-------------------------------------------------------------------------------
# Specify the hostname of the system as set during the install. It can be used
# to force the installation to use an alternative hostname rather than using the
# first hostname found on the system. (e.g., for systems with multiple hostnames
# and network interfaces)
#-------------------------------------------------------------------------------
ORACLE_HOSTNAME=oracle11gServer
#-------------------------------------------------------------------------------
# Specify the Unix group to be set for the inventory directory.
#-------------------------------------------------------------------------------
UNIX_GROUP_NAME=dba
#-------------------------------------------------------------------------------
# Specify the location which holds the inventory files.
#-------------------------------------------------------------------------------
INVENTORY_LOCATION=/u01/app/oracle/oraInventory
#-------------------------------------------------------------------------------
# Specify the languages in which the components will be installed.  
#
# en   : English                  ja   : Japanese  
# fr   : French                   ko   : Korean
# ar   : Arabic                   es   : Latin American Spanish
# bn   : Bengali                  lv   : Latvian
# pt_BR: Brazilian Portuguese     lt   : Lithuanian
# bg   : Bulgarian                ms   : Malay
# fr_CA: Canadian French          es_MX: Mexican Spanish
# ca   : Catalan                  no   : Norwegian
# hr   : Croatian                 pl   : Polish
# cs   : Czech                    pt   : Portuguese
# da   : Danish                   ro   : Romanian
# nl   : Dutch                    ru   : Russian
# ar_EG: Egyptian                 zh_CN: Simplified Chinese
# en_GB: English (Great Britain)  sk   : Slovak
# et   : Estonian                 sl   : Slovenian
# fi   : Finnish                  es_ES: Spanish
# de   : German                   sv   : Swedish
# el   : Greek                    th   : Thai
# iw   : Hebrew                   zh_TW: Traditional Chinese
# hu   : Hungarian                tr   : Turkish
# is   : Icelandic                uk   : Ukrainian
# in   : Indonesian               vi   : Vietnamese
# it   : Italian
#
# Example : SELECTED_LANGUAGES=en,fr,ja
#------------------------------------------------------------------------------
SELECTED_LANGUAGES=en,zh_CN
#------------------------------------------------------------------------------
# Specify the complete path of the Oracle Home.
#------------------------------------------------------------------------------
ORACLE_HOME=/u01/app/product/11.2.0/db_1
#------------------------------------------------------------------------------
# Specify the complete path of the Oracle Base.
#------------------------------------------------------------------------------
ORACLE_BASE=/u01/app/oracle
#------------------------------------------------------------------------------
# Specify the installation edition of the component.
#
# The value should contain only one of these choices.
# EE     : Enterprise Edition
# SE     : Standard Edition
# SEONE  : Standard Edition One
# PE     : Personal Edition (WINDOWS ONLY)
#------------------------------------------------------------------------------
oracle.install.db.InstallEdition=EE
#------------------------------------------------------------------------------
# This variable is used to enable or disable custom install.
#
# true  : Components mentioned as part of 'customComponents' property
#         are considered for install.
# false : Value for 'customComponents' is not considered.
#------------------------------------------------------------------------------
oracle.install.db.isCustomInstall=true
#------------------------------------------------------------------------------
# This variable is considered only if 'IsCustomInstall' is set to true.
#
# Description: List of Enterprise Edition Options you would like to install.
#
#              The following choices are available. You may specify any
#              combination of these choices.  The components you choose should
#              be specified in the form "internal-component-name:version"
#              Below is a list of components you may specify to install.
#
#              oracle.rdbms.partitioning:11.2.0.1.0 - Oracle Partitioning
#              oracle.rdbms.dm:11.2.0.1.0 - Oracle Data Mining
#              oracle.rdbms.dv:11.2.0.1.0 - Oracle Database Vault
#              oracle.rdbms.lbac:11.2.0.1.0 - Oracle Label Security
#              oracle.rdbms.rat:11.2.0.1.0 - Oracle Real Application Testing
#              oracle.oraolap:11.2.0.1.0 - Oracle OLAP
#------------------------------------------------------------------------------
oracle.install.db.customComponents=oracle.server:11.2.0.1.0,oracle.sysman.ccr:10.2.7.0.0,oracle.xdk:11.2.0.1.0,oracle.rdbms.oci:11.2.0.1.0,oracle.network:11.2.0.1.0,oracle.network.listener:11.2.0.1.0,oracle.rdbms:11.2.0.1.0,oracle.options:11.2.0.1.0,oracle.rdbms.partitioning:11.2.0.1.0,oracle.oraolap:11.2.0.1.0,oracle.rdbms.dm:11.2.0.1.0,oracle.rdbms.dv:11.2.0.1.0,orcle.rdbms.lbac:11.2.0.1.0,oracle.rdbms.rat:11.2.0.1.0
###############################################################################
#                                                                             #
# PRIVILEGED OPERATING SYSTEM GROUPS                                          #
# ------------------------------------------                                  #
# Provide values for the OS groups to which OSDBA and OSOPER privileges       #
# needs to be granted. If the install is being performed as a member of the   #
# group "dba", then that will be used unless specified otherwise below.       #
#                                                                             #
###############################################################################
#------------------------------------------------------------------------------
# The DBA_GROUP is the OS group which is to be granted OSDBA privileges.
#------------------------------------------------------------------------------
oracle.install.db.DBA_GROUP=dba
#------------------------------------------------------------------------------
# The OPER_GROUP is the OS group which is to be granted OSOPER privileges.
#------------------------------------------------------------------------------
oracle.install.db.OPER_GROUP=dba
#------------------------------------------------------------------------------
# Specify the cluster node names selected during the installation.
#------------------------------------------------------------------------------
oracle.install.db.CLUSTER_NODES=
#------------------------------------------------------------------------------
# Specify the type of database to create.
# It can be one of the following:
# - GENERAL_PURPOSE/TRANSACTION_PROCESSING
# - DATA_WAREHOUSE
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.type=GENERAL_PURPOSE
#------------------------------------------------------------------------------
# Specify the Starter Database Global Database Name.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.globalDBName=orcl
#------------------------------------------------------------------------------
# Specify the Starter Database SID.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.SID=orcl
#------------------------------------------------------------------------------
# Specify the Starter Database character set.
#
# It can be one of the following:
# AL32UTF8, WE8ISO8859P15, WE8MSWIN1252, EE8ISO8859P2,
# EE8MSWIN1250, NE8ISO8859P10, NEE8ISO8859P4, BLT8MSWIN1257,
# BLT8ISO8859P13, CL8ISO8859P5, CL8MSWIN1251, AR8ISO8859P6,
# AR8MSWIN1256, EL8ISO8859P7, EL8MSWIN1253, IW8ISO8859P8,
# IW8MSWIN1255, JA16EUC, JA16EUCTILDE, JA16SJIS, JA16SJISTILDE,
# KO16MSWIN949, ZHS16GBK, TH8TISASCII, ZHT32EUC, ZHT16MSWIN950,
# ZHT16HKSCS, WE8ISO8859P9, TR8MSWIN1254, VN8MSWIN1258
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.characterSet=AL32UTF8
#------------------------------------------------------------------------------
# This variable should be set to true if Automatic Memory Management
# in Database is desired.
# If Automatic Memory Management is not desired, and memory allocation
# is to be done manually, then set it to false.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.memoryOption=true
#------------------------------------------------------------------------------
# Specify the total memory allocation for the database. Value(in MB) should be
# at least 256 MB, and should not exceed the total physical memory available
# on the system.
# Example: oracle.install.db.config.starterdb.memoryLimit=512
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.memoryLimit=512
#------------------------------------------------------------------------------
# This variable controls whether to load Example Schemas onto the starter
# database or not.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.installExampleSchemas=false
#------------------------------------------------------------------------------
# This variable includes enabling audit settings, configuring password profiles
# and revoking some grants to public. These settings are provided by default.
# These settings may also be disabled.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.enableSecuritySettings=true
###############################################################################
#                                                                             #
# Passwords can be supplied for the following four schemas in the             #
# starter database:                                                           #
#   SYS                                                                       #
#   SYSTEM                                                                    #
#   SYSMAN (used by Enterprise Manager)                                       #
#   DBSNMP (used by Enterprise Manager)                                       #
#                                                                             #
# Same password can be used for all accounts (not recommended)                #
# or different passwords for each account can be provided (recommended)       #
#                                                                             #
###############################################################################
#------------------------------------------------------------------------------
# This variable holds the password that is to be used for all schemas in the
# starter database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.password.ALL=oracle
#-------------------------------------------------------------------------------
# Specify the SYS password for the starter database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.password.SYS=
#-------------------------------------------------------------------------------
# Specify the SYSTEM password for the starter database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.password.SYSTEM=
#-------------------------------------------------------------------------------
# Specify the SYSMAN password for the starter database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.password.SYSMAN=
#-------------------------------------------------------------------------------
# Specify the DBSNMP password for the starter database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.password.DBSNMP=
#-------------------------------------------------------------------------------
# Specify the management option to be selected for the starter database.
# It can be one of the following:
# 1. GRID_CONTROL
# 2. DB_CONTROL
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.control=DB_CONTROL
#-------------------------------------------------------------------------------
# Specify the Management Service to use if Grid Control is selected to manage
# the database.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.gridcontrol.gridControlServiceURL=
#-------------------------------------------------------------------------------
# This variable indicates whether to receive email notification for critical
# alerts when using DB control.  
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.dbcontrol.enableEmailNotification=false
#-------------------------------------------------------------------------------
# Specify the email address to which the notifications are to be sent.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.dbcontrol.emailAddress=
#-------------------------------------------------------------------------------
# Specify the SMTP server used for email notifications.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.dbcontrol.SMTPServer=

###############################################################################
#                                                                             #
# SPECIFY BACKUP AND RECOVERY OPTIONS                                         #
# ------------------------------------                                        #
# Out-of-box backup and recovery options for the database can be mentioned    #
# using the entries below.                                                    #
#                                                                             #
###############################################################################
#------------------------------------------------------------------------------
# This variable is to be set to false if automated backup is not required. Else
# this can be set to true.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.automatedBackup.enable=false
#------------------------------------------------------------------------------
# Regardless of the type of storage that is chosen for backup and recovery, if
# automated backups are enabled, a job will be scheduled to run daily at
# 2:00 AM to backup the database. This job will run as the operating system
# user that is specified in this variable.
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.automatedBackup.osuid=
#-------------------------------------------------------------------------------
# Regardless of the type of storage that is chosen for backup and recovery, if
# automated backups are enabled, a job will be scheduled to run daily at
# 2:00 AM to backup the database. This job will run as the operating system user
# specified by the above entry. The following entry stores the password for the
# above operating system user.
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.automatedBackup.ospwd=
#-------------------------------------------------------------------------------
# Specify the type of storage to use for the database.
# It can be one of the following:
# - FILE_SYSTEM_STORAGE
# - ASM_STORAGE
#------------------------------------------------------------------------------
oracle.install.db.config.starterdb.storageType=
#-------------------------------------------------------------------------------
# Specify the database file location which is a directory for datafiles, control
# files, redo logs.
#
# Applicable only when oracle.install.db.config.starterdb.storage=FILE_SYSTEM
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.fileSystemStorage.dataLocation=
#-------------------------------------------------------------------------------
# Specify the backup and recovery location.
#
# Applicable only when oracle.install.db.config.starterdb.storage=FILE_SYSTEM
#-------------------------------------------------------------------------------
oracle.install.db.config.starterdb.fileSystemStorage.recoveryLocation=
#-------------------------------------------------------------------------------
# Specify the existing ASM disk groups to be used for storage.
#
# Applicable only when oracle.install.db.config.starterdb.storage=ASM
#-------------------------------------------------------------------------------
oracle.install.db.config.asm.diskGroup=
#-------------------------------------------------------------------------------
# Specify the password for ASMSNMP user of the ASM instance.
#
# Applicable only when oracle.install.db.config.starterdb.storage=ASM_SYSTEM
#-------------------------------------------------------------------------------
oracle.install.db.config.asm.ASMSNMPPassword=
#------------------------------------------------------------------------------
# Specify the My Oracle Support Account Username.
#
#  Example   : MYORACLESUPPORT_USERNAME=metalink
#------------------------------------------------------------------------------
MYORACLESUPPORT_USERNAME=
#------------------------------------------------------------------------------
# Specify the My Oracle Support Account Username password.
#
# Example    : MYORACLESUPPORT_PASSWORD=password
#------------------------------------------------------------------------------
MYORACLESUPPORT_PASSWORD=
#------------------------------------------------------------------------------
# Specify whether to enable the user to set the password for
# My Oracle Support credentials. The value can be either true or false.
# If left blank it will be assumed to be false.
#
# Example    : SECURITY_UPDATES_VIA_MYORACLESUPPORT=true
#------------------------------------------------------------------------------
SECURITY_UPDATES_VIA_MYORACLESUPPORT=
#------------------------------------------------------------------------------
# Specify whether user wants to give any proxy details for connection.
# The value can be either true or false. If left blank it will be assumed
# to be false.
#
# Example    : DECLINE_SECURITY_UPDATES=false
#------------------------------------------------------------------------------
DECLINE_SECURITY_UPDATES=true
#------------------------------------------------------------------------------
# Specify the Proxy server name. Length should be greater than zero.
#
# Example    : PROXY_HOST=proxy.domain.com
#------------------------------------------------------------------------------
PROXY_HOST=
#------------------------------------------------------------------------------
# Specify the proxy port number. Should be Numeric and atleast 2 chars.
#
# Example    : PROXY_PORT=25
#------------------------------------------------------------------------------
PROXY_PORT=
#------------------------------------------------------------------------------
# Specify the proxy user name. Leave PROXY_USER and PROXY_PWD
# blank if your proxy server requires no authentication.
#
# Example    : PROXY_USER=username
#------------------------------------------------------------------------------
PROXY_USER=
#------------------------------------------------------------------------------
# Specify the proxy password. Leave PROXY_USER and PROXY_PWD
# blank if your proxy server requires no authentication.
#
# Example    : PROXY_PWD=password
#------------------------------------------------------------------------------
PROXY_PWD=
```

## 第十六步:开始安装oracle11g软件,在此我们只能选择silent(静默)安装装，不能使用图形界面，所以要配置以上文件

```shell
-bash-4.1# su - oracle
[oracle@f24801aaf1b5 database]$ ./runInstaller -ignoreSysPrereqs -ignorePrereq -silent -responseFile /home/oracle/db_install.rsp
Starting Oracle Universal Installer...
Checking Temp space: must be greater than 120 MB.   Actual 6277 MB    Passed
Checking swap space: must be greater than 150 MB.   Actual 3967 MB    Passed
Preparing to launch Oracle Universal Installer from /tmp/OraInstall2014-11-06_09-31-53PM. Please wait ...[oracle@f24801aaf1b5 database]$ [WARNING] [INS-32018] The selected Oracle home is outside of Oracle base.
   CAUSE: The Oracle home selected was outside of Oracle base.
   ACTION: Oracle recommends installing Oracle software within the Oracle base directory. Adjust the Oracle home or Oracle base accordingly.
[WARNING] [INS-32055] The Central Inventory is located in the Oracle base.
   CAUSE: The Central Inventory is located in the Oracle base.
   ACTION: Oracle recommends placing this Central Inventory in a location outside the Oracle base directory.
[WARNING] [INS-32018] The selected Oracle home is outside of Oracle base.
   CAUSE: The Oracle home selected was outside of Oracle base.
   ACTION: Oracle recommends installing Oracle software within the Oracle base directory. Adjust the Oracle home or Oracle base accordingly.
[WARNING] [INS-32055] The Central Inventory is located in the Oracle base.
   CAUSE: The Central Inventory is located in the Oracle base.
   ACTION: Oracle recommends placing this Central Inventory in a location outside the Oracle base directory.
You can find the log of this install session at:
 /u01/app/oracle/oraInventory/logs/installActions2014-11-06_09-31-53PM.log
```

## 第十七步:查看安装日志

```shell
[oracle@f24801aaf1b5 database]$ tail -100f /u01/app/oracle/oraInventory/logs/installActions2014-11-06_09-31-53PM.log
看到此输出，所明软件安装完成
/u01/app/oracle/oraInventory/orainstRoot.sh
/u01/app/product/11.2.0/db_1/root.sh
To execute the configuration scripts:
         1. Open a terminal window
         2. Log in as "root"
         3. Run the scripts
         4. Return to this window and hit "Enter" key to continue
INFO: Shutting down OUISetupDriver.JobExecutorThread
INFO: Cleaning up, please wait...
INFO: Dispose the install area control object
INFO: Update the state machine to STATE_CLEAN
Successfully Setup Software.
INFO: All forked task are completed at state setup
INFO: Completed background operations
INFO: Moved to state
INFO: Waiting for completion of background operations
INFO: Completed background operations
INFO: Validating state
WARNING: Validation disabled for the state setup
INFO: Completed validating state
INFO: Verifying route success
INFO: Waiting for completion of background operations
INFO: Completed background operations
INFO: Executing action at state finish
INFO: FinishAction Actions.execute called
INFO: Completed executing action at state
INFO: Waiting for completion of background operations
INFO: Completed background operations
INFO: Moved to state
INFO: Waiting for completion of background operations
INFO: Completed background operations
INFO: Validating state
WARNING: Validation disabled for the state finish
INFO: Completed validating state
INFO: Terminating all background operations
INFO: Terminated all background operations
INFO: Successfully executed the flow in SILENT mode
INFO: Finding the most appropriate exit status for the current application
INFO: Exit Status is 0
INFO: Shutdown Oracle Database 11g Release 2 Installer
```

## 第十八步:按照日志输出，执行脚本root用户

```shell
-bash-4.1# /u01/app/oracle/oraInventory/orainstRoot.sh
Changing permissions of /u01/app/oracle/oraInventory.
Adding read,write permissions for group.
Removing read,write,execute permissions for world.
Changing groupname of /u01/app/oracle/oraInventory to dba.
The execution of the script is complete.
-bash-4.1# /u01/app/product/11.2.0/db_1/root.sh
Check /u01/app/product/11.2.0/db_1/install/root_f24801aaf1b5_2014-11-06_21-42-13.log for the output of root script
```

## 第十九步:查看root.sh脚本日志

```shell
-bash-4.1# cat  /u01/app/product/11.2.0/db_1/install/root_f24801aaf1b5_2014-11-06_21-42-13.log
Running Oracle 11g root.sh script...
The following environment variables are set as:
    ORACLE_OWNER= oracle
    ORACLE_HOME=  /u01/app/product/11.2.0/db_1
Creating /etc/oratab file...
Entries will be added to the /etc/oratab file as needed by
Database Configuration Assistant when a database is created
Finished running generic part of root.sh script.
Now product-specific root actions will be performed.
Finished product-specific root actions.
到此。oracle11g软件安装成功。
```

## 第二十步:安装listener

```shell
[oracle@f24801aaf1b5 admin]$ netca /silent /responsefile /home/oracle/netca.rsp
UnsatisfiedLinkError exception loading native library: njni11
java.lang.UnsatisfiedLinkError: /u01/app/product/11.2.0/db_1/lib/libnjni11.so: libaio.so.1: cannot open shared object file: No such file or directory
java.lang.UnsatisfiedLinkError: jniGetOracleHome
        at oracle.net.common.NetGetEnv.jniGetOracleHome(Native Method)
        at oracle.net.common.NetGetEnv.getOracleHome(Unknown Source)
        at oracle.net.ca.NetCALogger.getOracleHome(NetCALogger.java:230)
        at oracle.net.ca.NetCALogger.initOracleParameters(NetCALogger.java:215)
        at oracle.net.ca.NetCALogger.initLogger(NetCALogger.java:130)
        at oracle.net.ca.NetCA.main(NetCA.java:404)
Error: jniGetOracleHome
Oracle Net Services configuration failed.  The exit code is 1
启动listener报错，查看oracle的lib目录下没有libaio.so.1包，
-bash-4.1# yum -y install libaio*
安装好后，将libaio.so.1文件复制到$ORACLE_HOME/lib目录下。
重新安装listener
[oracle@f24801aaf1b5 ~]$ netca /silent /responsefile /home/oracle/netca.rsp
Parsing command line arguments:
    Parameter "silent" = true
    Parameter "responsefile" = /home/oracle/netca.rsp
Done parsing command line arguments.
Oracle Net Services Configuration:
Profile configuration complete.
Oracle Net Listener Startup:
    Running Listener Control:
      /u01/app/product/11.2.0/db_1/bin/lsnrctl start LISTENER
    Listener Control complete.
    Listener started successfully.
Listener configuration complete.
Oracle Net Services configuration successful. The exit code is 0
成功安装
```

## 第二十一步:查看listener

```shell
[oracle@f24801aaf1b5 ~]$ lsnrctl status
LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 06-NOV-2014 22:01:52
Copyright (c) 1991, 2009, Oracle.  All rights reserved.
Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
Start Date                06-NOV-2014 21:59:15
Uptime                    0 days 0 hr. 2 min. 38 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/f24801aaf1b5/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=f24801aaf1b5)(PORT=1521)))
The listener supports no services
The command completed successfully
到此，listener安装成功。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。。
第二十二步:安装实例前可以修改dbca.rsp内容
主要修改globalname和sid就行了,太长了，不贴内容了
[oracle@f24801aaf1b5 ~]$ dbca -silent -responseFile /home/oracle/dbca.rsp  
Enter SYS user password:

Enter SYSTEM user password:

sh: /bin/ksh: No such file or directory
sh: /bin/ksh: No such file or directory
Copying database files
1% complete
3% complete
11% complete
18% complete
26% complete
37% complete
Creating and starting Oracle instance
40% complete
45% complete
50% complete
55% complete
56% complete
60% complete
62% complete
Completing Database Creation
66% complete
70% complete
73% complete
85% complete
96% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/oracle11g/oracle11.log" for further details.
```

## 第二十三步:查看日志

```shell
[oracle@f24801aaf1b5 ~]$ cat /u01/app/oracle/cfgtoollogs/dbca/oracle11g/oracle11.log
Following parameters do not meet the recommended minimum size requirements:
An SGA size of at least 210MB is recommended.
Do you want Database Configuration Assistant to change the parameters?.
Copying database files
DBCA_PROGRESS : 1%
DBCA_PROGRESS : 3%
DBCA_PROGRESS : 11%
DBCA_PROGRESS : 18%
DBCA_PROGRESS : 26%
DBCA_PROGRESS : 37%
Creating and starting Oracle instance
DBCA_PROGRESS : 40%
DBCA_PROGRESS : 45%
DBCA_PROGRESS : 50%
DBCA_PROGRESS : 55%
DBCA_PROGRESS : 56%
DBCA_PROGRESS : 60%
DBCA_PROGRESS : 62%
Completing Database Creation
DBCA_PROGRESS : 66%
DBCA_PROGRESS : 70%
DBCA_PROGRESS : 73%
DBCA_PROGRESS : 85%
DBCA_PROGRESS : 96%
DBCA_PROGRESS : 100%
Database creation complete. For details check the logfiles at:
 /u01/app/oracle/cfgtoollogs/dbca/oracle11g.
Database Information:
Global Database Name:oracle11g
System Identifier(SID):orcl
```

## 第二十四步:查看listener，有没有实例

```shell
[oracle@f24801aaf1b5 ~]$ lsnrctl status
LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 06-NOV-2014 22:17:09
Copyright (c) 1991, 2009, Oracle.  All rights reserved.
Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------
Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
Start Date                06-NOV-2014 21:59:15
Uptime                    0 days 0 hr. 17 min. 55 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/f24801aaf1b5/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=f24801aaf1b5)(PORT=1521)))
Services Summary...
Service "oracle11g" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
Service "orclXDB" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
The command completed successfully
```

## 第二十五步:尝试连接sqlplus

```shell
[oracle@f24801aaf1b5 ~]$ sqlplus / as sysdba
SQL*Plus: Release 11.2.0.1.0 Production on Thu Nov 6 22:18:21 2014
Copyright (c) 1982, 2009, Oracle.  All rights reserved.

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, Oracle Label Security, OLAP, Data Mining,
Oracle Database Vault and Real Application Testing options
SQL> select instance_name from v$instance;
INSTANCE_NAME
----------------
orcl
到次docker下成功完成oracle11g实例的安装^_^
```

## windows系统下客户端pl/sql连接docker

### 第一步:查看镜像

```shell
[root@dockerServer ~]# docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
centos6.5/orcl11g   fs                  2c4372b5fb84        2 hours ago         8.831 GB
```

### 第二步:启动容器,设置相应的端口

```shell
[root@dockerServer ~]# docker run -d -p 22 -p 1521:1521 centos6.5/orcl11g:fs
```

## 第三步:查看 容器

```shell
[root@dockerServer ~]# docker ps
CONTAINER ID        IMAGE                  COMMAND                CREATED             STATUS              PORTS                                           NAMES
d2126eb6637e        centos6.5/orcl11g:fs   "/bin/sh -c '/usr/sb   39 minutes ago      Up 11 minutes       0.0.0.0:1521->1521/tcp, 0.0.0.0:49154->22/tcp   determined_hypatia
```

## 第四步:ssh连接容器

## 第五步:启动listener

```shell
[oracle@d2126eb6637e ~]$ lsnrctl start
LSNRCTL for Linux: Version 11.2.0.1.0 - Production on 07-NOV-2014 01:28:05
Copyright (c) 1991, 2009, Oracle.  All rights reserved.
Connecting to (DESCRIPTION=(ADDRESS=(PROTOCOL=IPC)(KEY=EXTPROC1521)))
STATUS of the LISTENER
------------------------

Alias                     LISTENER
Version                   TNSLSNR for Linux: Version 11.2.0.1.0 - Production
Start Date                07-NOV-2014 01:17:59
Uptime                    0 days 0 hr. 10 min. 6 sec
Trace Level               off
Security                  ON: Local OS Authentication
SNMP                      OFF
Listener Parameter File   /u01/app/product/11.2.0/db_1/network/admin/listener.ora
Listener Log File         /u01/app/oracle/diag/tnslsnr/d2126eb6637e/listener/alert/log.xml
Listening Endpoints Summary...
  (DESCRIPTION=(ADDRESS=(PROTOCOL=ipc)(KEY=EXTPROC1521)))
  (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=d2126eb6637e)(PORT=1521)))
Services Summary...
Service "oracle11g" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
Service "orclXDB" has 1 instance(s).
  Instance "orcl", status READY, has 1 handler(s) for this service...
The command completed successfully
```

## 第六步:启动数据库

```shell
[oracle@d2126eb6637e ~]$ sqlplus / as sysdba
SQL*Plus: Release 11.2.0.1.0 Production on Fri Nov 7 01:29:27 2014
Copyright (c) 1982, 2009, Oracle.  All rights reserved.

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, Oracle Label Security, OLAP, Data Mining,
Oracle Database Vault and Real Application Testing options
SQL>
```

## 第七步:windows系统配置tnsname.ora文件

```shell
docker =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.114.130)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = oracle11g)
    )
  )
```

## 第八步:用pl/sql连接

注1:192.168.114.130为宿主机IP,1521也是宿主机端口,而容器中oracle服务端口1521是映射到宿主机的1521端口
注2:在容器中启动listener,要主意$ORACLE_HOME/network/admin下的listener.ora和tnsname.ora文件中的host参数，这个参数不能写成IP,也不能写成/etc/sysconfig/network中的hostname，只能写成
容器启动时的容器ID号,也可在容器中使用命令查看主机信息uname -a
如：

```shell
[oracle@d2126eb6637e ~]$ uname -a
Linux d2126eb6637e 2.6.32-431.el6.x86_64 #1 SMP Fri Nov 22 03:15:09 UTC 2013 x86_64 x86_64 x86_64 GNU/Linux
```
