# Try something in C

*一些值得尝试的代码*


>👉
>
>**Welcome to my C programming notes!**
>
>This is a collection of interesting C code snippets and concepts I've gathered. Feel free to explore, learn, and experiment with these examples. This page is being continuously updated, so be sure to check back often for new content!
>
>If you have any questions, suggestions, or want to discuss any of these topics further, please don't hesitate to make a comment or reach out to me. Your input is valuable and appreciated! 


## 响铃符，在外部终端运行时会发出声音🔔

```c
#include<stdio.h>
int main(){
    printf("\a");
    return 0;
}
```

## float 和 double 并非准确值

```c
#include<stdio.h>
int main(){
    for (float i = 0; i < 100; i+=1)
    {
        printf("%10.20f\n", i/100);
    }
    return 0;
}
```


>⚠️
>
>float 和 double 不能进行位运算和取模（或被取模）操作



## 字符串末尾都是空字符 ‘\0’

```c
#include<stdio.h>
int main(){
    char str[10]="world";
    char str2[10]="";
    for (int i = 0; i < 10; i++)
    {
        if(str[i]=='\0'){
            printf("\\0 ");
        }
        else{
            printf("%c ",str[i]);
        }
    }
    printf("\n");
    for (int i = 0; i < 10; i++)
    {
        if(str2[i]=='\0'){
            printf("\\0 ");
        }
        else{
            printf("%c ",str2[i]);
        }
    }

    return 0;
}
```

## 输出一个数的二进制（是补码）

```c
#include<stdio.h>
int main(){
    while(1){
        short a;
        scanf("%hd",&a);
        for (int i = 15; i >=0; i--)
        {
            printf("%d",a>>i&1);
            if(i==8)printf(" ");
        }
        printf("\n");

    }
    return 0;

}
```

## 开一个长度为*变量*的数组

```c
#include<stdio.h>
int main(){
    int n;
    scanf("%d",&n);
    int a[n];
    for (int i = 0; i < n; i++)
    {
        printf("%d ",a[i]);
    }
}
```

a[n]必须是**局部变量**，若在全局定义，会报错 `variably modified 'a' at file scope`

```c
#include<stdio.h>
int n=10; //在c中，即使改为const int 也会报错，改为cpp文件后正常
int a[n];

int main(){
    for (int i = 0; i < n; i++)
    {
        printf("%d ",a[i]);
    }
}
```

## 局部变量是未经初始化的，全局变量被初始化为零。

```c
#include<stdio.h>
int a[100];
int main(){
    for (int i = 0; i < 100; i++)
    {
        printf("%d ",a[i]);
    }
}
```

结果全为`0`

```c
#include<stdio.h>
int main(){
    int a[100];
    for (int i = 0; i < 100; i++)
    {
        printf("%d ",a[i]);
    }
}
```

结果为乱七八糟的数

## 用空白列表初始化局部数组

```c
#include <stdio.h>
int main(){
    int arr[5] = {}; // Initialize all elements to 0
    for(int i = 0; i < 5; i++){
        printf("%d ", arr[i]);
    }
    return 0;
}
```

使用空白列表 `{}` 可以将局部数组的所有元素初始化为0。这比手动初始化每个元素更方便。

## 枚举 Enum

### 不出现枚举名

```c
enum {SUN,MON,TUE,WED,THU,FRI,SAT}week1,week2; //定义了两个枚举变量
```

由于没有名称，无法引用，不能再定义更多这种类型的枚举变量

### 指定枚举数值时重复

```c
enum week{SUN,MON,TUE,WED=0,THU,FRI,SAT};
```

值分别为 `0 1 2 0 1 2 3 4`

### 当枚举的数值不连续时枚举变量的范围

```c
#include <stdio.h>

enum week{SUN,MON,TUE,WED,THU,FRI,SAT=10};

int main()
{
    enum week week1;
    week1=6;
    return 0;
}
```

week 的值不能为6 `error: invalid conversion from 'int' to 'week' [-fpermissive]`

## scanf 的格式控制

### 空白字符

以下的表达全都等价

```c
scanf("%s%s",&a,&b);
scanf("%s %s",&a,&b);
scanf("%s\n%s",&a,&b);
scanf("%s\t%s",&a,&b);
scanf("%s\n \t\n\t     %s",&a,&b);
```

空白字符: `\n` `\t` `' '` 的作用都相同，连续的空白字符将被视为一处。

scanf 读取数字，字符串时会自动跳过空白字符。


>⚠️
>
>当读取`%c`时，空白字符可用于跳过输入中的空白字符

e.g. 输入`a b`:

```c
scanf("%c%c",&a,&b); //a='a', b=' '
scanf("%c %c",&a,&b); //a='a', b='b'
```

