#CDC类
```text
CObject
public |---CDC
public |---|---CClientDC
public |---|---CPaintDC
public |---|---CWindowDC
public |---|---CMetaFileDC
```
CDC是MDC的DC的一个类               
CDC所有类都包含一个成员变量: m_nHdc,即DC句柄          

###CPaintDC
封装了BeginPaint和EndPaint两个API的调用             
1.用于响应WM_PAINT重绘消息的绘图输出     
2.CPaintDC在构造函数中调用BeginPaint取得DC句柄，在析构函数中调用EndPaint释放DC句柄，还负责从消息队列中清除WM_PAINT消息，因此，在处理窗口重绘时，必须使用CPaintDC,都在WM_PAINT消息无法从消息队列中消除，将引起不断的消息重绘             
3.CPaintDC只能用在WM_PAINT消息处理之中         
###CClientDC
窗口客户区,构造时自动调用GetDC函数，析构时自动调用ReleaseDC函数，一般应用于客户区窗口的绘制。         
当需要处理一个鼠标的点击，然后马上画出一个圆，你不能等到下一个WM_PAINT消息到来才画图，而是马上，这时就需要CClientDC了，它可以在OnPaint的外面创建一个客户区域DC          

###CWindowDC
处理显示器相关的整个窗体区域，包括了框架和控件(子窗口)         
1.可在非客户区绘制图形，而CClientDC、CPaintDC只能在客户区绘制图形      
2.坐标原点是在屏幕的左上角，CClientDC、CPaintDC坐标原点是在客户区      
3.关联一个特定窗口，允许开发者在目标窗口的任何一部分进行绘图，包含边界和标题，这种DC同WM_NCPAINT消息一起发送          
###CMetaFileDC
与元文件相关的设备描述表关联
####下面说下一些细点的知识点
1.CClientDC,CWindowDC 区别不大, 可以说 CWindowDC包含了CClientDC。 就拿记事本来说，CClientDC 就只是我们可以编辑文字的那个区域，是客户区，CWindowDC 除了上面说的区域, 还包括菜单栏和工具栏等                                         
2.CClientDC和CWindowDC与 CPaintDC 的区别大点，在DC的获取方面 CClientDC和CWindowDC 使用的是并只能是GetDC 和 ReleaseDC。CPaintDC 使用的是并只能是 BeginPaint 和 EndPaint                                    
3.CPaintDC只能用在响应 WM_PAINT 事件CClientDC,CWindowDC 只能用在响应非WM_PAINT 事件
###关于 WM_PAINT事件
 系统会在多个不同的时机发送WM_PAINT消息：当第一次创建一个窗口时，当改变窗口的大小时，当把窗口从另一个窗口背后移出时，当最大化或最小化窗口时，等等，这些动作都是由系统管理的，应用只是被动地接收该消息，在消息处理函数中进行绘制操作；大多数的时候应用也需要能够主动引发窗口中的绘制操作，比如当窗口显示的数据改变的时候，这一般是通过InvalidateRect和InvalidateRgn函数来完成的。InvalidateRect和 InvalidateRgn把指定的区域加到窗口的Update Region中，当应用的消息队列没有其他消息时，如果窗口的Update Region不为空时，系统就会自动产生WM_PAINT消息             

 系统为什么不在调用Invalidate时发送WM_PAINT消息呢？又为什么非要等应用消息队列为空时才发送WM_PAINT消息呢？这是因为系统把在窗口中的绘制操作当作一种低优先级的操作，于是尽可能地推后做。不过这样也有利于提高绘制的效率：两个WM_PAINT消息之间通过 InvalidateRect和InvaliateRgn使之失效的区域就会被累加起来，然后在一个WM_PAINT消息中一次得到更新，不仅能避免多次重复地更新同一区域，也优化了应用的更新操作。像这种通过InvalidateRect和InvalidateRgn来使窗口区域无效，依赖于系统在合适的时机发送WM_PAINT消息的机制实际上是一种异步工作方式，也就是说，在无效化窗口区域和发送WM_PAINT消息之间是有延迟的；有时候这种延迟并不是我们希望的，这时我们当然可以在无效化窗口区域后利用SendMessage 发送一条WM_PAINT消息来强制立即重画，但不如使用Windows GDI为我们提供的更方便和强大的函数：UpdateWindow和RedrawWindow。UpdateWindow会检查窗口的Update Region，当其不为空时才发送WM_PAINT消息；RedrawWindow则给我们更多的控制：是否重画非客户区和背景，是否总是发送 WM_PAINT消息而不管Update Region是否为空等