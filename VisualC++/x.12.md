#WIN下显示图片的原理
####HDC、CDC
我们在很多写WIN下贴图的文章都看到，HDC、CDC，那么它们是什么呢          
HDC 设备上下文句柄     
CDC 设备上下文类        
这个是比较官方的解释，其实我看了也不知道是什么意思，       
####设备上下文 DC
所以我们先来讲讲什么是设备上下文        
Device Context 设备描述表，又称设备上下文，也就是我们常讲的DC         
设备描述表示一种数据结构，它包含了一个设备(显示器或打印机)的绘制属性的信息，所有的绘制操作通过设备描述表进行，但是程序是不能直接访问设备描述表的，只能通过API句柄间接访问该结构         

Windows程序通过为指定设备(屏幕，打印机)创建一个设备描述表，在DC表示的逻辑“画布”上进行图形的绘制，DC包含了物理设备所需的各种状态信息                 

那么我们可以这么理解，DC就是沟通程序与显示设备的一座桥梁，没有它，我们肯定过不了河             

####HDC
再讲HDC ，上面我们已经知道什么是设备上下文了，也就是DC ,沟通程序与显示设备的中间件，那么HDC 呢，我们可以理解为，指向DC的指针，被定义为32位无符号整数             
HDC就是DC的句柄，也就是DC的指针      
####CDC
现在我们来看看CDC， CDC是MFC中的DC的一个类           
那么它的句柄呢，CDC的类中，有一个成员变量m_nHdc , 即HDC类型的句柄， 也就是HDC            
####创建一个DC句柄
今天我们主要讲的WIN32 API显示图像的原理，所以后面就不再讲解MFC的CDC类       
上面我们已经讲解了HDC就是DC的句柄，那么，我们来创建一个
```cpp
HDC hdc;
```
OK，如上，我们就创建了一个DC的句柄变量hdc         
####WM_PAINT
WM_PAINT是一个系统消息，这个是一个非常重要的消息。       
在最初建立窗口的时候，整个显示区域都是无效的，因为程序还没有在窗口上画任何东西，第一条WM_PAINT消息(通常发生在WinMain中调用UpdateWindow时)指示窗口消息处理程序在显示区域上面画一些东西,在用户改变窗口的大小后，显示区域的显示内容重新变得无效，WNDCLASSEX结构的style字段设定的标志 CS_HREDRAW和CS_VERDRAW，这样的格式设定指示windows,在窗口大小改变后，就把整个窗口显示内容当成无效，然后窗口消息处理程序将收到一条WM_PAINT消息，当用户将程序最小化，然后再次将窗口恢复为以前的大小时，WINDOWS将不会保存显示区域的内容，在图形环境下，窗口显示区域涉及的数据量很大，因此，windows令窗口无效，窗口消息处理程序接收条WM_PAINT消息，并自动恢复其窗口的内容，在移动窗口以致其相互重叠时，windows不保存一个窗口中被另一个窗口所遮盖的内容，在这一部分不再遮盖之后，它就被标志为无效，窗口消息处理程序收到一条WM_PAINT消息，以更新窗口的内容            
从以上我们可以得出以下几点:              

1.UpdateWindow时，发送WM_PAINT消息           
2.改变窗口大小时，发送WM_PAINT消息         
3.移动窗口位置时，发送WM_PAINT消息           
4.窗口最小化，还原时，发送WM_PAINT消息          
5.窗口被遮盖，点击该程序窗口区域时，发送WM_PAINT消息            

####BeginPaint准备绘制
上面我们讲了HDC,WM_PAINT，那么我们怎么把它们链接起来呢，使用BeginPaint               
BeginPaint必须在WM_PAINT消息处理程序中使用，并且使用EndPaint结束       
```cpp
HDC BeginPaint(
  _In_   HWND hwnd,
  _Out_  LPPAINTSTRUCT lpPaint
);
```
如上是函数的原型，hwnd就是窗口的句柄，需要在哪个窗口绘制，就输入哪个窗口的句柄，lpPaint是一个指向PAINTSTRUCT结构的指针                 
```cpp
typedef struct tagPAINTSTRUCT {
  HDC  hdc;
  BOOL fErase;
  RECT rcPaint;
  BOOL fRestore;
  BOOL fIncUpdate;
  BYTE rgbReserved[32];
} PAINTSTRUCT, *PPAINTSTRUCT;
```
PAINTSTRUCT结构，是用来接收绘画信息的           

通过上面我们已经知道，如何创建一个DC句柄，那么如何把程序和DC句柄链接起来呢，我们使用BeginPaint函数，它返回的DC句柄，就是指定窗口的DC句柄，然后系统把相应的绘制信息写入 PAINTSTRUCT结构指针            
```cpp
HDC hdc;
PAINTSTRUCT ps;
hdc = BeginPaint(hWnd, &ps);
```
####EndPaint
```cpp
BOOL EndPaint(
  _In_  HWND hWnd,
  _In_  const PAINTSTRUCT *lpPaint
);
```
用法，同BeginPaint函数，不过EndPaint返回的是BOOL值，返回值始终是非零，也就是永远都是返回true           
该函数用于，BeginPaint后面，表示绘制工作的接结束。 


那么我们现在可以理解为,BeginPaint->开始绘制->Endpaint->绘制结束，代码如下:
```cpp
HDC hdc;
PAINTSTRUCT ps;
hdc = BeginPaint(hWnd, &ps);  /* 开始绘制 */
/* 绘制代码 */
EndPaint(hWnd, &ps); /* 结束绘制 */
```

