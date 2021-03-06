{{noteTA
|G1=IT
}}
'''大O符号'''（{{lang-en|Big O notation}}），又稱為'''漸進符號'''，是用于描述[[函数|函数]][[渐近分析|渐近行为]]的[[数学|数学]]符号。更确切地说，它是用另一个（通常更简单的）函数来描述一个函数[[数量级|数量级]]的'''渐近上界'''。在[[数学|数学]]中，它一般用来刻画被截断的[[无穷级数|无穷级数]]尤其是[[渐近级数|渐近级数]]的剩余项；在[[计算机科学|计算机科学]]中，它在[[算法分析|分析]][[算法|算法]][[計算複雜性理論|复杂性]]的方面非常有用。

大O符号是由[[德国|德国]][[数论|数论]]学家{{Link-en|保罗·巴赫曼|Paul Bachmann}}在其1892年的著作《解析数论》（''Analytische Zahlentheorie''）首先引入的。而这个记号则是在另一位德国数论学家{{Link-en|艾德蒙·朗道|Edmund Landau}}的著作中才推广的，因此它有时又称为'''朗道符号'''（Landau symbols）。代表“order of ...”（……阶）的大'''O'''，最初是一个大写[[希腊字母|希腊字母]]“[[Ο|Ο]]”（omicron），现今用的是大写[[拉丁字母|拉丁字母]]“[[O|O]]”。

== 使用 ==

=== 无穷大渐近 ===
大O符号在分析算法效率的时候非常有用。举个例子，解决一个规模为<math>n</math>的问题所花费的时间（或者所需步骤的数目）可以表示為：<math>T(n)=4n^2-2n+2</math>。当<math>n</math>增大时，<math>n^2</math>项将开始占主导地位，而其他各项可以被忽略。举例说明：当<math>n=500</math>，<math>4n^2</math>项是<math>2n</math>项的1000倍大，因此在大多数场合下，省略后者对表达式的值的影响将是可以忽略不计的。

进一步看，如果我们与任一其他级的表达式比较，<math>n^2</math>项的[[系数|系数]]也是无关紧要的。例如：一个包含<math>n^3</math>或<math>n^2</math>项的表达式，即使<math>T(n)=1,000,000\cdot n^2</math>，假定<math>U(n)=n^3</math>，一旦<math>n</math>增长到大于1,000,000，后者就会一直超越前者（<math>T(1,000,000)=1,000,000^3=U(1,000,000)</math>）。


这样，針對第一個例子<math>T(n)=4n^2-2n+2</math>, 大O符号就记下剩余的部分，写作：

:<math>T(n)\in\Omicron(n^2)</math>
或
:<math>T(n)=\Omicron(n^2)</math>

并且我们就说该算法具有<math>n^2</math>阶（平方阶）的[[时间复杂度|时间复杂度]]。

===无穷小渐近===
大O也可以用来描述数学函数估计中的误差项。例如<math>e^x</math>的[[泰勒展开|泰勒展开]]：

:<math>e^x=1+x+\frac{x^2}{2}+\hbox{O}(x^3)\qquad</math>当<math>x \to 0</math>时

这表示，如果<math>x</math>足够接近于0，那么误差<math>e^x - \left(1 + x + \frac{x^2}{2}\right)</math>的[[绝对值|绝对值]]小于<math>x^3</math>的某一常数倍。

注：泰勒展开的误差余项<math>r_3(x)</math>是关于<math>x^3</math>一个高阶无穷小量，用小o来表示，即：<math>r_3(x)</math>=<math>o(x^3)</math>，<math>\textstyle \lim_{x \to 0} \displaystyle \frac{r_3(x)}{x^3}=0</math>

== 形式化定义 ==

