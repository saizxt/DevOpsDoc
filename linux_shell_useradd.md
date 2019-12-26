# linux_shell_useradd

```shell
#!/bin/bash
user=root
top=20
num=2
ssh-keygen
while [ $num -le $top ]
do
        ssh-copy-id -i ~/.ssh/id_rsa.pub $user@172.22.4.$num
        ssh $user@172.22.4.$num "groupadd admin"
        ssh $user@172.22.4.$num "groupadd toe"
        ssh $user@172.22.4.$num "useradd -g admin zxt"
        ssh $user@172.22.4.$num "echo 'Zhangxt12#$' | passwd --stdin zxt"
        ssh $user@172.22.4.$num "useradd -g admin admin"
        ssh $user@172.22.4.$num "echo 'admin' | passwd --stdin admin"
        ssh $user@172.22.4.$num "useradd -g toe xxm"
        ssh $user@172.22.4.$num "echo 'xxm@awifi' | passwd --stdin xxm"
        echo "172.22.4.$num done!"
        num=$(($num+1))
done

exit 0
```
