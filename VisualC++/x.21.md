#输入框控件
EDIT控件

###输入框样式
CreateWindow和CreateWindowEx函数创建输入框控件的时候，可以指定输入框的样式，可以是以下的一个值或多个值    
```text
常量值               说明
ES_AUTOHSCROLL       如果字符串长度，超过输入框宽度，那么只显示最后输入的字符,如果同时使用了ES_MULTILINE样式，按回车后，换行
ES_AUTOVSCROLL       只能输入输入框宽度的字符串，超过就不能输入了,如果同时使用了ES_MULTILINE样式，按回车后，换行
ES_CENTER            文字水平居中
ES_LFET              左对齐
ES_LOWERCASE         所有字符转换为小写，要改变这种样式，使用SetWindowLong函数
ES_MULTILINE         多行文本输入框，超出输入框宽度，自动换行
ES_NOHIDESEL         用鼠标选中一段文字，再点击其他地方那么选中的文字，将自动取消，但设置该样式后，选中的文字始终是选中的，除非你取消选中
ES_NUMBER            值允许输入数字，到输入框，但是可以复制非数字到输入框，要改变样式，使用SetWindowLong函数，对话框可以使用GetDlgItemInt函数，SetDlgItemInt函数
ES_OEMCONVERT        输入框中的正文可以在ASCII和UNICODE之间转换，这在输入框中是文件名的时候，非常有用，该变样式，使用SetWindowlong函数
ES_PASSWORD          密码输入框，要改变视觉效果，使用EM_SETPASSWORDCHAR消息
ES_READONLY          只读输入框，不能输入，改变样式，使用EM_SETREADONLY消息
ES_RIGHT             右对齐
ES_UPPERCASE         所有字符转换为大写，要改变这种样式，使用SetWindowLong函数
ES_WANTRETURN        指定该样式，在多行输入框，按回车键，自动换行，单行输入框不受该值影响，要改变这种样式，使用SetWindowLong函数
```
###SetWindowLong改变指定窗口的属性
SetWindowLong改变指定窗口的属性,只支持32位系统
```cpp
LONG WINAPI SetWindowLong(
  _In_  HWND hWnd,
  _In_  int nIndex,
  _In_  LONG dwNewLong
);
```
SetWindowLongPtr同上，同时支持32位和64位系统
```cpp
LONG_PTR WINAPI SetWindowLongPtr(
  _In_  HWND hWnd,
  _In_  int nIndex,
  _In_  LONG_PTR dwNewLong
);
```

先来讲解SetWindowLong函数            
HWND hWnd 窗口句柄                      
int nIndex 从0开始偏移量要设置的值，有效值的范围是0到额外窗口内存的字节数，减去一个整数的大小，设置任何其他值，下列值之一          
```text
常量值                                    
GWL_EXSTYLE         -20          设置新的窗口扩展样式WS_EX开头的扩展样式
GWLP_HINSTANCE      -6           设置一个新的实例句柄
GWLP_ID             -12          设置子窗口一个新的标识符，不能是顶层窗口
GWL_STYLE           -16          设置新的窗口样式WS_开头的样式
GWLP_USERDATA       -21          设置与该窗口关联的用户数据，该数据用于由创建该窗口的程序使用，初始值为0
GWLP_WNDPROC        -4           为wndproc回调函数，设置一个新的地址
```
当窗口句柄是一个对话框的时候也可以使用如下值
```text
DWLP_DLGPROC      DWLP_MSGRESULT + sizeof(LRESULT)       设置新的指针指向对话框程序
DWLP_MSGRESULT    0                                      设置对话框消息过程的返回值
DWLP_USER         DWLP_DLGPROC + sizeof(DLGPROC)         设置程序私有的新的额外的信息，如句柄或指针
```

LONG_PTR dwNewLong 指定的替换值

返回值，成功，返回设置的偏移值，失败返回0            

数据在缓存中，所以设置SetWindowLong 函数后，需要设置SetWindowPos函数来更新

