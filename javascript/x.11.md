#cookie
cookie是存储在用户计算机中的变量           
cookie有名称，值和失效日期,一旦电脑的时钟过了失效日期，这个cookie就会被删除，我们不能直接删除一个cookie,但是可以设定失效日期早于现在时刻的方法来间接删除它                   
```js
document.cookie = 'user=allen';
```
每一个cookie都是一个名/值对，可以设置多个cookie
```js
document.cookie = 'user=allen';
document.cookie = 'pwd=123';
```
存储在电脑的格式是用分号和空格分隔开的`; `
```text
user=allen; pwd=123
```
如果要改变一个cookie的值，只需要重新赋值就可以了         
```js
document.cookie = 'user=iro';
```
如果设置过期时间，首先我们需要知道怎么设置时间
```js
var t = new Date(); //获取当前时间
t.getTime();  //当前日期的毫秒数
t.setTime(1000);  //设置毫秒数
```
这里我写了一个函数，参数为毫秒，返回一个UTC的时间格式，因为过期时间必须是UTC时间或GMT时间        
```js
function cookieTime(m)
{
	var t = new Date();
	t.setTime(t.getTime() + m);
	return t.toUTCString();
}
```
expires参数设置cookie的过期时间，如下代码，这个cookie将在1秒后失效删除，如果不设置该参数，那么当关闭浏览器自动删除 
```js
document.cookie = 'user=allen; expires=' + cookieTime(1000);
```
path参数，可以设置可以访问cookie的路径，如果没有该参数，默认为`path=/`就是根目录，该网站下的所有页面都可以访问，如果我们设置为`path=\c\`那么就是跟目录下的c文件夹下的所有页面才可以访问，其他根目录下非c文件夹下的文件都不能访问到这个cookie
```js
document.cookie = 'user=allen; expires=' + cookieTime(1000) + '; path=/c/';
```
domain参数，可以设置该cookie所在的域，访问`http://www.test.com/test/a.html`,那么设置`domain=www.test.com`就可以了，但如果有t1.test.com，t2.test.com两个网站，我们可以设置`domain=.test.com`，就可以使两个网站都可以使用该cookie了，如果只希望一个可以使用，而另一个不可以访问`domain=t1.test.com`，经过测试domain的值，不能设置为localhost，可以设置IP地址和域名，如果不设置那么就是该网站的域名
```js
document.cookie = 'user=allen; expires=' + cookieTime(1000) + '; domain=.test.com';
```
因为cookie只支持一次添加一个cookie值，所以我写了一个函数，参数是一个对象，这样就实现了一次添加多个cookie值的方法        
```js
function setCookie(o)
{
	if (typeof o != 'object')
		return;
	for (var i in o) {
		document.cookie = i + '=' + o[i];
	}
}
```
当我们使用这个函数后，输出的字符串为
```js
setCookie({"name":"allen","age":27});
alert(document.cookie);
```
```text
name=allen; age=27
```
如果我们要取出cookie中的一个值的话，那么使用下面的函数           
document.cookie.length检查cookie的长度大于0               
document.cookie.indexOf检查要获取的属性是否存在，不存在返回-1            
document.cookie.substring从cookie字符串中提取值        
```js
function getCookie(d)
{
	var start, end;
	if (0 < document.cookie.length) {
		var c = d + '=';
		start = document.cookie.indexOf(c);
		if (-1 < start) {
			start += c.length;
			end = document.cookie.indexOf(';', start);
			if (0 > end) {
				end = document.cookie.length;
			}
			return unescape(document.cookie.substring(start, end));
		}
	}
	return null;
}
```
例如我们先创建一个cookie,再获取它的值
```js
setCookie({"name":"allen","age":27});
alert(getCookie("age"));
```


