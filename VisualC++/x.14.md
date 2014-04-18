#DC的操作
####BitBlt
BitBlt函数的功能就是把图像数据块从一个位置搬移到另一个位置，源和目标可以在同一个DC,也可以在不同的DC上               
我们可以简单的理解为，BitBlt最主要的功能就是把内存DC中的图像数据块拷贝到设备DC中                 
```cpp
BOOL BitBlt(
  _In_  HDC hdcDest,
  _In_  int nXDest,
  _In_  int nYDest,
  _In_  int nWidth,
  _In_  int nHeight,
  _In_  HDC hdcSrc,
  _In_  int nXSrc,
  _In_  int nYSrc,
  _In_  DWORD dwRop
);
```
hdcDest 就是目标设备DC句柄      
nXDest  左上角X坐标，目标设备       
nYDest 左上角Y坐标，目标设备    
nWidth 源在目标矩形区域的宽度     
nHeight 源在目标矩形区域的高度
hdcSrc 就是源设备的DC句柄    
nXSrc  指定源矩形区域左上角X坐标
nYSrc 指定源矩形预期左上角Y坐标    
dwRop指定光栅操作代码，这些代码将定义源矩形区域的颜色数据，如何与目标矩形区域的颜色数据组合以完成最后的颜色          
```text
值           说明
BLACKNESS    使用与物理调色板的索引0相关的色彩来填充目标矩形区域，(缺省的物理调色板颜色为黑色)
CAPTUREBLT   
DSTINVERT    表示使目标矩形区域颜色取反
MERGECOPY    表示使用BOOL的AND(&&)操作符将矩形颜色与特定模式组合一起
MERGEPAINT   表示使用BOOL的OR(||)操作符将反向的源矩形区域的颜色与目标矩形区域的颜色合并
NOMIRRORBITMAP
NOTSRCCOPY   将源矩形区域颜色取反，于拷贝到目标矩形区域
NOTSRCERASE  使用布尔类型的OR（或）操作符组合源和目标矩形区域的颜色值，然后将合成的颜色取反
PATCOPY      将特定的模式拷贝到目标位图上
PATINVERT    通过使用XOR（异或）操作符将源和目标矩形区域内的颜色合并
PATPAINT     通过使用布尔OR（或）操作符将源矩形区域取反后的颜色值与特定模式的颜色合并。然后使用OR（或）操作符将该操作的结果与目标矩形区域内的颜色合并
SRCAND       通过使用AND（与）操作符来将源和目标矩形区域内的颜色合并
SRCCOPY      将源矩形区域直接拷贝到目标矩形区域
SRCERASE     通过使用AND（与）操作符将目标矩形区域颜色取反后与源矩形区域的颜色值合并
SRCINVERT    通过使用布尔型的XOR（异或）操作符将源和目标矩形区域的颜色合并
SRCPAINT     通过使用布尔型的OR（或）操作符将源和目标矩形区域的颜色合并
WHITENESS    使用与物理调色板中索引1有关的颜色填充目标矩形区域。（对于缺省物理调色板来说，这个颜色就是白色）
```
如果函数成功，那么返回值非零，如果失败，返回零       