###GetDlgItemInt函数
转换对话框中的文本为整数值
```cpp
UINT WINAPI GetDlgItemInt(
  _In_       HWND hDlg,
  _In_       int nIDDlgItem,
  _Out_opt_  BOOL *lpTranslated,
  _In_       BOOL bSigned
);
```
HWND hDlg 窗口句柄               
int nIDDlgItem 需要转换的控件的标识符            
BOOL *lpTranslated 一个BOOL指针，如果转换成功，它执行的变量为TRUE，失败为FALSE,如果值设置为NULL,放弃该功能           
BOOL bSigned 检查文本是否有`-`减号开头，TRUE处理，FALSE不处理            

###SetDlgItemInt函数
更该对话框中的内容为整数值
```cpp
BOOL WINAPI SetDlgItemInt(
  _In_  HWND hDlg,
  _In_  int nIDDlgItem,
  _In_  UINT uValue,
  _In_  BOOL bSigned
);
```
HWND hDlg 窗口句柄               
int nIDDlgItem 需要更改的控件的标识符           
UINT uValue 整数值，用于指定标识符的文本           
BOOL bSigned 检查uValue的值，如果有`-`减号，并且bSigned为TRUE，那么输入框显示`-`号，如果bSigned为FALSE，那么就是无符号           

###EM_SETPASSWORDCHAR消息
该消息，只对ES_PASSWORD样式输入框，有效，该消息的作用就是，是否显示星号,也就是是否可以直接看到密码   

wParam 如果值为0，直接显示密码           
lParam 值为空，NULL     

如下，在我们输入密码的时候就可以直接看到了
```cpp
HWND xs_pwd = CreateWindow(_T("EDIT"), _T(""), ES_PASSWORD | WS_VISIBLE | WS_CHILD, 0, 0, 100, 100, hWnd, (HMENU) 2001, hInst, NULL);
SendMessage(xs_pwd, EM_SETPASSWORDCHAR, 0, NULL);
```

###EM_SETREADONLY消息
设置或删除ES_READONLY只读样式             

wParam 指定是否使用ES_READONLY样式，值为TRUE使用该样式，值为FALSE删除该样式            
lParam 不使用， NULL      

如下，我们把一个只读输入框，变得可写          
```cpp
HWND xs_ro = CreateWindow(_T("EDIT"), _T(""), ES_READONLY | WS_VISIBLE | WS_CHILD, 0, 0, 100, 100, hWnd, (HMENU) 2001, hInst, NULL);
SendMessage(xs_ro, EM_SETREADONLY, FALSE, NULL);
```

