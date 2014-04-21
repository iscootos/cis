#按钮控件
BUTTON样式       

###按钮样式
```text
常量值               说明
BS_3STATE            小方框复选框，但是复选框，为灰色不可用
BS_AUTO3STATE        小方框复选框，复选框，有三种状态，可以使用,选中，取消选中，选中变为灰色
BS_AUTOCHECKBOX      小方框复选框，复选框，有两种状态，可以使用,选中，取消选中   
BS_AUTORADIOBUTTON   小圆点单选按钮，可以选中，但必须点选同一组的其他单选按钮，才能取消选中
BS_CHECKBOX          小方框复选框，但是复选框，不可用
BS_DEFPUSHBUTTON     按钮，可以点击
BS_GROUPBOX          这个不太好描述，就是一个矩形，然后lpWindowName的文字显示在矩形左上角的线上面，应用的时候多用到
BS_LEFTTEXT          默认文字都是在小圆点单选按钮后面的，这个参数的意思就是文字放在最左边，小圆点单选按钮到后面去了 
BS_OWNERDRAW         自绘按钮，发送WM_DRAWITEM消息，该按钮的视觉效果发生了变化，BS_OWNERDRAW不能与其他风格一起使用
BS_PUSHBUTTON        创建按钮，并发送一个WM_COMMAND消息
BS_RADIOBUTTON       小圆点单选按钮，不可用
BS_USERBUTTON        过时，请使用BS_OWNERDRAW 
BS_BITMAP            指定按钮显示的位图,BS_ICON
BS_BOTTOM            文字在按钮的底部
BS_CENTER            文字在按钮水平居中
BS_ICON              指定按钮会显示一个图标，BS_BITMAP
BS_FLAT              指定按钮是2D的，不使用3D阴影按钮
BS_LEFT              文字或者复选框，都按左边对齐
BS_MULTILINE         如果lpWindowName字符串太长，将多行显示在按钮上
BS_NOTIFY            使一个按钮发送BN_KILLFOCUS 和 BN_SETFOCUS消息给它的父窗口，没有该参数的按钮，也会发送BN_CLICKED消息，为了得到BN_DBLCLK 消息，必须设置BS_RADIOBUTTON 和BS_OWNERDRAW风格
BS_PUSHLIKE          把复选框，单选框，之类的外观变得跟按钮一样
BS_RIGHT             文字或者复选框，都按右边对齐
BS_RIGHTBUTTON       让小方框复选框,小圆点单选按钮显示在按钮右边，效果同BS_LEFTTEXT
BS_TEXT              指定按钮显示文本
BS_TOP               文字显示在按钮顶部
BS_TYPEMASK          
BS_VCENTER           文字在按钮垂直居中
BS_SPLITBUTTON
BS_DEFSPLITBUTTON
BS_COMMANDLINK
BS_DEFCOMMANDLINK
```

###CheckDlgButton函数
改变按钮的选中状态          
```cpp
BOOL CheckDlgButton(
  _In_  HWND hDlg,
  _In_  int nIDButton,
  _In_  UINT uCheck
);
```
HWND hDlg 对话框句柄           
int nIDButton 按钮标识符         
UINT uCheck 按钮的选择状态值: BST_CHECKED(设置按钮状态进行检查)、BST_INDETERMINATE(BS_3STATE和BS_AUTO3STATE样式时，设置按钮为灰色，表示不确定)、BST_UNCHECKED(重置按钮状态)

返回值：成功返回非零，失败返回零     

###CheckRadioButton函数
添加一个复选标记，再删除组中其他单选按钮           
```cpp
BOOL CheckRadioButton(
  _In_  HWND hDlg,
  _In_  int nIDFirstButton,
  _In_  int nIDLastButton,
  _In_  int nIDCheckButton
);
```
HWND hDlg 对话框句柄      
int nIDFirstButton 改组的第一个单选按钮标识符           
int nIDLastButton 本组最后一个单选按钮标识符           
int nIDCheckButton 单选按钮标识符来选择            

返回值成功返回非零，失败返回零          

###IsDlgButtonChecked函数
该函数确定按钮是否被选择或三种状态之一
```cpp
UINT IsDlgButtonChecked(
  _In_  HWND hDlg,
  _In_  int nIDButton
);
```
HWND hDlg 对话框句柄          
int nIDButton 按钮控件标识符     

返回值 如果按钮是 BS_AUTOCHECKBOX, BS_AUTORADIOBUTTON, BS_AUTO3STATE, BS_CHECKBOX, BS_RADIOBUTTON, 和 BS_3STATE 这些样式，返回值如下，如果是其他样式，返回值零            
```text
BST_CHECKED             选中
BST_INDETERMINATE       灰色，不确定(BS_3STATE or BS_AUTO3STATE样式)
BST_UNCHECKED           没有选中
```


###按钮消息
####BCM_GETIDEALSIZE
获取最合适的文本和图像大小，成功返回TRUE,失败返回FALSE        
wParam 不使用，必须为零         
lParam 指针，指向按钮所需要的大小，如果是一个尺寸结构，程序分配该结构，cx cy 为零，以获得理想的宽度和高度，要指定按钮宽度，设置cx值，系统会计算理想高度并返回          

