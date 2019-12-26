# mysql_semisync_install

```sql
mysql> install plugin rpl_semi_sync_master soname 'semisync_master.so';
mysql> install plugin rpl_semi_sync_slave soname 'semisync_slave.so';
SHOW PLUGINS
=======================================================================
[MASTER]
SET GLOBAL rpl_semi_sync_master_enabled = 1;
SET GLOBAL rpl_semi_sync_master_timeout = 200;
[SLAVE]
SET GLOBAL rpl_semi_sync_slave_enabled = 1;
STOP SLAVE IO_THREAD;
START SLAVE IO_THREAD;
=======================================================================
show variables like 'rpl_semi%';
+------------------------------------+-------+
| Variable_name                      | Value |
+------------------------------------+-------+
| rpl_recovery_rank                  | 0     |
| rpl_semi_sync_master_enabled       | OFF   |
| rpl_semi_sync_master_timeout       | 10000 |
| rpl_semi_sync_master_trace_level   | 32    |
| rpl_semi_sync_master_wait_no_slave | ON    |
| rpl_semi_sync_slave_enabled        | OFF   |
| rpl_semi_sync_slave_trace_level    | 32    |
+------------------------------------+-------+
=======================================================================
show status like 'rpl_semi%';
+--------------------------------------------+-------+
| Variable_name                              | Value |
+--------------------------------------------+-------+
| Rpl_semi_sync_master_clients               | 0     | 记录支持slave连接的个数
| Rpl_semi_sync_master_net_avg_wait_time     | 0     | master 等待slave 回复的平均等待时间。 单位毫秒.
| Rpl_semi_sync_master_net_wait_time         | 0     | master 总的等待时间。
| Rpl_semi_sync_master_net_waits             | 0     | master 等待slave 回复的的总的等待次数
| Rpl_semi_sync_master_no_times              | 0     | master 关闭半同步复制的次数。
| Rpl_semi_sync_master_no_tx                 | 0     | master 没有收到slave的回复而提交的次数，（应该可以理解为master 等待超时的次数)
| Rpl_semi_sync_master_status                | ON    | 标记master现在是否是半同步复制状态
| Rpl_semi_sync_master_timefunc_failures     | 0     | 时间函数未正常工作的次数
| Rpl_semi_sync_master_tx_avg_wait_time      | 0     | master 花在每个事务上的平均等待时间。  
| Rpl_semi_sync_master_tx_wait_time          | 0     | master 总的等待次数。
| Rpl_semi_sync_master_tx_waits              | 0     | 事务等待备库响应的总次数
| Rpl_semi_sync_master_wait_pos_backtraverse | 0     | 改变当前等待最小二进制日志的次数
| Rpl_semi_sync_master_wait_sessions         | 0     | 当前有多少个session 因为slave 的回复而造成等待。
| Rpl_semi_sync_master_yes_tx                | 0     | master 成功接收到slave的回复的次数。
==========================================================================================================================
| Rpl_semi_sync_slave_status                 | ON    | 标记slave 是否在半同步状态。
+--------------------------------------------+-------+
+--------------------------------------------+----------+----------+----------+---------+
| Rpl_semi_sync_master_clients               |1         |1         |1         |1    |
| Rpl_semi_sync_master_net_avg_wait_time     |294       |301       |306       |326  |
| Rpl_semi_sync_master_no_times              |0         |209       |201       |102  |
| Rpl_semi_sync_master_no_tx                 |0         |211       |202       |105  |
| Rpl_semi_sync_master_status                |ON        |ON        |ON        |ON   |
| Rpl_semi_sync_master_tx_avg_wait_time      |351       |359       |365       |380  |
| Rpl_semi_sync_master_yes_tx                |1         |39700     |39799     |39896|
+--------------------------------------------+----------+----------+----------+---------+
```
