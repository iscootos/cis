#让IE支持getElementsByClassName
IE9以下的版本是不支持getElementsByClassName函数的，所以我们可以使用下面的js代码实现
```js
if(!document.getElementsByClassName){ 
	document.getElementsByClassName = function(className, element){ 
		var children = (element || document).getElementsByTagName('*'); 
		var elements = new Array(); 
		for (var i=0; i<children.length; i++){ 
			var child = children[i]; 
			var classNames = child.className.split(' '); 
			for (var j=0; j<classNames.length; j++){ 
				if (classNames[j] == className){ 
				elements.push(child); 
				break; 
				} 
			} 
		} 
		return elements; 
	}; 
}
```
这样就让不支持getElementsByClassName函数的IE浏览器支持该函数了