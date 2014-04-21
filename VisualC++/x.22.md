#组合框控件
COMBOBOX控件          
CreateWindow函数和CreateWindowEx函数的组合框样式           

###组合框样式
```text
常量值                     说明
CBS_AUTOHSCROLL            如果字符串长度，超过输入框长度，就显示最后输入的内容                 
CBS_DISABLENOSCROLL        只能输入输入框长度的字符，超过的不能输入         
CBS_DROPDOWN               下拉式组合框，可以点击左边的下拉按钮出现下拉框,可以输入内容                    
CBS_DROPDOWNLIST           下拉式组合框，有下拉菜单，只能选择，不能输入
CBS_HASSTRINGS             指定含有字符串的自绘列表框,可以使用CB_GETLBTEXT 消息
CBS_LOWERCASE              字符转换为小写字母
CBS_NOINTEGRALHEIGHT       组合框的尺寸是程序指定，而不是windows.由windows创建的会使部分列表项某些部分隐藏起来
CBS_OEMCONVERT             使文本可以在ANSI和Unicode直接转换，与 CBS_SIMPLE or CBS_DROPDOWN一起使用 
CBS_OWNERDRAWFIXED         自绘列表框，父窗口进行绘制，列表项高度一样，WM_MEASUREITEM 消息， WM_DRAWITEM消息
CBS_OWNERDRAWVARIABLE      自绘列表框，父窗口进行绘制，列表项高度不一样，WM_MEASUREITEM 消息， WM_DRAWITEM消息
CBS_SIMPLE                 简易组合框
CBS_SORT                   自动对列表框组件的项进行排序
CBS_UPPERCASE              字符转换为大写字母
```

####CB_GETLBTEXT
获取组合框的字符串，返回值 字符串的长度，不包含终止符，如果wParam无效，返回CB_ERR           
wParam 字符串从0开始的索引         
lParam 指针，接收字符串的缓冲区，可以在发送CB_GETLBTEXT消息前，发送CB_GETLBTEXTLEN消息获取字符串长度             

####CB_GETLBTEXTLEN
获取组合框字符串长度，单位字符，返回值字符的长度，不包含终止符，ANSI返回字节，UNICODE返回字符,如果wPram无效，返回CB_ERR                   
wParam 字符串从零开始的索引       
lParam 不使用          

####WM_MEASUREITEM
WindowProc接收该消息，如果程序处理此消息，返回TRUE                       
wParam MEASUREITEMSTRUCT结构的CtlID成员指向lParam参数该值发送WM_MEASUREITEM消息的控制，值零，表示消息是由菜单发出，值不为零，是由组合框或列表框发送 如果MEASUREITEMSTRUCT结构的itemID成员值指向lParam值-1，该消息是由一个组合编辑字段发送           
lParam 指向包含绘制控件或菜单项的尺寸的MEASUREITEMSTRUCT结构

####WM_DRAWITEM
WindowProc接收该消息，如果程序处理此消息，返回TRUE           
wParam 指定发送WM_DRAWITEM消息的控件标识符，如果消息由菜单发送，这个参数是零，否则非零        
lParam 指向包含绘制信息的DRAWITEMSTRUCT 结构            

####CB_ADDSTRING消息
添加一个字符串到组合框            
wParam 不使用            
lParam 