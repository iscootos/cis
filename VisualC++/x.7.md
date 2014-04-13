#位图信息头结构
需要头文件
```cpp
#include <Wingdi.h>
```
```cpp
typedef struct tagBITMAPINFOHEADER {
  DWORD biSize;
  LONG  biWidth;
  LONG  biHeight;
  WORD  biPlanes;
  WORD  biBitCount;
  DWORD biCompression;
  DWORD biSizeImage;
  LONG  biXPelsPerMeter;
  LONG  biYPelsPerMeter;
  DWORD biClrUsed;
  DWORD biClrImportant;
} BITMAPINFOHEADER, *PBITMAPINFOHEADER;
```
biSize 本结构的大小  

biWidth 位图的宽度，以像素为单位,如果biCompression 值为BI_JPEG 或者 BI_PNG,那么指定解压缩JPEG或PNG的图片文件的宽度  

biHeight 位图的高度，以像素为单位，如果biHeight是正数，位图是一个自下而上的DIB和它的起点是左下角，如果biHeight是负数，位图是一个自上而下的DIB和它的起点是左上角          
如果biHeight为负数，表明是自顶向下的DIB，biCompression 必须是BI_RGB 或BI_BITFIELDS,自顶向下的DIB不能被压缩       
如果biCompression 值为BI_JPEG 或BI_PNG, biHeight 指定解压缩JPEG 或PNG图片文件的高度  

biPlanes 显示设备的数量，值必须是1  

biBitCount 像素深度,每像素所占位数
```text
值        说明
0         指定的像素数目，或JPEG或PNG默认的数目
1         位图是单色的,而BITMAPINFO结构的bmiColors成员有2个数组，位图数组的每个位代表一个像素，如果该位是明确的，像素会显示bmiColors表中的第一个数组的颜色，如果该位被设置，该像素显示数组中第二项的颜色   
4         16色， BITMAPINFO结构bmiColors,有16项可选，位图中每个像素由一个4位索引颜色表来表示，如果位图的第一个字节是0x1F,该字节表示两个像素，第一个像素中包含颜色中的第二表现，和第二像素中包含的色彩
8         256色BITMAPINFO结构bmiColors,有256项可选，阵列中的每个字节表示一个像素 
16        2^16色，BITMAPINFOHEADER 结构biCompression值为BI_RGB, BITMAPINFO结构bmiColors值NULL。位图阵列中的每个字节表示一个像素，red,green,blue的相对强度，表示与5位每个颜色分量，该bmiColors颜色表用于优化对基于调色板的设备中使用的颜色，而且必须包含BITMAPINFOHEADER结构biClrUsed 指定的数目
如果BITMAPINFOHEADER 结构biCompression 值为BI_BITFIELDS,bmiColors包含3个DWORD彩色掩码用于指定red,green,blue分量的每个像素,位图阵列每个WORD代表一个像素
biCompression 值BI_BITFIELDS，每个DWORD 掩码中设置的位必须是连续的，
24        2^24 BITMAPINFO结构bmiColors 值NULL，位图阵列每3个字节代表blue,green,red,相对强度分别为一个像素，该bmiColors 颜色表用于优化基于调色板设备使用的颜色，必须包含BITMAPINFOHEADER结构biClrUsed指定的数目
32        2^32 BITMAPINFOHEADER 结构biCompression值BI_RGB，BITMAPINFO结构bmiColors 值NULL，位图阵列中每个DWORD表示blue,green,red像素的相对强度，每个DWORD的高字节不使用，该bmiColors 颜色表用于优化，必须包含BITMAPINFOHEADER结构biClrUsed 指定的数目

biCompression 值BI_BITFIELDS每个DWORD 掩码中设置的位必须是连续的，
```
biCompression 压缩自底向上的位图，(自顶向下的不能被压缩)
```text
值        说明
BI_RGB    不压缩
BI_RLE8   8bpp压缩
BI_RLE4   4bpp压缩
BI_BITFIELDS  不压缩，并且颜色由3个DWORD彩色掩码指定red,green,blue,必须是16和32bpp的位图
BI_JPEG   JPEG图像
BI_PNG    PNG图像
```

biSizeImage 位图点阵的长度,图像大小，以字节为单位，还可以设置为零 BI_RGB位图，如果biCompression是BI_JPEG或BI_PNG，biSizeImage表示JPEG图像缓冲区的大小        

biXPelsPerMeter 水平分辨率,以像素为单位    

biYPelsPerMeter 垂直分辨率，以像素为单位  

biClrUsed 有效颜色数，颜色索引表中的颜色数量被实际使用的位图，值为零，则位图使用的对应于biBitCount通过biCompression指定的压缩模式的值的最大色彩数     
如果biClrUsed非零，和biBitCount小于16，则biClrUsed指定颜色的实际数目，如果biBitCount是16，或者更大，biClrUsed指定用于优化系统调色板的颜色表现表的大小，如果biBitCount等于16或32，最优的调色板使用以下3个DWORD    
biClrUsed值必须是零或颜色表的实际大小 

biClrImportant 重要的颜色索引个数，值为零，所有颜色都很重要 

####BITMAPINFO       
```cpp
typedef struct tagBITMAPINFO {
  BITMAPINFOHEADER bmiHeader;
  RGBQUAD          bmiColors[1];
} BITMAPINFO, *PBITMAPINFO;
```
bmiHeader 一个BITMAPINFOHEADER结构  

bmiColors RGBQUAD数组，数组组成颜色表中的元素      
16位无符号整数数组，用于指定索引到当前实现的逻辑调色板，这种使用bmiColors是允许使用DIB函数，当bmiColors元素包含索引来实现一个逻辑调色板，它们还必须调用下面的位图功能 
CreateDIBitmap、CreateDIBPatternBrush、CreateDIBSection       
CreateDIBSection 的iUsage 参数必须设置为DIB_PAL_COLORS    
数组中数目，取决于BITMAPINFOHEADER结构biBitCount 和biClrUsed的值

####BITMAPFILEHEADER 
```cpp
typedef struct tagBITMAPFILEHEADER {
  WORD  bfType;
  DWORD bfSize;
  WORD  bfReserved1;
  WORD  bfReserved2;
  DWORD bfOffBits;
} BITMAPFILEHEADER, *PBITMAPFILEHEADER;
```
bfType 文件类型，必须是字符BM,0x4D42               
bfSize 位图文件的大小，以字节为单位              
bfReserved1 保留字，必须是0             
bfReserved2 保留字，必须是0              
bfOffBits 偏移量，以字节为单位，位图的数据信息到BITMAPFILEHEADER结构的偏移量  ,位图的数据信息离文件头的偏移量               

####RGBQUAD
```cpp
typedef struct tagRGBQUAD {
  BYTE rgbBlue;
  BYTE rgbGreen;
  BYTE rgbRed;
  BYTE rgbReserved;
} RGBQUAD;
```
rgbBlue 蓝色的颜色强度                
rgbGreen 绿色的颜色强度              
rgbRed 红色的颜色强度          
rgbReserved 保留字，必须为零               

