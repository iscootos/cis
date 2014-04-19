#窗口居中的原理
前面我们已经讲过，可以通过GetSystemMetrics获取屏幕的宽度和高度分辨率
```cpp
int width,height;
width = GetSystemMetrics(SM_CXFULLSCREEN);
height = GetSystemMetrics(SM_CYFULLSCREEN);
```
那么通过计算，我们可以确定窗口水平垂直居中显示的X、Y坐标       
```cpp
int xs_width, xs_height;
RECT xs_rect;
xs_width = GetSystemMetrics(SM_CXFULLSCREEN);
xs_height = GetSystemMetrics(SM_CYFULLSCREEN);
GetWindowRect(hWnd, &xs_rect);
xs_rect.left = (xs_width - xs_rect.right) / 2;
xs_rect.top = (xs_height - xs_rect.bottom) / 2;
```
由此，我们就获得了该窗口区域在屏幕上水平垂直居中的左上角的X、Y坐标
###SetWindowPos
该函数改变一个子窗口，弹出窗口，顶层窗口的尺寸，位置和Z序
```cpp
BOOL WINAPI SetWindowPos(
  _In_      HWND hWnd,
  _In_opt_  HWND hWndInsertAfter,
  _In_      int X,
  _In_      int Y,
  _In_      int cx,
  _In_      int cy,
  _In_      UINT uFlags
);
```
hWnd 窗口句柄        
hWndInsertAfter 在z序中的位置
```text
值                说明
HWND_BOTTOM       将窗口置于Z序的底部
HWND_NOTOPMOST    将窗口置于所有非顶层窗口之上，即在所有顶层窗口之后
HWND_TOP          将窗口置于Z序的顶部
HWND_TOPMOST      将窗口置于所有非顶层窗口之上，即使窗口未被激活也将保持顶级位置
```
X 指定窗口左上角X坐标               
Y 指定窗口左上角Y坐标                  
cx 指定窗口宽度                
cy 指定窗口高度         
uFlags 窗口尺寸和定位的标志
```text
值                     说明
SWP_ASYNCWINDOWPOS     如果调用进程不拥有窗口，系统会向拥有窗口的线程发出需求。这就防止调用线程在其他线程处理需求的时候发生死锁
SWP_DEFERERASE         防止产生WM_SYNCPAINT消息
SWP_DRAWFRAME          在窗口周围画一个边框（定义在窗口类描述中）
SWP_FRAMECHANGED       给窗口发送WM_NCCALCSIZE消息，即使窗口尺寸没有改变也会发送该消息。如果未指定这个标志，只有在改变了窗口尺寸时才发送WM_NCCALCSIZE
SWP_HIDEWINDOW         隐藏窗口
SWP_NOACTIVATE         不激活窗口。如果未设置标志，则窗口被激活，并被设置到其他最高级窗口或非最高级组的顶部（根据参数hWndlnsertAfter设置）
SWP_NOCOPYBITS         清除客户区的所有内容。如果未设置该标志，客户区的有效内容被保存并且在窗口尺寸更新和重定位后拷贝回客户区。
SWP_NOMOVE             维持当前位置（忽略X和Y参数）
SWP_NOOWNERZORDER      不改变z序中的所有者窗口的位置
SWP_NOREDRAW           不重画改变的内容。如果设置了这个标志，则不发生任何重画动作。适用于客户区和非客户区（包括标题栏和滚动条）和任何由于窗回移动而露出的父窗口的所有部分。如果设置了这个标志，应用程序必须明确地使窗口无效并区重画窗口的任何部分和父窗口需要重画的部分
SWP_NOREPOSITION      与SWP_NOOWNERZORDER标志相同
SWP_NOSENDCHANGING    防止窗口接收WM_WINDOWPOSCHANGING消息
SWP_NOSIZE            维持当前尺寸（忽略cx和Cy参数）
SWP_NOZORDER         维持当前Z序（忽略hWndlnsertAfter参数）
SWP_SHOWWINDOW       显示窗口
```       
返回值：如果函数成功，返回值为非零；如果函数失败，返回值为零

