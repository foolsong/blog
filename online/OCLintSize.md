##<center>OCLint的部分规则（Size 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：



####1、high cyclomatic complexity
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/CyclomaticComplexityRule.cpp)
> Cyclomatic complexity is determined by the number of linearly independent paths through a program’s source code. In other words, cyclomatic complexity of a method is measured by the number of decision points, like if, while, and for statements, plus one for the method entry.

>简单解释：圈复杂度过高。统计一个函数有多少个分支(if, while,for,等等)，没有的话复杂度为一，每增加一个分支复杂度加一。简单计算的话V(G)=e-n+2。其中，e表示控制流图中边的数量，n表示控制流图中节点的数量，或者V(G)=区域数=判定节点数+1。当然可以数一数。

```
	void example(int a, int b, int c) { // 1
	    if (a == b) {                   // 2
	        if (b == c) {               // 3 
	        } else if (a == c){          // 3
	        }
	        else {
	        }
	    }
	    for (int i = 0; i < c; i++)  {  // 4
	    }
	    switch(c)  {
	        case 1:                   // 5
	            break;
	        case 2:                   // 6
	            break;
	        default:                  // 7
	            break;
	    }
	}
```

> Thresholds:
CYCLOMATIC_COMPLEXITY The cyclomatic complexity reporting threshold, default value is 10. Suppress:


>Suppress:
`__attribute__((annotate("oclint:suppress[high cyclomatic complexity]")))`

> References:
McCabe (December 1976). [“A Complexity Measure”](http://www.literateprogramming.com/mccabe.pdf). IEEE Transactions on Software Engineering: 308–320

####2、long class
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/LongClassRule.cpp)
> Long class generally indicates that this class tries to do many things. Each class should do one thing and that one thing well.

>简单解释：类行数太多。

```
	class Foo {
	    void bar() {
	        // 1001 lines of code
	    }
	}
```

> Thresholds:
LONG_CLASS The class size reporting threshold, default value is 1000.


####3、 long line
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/LongLineRule.cpp)
> When the number of characters for one line of code is very high, it largely harms the readability. Break long lines of code into multiple lines.

>简单解释：单行代码太长，影响可读性。

```
	void example()
	{
	    int a012345678901234567890123456789012345678901234567890123456789012345678901234567890123456789;
	}
```

> Thresholds:
LONG_LINE The long line reporting threshold, default value is 100.

####4、long method
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/LongMethodRule.cpp)
> Long method generally indicates that this method tries to do many things. Each method should do one thing and that one thing well.

>简单解释：方法太长，影响阅读，应该实现单一职责。

```
	void example() {
	    cout << "hello world";
	    cout << "hello world";
	    // repeat 48 times
	}
```

> Thresholds:
LONG_METHOD The long method reporting threshold, default value is 50.

####5、 high ncss method
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/NcssMethodCountRule.cpp)
> This rule counts number of lines for a method by counting Non Commenting Source Statements (NCSS). NCSS only takes actual statements into consideration, in other words, ignores empty statements, empty blocks, closing brackets or semicolons after closing brackets. Meanwhile, a statement that is broken into multiple lines contribute only one count.

>简单解释：其实是指某个代码块中代码行数过多（只统计有效的语句），查看代码块中代码是否能拆分，公共功能能否提供一个公共接口。空语句，空块，右括号或分号后的右括号会被忽略。

```
	void example()          // 1
	{
	    if (1)              // 2
	    {
	    }  else                // 3
	    {
	    }
	}
```

> Thresholds:
NCSS_METHOD The high NCSS method reporting threshold, default value is 30.

> Suppress:
__attribute__((annotate("oclint:suppress[high ncss method]")))


####6、deep nested block
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/NestedBlockDepthRule.cpp)
> This rule indicates blocks nested more deeply than the upper limit.

>简单解释：嵌套块是否超过指定的深度值.

```
	if (1) {               // 1
	    {           // 2
	        {       // 3
	        }
	    }
	}
```

> Thresholds:
NESTED_BLOCK_DEPTH The depth of a block or compound statement reporting threshold, default value is 5.

####7、high npath complexity
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/NPathComplexityRule.cpp)
> NPath complexity is determined by the number of execution paths through that method. Compared to cyclomatic complexity, NPath complexity has two outstanding characteristics: first, it distinguishes between different kinds of control flow structures; second, it takes the various type of acyclic paths in a flow graph into consideration.
Based on studies done by the original author in AT&T Bell Lab, an NPath threshold value of 200 has been established for a method.

>简单解释：NPath复杂度是一个方法中各种可能的执行路径总和,一般把200作为考虑降低复杂度的临界点，这里提示NPath复杂度过高。

```
	void example() {
	    // complicated code that is hard to understand
	}
```

>Thresholds:
NPATH_COMPLEXITY The NPath complexity reporting threshold, default value is 200. 

>Suppress:
`__attribute__((annotate("oclint:suppress[high npath complexity]"))) `

>References:
Brian A. Nejmeh (1988). [“NPATH: a measure of execution path complexity and its applications”](http://dl.acm.org/citation.cfm?id=42379). Communications of the ACM 31 (2) p. 188-200

####8、too many fields
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/TooManyFieldsRule.cpp)
> A class with too many fields indicates it does too many things and lacks proper abstraction. It can be redesigned to have fewer fields.

>简单解释：一个类中有定义太多东西，需要进行适当的抽象、设计。

```
	class c {
	    int a, b;
	    int c;
	    // ...
	    int l;
	    int m, n;
	    // ...
	    int x, y, z;
	    void m() {}
	};
```

> Thresholds:
TOO_MANY_FIELDS The reporting threshold for too many fields, default value is 20.

####9、too many methods
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/TooManyMethodsRule.cpp)
> A class with too many methods indicates it does too many things and is hard to read and understand. It usually contains complicated code, and should be refactored.

>简单解释：一个类有太多的方法，证明他做了太多的事儿，不利于理解。应该考虑重构。考虑单一职责。

```
	class c {
	    int a();
	    int b();
	    int c();
	    // ...
	    int l();
	    int m();
	    int n();
	    // ...
	    int x();
	    int y();
	    int z();
	    int aa();
	    int ab();
	    int ac();
	    int ad();
	    int ae();
	};
```

####10、too many parameters
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/size/TooManyParametersRule.cpp)
> Methods with too many parameters are hard to understand and maintain, and are thirsty for refactorings, like Replace Parameter With method, Introduce Parameter Object, or Preserve Whole Object.

>简单解释： 一个方法中参数过多。

```
	void example(int a, int b, int c, int d, int e, int f,
	    int g, int h, int i, int j, int k, int l) {
	}
```

> TOO_MANY_PARAMETERS The reporting threshold for too many parameters, default value is 10. 

> References:
Fowler, Martin (1999). Refactoring: Improving the design of existing code. Addison Wesley.
