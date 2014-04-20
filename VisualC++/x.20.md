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
SS_WHITEFRAME        默认黑色边框
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