与其他输入方式混用，e.g. input: `hello world`

```c
#include <stdio.h>

int main() {
    char a[100],b[100];
    scanf("%s",a);
    gets(b);
    printf("%s",b);
    return 0;
}
```

改为`scanf("%s ",a);`,b=”world”

### 普通字符

```c
#include <stdio.h>
char a[100],b[100];
int main()
{
	int success;
	success=scanf("%s and %s",a,b);
	printf("%s %s",a,b);
	return 0;
}
```

表达式加入了普通字符，必须输入 `abcde and fghij` 才能正确输入a和b

### 赋值禁止字符 *

```c
#include <stdio.h>
char a,b;
int main()
{
	scanf("%c %*s %c",&a,&b);
	printf("%c %c",a,b);

	return 0;
}
```

input `a bcdef g` output `a g`

### 指定最大域宽

```c
#include <stdio.h>
char a[100],b[100];
int main()
{
	scanf("%5s%5s",a,b);
	printf("%s %s",a,b);

	return 0;
}
```

input `abcdefghijklmnopqrstuvwxyz` output `abcde fghij`

## 带有空语句的for

```c
for ( a ; b ; c ) { ... } 
```

当 `b` 为空时，相当于填入了非空常量，即循环将一直进行（除非遇到转移语句）

`a` `c` 为空时，将不进行任何操作

## 伪随机数

***线性同余法：***

$$
a_0=seed, a_n=(A\times a_{n+1}+B)\%M.
$$

A,B,M`= RAND_MAX` 是产生器设定的常数。

设定种子：

```c
srand(time(NULL)); //用时间做种子，保证每次运行时不同
srand(a); //使用固定的种子，生成的序列是固定的
```

## 实参传递

### 实参求值顺序由编译器决定

```c
#include <stdio.h>

void printab(int a,int b){
    printf("%d %d",a,b);
}

int main()
{
    int a=0;
    printab(a,a++);
    return 0;
}
```

或者，

```c
#include <stdio.h>

int main()
{
    int a=0;
    printf("%d %d",a,a++);
    return 0;
}

```

## 内层变量与外层变量重名，外层的变量被屏蔽

```c
#include <stdio.h>

int main()
{
    for (int i = 0; i < 10; ++i) {
        for (int i = 0; i < 10; ++i) {
            printf("%d ",i);
        }
        printf("\n");
    }
    return 0;
}
```

### 在同一层重新定义一个重名变量会报错

```c
#include <stdio.h>

int main()
{
    int i=10;
    int i=20;
    return 0;
}
```

`error: redeclaration of 'int i'`

```c
**#include <stdio.h>

int main()
{
    int i=10;
    {
        int i=20;
        printf("%d\n",i);
    }
    printf("%d",i);
    return 0;
}**
```

用代码块就可定义内层同名变量。

## static 变量初始化只执行一次

```c
#include <stdio.h>

int main()
{
    for (int i = 0; i < 10; ++i)
    {
        static int a=0; //下次执行到这里，直接跳过这条语句
        printf("%d ",a);
        a++;
    }
    return 0;
}

```

## 宏

### 条件编译

重复定义宏时，编译预处理时从头到尾顺序操作，即每个位置a的值为上一次定义的值，但会收到一条警告

```c
#include <stdio.h>
#define a 1
int x = 1;
int main() {
		#if a
		    printf("a != %d\n", a);//执行
				#define a 10
		#else
		    printf("a == 0\n");
		#endif
		
		#if a == 10
		    printf("a == %d\n", a);//执行
		#else
		    printf("a != 10\n");
		#endif

    return 0;
}

```
### if 控制
if 可以接一些表达式，或是宏定义的“函数”，但是此时传入的实参必须在宏中定义

```c
#include <stdio.h>
#define ABS(x) (((x)>0)?(x):-(x))
#define a -1

int main()
{
    #if ABS(a)==1
    printf("abs if a is 1");
    #endif

    return 0;
}
```

若参数为在程序中定义的变量，将无法正确读取，被识别为0

```c
#include <stdio.h>
#define ABS(x) (((x)>0)?(x):-(x))
int a=-1;

int main()
{
    #if ABS(a)==1
    printf("abs if a is 1");
    #elif ABS(a)==0
    printf("abs if a is 0");  //executed
    #endif

    return 0;
}
```

### 预定义宏

```c
#include <stdio.h>

int main()
{
    printf("%s %s %d %s %s",__DATE__,__FILE__,__LINE__,__TIME__,__FUNCTION__);
    //将输出 当前日期，源文件路径和名称，行号，当前时间，函数名
    return 0;
}

```

## 参数数目可变的函数

