#随机生成MAC地址
因为有时候需要修改电脑的MAC地址，但是每次都要不一样，自己想太费事，所以就写了一个JS函数
```js
function macaddr(len) {
	var obj = new Object();
	obj.len = len;
	var MAC = ['0','1','2','3','4','5','6','7','8','9','A','B','C','D','E','F'];

	obj.addr = function(len) {
		var arr = [];
		if (len == undefined || len == '') {
			if (this.len != undefined && this.len != '') {
				len = this.len;
			} else {
				len = 12;
			}
		}
		
		var sum = 0;
		for (var i = 0; i < len; i++) {
			sum += Math.floor(Math.random() * 10);
			if (sum >= MAC.length)
				sum %= MAC.length;
			arr.push(MAC[sum]);
		}
		this.address = arr;
		return arr;
	};
	obj.address = obj.addr(len);

	obj.format = function(separtor) {
		var arr = [];
		for (var i = 0; i < this.address.length; i++) {
			arr.push(this.address[i]);
			if (i != 0 && (i + 1) != this.address.length && (i + 1) % 2 == 0 && separtor != undefined && separtor != '')
				arr.push(separtor);
		}
		
		return arr.join('');
	};

	return obj;
}
```
该函数，有一个参数，表示需要输出的MAC地址的长度，MAC默认长度为12位             
该函数，返回一个对象，该对象有两个函数，`addr()`函数用于重新生成MAC地址，也有一个长度参数         
`format()`函数，用于控制输出的MAC地址的格式，默认所有的MAC地址按顺序输出，如果使用了参数，那么，MAC地址每2位一组，参数指定的分隔符分开
```c
var mac = macaddr();
alert(mac.format());     //  0115A349E55E
alert(mac.format('-'));  //  01-15-A3-49-E5-5E
alert(mac.format(':'));  //  01:15:A3:49:E5:5E
```
