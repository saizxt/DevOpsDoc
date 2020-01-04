# oracle_rman_backup

文章模拟数据库在有rman全库备份并在备份后有事务产生后数据库崩溃的恢复过程，欢迎交流学习。
1、rman全库备份

```shell
RMAN> backup as compressed backupset database plus archivelog;
Starting backup at 21-FEB-13
current log archived
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=54 device type=DISK
channel ORA_DISK_1: starting compressed archived log backup set
channel ORA_DISK_1: specifying archived log(s) in backup set
input archived log thread=1 sequence=5 RECID=1 STAMP=802648878
input archived log thread=1 sequence=6 RECID=2 STAMP=802872780
input archived log thread=1 sequence=7 RECID=3 STAMP=802882819
input archived log thread=1 sequence=8 RECID=4 STAMP=803830095
input archived log thread=1 sequence=9 RECID=5 STAMP=803858440
input archived log thread=1 sequence=10 RECID=6 STAMP=803925370
input archived log thread=1 sequence=11 RECID=7 STAMP=803944823
input archived log thread=1 sequence=12 RECID=8 STAMP=803988826
input archived log thread=1 sequence=13 RECID=9 STAMP=804004461
input archived log thread=1 sequence=14 RECID=10 STAMP=804033270
input archived log thread=1 sequence=15 RECID=11 STAMP=804188254
input archived log thread=1 sequence=16 RECID=12 STAMP=804188442
input archived log thread=1 sequence=17 RECID=13 STAMP=804188460
input archived log thread=1 sequence=18 RECID=14 STAMP=804188472
input archived log thread=1 sequence=19 RECID=15 STAMP=804188487
input archived log thread=1 sequence=20 RECID=16 STAMP=804188501
input archived log thread=1 sequence=21 RECID=17 STAMP=804188540
input archived log thread=1 sequence=22 RECID=18 STAMP=804188579
input archived log thread=1 sequence=23 RECID=19 STAMP=804188604
input archived log thread=1 sequence=24 RECID=20 STAMP=804188634
input archived log thread=1 sequence=25 RECID=21 STAMP=804188664
input archived log thread=1 sequence=26 RECID=22 STAMP=804188715
input archived log thread=1 sequence=27 RECID=23 STAMP=804188757
input archived log thread=1 sequence=28 RECID=24 STAMP=804188802
input archived log thread=1 sequence=29 RECID=25 STAMP=804189879
input archived log thread=1 sequence=30 RECID=26 STAMP=804197844
input archived log thread=1 sequence=31 RECID=27 STAMP=804204223
input archived log thread=1 sequence=32 RECID=28 STAMP=804211264
input archived log thread=1 sequence=33 RECID=29 STAMP=804270525
input archived log thread=1 sequence=34 RECID=30 STAMP=804290676
input archived log thread=1 sequence=35 RECID=31 STAMP=804416154
input archived log thread=1 sequence=36 RECID=32 STAMP=804426537
input archived log thread=1 sequence=37 RECID=33 STAMP=804426540
input archived log thread=1 sequence=38 RECID=34 STAMP=804426601
input archived log thread=1 sequence=39 RECID=35 STAMP=804426601
input archived log thread=1 sequence=40 RECID=36 STAMP=804463141
input archived log thread=1 sequence=41 RECID=37 STAMP=804475568
input archived log thread=1 sequence=42 RECID=38 STAMP=804520233
input archived log thread=1 sequence=43 RECID=39 STAMP=804537121
input archived log thread=1 sequence=44 RECID=40 STAMP=804560345
input archived log thread=1 sequence=45 RECID=41 STAMP=804610472
input archived log thread=1 sequence=46 RECID=42 STAMP=804628837
channel ORA_DISK_1: starting piece 1 at 21-FEB-13
channel ORA_DISK_1: finished piece 1 at 21-FEB-13
piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T200936_8ld3n2lc_.bkp tag=TAG20130221T200936 comment=NONE
channel ORA_DISK_1: backup set complete, elapsed time: 00:02:46
channel ORA_DISK_1: starting compressed archived log backup set
channel ORA_DISK_1: specifying archived log(s) in backup set
input archived log thread=1 sequence=47 RECID=43 STAMP=804673672
input archived log thread=1 sequence=48 RECID=44 STAMP=804673673
input archived log thread=1 sequence=49 RECID=45 STAMP=804709138
input archived log thread=1 sequence=50 RECID=46 STAMP=804722452
input archived log thread=1 sequence=51 RECID=47 STAMP=804796562
input archived log thread=1 sequence=52 RECID=48 STAMP=804796562
input archived log thread=1 sequence=53 RECID=49 STAMP=804808853
input archived log thread=1 sequence=54 RECID=50 STAMP=804868664
input archived log thread=1 sequence=55 RECID=51 STAMP=804868664
input archived log thread=1 sequence=56 RECID=52 STAMP=804895235
input archived log thread=1 sequence=57 RECID=53 STAMP=804951767
input archived log thread=1 sequence=58 RECID=54 STAMP=804951767
input archived log thread=1 sequence=59 RECID=55 STAMP=804981632
input archived log thread=1 sequence=60 RECID=56 STAMP=804988637
input archived log thread=1 sequence=61 RECID=57 STAMP=805032763
input archived log thread=1 sequence=62 RECID=58 STAMP=805032764
input archived log thread=1 sequence=63 RECID=59 STAMP=805068008
input archived log thread=1 sequence=64 RECID=60 STAMP=805071612
input archived log thread=1 sequence=65 RECID=61 STAMP=805239055
input archived log thread=1 sequence=66 RECID=62 STAMP=805239055
input archived log thread=1 sequence=67 RECID=63 STAMP=805240856
input archived log thread=1 sequence=68 RECID=64 STAMP=805419023
input archived log thread=1 sequence=69 RECID=65 STAMP=805419023
input archived log thread=1 sequence=70 RECID=66 STAMP=805468859
input archived log thread=1 sequence=71 RECID=67 STAMP=805468860
input archived log thread=1 sequence=72 RECID=68 STAMP=805470667
input archived log thread=1 sequence=73 RECID=69 STAMP=805500035
input archived log thread=1 sequence=74 RECID=70 STAMP=805558457
input archived log thread=1 sequence=75 RECID=71 STAMP=805558457
input archived log thread=1 sequence=76 RECID=72 STAMP=805586429
input archived log thread=1 sequence=77 RECID=73 STAMP=805590580
input archived log thread=1 sequence=78 RECID=74 STAMP=805936234
input archived log thread=1 sequence=79 RECID=75 STAMP=805936235
input archived log thread=1 sequence=80 RECID=76 STAMP=807826921
input archived log thread=1 sequence=81 RECID=77 STAMP=807826921
input archived log thread=1 sequence=82 RECID=78 STAMP=807828728
input archived log thread=1 sequence=83 RECID=79 STAMP=807828741
input archived log thread=1 sequence=84 RECID=80 STAMP=807828756
input archived log thread=1 sequence=85 RECID=81 STAMP=807828767
input archived log thread=1 sequence=86 RECID=82 STAMP=807828780
input archived log thread=1 sequence=87 RECID=83 STAMP=807830939
input archived log thread=1 sequence=88 RECID=84 STAMP=807831016
input archived log thread=1 sequence=89 RECID=85 STAMP=807832552
input archived log thread=1 sequence=90 RECID=86 STAMP=807832619
input archived log thread=1 sequence=91 RECID=87 STAMP=807832856
input archived log thread=1 sequence=92 RECID=88 STAMP=807833032
input archived log thread=1 sequence=93 RECID=89 STAMP=807833246
input archived log thread=1 sequence=94 RECID=90 STAMP=807917963
input archived log thread=1 sequence=95 RECID=91 STAMP=807917964
input archived log thread=1 sequence=96 RECID=92 STAMP=807922825
input archived log thread=1 sequence=97 RECID=93 STAMP=807998703
input archived log thread=1 sequence=98 RECID=94 STAMP=807998706
input archived log thread=1 sequence=99 RECID=95 STAMP=807998975
channel ORA_DISK_1: starting piece 1 at 21-FEB-13
channel ORA_DISK_1: finished piece 1 at 21-FEB-13
piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T200936_8ld3sb6l_.bkp tag=TAG20130221T200936 comment=NONE
channel ORA_DISK_1: backup set complete, elapsed time: 00:02:15
Finished backup at 21-FEB-13
Starting backup at 21-FEB-13
using channel ORA_DISK_1
channel ORA_DISK_1: starting compressed full datafile backup set
channel ORA_DISK_1: specifying datafile(s) in backup set
input datafile file number=00001 name=/u01/app/oracle/oradata/ORCL/datafile/o1_mf_system_8f85xnc8_.dbf
input datafile file number=00002 name=/u01/app/oracle/oradata/ORCL/datafile/o1_mf_sysaux_8f85xng5_.dbf
input datafile file number=00003 name=/u01/app/oracle/oradata/ORCL/datafile/o1_mf_undotbs1_8f85xngn_.dbf
input datafile file number=00004 name=/u01/app/oracle/oradata/ORCL/datafile/o1_mf_users_8f85xnhk_.dbf
input datafile file number=00005 name=/u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_system_m_8hhgltcb_.dbf
input datafile file number=00006 name=/u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_data_8hhgv6k5_.dbf
input datafile file number=00007 name=/u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_myspace_8hhgyr6s_.dbf
channel ORA_DISK_1: starting piece 1 at 21-FEB-13
channel ORA_DISK_1: finished piece 1 at 21-FEB-13
piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_nnndf_TAG20130221T201442_8ld3xmkz_.bkp tag=TAG20130221T201442 comment=NONE
channel ORA_DISK_1: backup set complete, elapsed time: 00:01:56
channel ORA_DISK_1: starting compressed full datafile backup set
channel ORA_DISK_1: specifying datafile(s) in backup set
including current control file in backup set
including current SPFILE in backup set
channel ORA_DISK_1: starting piece 1 at 21-FEB-13
channel ORA_DISK_1: finished piece 1 at 21-FEB-13
piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp tag=TAG20130221T201442 comment=NONE
channel ORA_DISK_1: backup set complete, elapsed time: 00:00:01
Finished backup at 21-FEB-13
Starting backup at 21-FEB-13
current log archived
using channel ORA_DISK_1
channel ORA_DISK_1: starting compressed archived log backup set
channel ORA_DISK_1: specifying archived log(s) in backup set
input archived log thread=1 sequence=100 RECID=96 STAMP=807999402
channel ORA_DISK_1: starting piece 1 at 21-FEB-13
channel ORA_DISK_1: finished piece 1 at 21-FEB-13
piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T201643_8ld41c5f_.bkp tag=TAG20130221T201643 comment=NONE
channel ORA_DISK_1: backup set complete, elapsed time: 00:00:01
Finished backup at 21-FEB-13
查看备份集
RMAN> list backup;

List of Backup Sets
===================

BS Key  Size       Device Type Elapsed Time Completion Time
------- ---------- ----------- ------------ ---------------
1       429.41M    DISK        00:02:43     21-FEB-13  
        BP Key: 1   Status: AVAILABLE  Compressed: YES  Tag: TAG20130221T200936
        Piece Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T200936_8ld3n2lc_.bkp
  List of Archived Logs in backup set 1
  Thrd Seq     Low SCN    Low Time  Next SCN   Next Time
  ---- ------- ---------- --------- ---------- ---------
  1    5       980580     21-DEC-12 1003453    21-DEC-12
  1    6       1003453    21-DEC-12 1025677    24-DEC-12
  1    7       1025677    24-DEC-12 1033979    24-DEC-12
  1    8       1033979    24-DEC-12 1075110    04-JAN-13
  1    9       1075110    04-JAN-13 1091024    04-JAN-13
  1    10      1091024    04-JAN-13 1115480    05-JAN-13
  1    11      1115480    05-JAN-13 1134086    05-JAN-13
  1    12      1134086    05-JAN-13 1157745    06-JAN-13
  1    13      1157745    06-JAN-13 1173543    06-JAN-13
  1    14      1173543    06-JAN-13 1195163    06-JAN-13
  1    15      1195163    06-JAN-13 1218029    08-JAN-13
  1    16      1218029    08-JAN-13 1239821    08-JAN-13
  1    17      1239821    08-JAN-13 1245113    08-JAN-13
  1    18      1245113    08-JAN-13 1247066    08-JAN-13
  1    19      1247066    08-JAN-13 1250262    08-JAN-13
  1    20      1250262    08-JAN-13 1254095    08-JAN-13
  1    21      1254095    08-JAN-13 1277031    08-JAN-13
  1    22      1277031    08-JAN-13 1297733    08-JAN-13
  1    23      1297733    08-JAN-13 1303340    08-JAN-13
  1    24      1303340    08-JAN-13 1304675    08-JAN-13
  1    25      1304675    08-JAN-13 1308264    08-JAN-13
  1    26      1308264    08-JAN-13 1314921    08-JAN-13
  1    27      1314921    08-JAN-13 1322143    08-JAN-13
  1    28      1322143    08-JAN-13 1325821    08-JAN-13
  1    29      1325821    08-JAN-13 1336756    08-JAN-13
  1    30      1336756    08-JAN-13 1349786    08-JAN-13
  1    31      1349786    08-JAN-13 1365132    08-JAN-13
  1    32      1365132    08-JAN-13 1379402    09-JAN-13
  1    33      1379402    09-JAN-13 1399409    09-JAN-13
  1    34      1399409    09-JAN-13 1418504    09-JAN-13
  1    35      1418504    09-JAN-13 1443637    11-JAN-13
  1    36      1443637    11-JAN-13 1471861    11-JAN-13
  1    37      1471861    11-JAN-13 1471968    11-JAN-13
  1    38      1471968    11-JAN-13 1492039    11-JAN-13
  1    39      1492039    11-JAN-13 1492102    11-JAN-13
  1    40      1492102    11-JAN-13 1538172    11-JAN-13
  1    41      1538172    11-JAN-13 1559286    12-JAN-13
  1    42      1559286    12-JAN-13 1586188    12-JAN-13
  1    43      1586188    12-JAN-13 1611419    12-JAN-13
  1    44      1611419    12-JAN-13 1642843    13-JAN-13
  1    45      1642843    13-JAN-13 1660813    13-JAN-13
  1    46      1660813    13-JAN-13 1679993    13-JAN-13
BS Key  Size       Device Type Elapsed Time Completion Time
------- ---------- ----------- ------------ ---------------
2       365.04M    DISK        00:02:14     21-FEB-13  
        BP Key: 2   Status: AVAILABLE  Compressed: YES  Tag: TAG20130221T200936
        Piece Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T200936_8ld3sb6l_.bkp
  List of Archived Logs in backup set 2
  Thrd Seq     Low SCN    Low Time  Next SCN   Next Time
  ---- ------- ---------- --------- ---------- ---------
  1    47      1679993    13-JAN-13 1718553    14-JAN-13
  1    48      1718553    14-JAN-13 1718560    14-JAN-13
  1    49      1718560    14-JAN-13 1763550    14-JAN-13
  1    50      1763550    14-JAN-13 1785240    14-JAN-13
  1    51      1785240    14-JAN-13 1816740    15-JAN-13
  1    52      1816740    15-JAN-13 1816747    15-JAN-13
  1    53      1816747    15-JAN-13 1837674    15-JAN-13
  1    54      1837674    15-JAN-13 1868266    16-JAN-13
  1    55      1868266    16-JAN-13 1868273    16-JAN-13
  1    56      1868273    16-JAN-13 1905139    16-JAN-13
  1    57      1905139    16-JAN-13 1936319    17-JAN-13
  1    58      1936319    17-JAN-13 1936326    17-JAN-13
  1    59      1936326    17-JAN-13 1977644    17-JAN-13
  1    60      1977644    17-JAN-13 1990201    17-JAN-13
  1    61      1990201    17-JAN-13 2010700    18-JAN-13
  1    62      2010700    18-JAN-13 2010720    18-JAN-13
  1    63      2010720    18-JAN-13 2056142    18-JAN-13
  1    64      2056142    18-JAN-13 2068902    18-JAN-13
  1    65      2068902    18-JAN-13 2099167    20-JAN-13
  1    66      2099167    20-JAN-13 2099174    20-JAN-13
  1    67      2099174    20-JAN-13 2105985    20-JAN-13
  1    68      2105985    20-JAN-13 2127579    22-JAN-13
  1    69      2127579    22-JAN-13 2127586    22-JAN-13
  1    70      2127586    22-JAN-13 2152941    23-JAN-13
  1    71      2152941    23-JAN-13 2152948    23-JAN-13
  1    72      2152948    23-JAN-13 2157335    23-JAN-13
  1    73      2157335    23-JAN-13 2196416    23-JAN-13
  1    74      2196416    23-JAN-13 2218848    24-JAN-13
  1    75      2218848    24-JAN-13 2218855    24-JAN-13
  1    76      2218855    24-JAN-13 2257551    24-JAN-13
  1    77      2257551    24-JAN-13 2265576    24-JAN-13
  1    78      2265576    24-JAN-13 2290059    28-JAN-13
  1    79      2290059    28-JAN-13 2290066    28-JAN-13
  1    80      2290066    28-JAN-13 2316363    19-FEB-13
  1    81      2316363    19-FEB-13 2316370    19-FEB-13
  1    82      2316370    19-FEB-13 2322688    19-FEB-13
  1    83      2322688    19-FEB-13 2323053    19-FEB-13
  1    84      2323053    19-FEB-13 2323427    19-FEB-13
  1    85      2323427    19-FEB-13 2323819    19-FEB-13
  1    86      2323819    19-FEB-13 2324575    19-FEB-13
  1    87      2324575    19-FEB-13 2335797    19-FEB-13
  1    88      2335797    19-FEB-13 2336104    19-FEB-13
  1    89      2336104    19-FEB-13 2348678    19-FEB-13
  1    90      2348678    19-FEB-13 2355252    19-FEB-13
  1    91      2355252    19-FEB-13 2368217    19-FEB-13
  1    92      2368217    19-FEB-13 2379408    19-FEB-13
  1    93      2379408    19-FEB-13 2403316    19-FEB-13
  1    94      2403316    19-FEB-13 2447389    20-FEB-13
  1    95      2447389    20-FEB-13 2447396    20-FEB-13
  1    96      2447396    20-FEB-13 2465861    20-FEB-13
  1    97      2465861    20-FEB-13 2487381    21-FEB-13
  1    98      2487381    21-FEB-13 2487388    21-FEB-13
  1    99      2487388    21-FEB-13 2488151    21-FEB-13
BS Key  Type LV Size       Device Type Elapsed Time Completion Time
------- ---- -- ---------- ----------- ------------ ---------------
3       Full    286.35M    DISK        00:01:52     21-FEB-13  
        BP Key: 3   Status: AVAILABLE  Compressed: YES  Tag: TAG20130221T201442
        Piece Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_nnndf_TAG20130221T201442_8ld3xmkz_.bkp
  List of Datafiles in backup set 3
  File LV Type Ckp SCN    Ckp Time  Name
  ---- -- ---- ---------- --------- ----
  1       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/o1_mf_system_8f85xnc8_.dbf
  2       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/o1_mf_sysaux_8f85xng5_.dbf
  3       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/o1_mf_undotbs1_8f85xngn_.dbf
  4       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/o1_mf_users_8f85xnhk_.dbf
  5       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_system_m_8hhgltcb_.dbf
  6       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_data_8hhgv6k5_.dbf
  7       Full 2488484    21-FEB-13 /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_myspace_8hhgyr6s_.dbf
BS Key  Type LV Size       Device Type Elapsed Time Completion Time
------- ---- -- ---------- ----------- ------------ ---------------
4       Full    1.05M      DISK        00:00:01     21-FEB-13  
        BP Key: 4   Status: AVAILABLE  Compressed: YES  Tag: TAG20130221T201442
        Piece Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp
  SPFILE Included: Modification time: 21-FEB-13
  SPFILE db_unique_name: ORCL
  Control File Included: Ckp SCN: 2488682      Ckp time: 21-FEB-13
BS Key  Size       Device Type Elapsed Time Completion Time
------- ---------- ----------- ------------ ---------------
5       1.07M      DISK        00:00:00     21-FEB-13
        BP Key: 5   Status: AVAILABLE  Compressed: YES  Tag: TAG20130221T201643
        Piece Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T201643_8ld41c5f_.bkp
  List of Archived Logs in backup set 5
  Thrd Seq     Low SCN    Low Time  Next SCN   Next Time
  ---- ------- ---------- --------- ---------- ---------
  1    100     2488151    21-FEB-13 2488690    21-FEB-13

2、在数据库上产生事务，然后模拟数据库奔溃（这里是删除控制文件，也可以删除其他，达到目的即可）
SQL> select sal from scott .emp;
       SAL
----------
       800
      1600
      1250
      2975
      1250
      2850
      2450
      3000
      5000
      1500
      1100
       950
      3000
      1300
14 rows selected.

SQL> update scott.emp set sal=sal+10;
14 rows updated.
SQL>commit;
Commit complete.

SQL> alter system switch logfile;
System altered.
多做几次切换

SQL> update scott.emp set sal=sal+10;
14 rows updated.
SQL>commit;
Commit complete.

SQL> alter system switch logfile;
System altered.

SQL> update scott.emp set sal=sal+10;
14 rows updated.
SQL>commit;
Commit complete.

SQL> alter system switch logfile;
System altered.

SQL> update scott.emp set sal=sal+10;
14 rows updated.
SQL>commit;
Commit complete.

SQL> alter system switch logfile;
System altered.

SQL> select sal from scott.emp;
       SAL
----------
       840
      1640
      1290
      3015
      1290
      2890
      2490
      3040
      5040
      1540
      1140
       990
      3040
      1340
14 rows selected.
模拟数据库奔溃，删除控制文件，这是数据库还在继续运行，可能要过一会切换日志才奔溃。这里我直接重启一下数据库
[oracle@oracle dbs]$ cd /u01/app/oracle/oradata/ORCL/controlfile/
[oracle@oracle ORCL]$ cd [oracle@oracle controlfile]$ ls
o1_mf_8f860sx8_.ctl
[oracle@oracle controlfile]$ mv o1_mf_8f860sx8_.ctl o1_mf_8f860sx8_.ctl.bak
[oracle@oracle controlfile]$ ls
o1_mf_8f860sx8_.ctl.bak
[oracle@oracle controlfile]$ sqlplus / as sysdba

SQL> startup force
ORACLE instance started.
Total System Global Area  509411328 bytes
Fixed Size      2214816 bytes
Variable Size    343934048 bytes
Database Buffers   159383552 bytes
Redo Buffers      3878912 bytes
ORA-00205: error in identifying control file, check alert log for more info---------------提示查看告警信息了，无法启动！
3、恢复数据库
设置一下oracle_sid，进入rman，启动一个伪实例到nomount状态，因为数据库已经奔溃了，可以通过echo $ORACLE_SID查看原来的SID。这里是假设原来$ORACLE_SID没设置
[oracle@oracle dbs]$ export ORACLE_SID=test
[oracle@oracle dbs]$ echo $ORACLE_SID
test
[oracle@oracle dbs]$ rman target/
Recovery Manager: Release 11.2.0.1.0 - Production on Thu Feb 21 20:25:07 2013
Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.
connected to target database (not started)
查看备份集的位置，第一步备份后有查看。先启动到nomount状态，恢复spfile参数文件：

RMAN> startup nomount
startup failed: ORA-01078: failure in processing system parameters
LRM-00109: could not open parameter file '/u01/app/oracle/product/11.2.0/db_1/dbs/initttt.ora'----------------产生了一个文本格式参数文件
starting Oracle instance without parameter file for retrieval of spfile
Oracle instance started
Total System Global Area     158662656 bytes
Fixed Size                     2211448 bytes
Variable Size                 88080776 bytes
Database Buffers              62914560 bytes
Redo Buffers                   5455872 bytes
RMAN> restore spfile from '/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp';
Starting restore at 21-FEB-13
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=19 device type=DISK
channel ORA_DISK_1: restoring spfile from AUTOBACKUP /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp
channel ORA_DISK_1: SPFILE restore from AUTOBACKUP complete
Finished restore at 21-FEB-13
参数文件恢复完成，此时，在DBS目录下有一个以当前oracle_sid.ora为后缀的参数文件：
[oracle@oracle dbs]$ ll spfilettt.ora
-rw-r----- 1 oracle oinstall 4608 Feb 21 22:21 spfilettt.ora
这时候可以通过参数文件知道原来，数据库的ORACLE_SID,一般情况下和db_name一样，这样就可以还原原来的参数文件名
停掉伪实例，还原原来的参数文件名，我这里原来是orcl
RMAN> shutdown immediate
[oracle@oracle dbs]$ mv spfilettt.ora spfileorcl.ora
因为知道了原来的SID，要重新设置SID变量再进行恢复

[oracle@oracle dbs]$  export ORACLE_SID=orcl
[oracle@oracle dbs]$ rman target/
Recovery Manager: Release 11.2.0.1.0 - Production on Thu Feb 21 21:30:29 2013
Copyright (c) 1982, 2009, Oracle and/or its affiliates.  All rights reserved.
connected to target database: ORCL (not mounted)
RMAN>  startup nomount
恢复控制文件，备份集位置不在重复查看了

RMAN> restore controlfile from '/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp';
Starting restore at 21-FEB-13
using target database control file instead of recovery catalog
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=17 device type=DISK
channel ORA_DISK_1: restoring control file
channel ORA_DISK_1: restore complete, elapsed time: 00:00:01
output file name=/u01/app/oracle/oradata/ORCL/controlfile/o1_mf_8f860sx8_.ctl
output file name=/u01/app/oracle/flash_recovery_area/ORCL/controlfile/o1_mf_8f860t3v_.ctl
Finished restore at 21-FEB-13
控制文件恢复完成
启动数据库到mount

RMAN> alter database mount;
database mounted
released channel: ORA_DISK_1
恢复数据文件，注意最后有错误日志

RMAN> run{
2> restore database;
3> recover database;
4> };
Starting restore at 21-FEB-13
Starting implicit crosscheck backup at 21-FEB-13
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=17 device type=DISK
Crosschecked 3 objects
Finished implicit crosscheck backup at 21-FEB-13
Starting implicit crosscheck copy at 21-FEB-13
using channel ORA_DISK_1
Finished implicit crosscheck copy at 21-FEB-13
searching for all files in the recovery area
cataloging files...
cataloging done
List of Cataloged Files
=======================
File Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_ncsnf_TAG20130221T201442_8ld419dj_.bkp
File Name: /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T201643_8ld41c5f_.bkp
using channel ORA_DISK_1
channel ORA_DISK_1: starting datafile backup set restore
channel ORA_DISK_1: specifying datafile(s) to restore from backup set
channel ORA_DISK_1: restoring datafile 00001 to /u01/app/oracle/oradata/ORCL/datafile/o1_mf_system_8f85xnc8_.dbf
channel ORA_DISK_1: restoring datafile 00002 to /u01/app/oracle/oradata/ORCL/datafile/o1_mf_sysaux_8f85xng5_.dbf
channel ORA_DISK_1: restoring datafile 00003 to /u01/app/oracle/oradata/ORCL/datafile/o1_mf_undotbs1_8f85xngn_.dbf
channel ORA_DISK_1: restoring datafile 00004 to /u01/app/oracle/oradata/ORCL/datafile/o1_mf_users_8f85xnhk_.dbf
channel ORA_DISK_1: restoring datafile 00005 to /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_system_m_8hhgltcb_.dbf
channel ORA_DISK_1: restoring datafile 00006 to /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_data_8hhgv6k5_.dbf
channel ORA_DISK_1: restoring datafile 00007 to /u01/app/oracle/oradata/ORCL/datafile/ORCL/datafile/o1_mf_myspace_8hhgyr6s_.dbf
channel ORA_DISK_1: reading from backup piece /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_nnndf_TAG20130221T201442_8ld3xmkz_.bkp
channel ORA_DISK_1: piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_nnndf_TAG20130221T201442_8ld3xmkz_.bkp tag=TAG20130221T201442
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:02:05
Finished restore at 21-FEB-13
Starting recover at 21-FEB-13
using channel ORA_DISK_1
starting media recovery
archived log for thread 1 with sequence 106 is already on disk as file /u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_1_8f860yp8_.log
archived log for thread 1 with sequence 107 is already on disk as file /u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_2_8f8612s5_.log
archived log for thread 1 with sequence 108 is already on disk as file /u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_3_8f8616pf_.log
channel ORA_DISK_1: starting archived log restore to default destination
channel ORA_DISK_1: restoring archived log
archived log thread=1 sequence=100
channel ORA_DISK_1: reading from backup piece /u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T201643_8ld41c5f_.bkp
channel ORA_DISK_1: piece handle=/u01/app/oracle/flash_recovery_area/ORCL/backupset/2013_02_21/o1_mf_annnn_TAG20130221T201643_8ld41c5f_.bkp tag=TAG20130221T201643
channel ORA_DISK_1: restored backup piece 1
channel ORA_DISK_1: restore complete, elapsed time: 00:00:01
archived log file name=/u01/app/archive/1_100_802627484.arc thread=1 sequence=100
archived log file name=/u01/app/archive/1_101_802627484.arc thread=1 sequence=101
archived log file name=/u01/app/archive/1_102_802627484.arc thread=1 sequence=102
archived log file name=/u01/app/archive/1_103_802627484.arc thread=1 sequence=103
archived log file name=/u01/app/archive/1_104_802627484.arc thread=1 sequence=104
archived log file name=/u01/app/archive/1_105_802627484.arc thread=1 sequence=105
archived log file name=/u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_1_8f860yp8_.log thread=1 sequence=106
archived log file name=/u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_2_8f8612s5_.log thread=1 sequence=107
archived log file name=/u01/app/oracle/flash_recovery_area/ORCL/onlinelog/o1_mf_3_8f8616pf_.log thread=1 sequence=108
media recovery complete, elapsed time: 00:00:04
Finished recover at 21-FEB-13
RMAN-00571: ===========================================================
RMAN-00569: =============== ERROR MESSAGE STACK FOLLOWS ===============
RMAN-00571: ===========================================================
RMAN-00558: error encountered while parsing input commands
RMAN-01009: syntax error: found ";": expecting one of: "advise, allocate, alter, backup, @, catalog, change, configure, connect, convert, copy, create, crosscheck, delete, drop, duplicate, exit, flashback, grant, host, import, list, mount, open, print, quit, recover, register, release, repair, replace, report, reset, restore, resync, revoke, run, send, set, show, shutdown, spool, sql, startup, switch, transport, unregister, upgrade, validate, {, "
RMAN-01007: at line 0 column 2 file: standard input
因为除了恢复备份的日志之外，还要恢复备份后产生事务的日志，从错误提示可以看出已经应用到o1_mf_3_8f8616pf_.log
这时候去归档日志目录查看一下
[oracle@oracle dbs]$ cd /u01/app/oracle/flash_recovery_area/ORCL/onlinelog/
[oracle@oracle onlinelog]$ ll
total 153612
-rw-r-----. 1 oracle oinstall 52429312 Feb 21 21:26 o1_mf_1_8f860yp8_.log
-rw-r-----. 1 oracle oinstall 52429312 Feb 21 21:26 o1_mf_2_8f8612s5_.log
-rw-r-----. 1 oracle oinstall 52429312 Feb 21 21:26 o1_mf_3_8f8616pf_.log

应用到最后一份归档了 下一份就是 current redo. 如果没有的话直接resetlogs启库 这样就是不完全恢复
RMAN> alter database open resetlogs;
database opened
数据库正常启动
我们来查看一下数据是否和数据库崩溃之前一样

[oracle@oracle onlinelog]$ sqlplus / as sysdba
SQL*Plus: Release 11.2.0.1.0 Production on Thu Feb 21 21:40:07 2013
Copyright (c) 1982, 2009, Oracle.  All rights reserved.

Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.1.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
SQL> select sal from scott.emp;
       SAL
----------
       840
      1640
      1290
      3015
      1290
      2890
      2490
      3040
      5040
      1540
      1140
       990
      3040
      1340
14 rows selected.
一样的，证明没有数据丢失！
```
