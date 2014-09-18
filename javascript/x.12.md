#字符编码
####charAt()
字符串方法，用于返回指定位置的字符,参数为字符串下标从0开始        
```js
var str = 'hello world';
for (var i = 0; i < str.length; i++)
	document.write(str.charAt(i) + '<br />');
```
如上，所示就依次打印出了所有的字符串           
####charCodeAt()
字符串方法，用于返回指定位置的字符的Unicode编码，这个值是0-65535之间的整数          
```js
var str = 'hello world';
for (var i = 0; i < str.length; i++)
	document.write(str.charCodeAt(i) + '<br />');
```
如上，所示就依次打印出了所有的字符串的Unicode编码    
####fromCharCode()
String的静态方法，使用指定的Unicode编码，来创建字符，参数可以是1个或多个          
```js
for (var i = 0; i < 100; i++)
	document.write('Unicode: ' + i + '   char: ' + String.fromCharCode(i) + '<br />');
```
如上，所示就依次打印出了0-100所有Unicode编码对应的字符           
