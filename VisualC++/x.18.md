#CreateWindow函数的原理
首先再讲CreateWindow函数之前，我们来讲讲windows系统创建一个窗口的过程           
例如我们要建一个房子，首先我们需要准备相关材料(WNDCLASSEX结构)，然后去相关部门注册(RegisterEX函数),如果注册成功          
那么就可以开始打一个地基(CreateWindow函数创建一个主窗口),然后我们就可以在上面建我们喜欢的东西了(CreateWindow函数创建各种控件)        
注: 因为CreateWindow函数会发送WM_PAINT消息，所以CreateWindow函数不能在WM_PAINT函数中使用,否则会一直重新绘制窗口            
```cpp
HWND WINAPI CreateWindow(
  _In_opt_  LPCTSTR lpClassName,
  _In_opt_  LPCTSTR lpWindowName,
  _In_      DWORD dwStyle,
  _In_      int x,
  _In_      int y,
  _In_      int nWidth,
  _In_      int nHeight,
  _In_opt_  HWND hWndParent,
  _In_opt_  HMENU hMenu,
  _In_opt_  HINSTANCE hInstance,
  _In_opt_  LPVOID lpParam
);
```
lpClassName 是类的名称，是一个CONST 字符串指针，名称可以是系统预先定义的各种控件的名称，如下表
```text
系统预定义类      说明
BUTTON            按钮
COMBOBOX          下拉输入框
EDIT              输入框
LISTBOX           列表框
MDICLIENT         MDI客户窗口
RichEdit          富编辑框1.0
RICHEDIT_CLASS    富编辑框2.0 
SCROLLBAR         滚动框
STATIC            静态文本框
```

lpWindowName 是一个CONST 字符串指针，它的用途，在于lpClassName参数的值
```text
lpClassName值                说明
窗口类                       标题栏上的文字
按钮                         按钮上的文字
输入框                       输入框默认的文字
静态文件框                   文本框上的文字
```
dwStyle 风格，该参数依然，取决于lpClassName参数的值
####BUTTON
```text
按钮风格             说明
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
BS_TYPEMASK          文字在按钮垂直居中
BS_VCENTER
BS_SPLITBUTTON
BS_DEFSPLITBUTTON
BS_COMMANDLINK
BS_DEFCOMMANDLINK
```
一个按钮控件的图标和外观取决于BS_BITMAP和BS_ICON样式，以及 BM_SETIMAGE 消息是否发送               
所以我们需要手动发送一个 BM_SETIMAGE 消息         

wParam参数 :  IMAGE_BITMAP     IMAGE_ICON              
lParam参数 :  HICON或HBITMAP句柄         

下面我们实践一个指定位图的按钮
```cpp
HWND xsbu = CreateWindow(_T("BUTTON"), _T("登录"), BS_BITMAP | WS_VISIBLE | WS_CHILD, 110,263,58,26,hWnd, NULL, hInst, NULL);
HBITMAP SP = LoadBitmap(hInst, MAKEINTRESOURCE(IDB_BITMAP));
SendMessage(xsbu, BM_SETIMAGE, IMAGE_BITMAP, (LPARAM)SP);
```

