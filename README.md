 小弟，才疏学浅，随便写的一点各种语言的入门教程。

 关于国内用户经常打不开[https://github.com](https://github.com)的问题              
 打开`%SystemRoot%\system32\drivers\etc\`文件夹，把`hosts`文件复制到桌面，用notepad++，编辑该文件，并添加如下内容
 ```text
192.30.252.131 github.com
192.30.252.130 www.github.com
```
当然，你可以自行PING  github的服务器获得IP地址          
然后我们就可以自由的访问github的网站了

有时候git的时候，local和remote的数据，没有完全同步，可以使用如下方式
```text
TortoiseGit(T)->比较差异(D)->显示无版本控制的文件(V)->提交
```
之后再推送，就可以了