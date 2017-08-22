##<center>OCLint的部分规则（Redundant 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：



####1、redundant conditional operator
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/RedundantConditionalOperatorRule.cpp)
> This rule detects three types of redundant conditional operators:

> 1. true expression and false expression are returning true/false or false/true respectively; 
2. true expression and false expression are the same constant;
3. true expression and false expression are the same variable expression.

> They are usually introduced by mistake, and should be simplified.

>简单解释：冗余的条件判断会造成一些错误，应该让它变得简洁。

> 比如： 1.`true`对应`true`。`false`对应`false`。

> &#160; &#160; &#160; &#160; &#160; &#160;2 . `true`对应`false`。`false`对应`true`。

>   &#160; &#160; &#160; &#160; &#160; &#160;3 . `true`和`false`一致。

```
	void example(int a, int b, int c) {
	    bool b1 = a > b ? true : false;     // true/false: bool b1 = a > b;
	    bool b2 = a > b ? false : true;     // false/true: bool b2 = !(a > b);
	    int i1 = a > b ? 1 : 1;             // same constant: int i1 = 1;
	    float f1 = a > b ? 1.0 : 1.00;      // equally constant: float f1 = 1.0;
	    int i2 = a > b ? c : c;             // same variable: int i2 = c;
	}
```

####2、redundant if statement
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/RedundantIfStatementRule.cpp)
> This rule detects unnecessary if statements.

>简单解释：多余的if判断，可以省略。

```
	bool example(int a, int b) {
	    if (a == b)             // this if statement is redundant
	    {
	        return true;
	    }  else   {
	        return false;
	    }                       // the entire method can be simplified to return a == b;
	}
```

####3、redundant local variable
&#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/RedundantIfStatementRule.cpp)
> This rule detects cases where a variable declaration is immediately followed by a return of that variable.

>简单解释：冗余的局部变量，可以省略，直接`return`。

```
	int example(int a) {
	    int b = a * 2;
	    return b;   // variable b is returned immediately after its declaration,
	}
```

####4、redundant nil check
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/RedundantNilCheckRule.cpp)
> C/C++-style null check in Objective-C like foo != nil && [foo bar] is redundant, since sending a message to a nil object in this case simply returns a false-y value.

>简单解释：在C或者C++中适用的判空检查在OC中是多余的。因为在OC中向空对象发送消息会返回false值。

```
	+ (void)compare:(A *)obj1 withOther:(A *)obj2  {
	    if (obj1 && [obj1 isEqualTo:obj2]) // if ([obj1 isEqualTo:obj2]) is okay   {
	    }
	}
```

####5、 unnecessary else statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/UnnecessaryElseStatementRule.cpp)
> When an if statement block ends with a return statement, or all branches in the if statement block end with return statements, then the else statement is unnecessary. The code in the else statement can be run without being in the block.

>简单解释：如果if中已经带有return，则不需要写else语句。

```
	bool example(int a) {
	    if (a == 1)                 // if (a == 1)
	    {                           // {
	        cout << "a is 1.";      //     cout << "a is 1.";
	        return true;            //     return true;
	    }                           // }
	    else                        //
	    {                           //
	        cout << "a is not 1."   // cout << "a is not 1."
	    }                           //
	}
	
```

####6、unnecessary null check for dealloc
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/UnnecessaryNullCheckForCXXDeallocRule.cpp)
> char* p = 0; delete p;isvalid.Thisrulelocatesunnecessaryif (p)checks.

>简单解释：在dealloc中不需要判空，就能Delete元素。

```
	void m(char* c) {
	    if (c != nullptr) { // and be simplified to delete c;
	        delete c;
	    }
	}
```

####7、 useless parentheses
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/redundant/UselessParenthesesRule.cpp)
> This rule detects useless parentheses.

>简单解释：检查无用的括号。

```
	int example(int a) {
	    int y = (a + 1);    // int y = a + 1;
	    if ((y > 0))        // if (y > 0)
	    {
	        return a;
	    }
	    return (0);         // return 0;
	}
```

> <font color=red>PS：</font>检测了一下在括号里是逻辑判断时不会被检测到,如下（不会被检查到）：

```
    BOOL aaaa = YES;
    BOOL bbbb = NO;
     if((aaaa) && (bbbb)) {
        return YES;
      }
```