####COMBOBOX
因为组合框是由编辑框和列表框组合而成的，所以组合框的操作和编辑框与列表框的操作有很多相似之处
```text
组合框
CBS_AUTOHSCROLL         使编辑框组件具有水平滚动的风格
CBS_DISABLENOSCROLL     使列表框在不需要滚动时显示一个禁止的垂直滚动条
CBS_DROPDOWN            同CBS_SIMPLE，指定一个下拉式组合框
CBS_DROPDOWNLIST        同CBS_DROPDOWN，指定一个下拉列表式组合框
CBS_HASSTRINGS          指定一个含有字符串的自绘式组合框
CBS_LOWERCASE           将编辑框和列表框中的所有文本都自动转换为小写字符
CBS_NOINTEGRALHEIGHT    组合框的尺寸由应用程序而不是Windows 指定，通常，由Windows指定尺寸会使列表项的某些部分隐藏起来
CBS_OEMCONVERT          使编辑框组件中的正文可以在ANSI 字符集和OEM字符集之间相互转换。这在编辑框中包含文件名时是很有用的
CBS_OWNERDRAWFIXED      指定自绘式组合框，即由父窗口负责绘制列表框的内容，并且列表项有相同的高度
CBS_OWNERDRAWVARIABLE   指定自绘式组合框，并且列表项有不同的高度
CBS_SIMPLE              指定一个简易组合框
CBS_SORT                自动对列表框组件中的项进行排序
CBS_UPPERCASE           将编辑框和列表框中的所有文本都自动转换为大写字符
```
####EDIT
```text
编辑框
ES_AUTOHSCROLL          自动增加水平滚动条
ES_AUTOVSCROLL          当按下Enter键后，自动切换到下一页
ES_CENTER               文本居中
ES_LEFT                 文本左对齐
ES_LOWERCASE            把所有的字母都小写
ES_MULTILINE            建立多行文本编辑框
ES_NOHIDESEL            当失去输入焦点时，选中的文本将隐藏
ES_NUMBER
ES_OEMCONVERT           把输入的文本从ANSI码转换成OEM码，然后又转换成ANSI码，这样的目的是保证函数AnsiToOem的正确调用
ES_PASSWORD             控制编辑框作为密码文本框的字符形式
ES_READONLY             文本只读
ES_RIGHT                文本右对齐
ES_UPPERCASE            将所有的字符转换成大写字符
ES_WANTRETURN
```
####LISTBOX
```text
列表框       
LBS_COMBOBOX            通知列表框，它是一个组合框的一部分，这使得两个控件之间协调，组合框本身必须设置此样式
LBS_DISABLENOSCROLL     该样式就是显示滚动条的,如果不指定这个样式，滚动条是隐藏的，此样式必须使用WS_VSCROLL和WS_HSCROLL风格,
LBS_EXTENDEDSEL         可用SHIFT和鼠标或指定键组合来选择多个条目
LBS_HASSTRINGS          记忆了添加到列表中的字串
LBS_MULTICOLUMN         允许多列显示
LBS_MULTIPLESEL         允许条目多选
LBS_NODATA
LBS_NOINTEGRALHEIGHT    按程序设定尺寸创建列表框
LBS_NOREDRAW            当条目被增删后不自动更新列表显示
LBS_NOSEL               条目可视但不可选
LBS_NOTIFY              当用户选择或双击一个串时，发出消息通知父窗口
LBS_OWNERDRAWFIXED      创建一个具有相同条目高度的拥有者画列表框
LBS_OWNERDRAWVARIABLE   创建一个拥有者画列表框，条目高度可以不同
LBS_SORT                按字母排序
LBS_STANDARD            创建一个具有边界和垂直滚动条、当选择发生变化或条目被双击时能够通知父窗口的标准列表框。所有条目按字母排序
LBS_USETABSTOPS         允许使用TAB制表符
LBS_WANTKEYBOARDINPUT   当有键按下，向系统发送WM_VKEYTOITEM消息
```
####STATIC
```text
静态文本框
SS_BITMAP              指定在静态控件中显示一个被定义在资源文件中的位图。该风格将忽略静态控件的高度和宽度，静态控件将根据位图的大小自动调节自身的尺寸
SS_BLACKFRAME          黑色边框的静态文本框
SS_BLACKRECT           建立一个黑色的矩形
SS_CENTER              文字居中显示
SS_CENTERIMAGE         当显示文字时，文字居中显示，显示位图时，位图垂直居中
SS_EDITCONTROL
SS_ENDELLIPSIS
SS_ENHMETAFILE         指定在静态控件中显示一个增强型图元文件。该风格将不会忽略静态控件的高度和宽度，而图元文件将调节自身的大小来适应静态控件的尺寸
SS_ETCHEDFRAME        建立一个浮雕边框
SS_ETCHEDHORZ         建立一个边框，并将顶端边框设置为浮雕风格
SS_ETCHEDVERT         建立一个边框，并将左侧边框设置为浮雕风格
SS_GRAYFRAME          建立一个灰色的边框
SS_GRAYRECT           建立一个灰色的矩形
SS_ICON               指定在静态控件中显示一个被定义在资源文件中的图标。该风格将忽略静态控件的高度和宽度，静态控件将根据位图的大小自动调节自身的尺寸
SS_LEFT               使文字在静态控件中左对齐
SS_LEFTNOWORDWRAP     在缺省情况下，静态控件把’\n’和’\t’都作为换行标记。只有在设置本风格后，静态控件才把’\t’看作是制表键(缺省时制表键的宽度为8个字符的宽度)
SS_NOPREFIX           该标志表示终止对前缀字符的处理。通常，本成员函数将前缀助记符’&’解释为一个指令，即在’&’后面的字符下面划一下划线。并且将’&&’解释成一个单个的’&’指令。通过指令指定该标志，这种处理就不再进行了
SS_NOTIFY             在缺省情况下，静态控件是不响应鼠标事件的。只有在设置该风格后，当用户单击静态控件时，静态控件才向父窗口发送STN_CLICKED通知
SS_OWNERDRAW          在指定该风格后，当静态控件在视觉外观发生变化时，该静态控件的属主窗口将响应WM_DRAWITEM消息
SS_PATHELLIPSIS
SS_REALSIZECONTROL
SS_REALSIZEIMAGE
SS_RIGHT              使文字在静态控件中左对齐
SS_RIGHTJUST
SS_SIMPLE             只显示一行文本，文本不能被剪切或替换(父窗口不能处理CTLCOLOR消息)
SS_SUNKEN             设置一个下沉的静态控件，当静态控件为一个方框时，方框的四边下沉；当静态控件为一个矩形时，整个矩形下沉
SS_TYPEMASK
SS_WHITEFRAME         建立一个白色的边框
SS_WHITERECT          建立一个白色的矩形
SS_WORDELLIPSIS
```

int x : 左上角X坐标            
int y : 左上角Y坐标       
int nWidth : 宽度，单位像素      
int nHeight : 高度，单位像素       
HWND hWndParent : 如果创建的窗口是一个子窗口，输入该子窗口的父窗口句柄                

HMENU hMenu :  菜单句柄，如果不使用菜单，值NULL，如果是创建子窗口，该项为子窗口标识符，该值必须是唯一的,为一个整数,如 `(HMENU)2001`          
HINSTANCE hInstance : 实例句柄      
LPVOID lpParam : 默认NULL    