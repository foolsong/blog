##<center>OCLint的部分规则（Unuseed 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：





####1、unused local variable
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/unused/UnusedLocalVariableRule.cpp)
> This rule detects local variables that are declared, but not used.

>简单解释：有局部变量没有使用。

```
	int example(int a) {
	    int i;          // variable i is declared, but not used
	    return 0;
	}
```

> Suppress:
`__attribute__((annotate("oclint:suppress[unused local variable]")))`


####2、unused method parameter
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/unused/UnusedMethodParameterRule.cpp)
> This rule detects parameters that are not used in the method.

>简单解释：方法参数没有被使用.

```
	int example(int a)  // parameter a is not used {
	    return 0;
	}
```

> Suppress:
`__attribute__((annotate("oclint:suppress[unused method parameter]")))`