# BNF与EBNF

1、什么是BNF范式,什么又是EBNF范式?（在学习中经常会碰到用BNF范式描述的规则，老是忘记每个符号确切的作用，现在把他们一一罗列如下，亲手记录的东西应该能记住吧。。。-\_\_-\|\|\|） 

答：巴科斯范式及其扩展\(BNF & Augmented BNF\)





1\)巴科斯范式：巴科斯范式\(BNF: Backus-Naur Form 的缩写\)是由 John Backus 和 Peter Naur 首先引入的用来描述计算机语言语法的符号集。现在，几乎每一位新编程语言书籍的作者都使用巴科斯范式来定义编程语言的语法规则。





2\)巴科斯范式的内容： 

在双引号中的字\("word"\)代表着这些字符本身。而double\_quote用来代表双引号。 

在双引号外的字（有可能有下划线）代表着语法部分。 

&lt; &gt; : 内包含的为必选项。 

\[ \] : 内包含的为可选项。 

{ } : 内包含的为可重复0至无数次的项。 

\|  : 表示在其左右两边任选一项，相当于"OR"的意思。 

::= : 是“被定义为”的意思





3\)扩展的巴科斯范式\(Augmented BNF\):RFC2234 定义了扩展的巴科斯范式\(ABNF\)。近年来在Internet的定义中ABNF被广泛使用。ABNF做了更多的改进，比如说，在ABNF中，尖括号不再需要。





4\)EBNF的基本内容： 

  "..." : 术语符号 

  \[...\] : 选项:最多出现一次 

  {...} : 重复项: 任意次数，包括 0 次 

  \(...\) : 分组 

    \|   : 并列选项，只能选一个 

  斜体字: 参数，在其它地方有解释 

例子BNF应用较多，以下给出一个简单例子，这是用BNF来定义的Java语言中的For语句的实例： 

FOR\_STATEMENT ::= 

      "for" "\(" \( variable\_declaration \| 

  \( expression ";" \) \| ";" \) 

      \[ expression \] ";" 

      \[ expression \] ";" 

      "\)" statement

