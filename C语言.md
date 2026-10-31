# C语言概述

## C语言发展

### 三个发展阶段

程序设计语言的发展经历了三个阶段，C语言属于高级语言

**机器语言**

计算机可以直接识别的"0"，"1"组成的指令码，通过读取设备督导计算机内部。

**汇编语言**

使用助记符代替二进制0，1，例如：add表示加法，sub表示减法，mov表示移动。

计算机不能直接识别，需要转换为机器语言。

**高级语言**

更接近人类自然语言，例如：a=10+20。

计算机不能直接识别，需要转换为机器语言。

![](C语言.assets/2026-05-19-17-04-20-image.png)

### C语言的发展历程

K&R C语言标准，原始的C语言标准，C语言的第一个标准，目前已经不提了。

ANSI C标准，也叫C89标准或者C90标准，是目前和我们之后学习的标准。

C99标准是第一个扩展标准，目前各个主流编译器对C99的支持不一致，所以尽量避免使用C99的专属语法。

C11扩展标准，目前没有完整的支持。

### C语言的特点

C语言是编译型语言。

语言简洁紧凑，语法结构明确，拥有丰富的运算符和数据结构，程序执行效率高，可移植性好。

### C语言IDE

    IDE一般至少包含编辑器、编译器和调试器，一般也会在编译后直接运行代码。

        编辑器:写代码用的工具，C/C++领域单独使用的编辑器中，最常用的是Visual Studio Code (VSCode)、sublime

        编译器:将代码生成可执行文件的应用程序，例如:MSVC(Microsoft Visual C++ Compiler)，gcc，clang

        调试器:调试运行代码的工具，帮助开发者定位问题，例如gdb。

    Visual Studio(VS)

        Visual Studio(简称Vs)是微软公司开发的开发工具包系列产品，它是一个功能强大的IDE，支持多种编程语言，包括C语言。

        Visual Studio是Windows下的标准IDE，广泛应用于实际开发中。

        工具网站:https://learn.microsoft.com/zh-cn/cpp/?view=msvc-170

    DEV-C++

        Dev-C++是一款免费开源的C/C++ IDE,内嵌GCC编译器(LinuxGCC编译器的Windows移植版)。

        它是NOI(全国青少年信息学奥林匹克竞赛)和NQIP(全国青少年信息学奥林匹克联赛)等比赛的指定工具。

## 语法基础

    1.注释

        注释是说明性的文字，用来说明程序的功能，便于他人读懂程序，不执行。

    2.关键字

        由程序本身使用的,有专的意义和用途。例: $int,char, float, double,if,else, switch,case,for$$while,break,continue,return,const$等。

    3.标识符

        标识符就是名字：函数名，变量名，类名，对象名等。

        标识符命名要求：

            不能使用关键字作为标识符。

            大小写敏感，区分大小写。

            只能由字母、数字、下划线组成，且不能以数字开头。尽量不要使用下划线开头。

        变量命名常用方法

            下划线法：对于C语言来说比较常见，多个单词之间用下划线隔开，变量全小写，常量全大写。例如变量light_speed，常量LIGHT_SPEED
            驼峰式：多个单词以第二个开始的单词首字母大写的方式区分，变量首字母小写，常量首字母大写，例如变量lightspeed，常量LightSpeed

        变量

            变量是命名的其值可以改变的量，变量作用是保存数据。

            一个变量有三个要素：类型、变量名和值。

            定义变量的注意事项变量：

                使用前必须定义，确定变量名、类型及初值；

                变量类型可以是C支持的所有类型，包括基本类型和自定义类型；

                同一个作用域不能定义相同名称的变量。

                变量定义时系统会根据类型分配相应大小的内存空间。

                相同类型的多个变量可以在一个语句中定义，变量名用逗号分隔。

        常量

            在程序运行过程中，其值不能改变的量称为常量。C语言中常量分为直接常量和符号常量(命名常量)。

            直接常量：直接使用数字或文字表示的值。

            符号常量：使用标识符表示数值或文字，符号常量只能在定义时指定其值，一旦初始化后不能用复制语句修改。符号常量有两种定义方式:

                    使用宏定义：<u>#define</u> 宏名 值

                    使用const定义：const 类型 常量名=常量值。

### 输入和输出

输入长字符串，除了换行全部读取`scanf("%[^\n]%*c",str);`

输出使用`printf("格式控制字符串", ...);`

第一个参数"格式控制字符串"是一个常量字符串，它包含两类内容：

    普通字符：会被原样输出到屏幕上。

    格式说明符：以%开头，用于指示后续参数的输出格式（如%d表示整数）。

第二个参数：...（可变参数）：这是要输出的具体数据（变量、常量或表达式）。参数的数量和类型必须与format字符串中的格式说明符一一对应。

