#windows系统修改用户密码和设置自动登录
####windows 8
修改windows 8系统用户的密码        
`WIN + R`键，打开运行(Run)窗口，输入`compmgmt.msc`点击确定按钮            
在打开的`计算机管理(Computer Management)`窗口，依次打开
```text
系统工具->本地用户和组->用户
System Tools->Local Users and Groups->Users
```
右键点击要修改密码的用户名，选择`设置密码(S)...`(Set Password...)     

或者，`WIN + X`键，选择`控制面板(P)`(Control Panel)        
在打开的控制面板窗口中，依次打开
```text
用户账号->管理其他账号->点击需要修改密码的用户->更改密码
User Accounts->Manage another account->点击需要修改密码的用户->Change the password
```

设置window 8 系统用户自动登录             
`WIN + R`键，打开运行(Run)窗口，输入`netplwiz`点击确定按钮         
在打开的用户账户窗口中，取消`要使用本计算机，用户必须输入用户名和密码`(Users must enter a user name and password to use this computer)的勾选，再点击`应用`按钮

设置windows 8 马上关机的快捷键
```text
shutdown.exe -s -t 00
```
重启计算机
```text
shutdown.exe -r -t 00
```
锁定计算机
```text
rundll32.exe user32.dll,LockWorkStation
```
休眠计算机
```text
rundll32.exe powrProf.dll,SetSuspendState
```
睡眠计算机
```text
rundll32.exe powrprof.dll,SetSuspendState 0,1,0
```
####Windows XP
修改用户密码，依次点击         
```text
开始->控制面板->用户账号->点击需要修改密码的用户->更改我的密码
start->Control Panel->User Accounts->点击需要修改密码的用户->Change my password
```
设置系统自动登录          
依次点击开始(start)->运行(Run)，打开运行(Run)窗口，并输入
```text
rundll32 netplwiz.dll,UsersRunDll
```
或者
```
control userpasswords2
```
点击`确定(OK)`按钮，打开`用户账户(User Accounts)`窗口        
取消`要使用本机，用户必须输入用户名和密码(Users must enter a user name and password to use this computer)`的勾选，并点击`应用(Apply)`
按钮         
