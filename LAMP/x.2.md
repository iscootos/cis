如何添加禁止登录的用户
```bash
useradd username -s /sbin/nologin     //通过SSH软件登录服务器，键入如下代码建立用户
passwd username                       //设置用户密码
```
这样就建立了没有登录权限的用户。
```bash
vi /etc/ssh/sshd_config
```
```bash
#Port 22                        //第13行  取消注释，修改为Port 2222 指定SSH连接的端口号，不建议使用默认22端口
Protocol 2                      //第21行  2,1允许SSH1和SSH2连接，建议设置成 Protocal 2
#PermitRootLogin yes            //第42行  添加注释，允许root登录
#PermitEmptyPasswords no        //第65行  添加注释，不允许空密码登录
PasswordAuthentication yes      //第66行  取消注释，改为 PasswordAuthentication no   取消密码认证登录
```
重启SSH
```bash
/etc/init.d/sshd restart
```      
配置防火墙，打开需要端口：
```bash
vi /etc/sysconfig/iptables
```
```bash
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -F
iptables -X
iptables -Z
iptables -F -t nat
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1119 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1812 -j ACCEPT
iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 1813 -j ACCEPT
iptables -t nat -A POSTROUTING -o eth0  -j MASQUERADE
iptables --table nat --append POSTROUTING --jump MASQUERADE
/etc/init.d/iptables save
/etc/init.d/iptables restart
setenforce 0
```
添加NAT转发：
```bash
iptables -t nat -A POSTROUTING -o eth0  -j MASQUERADE
iptables -t nat -A POSTROUTING -o eth1  -j MASQUERADE
```
保存防火墙设置：
```bash
/etc/init.d/iptables save
/etc/init.d/iptables restart
```
查看NAT转发规则
```bash
iptables -F -t nat
iptables -L -n -v -t nat
```
创建公共密钥文件Public_key
```bash
mkdir -pv /root/.ssh
touch ~/.ssh/authorized_keys
vi ~/.ssh/authorized_keys
```
####出错信息
```bash
/etc/init.d/sshd restart
```
```text
[root@li414-184 ~]# /etc/init.d/sshd restart
Stopping sshd:                                             [  OK  ]
cat: /proc/sys/crypto/fips_enabled: No such file or directory
/etc/init.d/sshd: line 50: [: too many arguments
Starting sshd:                                             [  OK  ]
```
```bash
vi /etc/init.d/sshd
```
第50行  
```bash
if [ ! -s $RSA1_KEY -a `cat /proc/sys/crypto/fips_enabled` -eq 0 ]; then
```
修改为：
```bash
if [ ! -s "$RSA1_KEY" -a "'sysctl -n -e crypto.fips_enables'" = 0 ]; then
```
####关闭SELINUX   
```bash
vi /etc/selinux/config
```
```bash
#SELINUX=enforcing      #注释掉
#SELINUXTYPE=targeted   #注释掉
SELINUX=disabled        #增加
```
上面我们虽然关闭了SElinux但是重启电脑后才会生效，所以我们需要先执行下面的命令，该命令是临时关闭SElinux重启后恢复    
```bash
setenforce 0
```
把如上的命令，通过SHELL语句实现
```bash
#!/bin/sh

setenforce 0
echo -e "#SELINUX=enforcing\n#SELINUXTYPE=targeted\nSELINUX=disabled\nSETLOCALDEFS=0" > /etc/selinux/config
```
####iptables错误
在重启 iptables 时，我遇到如下报错：
```bash
/etc/init.d/iptables restart
```
```bash
Setting chains to policy ACCEPT: security raw nat mangle fi[FAILED]
```
出现这个错误的原因是 Linode VPS 安装 CentOS 5.5 以后内核版本造成的，解决方法如下：    
     
编辑 /etc/init.d/iptables 找到： #144行
```bash
vi /etc/init.d/iptables
```
```bash
echo -n $”${IPTABLES}: Setting chains to policy $policy: ”
ret=0
for i in $tables; do
echo -n “$i ”
case “$i” in
+    security)
+    $IPTABLES -t filter -P INPUT $policy \
+        && $IPTABLES -t filter -P OUTPUT $policy \
+        && $IPTABLES -t filter -P FORWARD $policy \
+        || let ret+=1
+    ;;
raw)
$IPTABLES -t raw -P PREROUTING $policy \
&& $IPTABLES -t raw -P OUTPUT $policy \
|| let ret+=1
;;

filter)
$IPTABLES -t filter -P INPUT $policy \
&& $IPTABLES -t filter -P OUTPUT $policy \
&& $IPTABLES -t filter -P FORWARD $policy \
|| let ret+=1
;;
```
前面有＋号的为添加的
```bash
security)
$IPTABLES -t filter -P INPUT $policy \
&& $IPTABLES -t filter -P OUTPUT $policy \
&& $IPTABLES -t filter -P FORWARD $policy \
|| let ret+=1
;;
```
然后保存退出          
重启 iptables 服务：
```bash
service iptables restart
```
```text
[root@localhost ~]# service iptables restart
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Setting chains to policy ACCEPT: security raw nat[  OK  ]filter
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
```