三类类型说明符：

| 分类    | 类型说明符 | 含义          |
| ----- | ----- | ----------- |
| 进制为基准 | %u    | 十进制，无符号     |
| 进制为基准 | %o    | 八进制，无符号     |
| 进制为基准 | %x、%X | 十六进制，无符号    |
| 进制为基准 | %d    | 有符号的十进制，有符号 |
| 类型为基准 | %i、%d | 整型          |
| 类型为基准 | %f    | 浮点型         |
| 类型为基准 | %c    | 字符型         |
| 类型为基准 | %s    | 字符串         |
| 类型为基准 | %p    | 指针          |
| 科学计数法 | %g    | 科学计数法显示     |

输入使用`scanf("格式控制字符串", 地址列表);`

    格式控制字符串: 由普通字符和格式说明符组成。格式说明符以 % 开头，用于指示输入数据的类型。

    格式控制字符串里尽量不要包含类型说明符和空白字符以外的字符。

**空白字符**：空格、制表符\t、换行符\n等。

**地址列表**：由一个或多个变量的地址组成，用逗号分隔。这些地址指向用于存储输入数据的内存位置。对于基本数据类型（如 int, float, char），需要使用取地址运算符 & 来获取变量的地址。

### 基本数据类型

**整数型**

有符号（一个字节是8位）

| 类型        | 长度   | 取值范围                                                   |
| --------- | ---- | ------------------------------------------------------ |
| short     | 16 位 | -32768 ~ 32767（−215∼215−1）                             |
| int       | 32 位 | -2147483648 ~ 2147483647（−231∼231−1）                   |
| long      | 32 位 | -2147483648 ~ 2147483647（−231∼231−1）                   |
| long long | 64 位 | -9223372036854775808 ~ 9223372036854775807（−263∼263−1） |

无符号

| 类型                   | 长度   | 取值范围                                |
| -------------------- | ---- | ----------------------------------- |
| `unsigned short`     | 16 位 | 0 ~ 65535（0 ~ 2¹⁶-1）                |
| `unsigned`           | 32 位 | 0 ~ 4294967295（0 ~ 2³²-1）           |
| `unsigned long`      | 32 位 | 0 ~ 4294967295（0 ~ 2³²-1）           |
| `unsigned long long` | 64 位 | 0 ~ 18446744073709551615（0 ~ 2⁶⁴-1） |

`long long` 是 C/C++ 中的**64 位长整型**，输出时不能用%d，必须用专用格式符：

| 类型                   | 格式符    |
| -------------------- | ------ |
| `long long`          | `%lld` |
| `unsigned long long` | `%llu` |

**整型变量三种初始化方式**

C语言整型变量三种初始化方式以 `int` 整型为例：

1. 定义时直接赋值（最常用）

```
int a = 10;
```

2. 先定义，后赋值

```
int b;
b = 20;
```

3. 多变量一起初始化

```
int c = 30, d = 40;
```

**浮点型**

float，4字节

double，8字节

**字符型**

char字符型，1字节，用于存放一个字符。

char取值范围：-128~127，unsigned char取范围:0~255

ASClI编码：

数字 0～9：48~57

大写字母 A～Z：65~90

小写字母 a～z：97~122

转义字符：

        英文包括各种符号在及全集存储时，对符号进行编码，AScI码时英文符号和数字编号的通用规则。

        有反斜杠开始的字符称为转义字，如:'\0'，'\n'，'\t'，'\\\'

| 转义字符   | 意义                      | ASCII 码（十进制） |
| ------ | ----------------------- | ------------ |
| `\a`   | 响铃 (BEL)                | 007          |
| `\b`   | 退格 (BS)，将当前位置移到前一列      | 008          |
| `\f`   | 换页 (FF)，将当前位置移到下页开头     | 012          |
| `\n`   | 换行 (LF)，将当前位置移到下一行开头    | 010          |
| `\r`   | 回车 (CR)，将当前位置移到本行开头     | 013          |
| `\t`   | 水平制表 (HT)（跳到下一个 TAB 位置） | 009          |
| `\v`   | 垂直制表 (VT)               | 011          |
| `\\`   | 代表一个反斜线字符'`\`'          | 092          |
| `\'`   | 代表一个单引号（撇号）字符           | 039          |
| `\"`   | 代表一个双引号字符               | 034          |
| `\?`   | 代表一个问号                  | 063          |
| `\0`   | 空字符 (NUL)               | 000          |
| `\ddd` | 1 到 3 位八进制数所代表的任意字符     | -            |
| `\xhh` | 十六进制所代表的任意字符            | -            |

### 运算符和表达式

**算数运算符**

基本运算符：加+，减-，乘*，除/，余%

自增运算符：

++a，先算后用        a++：先用后算（--同理）

