#静态文本框控件
静态文本框控件可以接收文字，和图片          
####style风格
在使用`CreateWindow`和`CreateWindowEx`函数的时候          
`LPCTSTR lpClassName`的值必须是`STATIC`的时候，可以使用如下常量值            
`DWORD dwStyle`参数的值可以是如下的风格常量值,可以是一个也可以是多个，用`|`操作符链接         
```text
常量值               说明
SS_BITMAP            显示位图，位图已经在资源中，该样式自动忽略控件的nWidth,nHeight参数，自动调整控件大小以适应位图          
SS_BLACKFRAME        默认黑色边框
SS_BLACKRECT         默认黑色填充文本框区域
SS_CENTER            文本水平居中显示，如果文本过长，就显示中间的那一段，其他的自动截断不显示       
SS_CENTERIMAGE       文本垂直居中显示，如果是位图，也是垂直居中显示，如果位图小于文本框，那么用位图左上角的点，填充空白区域 
SS_EDITCONTROL       多行显示文本框，不过如果文本太长，超出文本框，那么超出范围将不显示
SS_ENDELLIPSIS       如果字符串太长，超出了文本框的单行宽度的显示范围，那么将使用`...`代替超出部分
SS_ENHMETAFILE       增强型图元文件，该文本是一个图元文件的名称，图元文件缩放自己来适应文本框的宽度和高度
SS_ETCHEDFRAME       使用 EDGE_ETCHED 边缘样式，SS_ETCHEDFRAME 绘制该静态控件的框架，更多请参考DrawEdge函数
SS_ETCHEDHORZ        使用 EDGE_ETCHED 边缘样式，SS_ETCHEDHORZ 绘制该静态控件的上下，更多请参考DrawEdge函数
SS_ETCHEDVERT        使用 EDGE_ETCHED 边缘样式，SS_ETCHEDVERT 绘制该静态控件的左右边缘，更多请参考DrawEdge函数
SS_GRAYFRAME         默认灰色边框
SS_GRAYRECT          默认灰色填充文本框区域
SS_ICON              加载一个在资源中的图标，可以是动画图标，该样式忽略CreateWindow函数的nWidth,nHeight参数，文本框调节自己的宽度和高度以适应图标大小，SS_ICON默认只能加载SM_CXICON和SM_CYICON尺寸的图标，或者使用SS_REALSIZEIMAGE风格使用其他大小的图标，图标默认使用LoadIcon加载，如果失败，使用LoadCursor加载，如果还是失败，使用LoadImage加载。
SS_LEFT              文本左对齐，如果字符串太长，超出了文本框的单行宽度的显示范围，其他的自动截断不显示
SS_LEFTNOWORDWRAP    文本左对齐，关闭自动换行
SS_NOPREFIX          不再对前缀`&`字符的处理
SS_NOTIFY            默认，静态控件，是没有鼠标消息的，使用该样式，可以是用户点击或双击后，向父窗口发送STN_CLICKED, STN_DBLCLK, STN_DISABLE, STN_ENABLE 消息
SS_OWNERDRAW         绘制控件，发送一个WM_DRAWITEM消息，要求绘制
SS_PATHELLIPSIS      如果字符串中有`\`反斜杠换行，那么第一个反斜杠后面的字符串，自动截断，不显示
SS_REALSIZECONTROL   该样式最大的用处就是调整，位图来适应静态控件的大小，SS_BITMAP、SS_ICON样式的位图或图标都忽略了静态控件的宽度和高度，使用该样式后，就可以让位图或图标来适应静态控件的大小了，如果位图指定了SS_CENTERIMAGE，位图将居中，如果指定SS_CENTERIMAGE样式，位图将被拉伸或收缩
SS_REALSIZEIMAGE     指定实际资源使用LoadImage函数加载，与SS_ICON组合使用，使用实际资源的宽度，如果同时指定SS_CENTERIMAGE,将显示在nWidth,nHeight参数指定的中间
SS_RIGHT             文本右对齐，如果字符串太长，超出了文本框的单行宽度的显示范围，其他的自动截断不显示
SS_RIGHTJUST         静态控件的右下角是固定的，在样式SS_BITMAP、SS_ICON样式自动调整控件大小的时候，只能调整顶部和左右两侧，以适应位图和图标的大小
SS_SIMPLE            一个简单的单行静态文本，左对齐，不能缩放或改变
SS_SUNKEN            半下沉式边框，让文本框更具动态感
SS_TYPEMASK          过时的样式，不推荐使用
SS_WHITEFRAME        默认白色边框
SS_WHITERECT         默认白色填充文本框区域
SS_WORDELLIPSIS      截断任何不适应的文字，不显示
```

