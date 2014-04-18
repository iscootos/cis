#窗口的矩形尺寸
####GetWindowRect
获取指定窗口的边框矩形的尺寸,该尺寸相对于屏幕坐标左上角的屏幕坐标       
```cpp
BOOL WINAPI GetWindowRect(
  _In_   HWND hWnd,
  _Out_  LPRECT lpRect
);
```
hWnd 需要获取窗口的句柄          
lpRect 是一个RECT结构指针      
```cpp
typedef struct _RECT {
  LONG left;
  LONG top;
  LONG right;
  LONG bottom;
} RECT, *PRECT;
```
left 左上角X坐标      
top 左上角Y坐标        
right 右下角X坐标      
bottom 右下角Y坐标       
```cpp
RECT re;
GetWindowRect(hWnd, &re);
```
####GetClientRect
获取窗口客户区的坐标，该坐标是相对于窗口的,因此左上角坐标为(0,0)
```cpp
BOOL WINAPI GetClientRect(
  _In_   HWND hWnd,
  _Out_  LPRECT lpRect
);
```
使用方法同GetWindowRect,只不过，相对坐标是窗口的。
####GetSystemMetrics
获取屏幕宽度和高度分辨率          
该函数可以获取系统配置信息
```cpp
int WINAPI GetSystemMetrics(
  _In_  int nIndex
);
```
```text
值                          说明
SM_CXFULLSCREEN    16       全屏幕窗口的窗口区域宽度
SM_CYFULLSCREEN    17       全屏幕窗口的窗口区域高度
```
使用方法
```cpp
int width,height;
width = GetSystemMetrics(SM_CXFULLSCREEN);
height = GetSystemMetrics(SM_CYFULLSCREEN);
```
 