```
void text06()
{
    int i = 0, j = 0, m = 0;
    j = ++i;  //先算后用
    printf("%d\t%d\t\n", i, j);

    m = i++;  //先用后算
    printf("%d\t%d\t\n", i, m);
}
```

赋值运算符

    给变量赋值： =

复合运算符

```
void text08()
{
    int a = 1,i;
    for (i = 0; i < 99999; i++)
    {
        a += i;  //等同于a = a + i;
    }
    printf("a：%d\n", a);
}
```

**关系运算符**

<,<=,==,!=,>,>=

关系运算符的结果为0或者为1。

**逻辑运算符（false = 0, ture = 1）**

| 逻辑量 a | 逻辑量 b | !a    | a&&b  | a\|b  |
| ----- | ----- | ----- | ----- | ----- |
| true  | true  | false | true  | true  |
| true  | False | false | false | true  |
| false | True  | true  | false | true  |
| false | false | true  | false | false |

!逻辑非，||逻辑或，&&逻辑与

逻辑短路

    短路求值是指&&和都是先求左侧表达式的值，当左侧表达式结果无法确定逻辑运算结果时，才计算右侧表达式的值。

    A&&B，当A的结果为false时，不计算B;

    A||B，当A的结果为true时，不计算B。

```
void test04()
{
    int m = 90, n = 80;
    printf("%d\n", (m < n) ? (n - m) : (m + n));

    printf("(m<n)&&(m-n)<(m+n)：%d\n", (m < n) && (m - n) < (m + n));
    // m<n结果为0，不计算表达式(m - n) < (++m)，a的结果为90
    printf("(m<n)&&(m-n)<(++m)：%d\n", (m < n) && (m - n) < (++m));
    printf("%d\n", m);
    // m>n结果为1，不计算表达式(m - n) < (++m)，a的结果为91
    printf("(m>n)&&(m-n)<(++m)：%d\n", (m > n) && (m - n) < (++m));
    printf("%d\n", m);
}
```

**条件运算符**

格式：表达式1?表达式2:表达式3

当表达式1的结果为1，取表达式2的值，否则取表达式3的值。

```
#include<stdio.h>

void test01()
{
        int a = 1,b = 2;
        printf("%d\n",(a<b)?(a-b):(a+b));
}

int main()
{
        test01();   //(a<b)为真，输出结果为-1
        return 0;
}
```

**逗号运算符**

    逗号即是标点符号，也是运算符，其优先级最低，

    一个逗号运算符可将两个表达式连接起来，多个逗号可连接多个表达式。

```
/*逗号运算符
    逗号表达式有最低优先级
    逗号运算符，从左到右计算各个表达式，取最右边表达式的值作为整个表达式的结果。
*/
void test03()
{
    int a = 10, b = 20, c = 30;
    a = (b, c);   //先计算a = b，结果为a == 20, 再返回c。
    printf("a：%d\n", a);
}
```

**运算符优先级**

单目运算符，如：自增，自减等

双目运算符，如：加，减等

三目运算符，如：条件运算符

### 控制结构

任何复杂程序都是由三种基本机构组成：顺序结构，分支结构，循环结构。

单分支：

```
#include <stdio.h>
int main() {
 int score = 85;
 // 单分支：满足条件才执行
 if (score >= 60) {
 printf("成绩合格\n");
 }
 return 0;
}
```

双分支：

```
#include <stdio.h>

int main() {
    int score = 58;

    // 双分支：if + else
    if (score >= 60) {
        printf("成绩合格\n");
    } else {
        printf("成绩不合格\n");
    }
    return 0;
}
```

多分支：

if语句用法：

```
void test05()
{
    int age;
    printf("请输入年龄：");
    scanf("%d", &age);
    if (age < 18)
    {
        printf("写作业\n");
    }
    else if (age < 30)
    {
        printf("拧螺丝\n");
    }
    else if (age < 40)
    {
        printf("上班，带孩子\n");
    }
    else if (age < 65)
    {
        printf("摸鱼\n");
    }
    else   //以上条件都不满足
    {
        printf("养老\n");
    }
}
```

switch语法：

switch后面的表达式，只能是整型，字符型或枚举类型，每个case后面的“常量”值必须不同。

case通常要与break配合，否则执行case后面的语句后，就会一直执行下去。

```
switch(表达式){
case 常量1:
    语句组1; break;
case 常量2:
    语句组2; break;
case 常量n:
    语句组n; break;
default:
    语句组n+l; break;
}
```

if与switch多分支的区别

|     | if 分支         | switch 分支                 |
| --- | ------------- | ------------------------- |
| 相同点 | 都可以实现多分支      | 都可以实现多分支                  |
| 不同点 | 判断条件可以是一个取值范围 | case 后的值必须是整数或字符          |
| 不同点 | if 分支更通用      | 满足 switch 条件且 3 个分支以上建议使用 |

