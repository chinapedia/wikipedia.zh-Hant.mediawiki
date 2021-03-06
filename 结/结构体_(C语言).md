{{NoteTA
|G1= IT
}}
在[[C语言|C语言]]中，'''结构体'''（struct）指的是一种[[数据结构|数据结构]]，是C语言中[[複合型別|复合数据类型]]（aggregate data type）的一类。结构体可以被声明为[[变量|变量]]、[[指標_(電腦科學)|指针]]或[[数组|数组]]等，用以实现较复杂的数据结构。结构体同时也是一些元素的集合，这些元素称为结构体的成员（member），且这些成员可以为不同的类型，成员一般用名字访问。

==定义与声明==
结构体的定义如下所示，struct为结构体关键字，tag为结构体的标志，member-list为结构体成员列表，其必须列出其所有成员；variable-list为此结构体声明的变量。

<pre> struct tag { member-list } variable-list ; </pre>

在一般情况下，tag、member-list、variable-list这3部分至少要出现2个。以下为示例：

<source lang="c">

//此声明声明了拥有3个成员的结构体，分别为整型的a，字符型的b和双精度的c
//同时又声明了结构体变量s1
//这个结构体并没有标明其标签
struct 
{
    int a;
    char b;
    double c;
} s1;

//此声明声明了拥有3个成员的结构体，分别为整型的a，字符型的b和双精度的c
//结构体的标签被命名为SIMPLE,没有声明变量
struct SIMPLE
{
    int a;
    char b;
    double c;
};
//用SIMPLE标签的结构体，另外声明了变量t1、t2、t3
struct SIMPLE t1, t2[20], *t3;

//也可以用typedef创建新类型
typedef struct
{
    int a;
    char b;
    double c; 
} Simple2;
//现在可以用Simple2作为类型声明新的结构体变量
Simple2 u1, u2[20], *u3;

</source>

在上面的声明中，第一个和第二声明被编译器当作两个完全不同的类型，即使他们的成员列表是一样的，如果令t3=&s1，则是非法的。

结构体的成员可以包含其他结构体，也可以包含指向自己结构体类型的[[指针|指针]]，而通常这种[[指针|指针]]的应用是为了实现一些更高级的数据结构如链表和树等。

<source lang="c">

//此结构体的声明包含了其他的结构体
struct COMPLEX
{
    char string[100];
    struct SIMPLE a;
};

//此结构体的声明包含了指向自己类型的指针
struct NODE
{
    char string[100];
    struct NODE *next_node;
};

</source>

如果两个结构体互相包含，则需要对其中一个结构体进行不完整声明，如下所示：

<source lang="c">

struct B;    //对结构体B进行不完整声明

//结构体A中包含指向结构体B的指针
struct A
{
    struct B *partner;
    //other members;
};

//结构体B中包含指向结构体A的指针，在A声明完后，B也随之进行声明
struct B
{
    struct A *partner;
    //other members;
};

</source>

==结构体成员访问==

结构体成员依据结构体变量类型的不同，一般有2种访问方式，一种为直接访问，一种为间接访问。直接访问应用于普通的结构体变量，间接访问应用于指向结构体变量的指针。直接访问使用结构体变量名.成员名，间接访问使用(*结构体指针名).成员名或者使用结构体指针名->成员名。相同的成员名称依靠不同的变量前缀区分。
<source lang="c">

struct SIMPLE
{
    int a;
    char b;
};

//声明结构体变量s1和指向结构体变量的指针s2
struct SIMPLE s1, *s2;

//给变量s1和s2的成员赋值,注意s1.a和s2->a并不是同一成员
s1.a = 5;
s1.b = 6;
s2->a = 3;
s2->b = 4;

</source>

==结构体变量存储==

在内存中，编译器按照成员列表顺序分别为每个结构体变量成员分配内存，当存储-{}-过程中需要满足边界对齐的要求时，编译器会在成员之间留下额外的内存空间。如果想确认结构体占多少存储空间，则使用关键字sizeof，如果想得知结构体的某个特定成员在结构体的位置，则使用offsetof宏(定义于stddef.h)。

<source lang="c">

struct SIMPLE
{
    int a;
    char b;
};

//获得SIMPLE类型结构体所占内存大小
int size_simple = sizeof( struct SIMPLE );

#define offsetof(structName, memName) (int)&((structName*) 0)->memName

//获得成员b相对于SIMPLE储存地址的偏移量
int offset_b = offsetof( struct SIMPLE, b );

</source>

==匿名struct==
匿名struct、匿名union以及C++的匿名class，是指既没有类型名，也没有直接用这种类型定义了对象；如果紧随类型定义之后，又定义了该类型的对象，就不算是匿名类型，与普通情形的使用是一样的。

匿名类型作为嵌套定义，即在一个外部类（这里的类是指struct、union、class）的内部定义，则编译器就在匿名类型定义之后定义一个无名变量，并把该匿名类型的数据成员的名字提升到匿名类的外部类的作用域内。如果匿名类型是连续嵌套，则最内部的匿名类型的成员名字被提升到最外部的可用变量名字访问的类的作用域内。

例如，
<source lang="cpp">
    struct sn{
        struct { int i;};  /* 匿名 struct */
    } v1;

   v1.i=101;
</source>

==参考书目==
* {{Cite book | author = [美]Brian W.Kernighan,Dennis M.Ritchie | title = C程序设计语言（第2版·新版）  | location = 机械工业出版社 | language= 中文 }}
* {{Cite book | author = Kenneth A.Reek | title = C和指针 | location = 人民邮电出版社 | language= 中文 }}


[[Category:数据结构|Category:数据结构]]
[[Category:C語言|Category:C語言]]