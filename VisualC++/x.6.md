#加载VS资源中PNG图片
#####AfxGetResourceHandle()
AfxGetResourceHandle函数返回当前活动进程的资源文件的HINSTANCE          
头文件`afxwin.h`
```cpp
extern HINSTANCE AfxGetResourceHandle( );
```
一般作为FindResource()函数的参数使用                    
例如:
```cpp
FindResource(AfxGetResourceHandle(), MKAEINTRESOURCE(IDB_PNG1), L"PNG");
```

1. 我们在资源文件中添加了一个PNG图片，名称IDB_PNG1, 放在了系统自动创建的PNG类下面                 
2. 我们使用FindResource找到这个图片
```cpp
HRSRC WINAPI FindResource(
  _In_opt_  HMODULE hModule,
  _In_      LPCTSTR lpName,
  _In_      LPCTSTR lpType
);
```
hModule 如果此参数为NULL,则查找创建该进程的句柄             
lpName 资源名称，使用MAKEINTRESOURCE(ID)宏，ID是资源的名称                
lpType 资源类型，图片在PNG类下面，而我们使用的是Unicode编码，所以使用`L"PNG"`        

如果函数成功，返回指定资源的信息块的句柄，获得这个句柄资源，可以使用LoadResource函数           
如果函数失败，返回NULL        
```cpp
#include <Windows.h>
#include <iostream>
#include <stdint.h>

#include "resource.h"

using namespace std;

int main(int argc, char *argv [])
{
	if (FindResource(NULL, MAKEINTRESOURCE(IDB_PNG1), L"PNG"))
		cout << "PNG文件存在" << endl;
	else
		cout << "PNG文件不存在" << endl;

	return 0;
}
```

获取指定资源在存储器中的第一个字节的句柄
```cpp
HGLOBAL WINAPI LoadResource(
  _In_opt_  HMODULE hModule,
  _In_      HRSRC hResInfo
);
```
hModule 如果此参数为NULL,则查找创建该进程的句柄           
hResInfo 加载资源句柄，由FindResource或FindResourceEx函数返回             

如果函数成功，返回内存中资源的句柄       
如果函数失败，返回NULL          


```cpp
LPVOID WINAPI LockResource(
  _In_  HGLOBAL hResData
);
```
hResData 要访问的句柄资源，由LoadResource函数返回           

如果载入的资源是可用的，则返回值指向资源的第一个字节，否则返回NULL             


获取指定资源的大小，以字节为单位
```cpp
DWORD WINAPI SizeofResource(
  _In_opt_  HMODULE hModule,
  _In_      HRSRC hResInfo
);
```
hModule 可执行文件中，包含的资源           
hResInfo 句柄资源，由由FindResource或FindResourceEx函数返回          

如果函数成功，返回资源大小，单位字节
如果函数失败，返回值是零              