**循环结构**

for语句，while语句与do while语句

//  输出1到50之间能被3整除的数

```
void test03()
{
    for (int i = 1; i < 51; i++)
    {
        if (i % 3 == 0)
        {
            printf("%d ", i);
        }
    }
    printf("\n");
}
```

//  while语句求一个整数n的阶乘

```
void test07()
{
    int n, i = 1, jie = 1;
    printf("输入一个整数：");
    scanf("%d", &n);
    while (i < n+1)
    {
        jie *=  i;
        i++;
    }
    printf("%d的阶乘为%d\n", n, jie);
}
```

//  do while语句求1到100的和

```
void test08()
{
    int m = 1, sum = 0;
    do {
        sum += m;
        m++;    
        printf("%d\t", m);
    } while (m<=100);
    printf("\n");
    printf("sum=%d\n", sum);
}
```

for语句比较适合于循环次数固定的场景。

while语句与do while语句比较合适循环次数不固定，且循环结束条件比较明确的场景。

do while语句与while语句的区别是：while先判断循环条件，再执行循环体语句，而do while先执行循环体一句，再判断循环条件。

**跳转语句**

continue语句：结束本轮循环，进入下一轮循环。 

break语句：结束整个循环，转向循环语句下面的语句继续执行。

```
void test09()
{
    //  break结束整个循环
    for (int i = 1; i <= 10; i++)
    {
        if (i % 3 == 0)
        {
            break;
        }
        printf("%d ", i);
    }
    printf("\n");
    //  break结束本轮循环
    for (int i = 1; i <= 10; i++)
    {
        if (i % 3 == 0)
        {
            continue;
        }
        printf("%d ", i);
    }
}
```

### 数组

单个变量只能存一个数据值，而程序往往需要处理一组相同类型的数据，这时就需要使用数组。

数组是由相同类型的一组变量组成的一个有序集合，数组中每个变量称为一个元素，所有元素共用一个变量名，就是数组的名字。

数组中每个元素有一个序号称为下标(表示元素在数组中的位置)。数组下标从0开始。

数组元素在内存中是顺序存放的。

二维数组:

二维数组有两个下标，第一个下标通常称为行下标，第二个下标通常称为列下标。

二维数组在内存中的排列顺序是按行存放，即在内存中先顺序放第一行元素，再存放第二行元素。

//  顺序初始化和分行初始化

```
void test15() 
{
    printf("分行初始化部分元素：\n");
    int a[3][4] = { {1,2,3},{2,5,9},{5,15,64,24} };

for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 4; j++)
    {
        printf("%d\t", a[i][j]);
    }
    printf("\n");
}
printf("顺序初始化：\n");
int b[3][4] = { 1,2,3,5,2,5,9,11,5,15,64,24 };
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 4; j++)
    {
        printf("%d\t", b[i][j]);
    }
    printf("\n");
}
}
```

//  二维数组相加

```
void test16()
{
    int a[2][3] = { 1,2,3,4,5,6 }, b[2][3] = { 7,8,9,10,11,12 };
    printf("相加之后结果为：\n");
    for (int i = 0; i < 2; i++)
    {
        for (int j = 0; j < 3; j++)
        {
            printf("%d\t", a[i][j] + b[i][j]);
        }
        printf("\n");
    }
}
```

//  二维数组找最大值

```
void test17()
{
    int a[4][4] = {
        {10,2,5,32},
        {15,23,9,8},
        {15,2,1,5},
        { 4,5,8,22 }
    };
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            printf("%d\t", a[i][j]);
        }
        printf("\n");
    }
    int max = a[0][0];
    for (int i = 0; i < 4; i++)
    {
        for (int j = 0; j < 4; j++)
        {
            if (a[i][j] > max)
            {
                max = a[i][j];
            }
        }
    }
    printf("max：%d\n", max);
}
```

**字符串**

字符串本质上是以'\0'结尾的字符数组。空字符“\0’也被称为字符串结束标志。

```
//  字符串的定义与初始化
void test01()
{
    //  字符数组初始化
    char c[10] = { 'c','p','r','g','r','a','m' };
    for (int i = 0; i < 10; i++)
    {
        printf("%c ", c[i]);
    }
    printf("\n%s\n", c);

    //  字符串是以'\0'结束的字符数组
    char c1[10] = { 'c','p','r','g','\0','a','m' };
    printf("%s\n", c1);

    //  省略长度，按照赋值计算长度。
    char c2[] = "C Program";
    printf("%s\n", c2);
    printf("sizeof(c2)：%zd\n", sizeof(c2));

    //  指定长度时，数组长度应该大于字符串长度
    //  使用字符串"C Program”初始化时：
    //  c3的最小长度应该是9 + 1 = 10.虽然9也可以编译通过，但是%s输出时会出现乱码，因为没有字符串结束标志’\0’
    char c3[10] = "C Program";
    printf("%s\n", c3);
    printf("sizeof(c3)：%zd\n", sizeof(c3));

    //  计算字符串的长度
    char c4[20] = "Hello World!";
    int len = 0;
    while (c4[len] != '\0') len++;
    printf("len：%d\n", len);

    //  使用strlen计算字符串的长度
    printf("strlen(c4)：%d\n", strlen(c4));
}
```

