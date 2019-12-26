# linux_shell_sshcopyid

```shell
#!/bin/bash
user=zxt
top=20
num=2
ssh-keygen
while [ $num -le $top ]
do
        ssh-copy-id -i ~/.ssh/id_rsa.pub $user@172.22.4.$num
        echo "172.22.4.$num done!"
        num=$(($num+1))
done

exit 0
```
