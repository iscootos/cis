#改写一个MFC程序实例
用Visual Studio 2013创建了一个SDI单文档的MFC程序      
因为不需要标题栏、边框、菜单栏，所以先修改父窗口框架MainFrm.cpp            
```cpp
/*cs.style = WS_OVERLAPPED | WS_CAPTION | FWS_ADDTOTITLE
		;*/  //注释之前的样式
	cs.style = WS_POPUP;  //弹出窗口
	cs.cx = 300;  //宽度
	cs.cy = 400;  //高度
	cs.hMenu = NULL;  //不显示菜单
	cs.dwExStyle &= ~WS_EX_CLIENTEDGE;  //不显示边框阴影
```

然后去掉客户区的边框MFCApplicationView.cpp              
```cpp
cs.dwExStyle |= WS_EX_TRANSPARENT;  //透明窗口
	cs.cx = 300;  //宽度
	cs.cy = 400;  //高度
	cs.style &= ~WS_BORDER;  //无边框
```

让窗口水平垂直居中显示在桌面上MainFrm          
```cpp
afx_msg int OnCreate(LPCREATESTRUCT lpCreateStruct);
```
```cpp
ON_WM_CREATE()
```
```cpp
int CMainFrame::OnCreate(LPCREATESTRUCT lpCreateStruct)
{
	if (CFrameWnd::OnCreate(lpCreateStruct) == -1)
		return -1;

	int xs_width, xs_height;
	RECT xs_rect;
	xs_width = GetSystemMetrics(SM_CXSCREEN);
	xs_height = GetSystemMetrics(SM_CYSCREEN);
	GetWindowRect(&xs_rect);
	xs_rect.left = (xs_width - xs_rect.right) / 2;
	xs_rect.top = (xs_height - xs_rect.bottom) / 2;
	::SetWindowPos(m_hWnd, HWND_TOP, xs_rect.left, xs_rect.top, xs_rect.right, xs_rect.bottom, SWP_SHOWWINDOW);

	return 0;
}
```
点击客户区移动无标题栏的窗口MFCApplicationView         
```cpp
afx_msg void OnLButtonDown(UINT nFlags, CPoint point);
```
```cpp
ON_WM_LBUTTONDOWN()
```
```cpp
void CMFCApplication2View::OnLButtonDown(UINT nFlags, CPoint point)
{
	GetParent()->SendMessage(WM_SYSCOMMAND, SC_MOVE | HTCAPTION, 0);
}
```
