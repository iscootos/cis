#字符串数组的相互转换
split()方法是把字符串转换为数组        
join()方法是把数组转换为字符串
####split()
字符串方法，把一个字符串分隔为数组,参数为分隔符，如果使用空字符串`""`作为分隔符，那么每个字符之间都会被分隔          
```js
var str = "hello world";
document.write(str.split(""));
```
输出
```text
h,e,l,l,o, ,w,o,r,l,d
```
用空格作为分隔符
```js
var str = "hello world";
document.write(str.split(" "));
```
输出
```text
hello,world
```
####toString()
数组方法，把数组转换为字符串，并返回字符串,但是数组中的元素之间用逗号分隔
```js
var arr = ["one","two","three"];
document.write(arr.toString());
```
```text
one,two,three
```
####join()
数组方法，把数组中的所有元素放入一个字符串，并使用指定的分隔符分隔，如果没有使用参数，效果和toString是一样的，如果使用空字符串，就是没有分隔符
```js
var arr = ["one","two","three"];
document.write(arr.join());
```
```text
one,two,three
```
空字符串
```js
var arr = ["one","two","three"];
document.write(arr.join(""));
```
```text
onetwothree
```
冒号分隔符
```js
var arr = ["one","two","three"];
document.write(arr.join(":"));
```
```text
one:two:three
```
