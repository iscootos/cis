#defined宏

`defined`关键字，一般是跟在`#if`后面的       
如下，它的意思是，如果已经定义过CIS宏，那么执行`#if`后面的语句，否则不执行                  
```cpp
#define CIS 0

#if defined(CIS)
#define XS "hello world"
#endif
```

`!defined`关键字        
如下，意思是，如果没有定义过CIS宏，那么执行`#if`后面的语句，否则不执行
```cpp
//#define CIS 0

#if !defined(CIS)
#define XS "hello world"
#endif
```

`#if`关键字       
如下，如果`#if`关键字后面的值，大于0，那么执行`#if`后面的语句，否则不执行       
```cpp
#define CIS 1

#if CIS
#define XS "hello world"
#endif
```

如下，如果`#if`关键字后面，有`||`条件表达式，那么只要有一个结果大于0，就执行`#if`后面的语句，如果都为假则不执行
```cpp
//#define CIS 1
#define CXS 1

#if CIS || CXS
#define XS "hello world"
#endif
```

如下，如果`#if`关键字后面，有`&&`条件表达式，那么所有结果都为真，执行`#if`后面的语句，否则不执行
```cpp
#define CIS 1
#define CXS 1

#if CIS && CXS
#define XS "hello world"
#endif
```

如下，只要有一个宏定义过，那么就执行`#if`后面的语句，如果都没有，就不执行
```cpp
#define CIS 1
#define CXS 1

#if defined(CIS) || defined(CXS)
#define XS "hello world"
#endif
```

如下，必须两个宏都没有定义过，那么就执行`#if`后面的语句，否则不执行
```cpp
//#define CIS 1
//#define CXS 1

#if !defined(CIS) && !defined(CXS)
#define XS "hello world"
#endif
```