####BCM_GETIMAGELIST
获取按钮的图像列表，成功返回TRUE,失败返回FALSE            
wParam 不使用，必须为零          
lParam 指针，BUTTON_IMAGELIST结构，包含图像列表信息             

```cpp
typedef struct {
  HIMAGELIST himl;
  RECT       margin;
  UINT       uAlign;
} BUTTON_IMAGELIST, *PBUTTON_IMAGELIST;
```
HIMAGELIST himl 图像列表句柄           
RECT       margin RECT结构，指定该图标的RECT结构           
UINT       uAlign 指定要对齐的方式              
```text
常量值                               说明 
BUTTON_IMAGELIST_ALIGN_LEFT          左对齐
BUTTON_IMAGELIST_ALIGN_RIGHT         右对齐
BUTTON_IMAGELIST_ALIGN_TOP           顶部对齐
BUTTON_IMAGELIST_ALIGN_BOTTOM        底部对齐
BUTTON_IMAGELIST_ALIGN_CENTER        居中
```

####BCM_GETNOTE
获取与命令相关的说明文字，成功TRUE,失败FALSE        
wParam DWORD，指定缓冲区大小，指向lParam参数以字符为单位           
lParam 字符串缓冲区接收文本，缓冲区大小要包含终止符            

####BCM_GETNOTELENGTH
获取文本注释的长度，返回长度值，不包含终止符，如果没有注释返回NULL或0          
wParam 不使用，必须为0            
lParam 不使用，必须为0            

####BCM_GETSPLITINFO
获取按钮信息，成功TRUE,失败FALSE         
wParam 必须为0         
lParam 指针，指向接收BUTTON_SPLITINFO结构            

####BCM_GETTEXTMARGIN
获取绘制按钮文本边缘，成功TRUE,失败FALSE           
wParam 不使用，必须为0      
lParam 指针，指向RECT结构           

####BCM_SETDROPDOWNSTATE
设置下拉状态的按钮样式TBSTYLE_DROPDOWN, 成功TRUE,失败FALSE         
wParam BOOL值，BST_DROPDOWNPUSHED的状态，打开TRUE,关闭FALSE        
lParam 必须为0           

####BCM_SETIMAGELIST
设置一个按钮的图像列表，成功返回TRUE,失败返回FALSE          
wParam 不使用，必须为零       
lParam 指针，BUTTON_IMAGELIST 结构         

####BCM_SETNOTE
设置说明文字，成功返回TRUE,失败返回FALSE          
wParam 必须为0         
lParam 指针，指向wchar字符串           

####BCM_SETSHIELD
设置按钮的高度，成功返回1，失败错误代码          
wParam 必须0       
lParam BOOL值，            

####BCM_SETSPLITINFO
设置分隔按钮控制信息
wParam 必须0          
lParam 执行BUTTON_SPLITINFO结构         
####BCM_SETTEXTMARGIN
设置按钮页边距，成功TRUE,失败FALSE          
wParam 不使用，必须0        
lParam 指针，RECT结构         

####BM_CLICK
模拟点击一个按钮，不返回值       
wParam 不使用，必须0          
lParam 不使用，必须0          

####BM_GETCHECK
获取单选按钮的选中状态          
wParam 不使用，必须0          
lParam 不使用，必须0 

样式必须是BS_AUTOCHECKBOX, BS_AUTORADIOBUTTON, BS_AUTO3STATE, BS_CHECKBOX, BS_RADIOBUTTON, or BS_3STATE返回以下值        
```text
BST_CHECKED           选中
BST_INDETERMINATE     灰色，不确定，( BS_3STATE or BS_AUTO3STATE样式)
BST_UNCHECKED         没有选中
```

####BM_GETIMAGE
获取图像句柄，返回图像句柄，否则NULL             
wParam 值 IMAGE_BITMAP(位图), IMAGE_ICON(图标)            
lParam 不使用            

####BM_GETSTATE
检查复选框按钮状态，         
wParam 不使用，必须0          
lParam 不使用，必须0          

返回值
```text
BST_CHECKED             选中
BST_DROPDOWNPUSHED      vista,TBSTYLE_DROPDOWN样式，按钮处于下拉状态
BST_FOCUS               该按钮，具有键盘焦点
BST_HOT                 鼠标悬停在按钮上
BST_INDETERMINATE       灰色，不确定， BS_3STATE or BS_AUTO3STATE样式
BST_PUSHED             该按钮被按下的状态
BST_UNCHECKED          无特殊状态，相当于0
```

####BM_SETCHECK
设置按钮选中状态，不返回值        
wParam 值BST_CHECKED(选中)、BST_INDETERMINATE(不确定，灰色)、BST_UNCHECKED(没有选中)         
lParam 不使用           

####BM_SETDONTCLICK
设置单选按钮，BN_CLICKED 消息产生当按钮接收焦点的标志，没有返回值            
wParam BOOL值，TRUE设置标志，否则FALSE   
lParam 不使用，必须0           