```c
#include <stdio.h>
#include <stdarg.h>

int my_max(int n, ...){
	int ans=-2147483648;
	va_list a;
	va_start(a,n);
	for (int i = 0; i < n; i++)	{
		int tmp=va_arg(a,int);
		if(tmp >= ans)	ans=tmp;
	}
	va_end(a);
	return ans;
}

int main(){
	int a,b,c;
	scanf("%d%d%d",&a,&b,&c);
	printf("max of abc is: %d\n",my_max(3,a,b,c));
	return 0;
}
```

C++ 风格：

```cpp
#include <iostream>
#include <initializer_list>
using namespace std;

int my_max(initializer_list<int>arr){
    int ans=INT32_MIN;
    for (auto i = arr.begin(); i!=arr.end() ; i++){
        ans=max(ans,*i);
    }
    return ans;
}

int main()
{
    int a,b,c;
    cin>>a>>b>>c;    
    cout<<my_max({a,b,c});
    return 0;
}

```

## 两种定义字符串的方法

method 1:

```cpp
#include <stdio.h>

int main()
{
	char *s1="hello"; //字符串存储在常量区，地址赋给s1，s1是指针变量
	printf("%s ",s1);
	printf("%c ",*(s1+1));
	printf("%c ",s1[2]);
	s1="world"; //s1指向另一字符串常量
	printf("%s ",s1); 	
	return 0;
}
```

s1可修改，字符串不准更改 

method 2:

```cpp
#include <stdio.h>

int main()
{
	char s1[]="hello"; //字符串存储在变量区，s1为指针常量，指向字符串
	printf("%s ",s1);
	printf("%c ",*(s1+1));
	printf("%c ",s1[2]);
	s1[4]='0'; //更改字符串
	printf("%s ",s1);	
	return 0;
}
```

字符串可修改，s1不可修改

## 二维数组所占内存是连续的

```cpp
#include <stdio.h>

int main()
{
	int s[2][3]={{1,3,5},
				 {2,4,6}};
	for (int i = 0; i < 6; i++){
		printf("%d ",s[0][i]); //s[0]超出后进入s[1]
	}	
}
```

## 联合与字段

联合可以将同一段存储空间以多种类型的数据存取；字段可以将一段内存按位宽拆分。

💡可以利用联合与字段简便的实现01的枚举：

```cpp
#include <stdio.h>
union selection{
    struct {
        unsigned a: 1;
        unsigned b: 1;
        unsigned c: 1;
        unsigned d: 1;
    };
    unsigned D;
}e;

int main(){
    for (e.D=0; e.D<0xf; e.D++){
        printf("a:%d  b:%d  c:%d  d:%d\n",e.a,e.b,e.c,e.d);
    }
    return 0;
}
```

字段不能跨越一个字的边界

```cpp
#include <stdio.h>

union selection{
    struct {
        unsigned short a: 1;
        unsigned short b: 16; //存在下一个字中
    };
    unsigned long long D;
}e;

int main()
{
    e.a=1;
    e.b=1;
    printf("%x",e.D);
    return 0;
}
```

## exit() 退出程序

`exit();` 可直接退出程序，在主程序中，效果与return类似，而在调用的函数中，可以直接退出程序而非返回。

```c
#include <stdio.h>
#include <stdlib.h> //exit()函数需要的头文件

void work(){
    printf("message from work 1\n");
    exit(0);
    printf("message from work 2\n");
}

int main(){
    work();
    printf("message from main\n");
}
```

## 无类型指针 void *

void指针可以指向任意类型的数据，如果要将void指针p赋给其他类型的指针，则需要强制类型转换。

void指针不应用于计算，否则将收到警告

```c
#include <stdio.h>
int a[]={1,2,3,4,5,6,7,8,9,10};
int *p=a;
void *q=a; //无类型指针
int main(){
    printf("*p=%d *q=%d\n",*p,*(int *)q);
    return 0;
}
```

各指针所占的空间:

```c
#include <stdio.h>
int *p1;
char *p2;
double *p3;
void *p4; //char＊和void＊指向空间均为1byte
int a[10][10];
int main(){
    printf("%d %d %d %d %d\n",(p1+1)-p1,(p2+1)-p2,(p3+1)-p3,(a+1)-a,(a[0]+1)-a[0]);
    printf("%d %d %d %d %d %d",(int)((char*)(p1+1)-(char*)p1),(char*)(p2+1)-(char*)p2,(char*)(p3+1)-(char*)p3,(char*)(p4+1)-(char*)p4,(char*)(a+1)-(char*)a,(char*)(a[0]+1)-(char*)a[0]);
    return 0;
}
```

输出：

```
1 1 1 1 1
4 1 8 1 40 4
```