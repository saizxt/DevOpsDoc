# mysql_repl_skip

```sql
>show slave status \G;  
Last_SQL_Error: Worker 3 failed executing transaction '6c9da32f-0513-11e4-a949-00e04c8a7573:1' at master log mysqldb01-bin.000001, end_log_pos 4541041; Could not execute Delete_rows event on table wind.t1; Query execution was interrupted, Error_code: 1317; Got error -1 from storage engine, Error_code: 1030; handler error No Error!; the event's master log FIRST, end_log_pos 4541041  
Replicate_Ignore_Server_Ids:  
Master_Server_Id: 1001  
Master_Info_File: mysql.slave_master_info  
SQL_Delay: 1  
SQL_Remaining_Delay: NULL  
Slave_SQL_Running_State:  
Master_Retry_Count: 86400  
Master_Bind:  
Last_IO_Error_Timestamp:  
Last_SQL_Error_Timestamp: 140911 09:32:37  
Master_SSL_Crl:  
Master_SSL_Crlpath:  
Retrieved_Gtid_Set: 6c9da32f-0513-11e4-a949-00e04c8a7573:1  --跳过此事务  
Executed_Gtid_Set: 510e47c2-0669-11e4-b1ff-000c293812ad:1  
Auto_Position: 1

>stop slave;  
>set gtid_next="6c9da32f-0513-11e4-a949-00e04c8a7573:1";  
    begin;commit;
    set gtid_next="AUTOMATIC";  
>start slave;  
```
