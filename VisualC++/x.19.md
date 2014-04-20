#windows消息队列原理
GetMessage函数取得消息队列中的消息，DispatchMessage函数把消息发送给回调函数，进行处理                
这个是windows系统进行消息处理的正常流程，不过有时候我们需要手动生成消息，那么使用SendMessage函数,同时SendMessage发送的消息，不再经过消息队列，而是直接发送到指定对象
```cpp
LRESULT WINAPI SendMessage(
  _In_  HWND hWnd,
  _In_  UINT Msg,
  _In_  WPARAM wParam,
  _In_  LPARAM lParam
);
```
hWnd 窗口句柄，如果参数为HWND_BROADCAST ((HWND)0xffff)，则消息将被发送到系统中的所有顶层窗口，但消息不被发送到子窗口                      
Msg 指定被发送的消息            
wParam 指定附加的消息         
lParam 指定附加的消息           
