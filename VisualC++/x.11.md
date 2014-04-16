#改写一个WIN32程序实例
实现，因为并不需要菜单栏，所以做如下修改
```cpp
wcex.lpszMenuName	= NULL;
```
然后我们需要去掉标题栏，我们需要修改CreateWindow函数的第三个参数DWORD dwStyle  为WS_POPUP,即弹出窗口    
设置窗口的坐标值，第四、五个参数int x,int y                                
同时设置窗口的宽度和高度,第六、七个参数int nWidth,int nHeight     
```cpp
hWnd = CreateWindow(szWindowClass, szTitle, WS_POPUP,0,0,300,400,NULL,NULL,hInstance, NULL);
```
现在我们发现，窗口显示出来就在屏幕的左上角，因为上面我们设置了坐标，那么如果我们想让窗口水平垂直居中显示在桌面上呢，我们可以在WM_PAINT消息中，添加如下代码        
```cpp
case WM_PAINT:
	hdc = BeginPaint(hWnd, &ps);
	// TODO:  在此添加任意绘图代码...
	int xs_width, xs_height;
	RECT xs_rect;
	xs_width = GetSystemMetrics(SM_CXSCREEN);
	xs_height = GetSystemMetrics(SM_CYSCREEN);
	GetWindowRect(hWnd, &xs_rect);
	xs_rect.left = (xs_width - xs_rect.right) / 2;
	xs_rect.top = (xs_height - xs_rect.bottom) / 2;
	SetWindowPos(hWnd, HWND_TOP, xs_rect.left, xs_rect.top, xs_rect.right, xs_rect.bottom, SWP_SHOWWINDOW);

	EndPaint(hWnd, &ps);
	break;
```
现在窗口也居中显示了，但是我们发现移动不了窗口，因为没有标题栏了，那么怎么移动呢，我们手动让点击窗口区的鼠标消息，发送一个点击标题栏的消息，就可以了，如下 
```cpp
case WM_LBUTTONDOWN:
		SendMessage(hWnd, WM_NCLBUTTONDOWN, HTCAPTION, 0);
		break;
```
OK,我们试试，现在可以移动窗口，大功告成。