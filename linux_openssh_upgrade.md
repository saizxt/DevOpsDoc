# linux_openssh_upgrade

```shell
scp -r openssh-6.7p1/ root@172.30.2.121:/opt
ssh 172.30.2.121
service sshd stop
yum remove openssh -y
cd /opt/openssh-6.7p1/
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
mv CentOS-Base.repo /etc/yum.repos.d/
env http_proxy=http://172.16.0.90:3128 https_proxy=http://172.16.0.90:3128 yum install -y gcc
rm -rf /etc/ssh
tar -xvf zlib-1.2.7.tar.gz
cd zlib-1.2.7
./configure --prefix=/usr/local/zlib && make && make install
cd ..
tar -xvf openssl-1.0.2.tar.gz
cd openssl-1.0.2
./config --prefix=/usr/local/openssl && make && make install
cd ..
tar -xvf openssh-6.7p1.tar.gz
cd openssh-6.7p1
./configure --prefix=/usr/local/openssh --sysconfdir=/etc/ssh --with-ssl-dir=/usr/local/openssl --with-zlib=/usr/local/zlib --with-md5-passwords --without-hardening && make && make install
cp contrib/redhat/sshd.init /etc/init.d/sshd
chmod +x /etc/init.d/sshd
sed -i "25s/sbin/local\/openssh\/sbin/" /etc/init.d/sshd
sed -i "41s/bin/local\/openssh\/bin/" /etc/init.d/sshd
chkconfig --add sshd
service sshd start
telnet 172.30.2.121 22
```
