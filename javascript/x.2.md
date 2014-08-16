#IE兼容html5
如下，通过`split()`方法，分隔为数组，但是我们会发现太长了，不利于代码阅读
```js
(function() {
	var aTag = 'abbr, article, aside, audio, canvas, datalist, details, dialog, eventsource, figure, footer, header, hgroup, mark, menu, meter, nav, output, progress, section, time, video'.split(', '),
		i = aTag.length;
	while (i--)
		document.createElement(aTag[i]);
})();
```
如下，使用遍历的方法，创建html标签对象
```js
(function() {
	var aTag = [
		'abbr', 'article', 'aside', 'audio', 'canvas', 'datalist', 'details',
		'dialog', 'eventsource', 'figure', 'footer', 'header', 'hgroup',
		'mark', 'menu', 'meter', 'nav', 'output', 'progress', 'section',
		'time', 'video'
	],
		i = 0;
	for (i in aTag) {
		document.createElement(aTag[i]);
	}
})();
```
