#socket编程环境设置
更新EPEL源
```bash
rpm -Uvh http://mirrors.ustc.edu.cn/fedora/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm
```
```bash
rpm -Uvh http://mirrors.hustunique.com/epel/6/i386/epel-release-6-8.noarch.rpm
```
```bash
rpm -Uvh http://mirrors.yun-idc.com/epel/5/i386/epel-release-5-4.noarch.rpm
```
```bash
yum update -y
yum repolist
```
关闭SELinux
```bash
echo -e "#SELINUX=enforcing\n#SELINUXTYPE=targeted\nSELINUX=disabled\nSETLOCALDEFS=0" > /etc/selinux/config
setenforce 0
```
#####打开端口
打开1723、11119端口
```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
iptables -X
iptables -Z
iptables -F -t nat
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1119 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1723 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 11119 -j ACCEPT
/etc/init.d/iptables save
/etc/init.d/iptables restart
```
查看打开的端口用如下命令
```bash
iptables -L -n
```