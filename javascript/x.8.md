#对象与数组的高级应用关联数组
创建对象的简单方法，对象直接量
```js
var obj = {};
var obj = {name: 'max'};
var obj = {name: {}, text: []};
```
也可以使用new操作符后跟构造函数，所以
```js
var a = new Array();
var d = new Date();
var r = new RegExp('javascript', 'i');
var o = new Object();
```
以下全部返回function，Object是Function的实例，Function是特殊的对象，也是Object的实例 
```js
typeof Array  //返回function
typeof Object  //返回function
typeof Date  //返回function
typeof RegExp  //返回function
```
对象属性使用.符号来存取属性的值            
同时也可以使用[]，里面使用属性名(可使用变量，这点特别有用)       
```js
var t = {};
t.text = 'hello';
t.o = {};
t.o.name = 'rd';
t.n = [];

var t = {
	"text": "hello"
};

alert(t.text);
```
通常使用关键字`var`来声明变量，但是声明对象属性时，不能使用`var`声明       
下面我们就给window创建了一个属性            
```js
window['o'] = 'hello world';
alert(window.o);
```
那么从上面我们可以看出，我们可以像创建数组一样来创建对象的属性和方法