給定兩個定義在[[實數|實數]]某子集上的關於<math>x</math>的函數<math>f(x)</math>和<math>g(x)</math>，當<math>x</math>趨近於無窮大時，[[當且僅當|當且僅當]]存在正常數<math>M</math>，使得對於所有足夠大（[https://en.wikipedia.org/wiki/List_of_mathematical_jargon#sufficiently_large sufficiently_large]）的<math>x</math>，都有<math>f(x)</math>的絕對值小於等於<math>M</math>乘以<math>g(x)</math>的絕對值，那麼我們就可以說，當<math>x \to \infty</math>時，

:<math>f(x)=\Omicron(g(x))</math>

也就是說，假如[[當且僅當|當且僅當]]存在正實數<math>M</math>和實數<math>x</math><sub>0</sub>，使得對於所有的<math>x \ge x_0</math>，均有：<math>|f(x)| \le \ M |g(x)|</math>成立，我們就可以認爲，<math>f(x)=\Omicron(g(x))</math>。

在很多情況下，我們會省略“當<math>x</math>趨近於無限大時”這個前提，而簡寫爲：

:<math>f(x)=\Omicron(g(x))</math>

此概念也可以用於描述函數<math>f</math>在接近實數<math>a</math>時的行爲，通常<math>a = 0</math>。當我們說，當<math>x \to a</math>時，有<math>f(x)=\Omicron(g(x))</math>，也就相當於稱，[[當且僅當|當且僅當]]存在正實數<math>M</math>和實數<math>\delta</math>，使得對於所有的<math>0 \le |x - a| \le \delta</math>，均有<math>|f(x)| \le \ M |g(x)|</math>成立。

如果當<math>x</math>和<math>a</math>足夠接近（[https://en.wikipedia.org/wiki/List_of_mathematical_jargon#sufficiently_large sufficiently_close]）時，<math>g(x)</math>的值仍不爲0，這兩種定義就可以統一用[[上極限|上極限]]來表示：

:當且僅當<math>\limsup_{x\to a} \left|\frac{f(x)}{g(x)}\right| < \infty</math>時，有<math>f(x)=\Omicron(g(x))</math>。

== 例子 ==
在具体的运用中，我们不一定使用大O符号的标准定义，而是使用几条简化规则来求出关于函数<math>f</math>的大O表示：

* 假如<math>f(x)</math>是几项之和，那么只保留增长最快（通常是阶最高）的项，其他项省略。
* 假如<math>f(x)</math>是几项之积，那么常数（不取决于x的乘数）省略。

比如，使<math>f(x) = 6x^4 - 2x^3 + 5</math>，我们想要用大O符号来简化这个函数，来描述<math>x</math>接近无穷大时函数的增长情况。此函数由三项相加而成，<math>6x^4</math>，<math>-2x^3</math>和<math>5</math>。由于增长最快的是<math>6x^4</math>这一项（因为阶最高，在x接近无穷大时，其对和的影响会大大超过其余两项），应用第一条规则，保留它而省略其他两项。对于<math>6x^4</math>，由两项相乘而得，<math>6</math>和<math>x^4</math>；应用第二条规则，<math>6</math>是无关x的常数，所以省略。最后结果为<math>x^4</math>，也即<math>g(x) = x^4</math>。故有：

:由<math>f(x) = \Omicron(g(x))</math>，可得：
:<math>6x^4 - 2x^3 + 5 = O(x^4)</math>

我们可以将上式扩展为标准定义形式：

:对任意<math>x \ge x_0</math>，均有<math>|f(x)| \le M|g(x)|</math>，也就是<math>6x^4 - 2x^3 + 5 \le M|x^4|</math>

可以（粗略）求出<math>M</math>和<math>x_0</math>的值来验证。使<math>x_0=1</math>：

:<math>\begin{align}|6x^4 - 2x^3 + 5| &\le 6x^4 + |2x^3| + 5\\
                                      &\le 6x^4 + 2x^4 + 5x^4\\
                                      &\le 13x^4\end{align}</math>

故<math>M</math>可以为13。故两者都存在。

== 常用的函数阶 ==

下面是在[[算法分析|分析算法]]的时候常见的函数分类列表。所有这些函数都处于<math>n</math>趋近于无穷大的情况下，增长得慢的函数列在上面。<math>c</math>是一个任意常数。

{| class="wikitable"
! 符号 !! 名称
|-
|<math>\Omicron(1)\!</math>||[[常数|常数]]（阶，下同）
|-
|<math>\Omicron(\log n)\!</math>||[[对数|对数]]
|-
|<math>\Omicron[(\log n)^c]\!</math>||[[多对数|多对数]]
|-
|<math>\Omicron(n)\!</math>||[[线性|线性]]，次线性
|-
|<math>\Omicron(n\log^*n)\!</math>||<math>\log^*n</math>為[[迭代对数|迭代对数]]
|-
|<math>\Omicron(n \log n)\!</math>||[[线性对数|线性对数]]，或对数线性、拟线性、超线性
|-
|<math>\Omicron( n^2)\!</math> || [[平方|平方]]
|-
|<math>\Omicron(n^c), \operatorname{Integer}(c>1)</math>||[[多项式|多项式]]，有时叫作“代数”（阶）
|-
|<math>\Omicron(c^n)\!</math>||[[指数|指数]]，有时叫作“[[等比数列|几何]]”（阶）
|-
|<math>\Omicron(n!)\!</math>||[[阶乘|阶乘]]，有时叫做“组合”（阶）
|}

== 一些相关的渐近符号 ==

大O是最经常使用的比较函数的渐近符号。

{| class="wikitable"
!符号
!定义
|-
|<math> f(n)=\Omicron (g(n))</math>
|渐近上限
|-
|<math>f(n)=o(g(n))</math>
|Asymptotically negligible渐近可忽略不计（<math>\lim{} \frac{f(n)}{g(n)} = 0</math>）
|-
|<math> f(n)=\Omega(g(n))</math>
|渐近下限（[[当且仅当|当且仅当]]<math> g(n) = \Omicron(f(n))</math>）
|-
|<math> f(n) = \omega (g(n)) </math>
|Asymptotically dominant渐近主导（当且仅当<math>g(n)=o(f(n))</math>）
|-
|<math> f(n) = \Theta(g(n))</math>
|Asymptotically tight bound渐近紧约束（当且仅当<math>f(n) = \Omicron(g(n))</math>且<math>f(n)=\Omega(g(n))</math>）
|}

==注意==
大O符号经常被误用：有的作者可能会使用大O符号表达[[大Θ符号|大Θ符号]]的含义。因此在看到大O符号时应首先确定其是否为误用。

==参看==
*[[O|O]]（拉丁字母）
*[[大Ω符号|大Ω符号]]
*[[大Θ符号|大Θ符号]]

== 参考文献 ==
=== 引用 ===
{{Reflist}}

=== 来源 ===
{{refbegin}}
; 书籍
* 严蔚敏、吴伟民：《数据结构：C语言版》. 清华大学出版社，1996. ISBN 7-302-02368-9. 1.4节 算法和算法分析，pp. 14-17.
* 朱青：《計算機算法與程序設計》. 清华大学出版社，2009.10。ISBN 978-7-302-20267-7. 1.4节 算法的複雜性分析，pp. 16-17.
{{refend}}

==延伸閱讀==
{{refbegin|2}}
* {{cite book | first=G. H. | last=Hardy | authorlink=G. H. Hardy | title=Orders of Infinity: The 'Infinitärcalcül' of Paul du Bois-Reymond | date=1910 | url=https://archive.org/details/ordersofinfinity00harduoft | publisher=[[Cambridge_University_Press|Cambridge University Press]]}}
* {{cite book | first=Donald | last=Knuth | authorlink=Donald Knuth | title=Fundamental Algorithms | volume=1 | series=The Art of Computer Programming | edition=3rd | publisher=Addison–Wesley | date=1997 | isbn=0-201-89683-4 | section=1.2.11: Asymptotic Representations}}
* {{cite book | first1=Thomas H. | last1=Cormen | authorlink1=Thomas H. Cormen | first2=Charles E. | last2=Leiserson | authorlink2=Charles E. Leiserson | first3=Ronald L. | last3=Rivest | authorlink3=Ronald L. Rivest | first4=Clifford | last4=Stein | authorlink4=Clifford Stein | title=[[Introduction_to_Algorithms|Introduction to Algorithms]] | edition=2nd | publisher=MIT Press and McGraw–Hill | date=2001 | isbn=0-262-03293-7 | section=3.1: Asymptotic notation}}
* {{cite book | first=Michael | last=Sipser | authorlink=Michael Sipser | year = 1997 | title = Introduction to the Theory of Computation | publisher = PWS Publishing | isbn = 0-534-94728-X | pages=226–228}}
* {{cite conference | first1=Jeremy | last1=Avigad | first2=Kevin | last2=Donnelly | url=http://www.andrew.cmu.edu/~avigad/Papers/bigo.pdf | format=PDF | title=Formalizing O notation in Isabelle/HOL | doi=10.1007/978-3-540-25984-8_27 | conference=International Joint Conference on Automated Reasoning | date=2004}}
* {{cite web | first=Paul E. | last=Black | url=https://xlinux.nist.gov/dads/HTML/bigOnotation.html | title=big-O notation | work=Dictionary of Algorithms and Data Structures | editor-first=Paul E. | editor-last=Black | publisher=U.S. National Institute of Standards and Technology | date=11 March 2005 | access-date=December 16, 2006}}
* {{cite web | first=Paul E. | last=Black | url=https://xlinux.nist.gov/dads/HTML/littleOnotation.html | title=little-o notation | work=Dictionary of Algorithms and Data Structures | editor-first=Paul E. | editor-last=Black | publisher=U.S. National Institute of Standards and Technology | date=17 December 2004 | access-date=December 16, 2006}}
* {{cite web | first=Paul E. | last=Black | url=https://xlinux.nist.gov/dads/HTML/omegaCapital.html | title=Ω | work=Dictionary of Algorithms and Data Structures | editor-first=Paul E. | editor-last=Black | publisher=U.S. National Institute of Standards and Technology | date=17 December 2004 | access-date=December 16, 2006}}
* {{cite web | first=Paul E. | last=Black | url=https://xlinux.nist.gov/dads/HTML/omega.html | title=ω | work=Dictionary of Algorithms and Data Structures | editor-first=Paul E. | editor-last=Black | publisher=U.S. National Institute of Standards and Technology | date=17 December 2004 | access-date=December 16, 2006}}
* {{cite web | first=Paul E. | last=Black | url=https://xlinux.nist.gov/dads/HTML/theta.html | title=Θ | work=Dictionary of Algorithms and Data Structures | editor-first=Paul E. | editor-last=Black | publisher=U.S. National Institute of Standards and Technology | date=17 December 2004 | access-date=December 16, 2006}}
{{refend}}

[[Category:算法分析|Category:算法分析]]
[[Category:數學表示法|Category:數學表示法]]
[[Category:数学符号|Category:数学符号]]
[[Category:渐近分析|Category:渐近分析]]