####BM_SETIMAGE
设置图像，返回图像句柄，否则NULL          
wParam IMAGE_BITMAP,IMAGE_ICON      
lParam HICON或HBITMAP句柄       

####BM_SETSTATE
设置按钮的高亮度状态，总返回0         
wParam BOOL值，TRUE高亮，否则FALSE          
lParam 不使用        

####BM_SETSTYLE
设置按钮样式，总返回0        
wParam 按钮样式的值,或多个     
lParam BOOL值，TRUE重绘按钮，FALSE不重绘         


###经常使用的一些函数、消息、通知          
####BN_CLICKED
当用户点击一个按钮发送该通知代码，父窗口用WM_COMMAND消息接收该代码             
wParam LOWORD控件标识符，HIWORD通知代码           
lParam 按钮句柄           

现在我们来点击一个按钮，弹出一个对话框
```cpp
#define XS_LOGIN  2001;
HWND IS_LOGIN;

IS_LOGIN = CreateWindow(_T("BUTTON"), _T(""), BS_BITMAP | WS_VISIBLE | WS_CHILD, 0,0,100,100,hWnd, (HMENU) XS_LOGIN, hInst, NULL);

HBITMAP SP = LoadBitmap(hInst, MAKEINTRESOURCE(IDB_BITMAP1));

SendMessage(IS_LOGIN, BM_SETIMAGE, IMAGE_BITMAP, (LPARAM)SP);

/* 消息 */
switch(message)
{
case WM_COMMAND:
	wmId = LOWORD(wParam);
	wmEvent = HIWORD(wParam);

	/* 判断控件标识符和通知代码 */
	if (wmId == XS_LOGIN && wmEvent == BN_CLICKED)
	{
		MessageBox(NULL, _T("登录按钮"), _T("tit"), MB_OK);
	}

	break;
}
```
####WM_LBUTTONDOWN
按下鼠标左键，通过WindowProc函数接收该消息            
```cpp
#define WM_LBUTTONDOWN                  0x0201
```
wParam 表示是否有按键按下          
```text
常量值        十六进制      说明
MK_CONTROL    0x0008        CTRL
MK_LBUTTON    0x0001        鼠标左键       
MK_MBUTTON    0x0010        鼠标中键
MK_RBUTTON    0x0002        鼠标右键
MK_SHIFT      0x0004        SHIFT
MK_XBUTTON1   0x0020        第1个X按钮
MK_XBUTTON2   0x0040        第2个X按钮
```

lParam GET_X_LPARAM(LPARAM)左上角X坐标，GET_Y_LPARAM(LPARAM)左上角Y坐标                    

需要包含`windowsx.h`头文件，GET_X_LPARAM、GET_Y_LPARAM返回int类型             

如果程序处理此消息，返回零     

####WM_LBUTTONUP
放开鼠标左键，通过WindowProc函数接收           
```cpp
#define WM_LBUTTONUP                    0x0202
```
wParam 表示是否有按键按下          
```text
常量值        十六进制      说明
MK_CONTROL    0x0008        CTRL
MK_LBUTTON    0x0001        鼠标左键       
MK_MBUTTON    0x0010        鼠标中键
MK_RBUTTON    0x0002        鼠标右键
MK_SHIFT      0x0004        SHIFT
MK_XBUTTON1   0x0020        第1个X按钮
MK_XBUTTON2   0x0040        第2个X按钮
```
lParam GET_X_LPARAM(LPARAM)左上角X坐标，GET_Y_LPARAM(LPARAM)左上角Y坐标  

如果程序处理此消息，返回零 

####WM_MOUSEMOVE
在鼠标移动时，发送已捕获焦点的窗口，WindowProc函数接收           
```cpp
#define WM_MOUSEMOVE                    0x0200
```
wParam 表示是否有按键按下          
```text
常量值        十六进制      说明
MK_CONTROL    0x0008        CTRL
MK_LBUTTON    0x0001        鼠标左键       
MK_MBUTTON    0x0010        鼠标中键
MK_RBUTTON    0x0002        鼠标右键
MK_SHIFT      0x0004        SHIFT
MK_XBUTTON1   0x0020        第1个X按钮
MK_XBUTTON2   0x0040        第2个X按钮
```
lParam GET_X_LPARAM(LPARAM)左上角X坐标，GET_Y_LPARAM(LPARAM)左上角Y坐标  

如果程序处理此消息，返回零 

####WM_SETFOCUS
发送一个消息给窗口，告诉它已经获得键盘焦点               
```cpp
#define WM_SETFOCUS                     0x0007
```
wParam 已经失去键盘焦点的窗口句柄，值可以为NULL           
lParam 不使用           

如果程序处理此消息，返回零
```cpp
HWND xsi = CreateWindow(_T("COMBOBOX"), _T(""), CBS_DROPDOWN | WS_GROUP | WS_TABSTOP | WS_CHILD | WS_VISIBLE, 110, 175, 168, 26, hWnd, (HMENU) 2001, hInst, NULL);
SendMessage(xsi, WM_SETFOCUS, NULL, NULL);
```