字符串处理函数 

strlen获取字符串长度 

strcpy拷贝字符串 

strcat拼接字符串 

strcmp比较字符串 

toupper(字符)转大写，tolower(字符)转小写，满足则返回正数，反之返回负数

isupper，islower判断是否为大小写，满足则返回正数，反之返回负数。

```
//  字符串处理函数
void test02()
{
    //  strlen获取字符串长度
    char s1[20] = "C++";
    printf("strlen(s1)：%d\n", strlen(s1));

    //  字符串拷贝strcpy(目标，源)，确保目标长度足够大(大于源字符串的长度)
    char s2[] = "abcde";
    strcpy(s1, s2);
    printf("s1：%s\n", s1);

    char s3[20] = "Hello ";
    //  拼接字符串strcat(目标，源)，目标长度大于源字符串的长度
    strcat(s3, s2);
    printf("s3拼接后：%s\n", s3);

    //  相等返回0，s1>s2则返回正数，反之返回负数。
    char s4[20] = "Hello c";
    char s5[20] = "Hello ";
    strcmp(s4, s5);
    printf("strcmp(s4, s5)：%d\n", strcmp(s4, s5));

    //  转大小写
    char s6[20] = "Ccc Progaram";
    char c = toupper('s');
    printf("%c\n", c);
    for (int i = 0; i < strlen(s6); i++)
    {
        s6[i] = toupper(s6[i]);
    }
    printf("%s\n", s6);

    //  isupper，islower判断是否为大小写
    printf("%d\n", isupper('A'));
    printf("%d\n", islower('b'));
}
```

### 指针

内存地址

计算即的内存储器被划分成一个个的存储单元，这些存储单元按一定的规则编号，这个编号是存储单元的地址，也就是内存地址。

地址编码的最基本单元是字节，每个字节由8位二进制组成。基本内存单元的大小就是1字节，每个单元都有唯一的地址，这个存储单元可以存放指定的数据。

变量的地址

系统分配给变量的内存空间的起始单元地址称为该变量的地址。

指针运算符

解引用运算符：*，取地址运算符：&。

指针可以进行加减运算，指针进行加减运算的结果于指针的类型有关。

指针可以进行关系运算:>,>=,!=,==,<=,<

```
void test04()
{
    int* p1, a = 10;
    float* p2, b = 20.0;
    //  &取地址运算符，获取变量的地址。
    printf("&a：%p &b：%p\n", &a, &b);
    p1 = &a;  //  p1指向a
    p2 = &b;  //  p2指向b
    printf("p1：%p p2：%p\n", p1, p2);
    //  *解引用，访问指针变量指向内存地址的值
    printf("p1：%d p2：%f\n", *p1, *p2);

    *p1 = 1000;
    printf("a：%d b：%f\n", a, b);
    printf("&a：%p &b：%p\n", &a, &b);

    p1 = p1 + 1;
    printf("p1：%p p2：%p\n", p1, p2);
}
```

指针的关系运算符比较的是指针存储的代表地址的16进制数字的大小。

```
void test06()
{
    //  指针的关系运算符比较的是指针存储的代表地址的16进制数字的大小。
    int* p1, * p2, * p3, a, b;
    //  等、不等运算符常用于判断两个指针是否指向相同的变量或指针是否为空指针。
    p1 = &a;
    p2 = &b;
    p3 = &a;
    printf("p1 == p2：%d\n", p1 == p2);
    printf("p1 == p3：%d\n", p1 == p3);

    p1 = NULL;
    if (p1 == NULL)
    {
        printf("p1是空指针\n");
    }

    int a1[10];
    p1 = &a1[0];
    p2 = &a1[9];
    printf("p1<p2：%d\n", p1 < p2);
}
```

多级指针

![](C语言.assets/2026-05-19-17-05-05-0abd9ad5728e6c929d04b5b1e6bcec75.png)

指针数组

```
void test09()
{
    int a = 10, b = 20, c = 30, sum = 0;
    //  元素为指针的数组
    int* p[3] = { &a, &b, &c };
    for (int i = 0; i < 3; i++)
    {
        sum += *p[i];
    }
    printf("sum：%d\n", sum);
}
```

