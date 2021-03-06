'''系统F'''，也叫做'''多态lambda演算'''或'''二阶lambda演算'''，是[[有类型lambda演算|有类型lambda演算]]。它由[[逻辑学家|逻辑学家]][[Jean-Yves_Girard|Jean-Yves Girard]]和[[计算机科学家|计算机科学家]][[John_C._Reynolds|John C. Reynolds]]独立发现的。系统F形式化了[[编程语言|编程语言]]中的参数[[多态_(计算机科学)|多态]]的概念。

正如同[[lambda演算|lambda演算]]有取值于（rang over）函数的变量，和来自它们的粘合子（binder）；二阶lambda演算取值自'''类型'''，和来自它们的粘合子。

作为一个例子，恒等函数有形如A→ A的任何类型的事实可以在系统F中被形式化为判断

:<math>\vdash \Lambda\alpha. \lambda x^\alpha.x: \forall\alpha.\alpha \to \alpha</math>

这里的α是[[类型变量|类型变量]]。

在[[Curry-Howard同构|Curry-Howard同构]]下，系统F对应于[[二阶逻辑|二阶逻辑]]。

系统F，和甚至更加有表达力的lambda演算一起，可被看作[[Lambda立方体|Lambda立方体]]的一部分。

== 逻辑和谓词 ==
布尔类型被定义为：
<math>\forall\alpha.\alpha \to \alpha \to \alpha</math>，这里的α是[[类型变量|类型变量]]。这产生了下列对布尔值<tt>TRUE</tt>和<tt>FALSE</tt>的两个定义：

: TRUE := <math>\Lambda \alpha.\lambda x^\alpha \lambda y^\alpha.x</math>
: FALSE := <math>\Lambda \alpha.\lambda x^\alpha \lambda y^\alpha.y</math>

接着，通过这两个λ-项，我们可以定义一些逻辑算子：
: AND := <math>\lambda x^{Boolean} \lambda y^{Boolean}.((x (Boolean)) y) FALSE</math>
: OR := <math>\lambda x^{Boolean} \lambda y^{Boolean}.((x (Boolean)) TRUE) y</math>
: NOT := <math>\lambda x^{Boolean}. ((x (Boolean)) FALSE) TRUE </math>

实际上不需要''IFTHENELSE''函数，因为你可以只使用原始布尔类型的项作为判定（decision）函数。但是如果需要一个的话：
: IFTHENELSE := <math>\Lambda \alpha.\lambda x^{Boolean}\lambda y^{\alpha}\lambda z^{\alpha}.((x (\alpha)) y) z </math>

''谓词''是返回布尔值的函数。最基本的谓词是<tt>ISZERO</tt>，它返回<tt>TRUE</tt>当且仅当它的参数是[[邱奇数|邱奇数]]  <tt>0</tt>:
: <tt>ISZERO := λ ''n''. ''n'' (λ ''x''. FALSE) TRUE</tt>

==系统F结构==

系统F允许以同[[Martin-Löf类型论|Martin-Löf类型论]]有关的自然的方式嵌入递归构造。抽象结构（S）是使用''构造子''建立的。有函数被定类型为：
:<math>K_1\rightarrow K_2\rightarrow\dots\rightarrow S</math>

当<math>S</math>自身出现类型<math>K_i</math>中的一个内的时候递归就出现了。如果你有<math>m</math>个这种构造子，你可以定义<math>S</math>为：
:<math>\forall \alpha.(K_1^1[\alpha/S]\rightarrow\dots\rightarrow \alpha)\dots\rightarrow(K_1^m[\alpha/S]\rightarrow\dots\rightarrow \alpha)\rightarrow \alpha</math>

例如，自然数可以被定义为使用构造子的归纳数据类型<math>N</math>
:<math>\mathit{zero} : \mathrm{N}</math>
:<math>\mathit{succ} : \mathrm{N} \to \mathrm{N}</math>
对应于这个结构的系统F类型是
<math>\forall \alpha. \alpha \to (\alpha \to \alpha) \to \alpha</math>。这个类型的项由有类型版本的[[邱奇数|邱奇数]]构成，前几个是：
: <tt>0 := </tt><math>\Lambda \alpha . \lambda x^\alpha . \lambda f^{\alpha\to\alpha} . x</math>
: <tt>1 := </tt><math>\Lambda \alpha . \lambda x^\alpha . \lambda f^{\alpha\to\alpha} . f x</math>
: <tt>2 := </tt><math>\Lambda \alpha . \lambda x^\alpha . \lambda f^{\alpha\to\alpha} . f (f x)</math>
: <tt>3 := </tt><math>\Lambda \alpha . \lambda x^\alpha . \lambda f^{\alpha\to\alpha} . f (f (f x))</math>

如果我们反转curried参数的次序（比如<math>\forall \alpha.(\alpha \to \alpha)\to \alpha \to \alpha</math>），则<math>n</math>的邱奇数是接受函数<tt>''f''</tt>作为参数并返回<tt>''f''</tt>的<tt>''n''</tt>次幂的函数。就是说，邱奇数是一个[[高阶函数|高阶函数]] -- 它接受一个单一参数函数<tt>''f''</tt>，并返回另一个单一参数函数。

==用在编程语言中==

本文用的系统F版本是显式类型的，或邱奇风格的演算。包含在λ-项内的类型信息使[[类型检查|类型检查]]直接了当。[[Joe_Wells|Joe Wells]]（1994）设立了一个"难为人的公开问题"，证明系统  F的Curry-风格的变体是[[决定性问题|不可判定的]]，它缺乏明显的类型提示。[https://web.archive.org/web/20041209225820/http://www.cee.hw.ac.uk/~jbw/research-summary.html] [https://web.archive.org/web/20070216230916/http://www.church-project.org/reports/Wells:APAL-1999-v98-no-note.html]

Wells的结果暗含着系统F的[[类型推论|类型推论]]是不可能的。一个限制版本的系统F叫做"[[Hindley-Milner|Hindley-Milner]]"，或简称"HM"，有一个容易的类型推论算法，并用于了很多[[强类型|强类型]]的[[函数式编程语言|函数式编程语言]]，比如[[Haskell|Haskell]]和[[ML|ML]]。

== 参考文献 ==
{{Reflist}}
* Girard, Lafont and Taylor, 1997.  [http://www.cs.man.ac.uk/~pt/stable/prot.pdf ''Proofs and Types''].  Cambridge University Press.
* J. B. Wells. "Typability and type checking in the second-order lambda-calculus are equivalent and undecidable." In ''Proceedings of the 9th Annual [[IEEE|IEEE]] Symposium on Logic in Computer Science (LICS),'' pages 176-185, 1994. [http://www.macs.hw.ac.uk/~jbw/papers/Wells:Typability-and-Type-Checking-in-the-Second-Order-Lambda-Calculus-Are-Equivalent-and-Undecidable:LICS-1994.ps.gz]

==外部链接==
*[http://www.site.uottawa.ca/~fbinard/Intuitionism/TypeTheory/SystemF/ Summary of System F] by Franck Binard.

[[Category:Lambda演算|Category:Lambda演算]]
[[Category:类型论|Category:类型论]]