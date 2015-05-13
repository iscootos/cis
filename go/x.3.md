###go-sql-driver
下载地址: [https://github.com/go-sql-driver/mysql](https://github.com/go-sql-driver/mysql)            
点击`Download ZIP`下载          
首先讲下，我们创建GO工程就必须要先设置GOPATH变量，也就是工程目录         
例如工程calc,路径`/code/go/calc`，设置为GOPATH          
然后我们在该路径下创建`src`、`pkg`、`bin`三个文件夹       
所有的源文件放在`src`文件夹下，每个模块用一个文件夹包含           
例如我们想创建一个sim的模块，那么创建`/code/go/calc/src/sim`文件夹，并在该文件夹下创建go源文件              
如上面下载的`go-sql-driver`源文件，解压缩后，把该`mysql`文件夹复制到`/code/go/calc/src`目录下            
然后执行`go install mysql`命令，即安装了`mysql`包，然后就可以引用了        
为什么是`mysql`可以看go源文件，知道包的名称，然后就可以在`/code/go/calc/pkg/linux_amd64`目录下看到`mysql.a`包了          
最后编译程序的时候，只需要执行`go install calc`就可以了