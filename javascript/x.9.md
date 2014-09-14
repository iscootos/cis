#var关键字的高级应用
之前觉得js里面的var声明的局部变量，跟全局变量唯一的区别就是不会互相覆盖，其实还是有很大区别的         
```js
var tt = 10;
function test()
{
	alert(tt);
	var tt = 20;
	alert(tt);
}
test();
```
上面的代码，大家一定觉得，两次都应该输出20，但是实际输出为`undefined`，`20`，为什么呢?
```js
alert(tt);
var tt = 20;
```
上面我们可以看出，函数内部已经定义并初始化了tt变量，它的作用域就是在这个函数的内部         
但是，alert(tt)是在这个变量之前使用的，这句话就变成了，声明tt变量，但没有初始化所以就输出了`undefined`        
而如果要想调用外面的tt,我们可以使用`window.tt`或`this.tt`效果都是一样的，全局函数内部的this就是window      
