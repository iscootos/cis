#常用自写函数
####$id()
函数通过调用getElementById()方法，返回ID的元素节点
```js
function $id(sId)
{
	return document.getElementById(sId);
}
```
####fTrim()
函数通过正则表达式`\s`匹配空格，并去除首尾所有空格，返回首尾没有空格的字符串
```js
function fTrim(a){
	return a.replace(/(^\s*)|(\s*$)/g,"").replace(/(^　*)|(　*$)/g,"");
}
```
####fEventListen()
函数给指定HTML对象添加事件句柄
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
####fEventUnListen()
函数移除指定HTML对象的事件句柄
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
####fCls()
函数添加移除classname
```js
/**
 * 添加删除classname
 * @param  {Object} o     修改对象dom元素
 * @param  {String} sCls  classname
 * @param  {String} sFunc 修改classname方式：add/remove
 */
function fCls(o, sCls, sFunc){
	var oTar = o;
	var nRes = oTar.className.indexOf(sCls);
	if(sFunc == 'add'){
		if(nRes == -1){
			oTar.className = oTar.className + ' ' + sCls;
		}else{
			return;
		}
	}
	if(sFunc == 'remove'){
		if(nRes != -1){
			var sCls = '\\s' + sCls
			var rCls = new RegExp(sCls, 'g');
			oTar.className = oTar.className.replace(rCls, '');
		}else{
			return;
		}
	}
}
```