```
void test10()
{
    int a[5] = { 1,2,3,4,5 };
    int* p = a;
    printf("*(p + 1)：%d, a[1]：%d\n", *(p + 1), a[1]);

    p = &a[4];
    //  p[-1]等价于* (p + (-1))，取倒数第二个元素。
    printf("p[-1]：%d\n", p[-1]);
    printf("*(p+(-1))：%d\n", *(p + (-1)));
    p = &a[1];
    //  p[n] 表示指针 + n再取地址，相当于*((a+1)+1))==*(a+2)==a[2]结果为3
    printf("p[1]：%d\n", p[1]);
    printf("*(p+1)：%d\n", *(p + 1));
    printf("*((a + 1) + 1)：%d  a[2]：%d\n", *((a + 1) + 1), a[2]);
}
```

常量指针

常量指针指向的内存地址的内容不可修改。

指针常量

指向的内存中的值可以改变;但是指向的内存地址不能改变。

```
 //  常量指针
void test01()
{
    int a = 10, b = 10;
    const int C = 1000;
    const int* p1 = &a;
    //  常量指针指向的内存地址的内容不可修改。
    printf("*p1：%d\n", *p1);
    //  *p1 = 100;
    printf("*p1：%d\n", *p1);

    p1 = &b;
    const int *p2 = &C;
    //  *p2 = 1;
    printf("*C：%d\n", C);
}
//  指针常量
void test02()
{
    //  指针常量要在定义时初始化
    int a = 10;
    //  指针常量定义后，指向不能改变
    int* const p2 = &a;
    //  指针常量指向的内存地址的内容可以修改
    *p2 = 1000;
    printf("*p2：%d\ta：%d\n", *p2, a);
}
```

### 动态内存分配

**c语言内存区**

栈区

    由编译器自动分配释放，存放为运行程序而分配的局部变量、函数参数、返回值。程序结束后由系统释放。

堆区

    由程序员分配释放，若程序员不释放则会产会生内存泄漏。

全局区

    存放全局变量，静态数据，常量，程序结束后由系统释放。

文字常量区

    常量字符串就放在这里，程序结束后由系统释放。

程序代码区

    存放函数体的二进制代码，由操作系统管理。

**C++内存分配方式有三种：**

从栈上分配，系统自动分配。

从全局区分配，系统自动分配。

从堆上分配，称为动态分配，需要手动管理。

### 管理内存

malloc分配、free释放内存空间

void* malloc(size_t size)函数分配内存空间。

    malloc分配的内存空间是连续的，malloc不初始化分配的内存，申请的内存中包含随机的“垃圾值”，使用前必须手动赋值。

    返回值：成功：返回指向分配内存首地址的void*指针；失败：如果内存不足，返回NULL。

    参数：size是要申请的内存总字节数(通常写作元素个数*sizeof(类型))。

void free(void*ptr);，将ptr指向的堆内存归还给系统。

### 函数定义与使用

函数定义由函数类型、函数名称和参数组成，称为函数头。由大括号括起来的语句为函数体。

**函数的参数和返回值**

函数可以有返回值也可以没有返回值。

函数类型就是函数返回值的类型，可以是C语言支持的任意类型。

函数返回值由函数体中的return语句给出。

函数不需要返回值时，函数类型要用void指定类型。

函数参数:

    形参(形式参数)：定义函数时括号中的参数

    实参(实际参数)：调用函数时括号中的参数

    函数调用时，系统为形参分配内存单元并将实参的值传递到形参的单元中。

    根据函数功能的需要，函数参数可以选择数值或指针。

        数值做参数时，实参与形参在内存中占用不同的存储单元。此时函数不能修改实参的值。

​    指针作为函数参数时，传递的是变量的地址。此时函数可以通过地址修改实参的值。

```
//  数值作为函数参数
void my_swap1(int x,int y)
{
    //  函数调用时，系统为形参分配内存单元并将实参的值传递到形参的单元中。
    int tmp = x;
    x = y;
    y = tmp;                                
    printf("x：%d\ty：%d\n", x, y);
}
//  指针作为函数参数
void my_swap2(int* p1, int *p2)
{
    int tmp = *p1;
    *p1 = *p2;
    *p2 = tmp;
    printf("x：%d\ty：%d\n", *p1, *p2);
}
void test06()
{
    int m = 1, n = 2;
    printf("原始m：%d\tn：%d\n", m, n);
    my_swap1(m, n);
    printf("m：%d\tn：%d\n", m, n);  //  实参的值没有修改
    my_swap2(&m, &n);
    printf("m：%d\tn：%d\n", m, n);  //  实参的值被修改了
}
```

    数组作为函数的参数和返回值

        数组名作为参数实际上传递的时数组的首地址。

        c语言中函数一般不能返回数组，但是可以通过指针实现返回一个连续内存空间。

        字符数组(指针)作为函数的参数可以将字符串从一个函数传递到另外一个函数。

