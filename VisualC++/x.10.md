#关于Debug、Release两个版本的文件不能同时使用
今天用了个静态库lib,用Release生成的                  
然后在MFC程序里调用了这个库，用Debug编译的时候，老是提示各种外部引用函数错误， 百度了半天，最后看到Debug版的程序必须使用Debug的库，然后，重新生成了Debug的库，链接，正常了。 闹心的问题，总算是解决.

注：由此得出        
Debug 库文件， 只能用于 Debug程序          
Release 库文件， 只能用于 Release程序         
