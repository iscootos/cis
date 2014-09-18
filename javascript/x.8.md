#对象与数组的高级应用
创建对象的简单方法，对象直接量
```js
var o = {};
```
构造函数创建对象
```js
var o = new Object();
```
创建对象的时候，一般都推荐使用var声明来创建对象，以免创建的对象为全局对象，但是创建对象属性的时候，就不需要var声明了，如果使用了，反而会提示错误
```js
var o = {};
o.name = "allen";
```
```js
var o = {"name":"allen","age":27};
```
除了上面两种创建对象的属性的方法，还可以采用数组式的方式来创建对象，这在实际编程中非常有用
```js
var o = {};
o["name"] = "allen";
```
访问对象属性的时候，同样可以使用两种方式，它们的结果都是一样的
```js
document.write(o.name);
document.write(o[name]);
####遍历对象的属性
遍历对象的属性可以使用for...in，来循环获取对象的所有属性名称
```js
var o = {"name":"allen","age":27};
for (var i in o) {
	document.write(o[i]);
}
```
```text
allen27
```
####遍历数组的值
遍历数组使用for...in的话，获取到的是数组的下标，然后再通过下标来获取到值
```js
var o = ["name","age"];
for (var i in o) {
	document.write(o[i]);
}
```
```text
nameage
```
