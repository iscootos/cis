#URI编码
JS中可以把字符串作为URI进行编码,但是对在URI中具有特殊含义的ASCII标点符号，是不会进行转意义的         
`encodeURI()`函数，不转义的符号
```text
 , / ? : @ & = + $ #
 ```
`encodeURIComponent`函数不转义的符号
```text
- _ . ! ~ * ' ( )
```
`escape()`函数，不转义的符号
```text
* @ - _ + . / 
```
```js
url = 'http://127.0.0.1/demo?name&^%$%$89&*&&';
document.write(encodeURI(url));
document.write(encodeURIComponent(url));
document.write(escape(url));
```
如下，转换得最彻底的是`encodeURIComponent()`函数
```text
http://127.0.0.1/demo?name&%5E%25$%25$89&*&&
http%3A%2F%2F127.0.0.1%2Fdemo%3Fname%26%5E%25%24%25%2489%26*%26%26
http%3A//127.0.0.1/demo%3Fname%26%5E%25%24%25%2489%26*%26%26
```