###静态文本框消息
```text
STM_GETICON          获取SS_ICON样式设置的图标句柄
STM_GETIMAGE         获取SS_IMAGE样式设置的图像句柄
STM_SETICON          发送STM_SETICON消息，让图标与图标控件关联
STM_SETIMAGE         发送STM_SETIMAGE消息，让图像与图像控件关联
```

###静态文本框通知 
```text 
STN_CLICKED         静态文本框，必须设置了SS_NOTIFY样式，才能发送STN_CLICKED消息，父窗口通过WM_COMMAND消息接收该消息
STN_DBLCLK          静态文本框，必须设置了SS_NOTIFY样式，才能发送STN_DBLCLK消息，父窗口通过WM_COMMAND消息接收该消息
STN_DISABLE         静态文本框，必须设置了SS_NOTIFY样式，才能发送STN_DISABLE消息，父窗口通过WM_COMMAND消息接收该消息
STN_ENABLE          静态文本框，必须设置了SS_NOTIFY样式，才能发送STN_ENABLE消息，父窗口通过WM_COMMAND消息接收该消息
WM_CTLCOLORSTATIC   发送WM_CTLCOLORSTATIC消息，给父窗口，表示该控件将要被绘制，父窗口使用DC句柄类设置文本和背景颜色，父窗口在WM_CTLCOLORSTATIC中接收该消息
```

###我们来创建一个简单的图像静态文本框
边框:SS_BLACKFRAME(黑色)、SS_GRAYFRAME(灰色)、SS_WHITEFRAME(白色)、SS_ETCHEDFRAME、SS_ETCHEDHORZ、SS_ETCHEDVERT六种样式来设置文本框的边框样式                 

区域填充: SS_BLACKRECT(黑色)、SS_GRAYRECT(灰色)、SS_WHITERECT(白色)三种样式来设置文本框的填充色                     

注：以上的边框和区域填充一共9种样式，每次只能使用一种，多了也没用。然后就是使用了以上9种样式后，文本框中的文字不显示，如下
```cpp
HWND xs_frame = CreateWindow(_T("STATIC"), _T("文本框"), SS_BLACKFRAME, 0,0,100,100,hWnd, (HMENU)2001, hInst, NULL);
```
###文本框文字的样式
SS_LEFT(左对齐)、SS_LEFTNOWORDWRAP(左对齐，不能换行)、SS_CENTER(居中)、SS_CENTERIMAGE(垂直居中)、SS_RIGHT(右对齐)、SS_SIMPLE(简单)六种样式            

下面我们来创建一个让文本框的文字水平垂直居中的文本框，如下
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_CENTER | SS_CENTERIMAGE, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
```
###改变文本框中的文本
可以使用SetWindowText函数和WM_SETTEXT消息,在任何时候改变文本框中的文本                
```cpp
BOOL WINAPI SetWindowText(
  _In_      HWND hWnd,
  _In_opt_  LPCTSTR lpString
);
```
HWND hWnd 窗口句柄              
LPCTSTR lpString 新的标题或文本             

下面我们来把文本框的文字用SetWindowText函数改变为center，如下
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_CENTER | SS_CENTERIMAGE, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
SetWindowText(xs_center, _T("center"));
```

```cpp
#define WM_SETTEXT                      0x000C
```
wParam 值为NULL            
lParam 指向一个字符串的指针             

下面我们用WM_SETTEXT消息来改变文本框的文本，如下
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_CENTER | SS_CENTERIMAGE, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
SendMessage(xs_center, WM_SETTEXT, NULL, (LPARAM) _T("center"));
```

###文本框中加载图片
可以使用SS_BITMAP(位图)、SS_ICON(图标)、SS_ENHMETAFILE(图元文件)样式来加载图片               
使用以上样式后，还需要给系统发送STM_SETIMAGE(设置图像)、STM_SETICON(设置图标)消息来设置对应的图像或图标         

###STM_SETIMAGE消息          
wParam IMAGE_BITMAP(位图)、IMAGE_CURSOR(光标)、IMAGE_ENHMETAFILE(增强型图元文件)、IMAGE_ICON(图标)             
lParam HBITMAP或HCON句柄                   

现在我们SS_BITMAP来加载一个位图试试,如下,注:STM_SETICON的lParam值是位图句柄
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_BITMAP, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
HBITMAP bp = LoadBitmap(hInst, MAKEINTRESOURCE(IDB_BITMAP2));
SendMessage(xs_center, STM_SETIMAGE, IMAGE_BITMAP, (LPARAM) bp);
```
###STM_SETICON消息
wParam 图标句柄          
lParam 为空，NULL               

