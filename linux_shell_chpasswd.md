# linux_shell_chpasswd

```shell
#!/bin/bash
user=$1
newpasswd=$2
start=2
stop=20
#change admin
echo $newpasswd |passwd --stdin $user
echo "###The Password Of $user@172.22.4.1 Has Been Changed To $newpasswd !###"
#change other
while [ $start -le $stop ]
do
        ssh 172.22.4.$num "echo $newpasswd |passwd --stdin $user"
        echo "###The Password Of $user@172.22.4.$num Has Been Changed To $newpasswd !###"
        start=$(($start+1))
done
exit 0
```