```
//  数组作为参数和返回值
char* test()
{
    char* p = (char*)malloc(sizeof(char) * 10);
    //char a1[] = "Hello!";
    strcpy(p, "Hello!");
    printf("p1：%s\n", p);
    return p;
}
void test07()
{
    int a[5] = { 0 };
    char* p1 = test();
    printf("p1：%s\n", p1);
    free(p1);
}
```

### 变量的生命周期

每个标识符都有确定的说明位置，决定了它将在什么范围内有效（能被访问）。变量的声明周期也称变量的作用域，即变量的有效范围。

**变量的作用范围**

​    局部变量

​    函数内部定义的变量是内部变量，也称局部变量。它只在该函数范围内有效。

​    全局变量

​    函数外部定义的变量称为外部变量，也称全局变脸。在程序运行过程中都有效。

​    局部变量和全局变量

​    在一段程序中，如果出现同名的局部变量和全局变量，优先访问局部变量。

```
#include<stdio.h>
//  变量的作用范围
//  定义全局变量
int i = 100;

void test()
{
    //  访问全局变量
    printf("i：%d\n", i);
    i += 1;
}

int main()
{
    //  访问全局变量
    printf("i：%d\n", i);
    test();
    printf("i：%d\n", i);  
    int i = 10;  //  定义局部变量，如果局部变量和全局变量重名，局部变量覆盖全局变量。
    printf("i：%d\n", i); //  局部变量i覆盖全局变量i，没有特殊情况，尽量避免使用同名局部变量覆盖全局变量。
    test();  //  test函数中没有覆盖全局变量，使用全局变量。输出101
    return 0;
}
```

**变量的存储类别**

静态存储方式

​    静态存储变量是指在程序运行期间，分配固定的存储空间并且一直存在。

动态存储方式

​    动态存储变量是在程序执行过程中根据需要动态分配存储空间。

static关键字

​    static关键字可以把变量定义为静态存储方式。

### 内联函数

内联函数使用关键字inline定义，内联不是在调用时发生，而是在编译时将被调函数嵌入到主调函数中。

一般只有简单的函数才能成为内联函数，如果函数比较复杂，如包含循环、分支结构，即使定义为内联函数，编译器也会将其当成普通函数处理。

```
#include<stdio.h>

inline int add(int a, int b);  //  定义内联函数，函数声明和定义分开时，inline写在函数声明前。

int main()
{
    printf("add(1, 2)：%d\n", add(1, 2));   //  优化后，内联函数被嵌入主调函数中。
    return 0;
}

int add(int a, int b)
{
    return a + b;
}
```

### 函数指针

函数所占用内存的起始地址就是函数的地址，用于保存函数地址的指针变量称为函数指针变量，简称为函数指针。

函数指针可以用于调用函数或将函数作为参数传递。

```
#include<stdio.h>

int my_add(int x, int y)
{
    return x + y;
}

int my_minus(int x, int y)
{
    return x - y;
}

int test(int x, int y, int (*fp)(int, int))
{
    x += 20;
    y += 20;
    return fp(x, y);
}

int main()
{
    printf("my_add：%p\n", my_add);
    printf("my_minus：%p\n", my_minus);

    //  定义函数指针，用于指向具有两个int型参数，并返回int型参数的函数。
    int (*fp)(int, int);
    //  函数指针fp指向函数my_add
    fp = my_add;
    //  使用函数指针调用函数
    printf("fp(20, 10)：%d\n", fp(20, 10));

    printf("test(20, 10, my_add)：%d\n", test(20, 10, my_add));
    printf("test(20, 10, my_minus)：%d\n", test(20, 10, my_minus));
    return 0;
}
```

### 嵌套调用

示例：求多个阶乘的和

```
#include<stdio.h>

//  嵌套调用
//  求多个阶乘的和
int f1(int n);

int f2(int n);

int main()
{
    int n;
    printf("请输入数字：");
    scanf("%d", &n);
    printf("f1(%d)：%d\n", n, f1(n));
    return 0;
}

int f1(int n)
{
    int sum = 0;
    for (int i = 1; i <= n; i++)
    {
        sum += f2(i);
    }
    return sum;
}
int f2(int n)
{
    int jie = 1;
    for (int i = 1; i <= n; i++)
    {
        jie *= i;
    }
    return jie;
}
```

### 递归调用

被调用函数又调用了它本身称为递归调用，这种函数也叫递归函数。

递归是一种编程技巧，通常用来解决可以被分解为相似子问题的问题。