下面我们来用SS_ICON加载一个图标，因为SS_ICON默认只能加载SS_CXICON、SS_CYICON尺寸的图标，所以我们还需要使用SS_REALSIZEIMAGE样式指定资源实际的宽度，但是该样式指定必须使用LoadImage函数来加载图标，所以我们先了解LoadImage函数
```cpp
HANDLE WINAPI LoadImage(
  _In_opt_  HINSTANCE hinst,
  _In_      LPCTSTR lpszName,
  _In_      UINT uType,
  _In_      int cxDesired,
  _In_      int cyDesired,
  _In_      UINT fuLoad
);
```
HINSTANCE hinst 实例句柄                
LPCTSTR lpszName 资源文件名，使用MAKEINTRESOURCE宏获取               
UINT uType 图像类型，值可以是IMAGE_BITMAP(位图)、IMAGE_CURSOR(光标)、IMAGE_ICON(图标)            
int cxDesired 宽度， 单位像素 ,如果值为0,fuLoad值为LR_DEFAULTSIZE,使用SM_CXICON和SM_CXCURSOR系统默认值设置宽度，如果值为0，fuLoad值不是LR_DEFAULTSIZE，使用实际资源的宽度                     
int cyDesired 高度，单位像素 ,如果值为0,fuLoad值为LR_DEFAULTSIZE,使用SM_CYICON和SM_CYCURSOR系统默认值设置高度，如果值为0，fuLoad值不是LR_DEFAULTSIZE，使用实际资源的高度                            
UINT fuLoad 值为下列值
```text
LR_CREATEDIBSECTION          当UINT uType值IMAGE_BITMAP，返回一个DIB位图，而不是兼容位图
LR_DEFAULTCOLOR              默认值，什么也不做，not LR_MONOCHROME
LR_DEFAULTSIZE               如果指定该值，并且cxDesired和cyDesired设置为零，宽度和高度使用系统默认值，如果没有指定该值，使用实际资源的高度和宽度,如果资源中有多个图像，该函数使用第一个图像的尺寸
LR_LOADFROMFILE              从LPCTSTR lpszName字符串(文件名)值加载位图，光标，或图标文件
LR_LOADMAP3DCOLORS           小于8bpp的位图，把灰色调为对应的3D色彩来显示，大于8bpp的位图，不要使用该选项
LR_LOADTRANSPARENT           检索图像中第一个像素的颜色值，并替换为颜色表中缺省窗口的颜色对应的条目，如果位图大于8bpp不要使用该选项，如果同时设置了LR_LOADMAP3DCOLORS和LR_LOADTRANSPARENT，LR_LOADTRANSPARENT优先
LR_MONOCHROME                加载黑白图像
LR_SHARED                    如果图像，被多次加载，LR_SHARED又没有设置，那么第二次调用LoadImage，会返回一个不同的句柄，如果指定该值，那么直接使用该图像第一次请求的句柄，而不管该图像尺寸是否改变            
LR_VGACOLOR                  使用VGA颜色
```

OK，现在我们来试试,注:STM_SETICON的wParam值是图标句柄
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_ICON | SS_REALSIZEIMAGE, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
HICON cp = (HICON)LoadImage(hInst, MAKEINTRESOURCE(IDI_ICON1), IMAGE_ICON, 0, 0, LR_DEFAULTCOLOR);
SendMessage(xs_center, STM_SETICON, (WPARAM) cp, NULL);
```

###STM_GETIMAGE消息
wParam IMAGE_BITMAP(位图)、IMAGE_CURSOR(光标)、IMAGE_ENHMETAFILE(增强型图元文件)、IMAGE_ICON(图标)           
lParam 为空,NULL          

STM_GETIMAGE消息的用处是获取一个位图，光标，图元文件或图标的句柄，如果没有，返回NULL          
```cpp
HWND xs_center = CreateWindow(_T("STATIC"), _T("文本框"), SS_ICON | SS_REALSIZEIMAGE, 0, 0, 100, 100, hWnd, (HMENU)2002, hInst, NULL);
HICON cp = (HICON)LoadImage(hInst, MAKEINTRESOURCE(IDI_ICON1), IMAGE_ICON, 0, 0, LR_DEFAULTCOLOR);
SendMessage(xs_center, STM_SETICON, (WPARAM) cp, NULL);

HWND xs_get = CreateWindow(_T("STATIC"), _T("文本框"), SS_ICON | SS_REALSIZEIMAGE, 100, 0, 100, 100, hWnd, (HMENU)2003, hInst, NULL);
HICON sp = (HICON)SendMessage(xs_center, STM_GETIMAGE, IMAGE_ICON, NULL);
if (HICON)
	SendMessage(xs_get, STM_SETICON, (WPARAM) sp, NULL);
```

###STM_GETICON消息
wParam 为空，0或者NULL         
lParam 为空，0或者NULL             

STM_GETICON消息获取一个图标的句柄，如果没有，返回NULL              
使用方法同STM_GETIMAGE            

