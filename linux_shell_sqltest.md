# linux_shell_sqltest

```shell
#!/bin/bash

i=1;
MAX_INSERT_ROW_COUNT=$1;
while [ $i -le $MAX_INSERT_ROW_COUNT ]
do
    mysql -uroot -h172.22.4.4 -P3306 -proot@awifi  test -e "insert into test (name,age,createTime) values ('No.$i',$i % 100,NOW());"
    mysql -uroot -h172.22.4.4 -P3306 -proot@awifi  test -e "delete from test where age = ($i+1)%100;"
    mysql -uroot -h172.22.4.4 -P3306 -proot@awifi  test -e "select * from test;"
    d=$(date +%M-%d\ %H\:%m\:%S)
    echo "INSERT HELLO $i @@ $d"
    i=$(($i+1))
    sleep 0
done

exit 0
```
