##<center>OCLint的部分规则（Empty 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：




####1、empty catch statement 
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyCatchStatementRule.cpp)
> This rule detects instances where an exception is caught, but nothing is done about it.

>简单解释：捕获了异常，在Catch中什么都没有做的情况。

```
	void example()  {
	    try {
	        int* m= new int[1000];
	    }
	    catch(...)                  // empty catch statement, this swallows an exception
	    {
	    }
	}
```

####2、empty do/while statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyDoWhileStatementRule.cpp)
> This rule detects instances where do-while statement does nothing.

>简单解释：空的`do-while`检查。

```
	void example() {
	    do
	    {                           // empty do-while statement
	    } while(1);
	}
```

####3、empty else block
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyElseBlockRule.cpp)
> This rule detects instances where a else statement does nothing.

>简单解释：空的`else`检查。

```
	int example(int a)  {
	    if (1)  {
	        return a + 1;
	    }  else  {               // empty else statement, can be safely removed
	    }
	}
```

####4、empty finally statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyFinallyStatementRule.cpp)
> This rule detects instances where a finally statement does nothing.

>简单解释：空的`finally`检查。

```
	void example() {
	    Foo *foo;
	    @try {
	        [foo bar];
	    }
	    @catch(NSException *e) {
	        NSLog(@"Exception occurred: %@", [e description]);
	    }
	    @finally            // empty finally statement, probably forget to clean up?
	    {
	    }
	}
```

####5、empty for statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyForStatementRule.cpp)
> This rule detects instances where a for statement does nothing.

>简单解释：空的`for`语句检查。

```
	void example(NSArray *array) {
	    for (;;)                // empty for statement
	    {
	    }
	    for (id it in array)    // empty for-each statement
	    {
	    }
	}
```

####6、empty if statement
&#160; &#160; &#160;  `Since:0.2` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyIfStatementRule.cpp)
> This rule detects instances where a condition is checked, but nothing is done about it.

>简单解释：空的`if`检查。

```
	void example(int a) {
	    if (a == 1)                  // empty if statement
	    {
	    }
	}
```

####7、empty switch statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptySwitchStatementRule.cpp)
> This rule detects instances where a switch statement does nothing.

>简单解释：空的`switch`检查。

```
	void example(int i) {
	    switch (i)              // empty switch statement
	    {
	    }
	}
```

####8、empty try statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyTryStatementRule.cpp)
> This rule detects instances where a try statement is empty.

>简单解释：空的`try`检查。

```
	void example() {
	    try                     // but this try statement is empty
	    {
	    }
	    catch(...) {
	        cout << "Exception is caught!";
	    }
	}
	
```

####9、 empty while statement
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/empty/EmptyWhileStatementRule.cpp)
> This rule detects instances where a while statement does nothing.

>简单解释：空的`while`检查。

```
	void example(int a {
	    while(a--)              // empty while statement
	    {
	    }
	}
```


---
####-、-
&#160; &#160; &#160;  `Since:0.-` [定义类传送门～点击](--)
> -

>简单解释：-

```
	-
```
