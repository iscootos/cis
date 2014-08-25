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
`cJSON_CreateObject()`函数， 返回一个cJSON结构的指针，用于创建一个新的cJSON结构，可以用于创建根对象，也可以用于创建子对象
```c
extern cJSON *cJSON_CreateObject(void);
```
`cJSON_AddItemToObject()`函数，把一个key-value键值对添加到JSON数据中，cJSON中，添加的所有的数据，最后都是调用该函数实现的
```c
extern void	cJSON_AddItemToObject(cJSON *object,const char *string,cJSON *item);
```
添加几种数据格式的宏，可以发现都是调用的`cJSON_AddItemToObject()`函数实现的
```c
#define cJSON_AddNullToObject(object,name)		cJSON_AddItemToObject(object, name, cJSON_CreateNull())
#define cJSON_AddTrueToObject(object,name)		cJSON_AddItemToObject(object, name, cJSON_CreateTrue())
#define cJSON_AddFalseToObject(object,name)		cJSON_AddItemToObject(object, name, cJSON_CreateFalse())
#define cJSON_AddBoolToObject(object,name,b)	cJSON_AddItemToObject(object, name, cJSON_CreateBool(b))
#define cJSON_AddNumberToObject(object,name,n)	cJSON_AddItemToObject(object, name, cJSON_CreateNumber(n))
#define cJSON_AddStringToObject(object,name,s)	cJSON_AddItemToObject(object, name, cJSON_CreateString(s))
```
同时，如上我们发现，所有的数据都是调用对应的函数实现了创建cJSON的value对象
```c
extern cJSON *cJSON_CreateNull(void);
extern cJSON *cJSON_CreateTrue(void);
extern cJSON *cJSON_CreateFalse(void);
extern cJSON *cJSON_CreateBool(int b);
extern cJSON *cJSON_CreateNumber(double num);
extern cJSON *cJSON_CreateString(const char *string);
extern cJSON *cJSON_CreateArray(void);
extern cJSON *cJSON_CreateObject(void);
```
下面，我们来写一个实例
```c
cJSON *root, *fmt;
char* out;

root = cJSON_CreateObject();
cJSON_AddItemToObject(root, "name", cJSON_CreateString("Jack"));
cJSON_AddItemToObject(root, "format", fmt = cJSON_CreateObject());
cJSON_AddStringToObject(fmt, "type", "rect");
cJSON_AddNumberToObject(fmt, "height", 1920);
cJSON_AddNumberToObject(fmt, "width", 1080);
cJSON_AddFalseToObject(fmt, "interlace");
out = cJSON_Print(root);
cJSON_Delete(root);
printf("%s\n", out);
free(out);
```
输出
```text
{
	"name":	"Jack",
	"format":	{
		"type":	"rect",
		"height":	1920,
		"width":	1080,
		"interlace":	false
	}
}
```
创建数组对象
```c
extern cJSON *cJSON_CreateIntArray(const int *numbers,int count);
extern cJSON *cJSON_CreateFloatArray(const float *numbers,int count);
extern cJSON *cJSON_CreateDoubleArray(const double *numbers,int count);
extern cJSON *cJSON_CreateStringArray(const char **strings,int count);
```
下面，我们来实现一下
```c
cJSON *root;
char* out;

const char *strings[] = {"Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"};
root = cJSON_CreateStringArray(strings, 7);
out = cJSON_Print(root);
cJSON_Delete(root);
printf("%s\n", out);
free(out);
```
```text
["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
```
如果是二维数组
```c
cJSON *root;
char* out;

int numbers[3][3] = {{0,-1,0},{1,0,0},{0,0,1}};
root = cJSON_CreateArray();
int i;
for (i = 0; i < 3; i++)
	cJSON_AddItemToArray(root, cJSON_CreateIntArray(numbers[i], 3));
out = cJSON_Print(root);
cJSON_Delete(root);
printf("%s\n", out);
free(out);
```
```text
[[0, -1, 0], [1, 0, 0], [0, 0, 1]]
```
下面我们用更复杂一些的方法使用数据结构把多个结构添加到数组
```c
struct record {
	char* precision;
	double lat;
	double lon;
	char* address;
	char* city;
	char* state;
	char* zip;
	char* country;
};
```
```c
cJSON *root, *thm, *fld;
char* out;

struct record fields[2]={
		{"zip",37.7668,-1.223959e+2,"","SAN FRANCISCO","CA","94107","US"},
		{"zip",37.371991,-1.22026e+2,"","SUNNYVALE","CA","94085","US"}};

root=cJSON_CreateArray();
int i;
for (i=0;i<2;i++)
{
	cJSON_AddItemToArray(root,fld=cJSON_CreateObject());
	cJSON_AddStringToObject(fld, "precision", fields[i].precision);
	cJSON_AddNumberToObject(fld, "Latitude", fields[i].lat);
	cJSON_AddNumberToObject(fld, "Longitude", fields[i].lon);
	cJSON_AddStringToObject(fld, "Address", fields[i].address);
	cJSON_AddStringToObject(fld, "City", fields[i].city);
	cJSON_AddStringToObject(fld, "State", fields[i].state);
	cJSON_AddStringToObject(fld, "Zip", fields[i].zip);
	cJSON_AddStringToObject(fld, "Country", fields[i].country);
}

out = cJSON_Print(root);
cJSON_Delete(root);
printf("%s\n", out);
free(out);
```
输出
```text
[{
		"precision":	"zip",
		"Latitude":	37.766800,
		"Longitude":	-122.395900,
		"Address":	"",
		"City":	"SAN FRANCISCO",
		"State":	"CA",
		"Zip":	"94107",
		"Country":	"US"
	}, {
		"precision":	"zip",
		"Latitude":	37.371991,
		"Longitude":	-122.026000,
		"Address":	"",
		"City":	"SUNNYVALE",
		"State":	"CA",
		"Zip":	"94085",
		"Country":	"US"
	}]
```