```
#include <stdio.h>

long long power(int n)
{
    long long r;
    if (n > 1)
    {
        r = n * power(n - 1);
    }
    else
    {
        r = 1;  //  满足递归结束条件时停止递归
    }
    return r;
}

int main()
{
    printf("%lld\n", power(5));
    return 0;
}
```

### 自定义数据类型

**结构类型**

​    当相关的数据项的数据类型不同时，可以定义结构类型并使用结构类型变量来保存这些数据。

​    结构类型允许嵌套其它结构类型作为成员。

​    可以使用typedef给结构类型定义简洁名称。

​    通过结构指针访问结构中的成员时使用->操作符。

```
#include<stdio.h>

struct Student 
{
    int no;
    int age;
    char gender;
    float score;
} s3;

void student_info(struct Student stu)
{
    printf("学号：%d 年龄：%d 性别：%c 成绩：%.1f\n", stu.no, stu.age, stu.gender, stu.score);
}

int main()
{
    printf("%zd\n", sizeof(struct Student));
    struct Student s1;
    s1.age = 19;
    s1.no = 202601;
    s1.gender = 'b';
    s1.score = 99.5;
    struct Student s2 = { 202602 ,18 , 'g',97 };
    student_info(s1);
    student_info(s2);

    s3.age = 18;
    s3.no = 202603;
    s3.gender = 'b';
    s3.score = 90.5;
    student_info(s3);
    return 0;
}
```

**联合**

联合可以使几个不同变量占用同一内存空间，也称共同体。

共同体中，多个变量共享存储空间，一个变量改变了，其他变量也就跟着改变了；

结构体中，多个变量有独立的存储空间，一个变量改变了，其他变量也就跟着改变了。

```
#include <stdio.h>
//  定义联合类型
union MyData
{
    int i;
    char c;
    float f;
};

int main()
{
    union MyData d;
    d.i = 97;
    printf("d.c：%c\td.f：%f\n", d.c, d.f);
    d.c = 'A';
    printf("d.i：%d\td.f：%f\n", d.i, d.f);

    printf("sizeof(union MyData)：%zd\n", sizeof(union MyData));
    return 0;
}
```

**枚举**

 枚举是用标识符表示的整型常量集合。

```
#include<stdio.h>

//  定义枚举类型
//  枚举类型中各元素不能同名，如果不指定枚元素的起始值，
//  默认从0开始设置元素的值，后面的元素依次加一。
enum weekend { Sun, Mon, Tue , Wed, Thu, Fri, Sat };

int main()
{
    int n;
    printf("输入一个数字：");
    scanf("%d", &n);
    switch (n)
    {
    case Sun:
        printf("星期天\n"); break;
    case Mon:
        printf("星期一\n"); break;
    case Tue:
        printf("星期二\n"); break;
    case Wed:
        printf("星期三\n"); break;
    case Thu:
        printf("星期四\n"); break;
    case Fri:
        printf("星期五\n"); break;
    case Sat:
        printf("星期六\n"); break;
    }
}
```

### 多文件组织

Main.c

```
#include <stdio.h>
#include "Rectangle.h"
#include "Round.h"
#include "Cylinder.h"

int main()
{
    struct Rectangle rect = { { 24, 8}, {39, 20} };
    printf("面积为：%d\n", area_rec(&rect));
    printf("周长为：%d\n", perimeter_rec(&rect));

    struct Round rou = { 9 };
    printf("面积为：%.2f\n", area_rou(&rou));
    printf("周长为：%.2f\n", perimeter_rou(&rou));

    struct Cylinder cyl = { 9, 20 };
    printf("表面积为：%.2f\n", area_cyl(&cyl));
    printf("体积为：%.2f\n", volume_cyl(&cyl));
    return 0;
}
```

Point.h

```
#ifndef Point_H
#define Point_H
struct Point
{
    int x;
    int y;
}; 
#endif // !Point_H
```

Rectangle.h

```
#include "Point.h"

struct Rectangle
{
    struct Point point1;
    struct Point point2;
    //int length;
    //int width;
};

int area_rec(const struct Rectangle* rect);

int perimeter_rec(const struct Rectangle* rect);
```

Rectangle.c

```
#include"Rectangle.h"

//  取绝对值
int my_abs(int x) {
    if (x < 0) {
        return -x;
    }
    else {
        return x;
    }
}


int area_rec(const struct Rectangle* rect)
{
   int d1 = rect->point2.x - rect->point1.x;
   int d2 = rect->point2.y - rect->point1.y;
    return my_abs(d1) * my_abs(d2);
}
int perimeter_rec(const struct Rectangle* rect)
{
    int d1 = rect->point2.x - rect->point1.x;
    int d2 = rect->point2.y - rect->point1.y;
    return (my_abs(d1) + my_abs(d2)) * 2;
}
```
