##<center>OCLint的部分规则（Naming 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：


####1、long variable name
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/naming/LongVariableNameRule.cpp)
> Variables with long names harm readability.

>简单解释：变量名较长，影响可读性。

```
	void aMethod() {
	    int reallyReallyLongIntegerName;
	}
```

> Thresholds:
LONG_VARIABLE_NAME The long variable name reporting threshold, default value is 20.

####2、hort variable name
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/naming/ShortVariableNameRule.cpp)
> A variable with a short name is hard to understand what it stands for. Variable with name, but the name has number of characters less than the threshold will be emitted.

>简单解释：变量名太短，影响可读性。

```
	void aMethod(int i)  // i is short
	{
	    int ii;          // ii is short
	}
```

> Thresholds:
SHORT_VARIABLE_NAME The short variable name reporting threshold, default value is 3.