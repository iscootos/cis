#修改本地DNS
#####Google 公共DNS
```text
8.8.8.8
8.8.4.4
```
#####OpenDNS
```text
208.67.222.222
208.67.220.220
```
#####国内 公共DNS
```text
114.114.114.114
114.114.115.115
```
#####阿里DNS
```text
223.5.5.5
223.6.6.6
```
####Windows系统 查看系统的DNS地址
`开始`->`运行`->`cmd`->`回车`
```bat
nslookup
```
修改DNS后，有时候不能马上生效，需要清理DNS缓存，使用如下方式
`开始`->`运行`->`cmd`->`回车`
```bat
ipconfig /flushdns
```
