#GDI图形设备接口
上一讲，我们知道SelectObject函数需要HGDIOBJ对象，那么如何创建GDI对象呢，有很多方式                

####LoadBitmap加载位图
首先就从我们用的最多的加载资源中的位图，说起         
```cpp
HBITMAP LoadBitmap(
  _In_  HINSTANCE hInstance,
  _In_  LPCTSTR lpBitmapName
);
```
HINSTANCE 是程序的当前实例的句柄，也就是当前程序在内存中的句柄，windows是多任务操作系统，一个程序可以同时运行多个实例，不同的实例间，需要彼此区别，HINSTANCE就是干这个用的          
lpBitmapName是一个指针，指向包含需要加载的位图资源的名称的字符串，使用MAKEINTRESOURCE宏来获取    

返回值：如果成功，返回指定位图的句柄，如果失败，返回NULL           
```cpp
HDC hdc;
PAINTSTRUCT ps;
HBITMAP bp;
HDC pdc;
hdc = BeginPaint(hWnd, &ps);
bp = loadBitmap(hInst, MAKEINTRESOURCE(IDB_BITMAP1));
pdc = CreateCompatible(hdc);
SelectObject(pdc, bp);
BitBlt(hdc, 0,0,300,400,pdc, 0,0,SRCCOPY);
EndPaint(hWnd, &ps);
```

