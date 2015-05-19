#在重启 iptables 时，我遇到如下报错
```bash
/etc/init.d/iptables restart
```
```text
Setting chains to policy ACCEPT: security raw nat mangle fi[FAILED]
```
出现这个错误的原因是 Linode VPS 安装 CentOS 5.5 以后内核版本造成的，解决方法如下：         
编辑 /etc/init.d/iptables 找到： #144行
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
vi /etc/init.d/iptables
```
```bash
security)
$IPTABLES -t filter -P INPUT $policy \
&& $IPTABLES -t filter -P OUTPUT $policy \
&& $IPTABLES -t filter -P FORWARD $policy \
|| let ret+=1
;;
```
然后保存退出, 重启 iptables 服务：
```bash
service iptables restart
```
```text
iptables: Flushing firewall rules:                         [  OK  ]
iptables: Setting chains to policy ACCEPT: security raw nat[  OK  ]filter
iptables: Unloading modules:                               [  OK  ]
iptables: Applying firewall rules:                         [  OK  ]
```
shell脚本，修复该问题
```sh
#!/bin/sh

#/i 表示在raw)前一行添加 /a 表示后一行
sed -i '/raw)/i    security)' /etc/init.d/iptables
sed -i '/raw)/i    $IPTABLES -t filter -P INPUT $policy\\' /etc/init.d/iptables
sed -i '/raw)/i        && $IPTABLES -t filter -P OUTPUT $policy\\' /etc/init.d/iptables
sed -i '/raw)/i        && $IPTABLES -t filter -P FORWARD $policy\\' /etc/init.d/iptables
sed -i '/raw)/i        || let ret+=1' /etc/init.d/iptables
sed -i '/raw)/i    ;;' /etc/init.d/iptables

service iptables restart
```