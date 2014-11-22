#printf打印十六进制多ffffff的问题
我们在用C语言编程的时候，经常要用到
```c
printf("0x%x\n", "hello world");
```
但是有时候我们发现打印出来的字符多了6个f`ffffff`，这是因为有符号类型的char字符，超过127的时候，就会用`ffffff`来显示，所以切记使用`unsigned char`类型就可以了         
```c
char x = 128;
printf("0x%x\n", x);  //0xffffff80
```
```c
char x = 128;
printf("0x%x\n", (unsigned char)x); //0x80
```
