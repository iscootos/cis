#获取元素的节点
####getElementById()
我们可以使用getElementById()方法，通过元素的ID，获取到元素的节点，然后进行操作，但是代码重复并且冗余，所以可以使用下面的函数
```js
function $id(sId)
{
	return document.getElementById(sId);
}
```