###输入框宏
如下所有宏，必须添加`windowsx.h`头文件后，方可使用       
####Edit_CanUndo 撤消
```cpp
BOOL Edit_CanUndo(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄            
如果在撤消队列中，返回TRUE，否则返回FALSE,(撤消即撤消上一步的意思)                   
是否有输入框或Rich Edit控件在撤消队列中，使用这个宏，可以显式的发送EM_CANUNDO消息,EM_CANUNDO就是发送撤消消息的                
####Edit_EmptyUndoBuffer 
撤消命令，可以撤消很多步，这个宏的意思，就是清空撤消命令中的所有撤消记录，也就是说，现在重置了撤消命令，变为了灰色不可用               
或者你可以显式的发送EM_EMPTYUNDOBUFFER消息                 
```cpp
void Edit_EmptyUndoBuffer(
  HWND hwndCtl
);
```
####Edit_Enable
启用或禁用一个输入框控件
```cpp
BOOL Edit_Enable(
  HWND hwndCtl,
  BOOL fEnable
);
```
HWND hwndCtl 控件句柄            
BOOL fEnable 值为TRUE，启用控件，值为FALSE,禁用控件              
####Edit_FmtLines
设置一个标志，检索一个多行文本中的软换行符，由两个回车和换行，并插入一条线，或者你可以显式发送EM_FMTLINES消息
```cpp
void Edit_FmtLines(
  HWND hwndCtl,
  BOOL fAddEOL
);
```
HWND hwndCtl 控件句柄        
BOOL fAddEOL 值为TRUE,插入换行符,值为FALSE，不插入换行符           
####Edit_GetCueBannerText
获取横幅提示文本,或者显式发送EM_GETCUEBANNER消息
```cpp
BOOL Edit_GetCueBannerText(
  HWND hwnd,
  LPCWSTR lpcwText,
  LONG cchText
);
```
HWND hwnd 控件句柄          
LPCWSTR lpcwText 指针，指向Unicode字符串,获得的文本需要存储的字符串数组          
LONG cchText lpcwText字符串的大小，可以使用sizeof(lpcwText)获得         
####Edit_GetFirstVisibleLine
获取多行输入框最上面的索引，或者显式发送EM_GETFIRSTVISIBLELINE消息 
```cpp
int Edit_GetFirstVisibleLine(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄           
####Edit_GetHandle
获得一个句柄，分配给多行输入框的文本，局部句柄，或者显式发送EM_GETHANDLE消息
```cpp
HLOCAL Edit_GetHandle(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄         
####Edit_GetLine
```cpp
int Edit_GetLine(
  HWND hwndCtl,
  int line,
  LPTSTR lpch,
  int cchMax
);
```
HWND hwndCtl 控件句柄         
int line 从0开始的索引，表示行数，单行输入框，忽略该参数            
LPTSTR lpch 一个指针，指向接收字符串的缓冲区            
int cchMax 复制最大字符到缓冲区，设置最大值

返回值是复制的数量,失败返回0           

EM_GETLINE消息
wParam 从0开始的索引，表示行数，单行输入框，忽略该参数             
lParam 指针，指向一个字符串指针          
####Edit_GetLineCount
获取输入框的文本数，或者发送EM_GETLINECOUNT消息            
```cpp
int Edit_GetLineCount(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄            
####Edit_GetModify
修改标志，表示控件的内容是否被修改，或者发送 EM_GETMODIFY消息
```cpp
BOOL Edit_GetModify(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄
####Edit_GetPasswordChar
获取密码输入框文件，或者发送EM_GETPASSWORDCHAR消息
```cpp
TCHAR Edit_GetPasswordChar(
  HWND hwndCtl
);
```
HWND hwndCtl 控件句柄            
返回值，密码字符           

EM_GETPASSWORDCHAR消息           
wParam 值必须为0               
lParam 值必须为0               
返回密码字符，如果返回NULL，表示空密码            
####Edit_GetRect
获取输入框的RECT结构，或者发送EM_GETRECT消息         
```cpp
void Edit_GetRect(
  HWND hwndCtl,
  RECT *lprc
);
```
HWND hwndCtl 控件句柄          
RECT *lprc RECT结构指针        

####Edit_GetSel
获取输入框开始和结束字符的位置，或发送EM_GETSEL消息
```cpp
DWORD Edit_GetSel(
  HWND hwndCtl
);
```
####Edit_GetText
获取输入框文本       
```cpp
int Edit_GetText(
  HWND hwndCtl,
  LPTSTR lpch,
  int cchMax
);
```
HWND hwndCtl 控件句柄            
LPTSTR lpch 指针，字符缓冲区         
int cchMax 接收字符最大值         
####Edit_GetTextLength
获取输入框字符文本数       
```cpp
int Edit_GetTextLength(
  HWND hwndCtl
);
```
返回文本数     
####Edit_GetWordBreakProc
检索自动换行功能，或发送Edit_GetWordBreakProc消息 
```cpp
EDITWORDBREAKPROC Edit_GetWordBreakProc(
  HWND hwndCtl
);
```
####Edit_HideBalloonTip
隐藏或编辑气球提示,或发送EM_HIDEBALLOONTIP消息
```cpp
BOOL Edit_HideBalloonTip(
  HWND hwnd
);
```
宏成功，返回TRUE,失败返回FALSE     
####Edit_LimitText
限制可以输入到输入框的字符长度，或发送EM_LIMITTEXT消息
```cpp
void Edit_LimitText(
  HWND hwndCtl,
  int cchMax
);
```
HWND hwndCtl 控件句柄           
int cchMax 最大字符数         
####Edit_LineFromChar
获取多行输入框指定字符索引的行的索引，或发送EM_LINEFROMCHAR 消息   
```cpp
int Edit_LineFromChar(
  HWND hwndCtl,
  int ich
);
```
HWND hwndCtl 控件句柄        
int ich 从文本开头字符从零开始的索引           
返回从0开始的索引
####Edit_LineIndex
获取多行文本的第一个字符的字符索引，或发送EM_LINEINDEX消息
```cpp
int Edit_LineIndex(
  HWND hwndCtl,
  int line
);
```
####Edit_LineLength
获取输入框长度，单位字符，或发送EM_LINELENGTH消息
```cpp
int Edit_LineLength(
  HWND hwndCtl,
  int line
);
```
####Edit_ReplaceSel
替换选中文本，或发送EM_REPLACESEL消息
```cpp
void Edit_ReplaceSel(
  HWND hwndCtl,
  LPCTSTR lpszReplace
);
```
####Edit_Scroll
垂直滚动多行文本，或发送EM_SCROLL消息
```cpp
void Edit_Scroll(
  HWND hwndCtl,
  int dv,
  int dh
);
```
HWND hwndCtl 控件句柄          
int dv 垂直滚动量          
int dh 水平滚动量            
####Edit_ScrollCaret
滚动光标到输入框中，或发送EM_SCROLLCARET消息
```cpp
BOOL Edit_ScrollCaret(
  HWND hwndCtl
);
```
####Edit_SetCueBannerText
设置横幅提示文本,或发送EM_SETCUEBANNER消息
```cpp
BOOL Edit_SetCueBannerText(
  HWND hwnd,
  LPCWSTR lpcwText
);
```
####Edit_SetCueBannerTextFocused
设置文本提示，或发送EM_SETCUEBANNER消息
```cpp
BOOL Edit_SetCueBannerTextFocused(
  HWND hwnd,
  LPCWSTR lpcwText,
  BOOL fDrawFocused
);
```
HWND hwnd 控件句柄           
LPCWSTR lpcwText Unicode 字符串           
BOOL fDrawFocused 是否当控件有键盘焦点再显示            
####Edit_SetHandle
设置将要使用的多行文本的内存句柄,或发送EM_SETHANDLE 消息
```cpp
void Edit_SetHandle(
  HWND hwndCtl,
  HLOCAL h
);
```
####Edit_SetModify
设置或清除修改标志，或发送 EM_SETMODIFY消息 
```cpp
void Edit_SetModify(
  HWND hwndCtl,
  BOOL fModified
);
```
####Edit_SetPasswordChar
设置或删除密码框字符串,或发送 EM_SETPASSWORDCHAR消息
```cpp
void Edit_SetPasswordChar(
  HWND hwndCtl,
  UINT ch
);
```
####Edit_SetReadOnly
设置或删除只读文本，或发送 EM_SETREADONLY消息
```cpp
BOOL Edit_SetReadOnly(
  HWND hwndCtl,
  BOOL fReadOnly
);
```
####Edit_SetRect
设置文本RECT结构，或发送 EM_SETRECT消息
```cpp
void Edit_SetRect(
  HWND hwndCtl,
  RECT *lprc
);
```
####Edit_SetRectNoPaint
这个宏相当于不重绘SetRect,或发送 EM_SETRECTNP消息
```cpp
void Edit_SetRectNoPaint(
  HWND hwndCtl,
  RECT *lprc
);
```
####Edit_SetSel
选择一个范围在文本内的字符，或发送 EM_SETSEL消息
```cpp
void Edit_SetSel(
  HWND hwndCtl,
  int ichStart,
  int ichEnd
);
```
####Edit_SetTabStops
设置多行文本的制表符，或发送EM_SETTABSTOPS消息   
```cpp
void Edit_SetTabStops(
  HWND hwndCtl,
  int cTabs,
  int *lpTabs
);
```
####Edit_SetText
设置文本框的文本
```cpp
int Edit_SetText(
  HWND hwndCtl,
  LPTSTR lpsz
);
```
####Edit_SetWordBreakProc
替换控件和程序默认的换行功能，或发送EM_SETWORDBREAKPROC 消息
```cpp
void Edit_SetWordBreakProc(
  HWND hwndCtl,
  EDITWORDBREAKPROC lpfnWordBreak
);
```
####Edit_ShowBalloonTip
显示或编辑气球提示，或发送 EM_SHOWBALLOONTIP消息
```cpp
BOOL Edit_ShowBalloonTip(
  HWND hwnd,
  PEDITBALLOONTIP peditballoontip
);
```
####Edit_Undo
撤消最后一个操作，或发送EM_UNDO消息
```cpp
BOOL Edit_Undo(
  HWND hwndCtl
);
```

###输入框消息

####EM_CANUNDO
如果撤消队列中有撤消命令，返回值非零，如果撤消队列为空，返回零            
wParam 不使用，必须为零         
lParam 不使用，必须为零

####EM_CHARFROMPOS
获取文本框指定坐标的字符信息              
wParam 不使用                    
lParam POINTL结构包含一个点的坐标 

####EM_EMPTYUNDOBUFFER
重置撤消队列中的撤消命令，没有返回值           
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_FMTLINES
多行文本软换行符,返回相同的wParam参数           
wParam 指定软换行符是否插入，TRUE插入，FALSE不插入             
lParam 不使用，必须为零          

此消息值影响EM_GETHANDLE消息，并通过WM_GETTEXT消息返回文本的缓冲区

####EM_GETCUEBANNER
获取文本提示,成功返回TRUE,失败返回FALSE                
wParam 指针，执行Unicode缓冲区,接收文本提示          
lParam 缓冲区的大小，包括终止符 

####EM_GETFIRSTVISIBLELINE
获取多行文本，从零开始的索引，返回从零开始的索引               
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETHANDLE
获取多行文本的内存句柄，返回值内存句柄，内容缓冲区，错误，返回零               
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETIMESTATUS
获取输入法编辑器IME的交互状态标志 ,返回值有三个，EIMES_GETCOMPSTRATONCE | EIMES_CANCELCOMPSTRINFOCUS | EIMES_COMPLETECOMPSTRKILLFOCUS                        
wParam 值EMSIS_COMPOSITIONSTRING，处理组成的字符串行为              
lParam 不使用

####EM_GETLIMITTEXT
获取文本的长度限制，返回值为文本长度限制值           
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETLINE
复制一行文本并放在缓冲区，返回值为复制的数量，如果wParam指定的行数大于文本框，返回零        
wParam 指定行数，从零开始，单行文本忽略该参数             
lParam 指针，指向拷贝缓冲区，设置第一个字符的大小，ANSI为字节，Unicode为文本          

####EM_GETLINECOUNT
获取多行文本的数量，返回值是整数，返回文本总行数，如果没有文本，返回1            
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETMARGINS
获取左、右页边距的宽度，返回值，LOWORD左边距，HIWORD右边距           
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETMODIFY
获取文本修改标志的状态，返回值，已被修改返回非零，否则零           
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETPASSWORDCHAR
获取密码输入框的密码字符，返回值为密码字符，如果没有密码，返回NULL               
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETRECT
获取控件RECT结构，没有返回值            
wParam 不使用，必须为零            
lParam 一个指针，接收RECT结构          

####EM_GETSEL
获取选定内容的起始和结束字符位置            
wParam 指针，指向接收起始位置的DWORD值，可以为NULL          
lParam 指针，指向接收结束位置的DWORD值，可以为NULL           

####EM_GETTHUMB
获取多行文本垂直滚动条的位置，返回值滚动条的位置            
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_GETWORDBREAKPROC
获取当前换行功能的地址，返回值指定程序定义的换行功能的地址，不存在换行功能，返回NULL              
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_HIDEBALLOONTIP
隐藏或显示气球提示，返回值消息成功，返回TRUE，失败返回FALSE          
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_LIMITTEXT
设置文本的长度限制，没有返回值            
wParam 返回最大输入数量，ANSI，字节数，Unicode字符数,这个数字不包含终止符            
lParam 不使用

####EM_LINEFROMCHAR
获取要检索行的字符索引，返回wParam参数指定的行号            
wParam 包含要检索行的字符索引,行号                   
lParam 不使用

####EM_LINEINDEX
获取多行文本第一个字符的字符索引，返回值wParam的行号 ，如果返回-1，表示wParam大于文本行数                       
wParam 从零开始的行号，-1表示当前行号           
lParam 不使用          

####EM_LINELENGTH
获取指定行的长度，返回值 ANSI返回字节数，Unicode返回字符数           
wParam 要检索行的字符长度，如果大于字符数，返回零，参数为-1，           
lParam 不使用             

####EM_LINESCROLL
滚动多行文本，返回值，多行文本TRUE,单行文本FALSE            
wParam 字符数水平滚动           
lParam 行数上下滚动           

####EM_POSFROMCHAR
检索指定坐标的字符，返回字符的坐标  LOWORD水平坐标，HIWORD垂直坐标           
wParam 指向接收字符坐标的POINTL结构          
lParam 不使用           

####EM_REPLACESEL
替换文本，没有返回值            
wParam 指定替换操作是否可以撤消，可以TRUE,不可以FALSE       
lParam 指向要替换的字符串           

####EM_SCROLL
垂直滚动多行文本，消息成功，返回值HIWORD值TRUE,LOWORD是行的滚动数量,返回FALSE，说明wParam值无效     
wParam 有多个参数，SB_LINEDOWN(向下滚动一行)、SB_LINEUP(向上滚动一行)、SB_PAGEDOWN(向下滚动一页)、SB_PAGEUP(向上滚动一页)      
lParam 不使用           

####EM_SCROLLCARET
滚动光标到文本中，没有返回值           
wParam 不使用，必须为零            
lParam 不使用，必须为零

####EM_SETCUEBANNER
设置横幅文字提示，成功返回TRUE,失败FALSE          
wParam 键盘焦点显示，TRUE，否则FALSE            
lParam 指针，执行Unicode字符串           

####EM_SETHANDLE
设置货航文本内存句柄，没有返回值        
wParam 句柄到内存缓冲区用来存储当前显示的文本           
lParam 不使用     

####EM_SETIMESTATUS
IME交互状态            
wParam 值EMSIS_COMPOSITIONSTRING            
lParam 值EIMES_GETCOMPSTRATONCE | EIMES_CANCELCOMPSTRINFOCUS | EIMES_COMPLETECOMPSTRKILLFOCUS

####EM_SETLIMITTEXT
设置文本可以输入的长度上限，没有返回值            
wParam 长度值，ANSI字节，Unicode字符,如果值为0,默认64000个字符            
lParam 不使用          

####EM_SETMARGINS
设置左右页边距的宽度，          
wParam EC_LEFTMARGIN(左边距)、EC_RIGHTMARGIN(右边距)、EC_USEFONTINFO、             
lParam 新的宽度，单位像素           

####EM_SETMODIFY
设置或清除修改标志，没有返回值            
wParam 值TRUE表示已修改，值FALSE没有修改         
lParam 不使用       

####EM_SETPASSWORDCHAR
设置是否用星号显示密码输入框的字符，没有返回值          
wParam 值0，显示字符            
lParam 不使用                   

####EM_SETREADONLY
设置文本是否只读，成功返回非零，失败返回零            
wParam 值TRUE设置ES_READONLY样式，值FALSE删除ES_READONLY样式           
lParam 不使用            

####EM_SETRECT
设置多行文本RECT结构，没有返回值          
wParam 不使用，必须为零                
lParam 指针，RECT结构，值NULL, 格式化矩形为默认值           

####EM_SETRECTNP
设置多行文本格式化RECT结构，没有返回值             
wParam 不使用，必须为零                
lParam 指针，RECT结构，值NULL, 格式化矩形为默认值  

####EM_SETSEL
选择文本内一个范围内的字符，没有返回值           
wParam 选择的起始字符位置         
lParam 选择的结束字符位置           

####EM_SETTABSTOPS
设置多行文本的制表符，凡有选项卡设置返回TRUE,没有设置返回FALSE         
wParam 制表符的数目      
lParam 一个无符号整数数组指定制表符          

####EM_SETWORDBREAKPROC
替换默认换行功能，没有返回值           
wParam 不使用       
lParam 程序换行功能的地址            

####EM_SHOWBALLOONTIP
显示或隐藏气球提示，成功返回TRUE,失败返回FALSE        
wParam 不使用，必须为零        
lParam 指针，指向气球提示的EDITBALLOONTIP结构         

####EM_UNDO
撤消，撤消队列的最后一次撤消命令，单行文本始终返回TRUE,多行文本成功返回TRUE,失败返回FALSE               
wParam 不使用，必须为零            
lParam 不使用，必须为零

####WM_UNDO
撤消上一个操作，成功返回TRUE,失败返回FALSE        
wParam 不使用，必须为零            
lParam 不使用，必须为零







