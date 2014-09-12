#指定元素添加删除事件
给指定HTML元素添加事件句柄    
```js
document.getElementById('btn').onclick=myclick();
```
以上代码，当我们点击id为btn的元素时，执行myclick函数         
但是如果我们想在同一个元素里面添加多个同样的事件，那么最后的事件会覆盖掉之前的事件                              
####addEventListener()
addEventListener()方法可以向指定HTML元素添加事件句柄           
```text
element.addEventListener(event, function, useCapture)
```
`event`参数，必须指定参数值，字符串，指定事件名称，不带on前缀，如`click`、`dbclick`等               
`function`参数，必须指定参数值，指定事件触发时需要执行的函数                 
`useCapture`参数，为可选参数，布尔值，指定事件是否在捕获或冒泡阶段执行，可选值，true事件句柄在捕获阶段执行，false默认，事件在冒泡阶段执行           
```js
document.getElementById('btn').addEventListener('click', myFunction);
```
同一个元素可以添加多个事件，添加的事件不会覆盖已存在的事件
```js
document.getElementById("myBtn").addEventListener("click", myFunction);
document.getElementById("myBtn").addEventListener("click", someOtherFunction);
```
同一个元素中添加不同类型的事件
```js
document.getElementById("myBtn").addEventListener("mouseover", myFunction);
document.getElementById("myBtn").addEventListener("click", someOtherFunction);
document.getElementById("myBtn").addEventListener("mouseout", someOtherFunction);
```
当传递参数值时，使用"匿名函数"调用带参数的函数
```js
document.getElementById("myBtn").addEventListener("click", function() {
    myFunction(p1, p2);
});
```
修改 `<button>` 元素的背景
```js
document.getElementById("myBtn").addEventListener("click", function(){
    this.style.backgroundColor = "red";
});
```
####removeEventListener()
removeEventListener()方法用于移除由addEventListener()方法添加的事件句柄        
```js
/* 添加 <div> 事件句柄 */
document.getElementById("myDIV").addEventListener("mousemove", myFunction);

/* 移除 <div> 事件句柄 */
document.getElementById("myDIV").removeEventListener("mousemove", myFunction);
```
####attachEvent()
attachEvent()方法用于IE9以下的浏览器,用于给元素添加事件句柄           
```text
element.attachEvent(event, function);
```
`event`参数，就是事件名称，如`onclick`等            
`function`参数，就是触发事件时需要执行的函数          
```js
document.getElementById('btn').attachEvent('onclick', myfunction);
```
####detachEvent()
detachEvent()方法用于移除由attachEvent()方法添加的事件句柄         
```js
document.getElementById('btn').attachEvent('onclick', myfunction);
document.getElementById('btn').detachEvent('onclick', myfunction);
```
####备注
那么通过以上我们就知道了，如何给元素添加事件句柄，以及移除，我们需要自己写一个函数，来实现浏览器更好的兼容      
添加事件句柄函数
```js
function fEventListen(xelement, xevent, xfunction, xbool)
{
	xbool = !!xbool;
	if(xelement.addEventListener){
		xelement.addEventListener(xevent, xfunction, xbool);
	}else{
		if(xelement.attachEvent){
			xelement.attachEvent("on" + xevent, xfunction);
		}
	}
}
```
移除事件句柄函数
```js
function fEventUnListen(xelement, xevent, xfunction, xbool)
{
	xbool = !!xbool;
	if(xelement.removeEventListener){
		xelement.removeEventListener(xevent, xfunction, xbool);
	}else{
		if(xelement.detachEvent){
			xelement.detachEvent("on" + xevent, xfunction);
		}
	}
}
```
