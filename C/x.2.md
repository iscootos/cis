#JSON
JSON是一种无序的，可嵌套的key-value键值对集合            
同一级的key-value键值对之间用逗号`,`分隔            
每个key-value键值对是由一个key后面紧跟一个`:`冒号，冒号后面是这个key对应的value            
key是一个单词，由字母、数字、下划线组成，可以由双引号括起来，也可以不加双引号              
value值的类型包括`string`，`number`，`object`，`array`，`true`，`false`，`null`七种数据格式
```js
"{name:\"iro\", age:27, birth: {year:2015, month:08,day:20}}"
```
```js
"{\"name\":\"iro\", \"age\":\"27\", \"birth\": {\"year\":\"2015\", \"month\":\"08\",\"day\":\"20\"}}"
```
```js
"{\"colors\":[\"red\",\"green\",\"blue\",\"yellow\",\"white\"]}"
```
####cJSON库
cJSON是纯C语言写的用于解析JSON语句的库，只有两个文件`cJSON.h`、`cJSON.c`           
JSON数据格式和结构定义都在`cJSON.h`,分别对应JSON的七种数据格式
```c
#define cJSON_False   0
#define cJSON_True    1
#define cJSON_NULL    2
#define cJSON_Number  3
#define cJSON_String  4
#define cJSON_Array   5
#define cJSON_Object  6
```
如下就是表示JSON格式的，结构体
```c
typedef struct cJSON {
	struct cJSON *next,*prev;  //指向上一项/下一项
	struct cJSON *child;  //指向下一级

	int type;  //对应上面的7种类型

	char *valuestring;
	int valueint;
	double valuedouble;

	char *string;  //key-value键值对的key
} cJSON;
```
`type`表示该结构所对应的value的数据类型，不同的数据类型，放入对应的结构成员中如下
```text
JSON数据类型          cJSON结构成员
number                valuedouble
number、true、false   valueint
string                valuestring
null、object、array   child
```
`cJSON_Parse()`函数，传入一个JSON格式的字符串，返回cJSON结构的指针
```c
extern cJSON *cJSON_Parse(const char *value);
```
`cJSON_Delete()`函数，传入一个cJSON结构的指针，释放内存
```c
extern void   cJSON_Delete(cJSON *c);
```
`cJSON_GetObjectItem()`函数，传入一个cJSON结构的指针，和key的名称，返回对应的cJSON格式的指针
```c
extern cJSON *cJSON_GetObjectItem(cJSON *object,const char *string);
```
`cJSON_GetArraySize()`函数，传入一个cJSON结构的指针，返回该数组的长度
```c
extern int	  cJSON_GetArraySize(cJSON *array);
```
`cJSON_GetArrayItem()`函数，传入一个cJSON结构的指针，和数组小标，返回该数组小标的cJSON结构指针
```c
extern cJSON *cJSON_GetArrayItem(cJSON *array,int item);
```
那么如上，我们基本知道，解析一个JSON的格式的几个常用函数，那么我们试一试
```c
char person[] = "{\"name\":\"iro\", \"age\":25, \"birth\": {\"year\":2015, \"month\":08,\"day\":20}}";
cJSON *root, *child, *pItem;
root = cJSON_Parse(person);
if (root) {
	pItem = cJSON_GetObjectItem(root, "name");
	if (pItem) printf("%s\n", pItem->valuestring);
	pItem = cJSON_GetObjectItem(root, "age");
	if (pItem) printf("%d\n", pItem->valueint);
	child = cJSON_GetObjectItem(root, "birth");
	if (child) {
		pItem = cJSON_GetObjectItem(child, "year");
		if (pItem) printf("%d\n", pItem->valueint);
	}

	cJSON_Delete(root);
}
```
输出
```text
iro
25
2015
```
```c
char color[] = "{\"colors\":[\"red\",\"green\",\"blue\",\"yellow\",\"white\"]}";
cJSON *root, *pArray, *pItem;
int i, count;
root = cJSON_Parse(color);
if (root) {
	pArray = cJSON_GetObjectItem(root, "colors");;
	if (pArray) {
		count = cJSON_GetArraySize(pArray);
		for (i = 0; i < count; i++) {
			pItem = cJSON_GetArrayItem(pArray, i);
			if (pItem) printf("%s\n", pItem->valuestring);
		}
	}
	cJSON_Delete(root);
}
```
输出
```text
red
green
blue
yellow
white
```
