{{noteTA|G1=IT}}
'''块'''（blocks）是由[[苹果公司|LLVM]]提出的类似于[[lambda|lambda]]表达式的非标准[[C语言|C语言]]扩展，亦可以应用于[[Objective-C|Objective-C]]与[[C++|C++]]中。它的语法类似于这些函数中的[[闭包_(计算机科学)|闭包]]，即由大括号包括的语句块。

苹果设计块的一个目的是使设计基于[[Grand_Central_Dispatch|Grand Central Dispatch]]线程结构的程序更容易<ref name="gcdworks">[http://images.apple.com/macosx/technology/docs/GrandCentral_TB_brief_20090608.pdf Apple Technical Brief on Grand Central Dispatch] {{webarchive|url=https://web.archive.org/web/20090612043735/http://images.apple.com/macosx/technology/docs/GrandCentral_TB_brief_20090608.pdf |date=2009-06-12 }}, retrieved on June 9, 2009.</ref><ref>[http://arstechnica.com/apple/reviews/2009/08/mac-os-x-10-6.ars/10 Mac OS X 10.6 Snow Leopard: the Ars Technica review: Blocks]</ref>，但块是独立于这一构架的，它也可以在其它程序中以与普通语句块十分相似的方式应用。苹果已经在苹果修改版的[[GCC|GCC]]编译器以及[[Clang|Clang]] [[LLVM|LLVM]]编译器前端中实现了这一特性；同时，LLVM计划，包括了支持块特性的[[运行时库|运行时库]]。

与函数定义类似，块可以有参数，也可以在其内部声明私有变量。与普通的C函数定义不同，块可以使用其上文中定义的变量。一个块定义会产生一个不透明的值，该值同时包括了块内代码的引用和定义时栈内局部变量的快照（而非调用时）。块可以在定义后被调用，其行为与函数指针相同。块可以如同函数指针一般被赋值到变量中，作为函数的参数传递，但若块需要在其被定义的范围之外被使用时，程序员（或API）需要将该块用特别的运算符（Block_copy）标记。

在定义块之后，块内的代码可以在任何时间被调用，语法与调用函数相同。

== 示例 ==
一个简单的计数器的例子：<ref name=lua5impl>:
{{Cite web
 |last1        = Bengtsson
 |first1       = J.
 |author1-link = 
 |title        = Programming with C Blocks on Apple Devices
 |url          = http://thirdcog.eu/pwcblocks/
 |postscript   = <!--None-->
}}{{dead link|date=2018年2月 |bot=InternetArchiveBot |fix-attempted=yes }}</ref>
<source lang="c">
#include <stdio.h>
#include <Block.h>
typedef int (^IntBlock)();

IntBlock MakeCounter(int start, int increment) {
	__block int i = start;
	
	return Block_copy( ^ {
		int ret = i;
		i += increment;
		return ret;
	});
	
}

int main(void) {
	IntBlock mycounter = MakeCounter(5, 2);
	printf("First call: %d\n", mycounter());
	printf("Second call: %d\n", mycounter());
	printf("Third call: %d\n", mycounter());
	
	/* 由于是复制的块，因此需要释放 */
	Block_release(mycounter);
	
	return 0;
}
/* Output:
	First call: 5
	Second call: 7
	Third call: 9
*/
</source>

== 编译 ==
Linux:
<source lang="bash">
clang -fblocks blocks-test.c -lBlocksRuntime
</source>

== 与GCC嵌套函数的关系 ==
块在外表上与GCC的C扩展语句块内的[[嵌套函数|嵌套函数]]相似<ref>{{cite web|url=http://gcc.gnu.org/onlinedocs/gcc-4.0.4/gcc/Nested-Functions.html#Nested-Functions|title=Nested Functions: Using the GNU Compiler Collection (GCC)}}</ref>。然而，嵌套函数与块不同，在退出当前语句块后就不能被调用了。

== C语言标准 ==
块特性已经被提交到[[C标准委员会|C标准委员会]]，作为[[C1x|C1x]]标准的一系列提案<ref>http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1370.pdf BLOCKS PROPOSAL, N1451</ref><ref>http://www.open-std.org/jtc1/sc22/wg14/www/docs/n1451.pdf APPLE’S EXTENSIONS TO C, N1451</ref>。

== 参见 ==
* [[闭包_(计算机科学)|闭包 (计算机科学)]]
* [[LLVM|LLVM]]
* [[Lambda_演算|Lambda 演算]]
* [[XNU|XNU]]
* [[Thunk|Thunk]]

== 参考资料 ==
{{reflist|2}}

== 外部链接 ==
* {{cite web|url=http://clang.llvm.org/docs/LanguageExtensions.html#blocks|title=Clang Language Extensions: Blocks|publisher=LLVM Project|accessdate=2010-06-09}}
* {{cite web|url=http://compiler-rt.llvm.org|title="compiler-rt" Blocks Runtime Library|Blocks Runtime|publisher=LLVM Project|accessdate=2011-03-25}}

[[Category:并发计算|Category:并发计算]]
[[Category:C语言|Category:C语言]]