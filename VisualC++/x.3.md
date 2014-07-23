#编译zlib库和libpng库
1.在[http://www.libpng.org/pub/png/libpng.html](http://www.libpng.org/pub/png/libpng.html)下载最新的libpng源码             
2.在[http://www.zlib.net/](http://www.zlib.net/)下载最新的zlib源码       
3.（非常重要）在libpng的解压目录下有`projects/vstudio`文件夹，最新的代码里面有vs2010的工程文件，在`.sln`旁边有`zlib.props`这个属性文件，这个文件非常重要，工程文件使用它来定位zlib的目录。默认情况下，该文件的指定的zlib目录是位于和libpng解压文件夹顶层目录同级的目录，而且文件夹叫做zlib-1.2.5。但是现在下载的zlib版本已经到了1.2.8，你放置zlib文件夹的位置也可能在其它地方。这时你需要修改`zlib.props`里面的`<ZLibSrcDir>zlib path</ZLibSrcDir>`标签的`zlib path`为你的zlib目录所在位置，一般指定相对路径                
4.编译链接（如果你像我一样需要32和64位的lib，那么可以分别把zlib和另外几个工程生成的文件文件名都修改成你想要的，比如我把64位下的lib名称分别改为了zlib_x64, zlibd_x64, libpng_x64, libpngd_x64）               
5.把编译出来的lib和zlib.h, zconf.h, png.h, pngconf.h, pnglibconf.h拷贝到你的工程中使用

其它需要注意的地方：在编译debug模式的时候一定要保证`属性页`->`c/c++`->`代码生成中的运行库`为MTd或者MDd。我开始没注意到这个问题，默认的选项在debug模式下也为MD，这样在测试的时候发现在写入png图像的时候在Debug模式下总是会出错，但是在Release下工作正常。后来修改这个选项后正确修复问题
