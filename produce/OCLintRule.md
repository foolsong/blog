##<center>OCLint的部分规则（Basic 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：


####1、bitwise operator in conditional    
&#160; &#160; &#160;  `since:0.6`  [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/BitwiseOperatorInConditionalRule.cpp)
> Checks for bitwise operations in conditionals. Although being written on purpose in some rare cases, bitwise opera- tions are considered to be too “smart”. Smart code is not easy to understand.

> 简单解释：OCLint认为`位运算符`可读性不好

```
	void example(int a, int b) {
	    if (a | b) {
	    }
	    if (a & b) {
	    }
 	 }
```

####2、broken null check    
&#160; &#160; &#160;  `since:0.7`   [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/BrokenNullCheckRule.cpp)
>The broken null check itself will crash the program.

>简单解释：错误的null检查会导致程序`crash`。

```
	void m(A *a, B *b) {
	    if (a != NULL || a->bar(b))
	    {
	    }
	    if (a == NULL && a->bar(b))
	    {
	    }
	}	
```

> <font color=red>PS：</font>OC(objective-c)向空对象可以发消息，不会发生`crash`，但是上面这样的代码的逻辑是错误的。

####3、broken nil check      
&#160; &#160; &#160;  `since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/BrokenNullCheckRule.cpp)
> The broken nil check in Objective-C in some cases returns just the opposite result.

>简单解释：Objective-C中判空，有些情况下会返回预期相反的结果

```Objective-C
	+ (void)compare:(A *)obj1 withOther:(A *)obj2 {
	    if (obj1 || [obj1 isEqualTo:obj2])
	    {
	    }
	    if (!obj1 && ![obj1 isEqualTo:obj2])
	    {
	    }
	}
```

> <font color=red>PS：</font>这样写应该是逻辑错误

####4、broken oddness check       
&#160; &#160; &#160;  `since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/BrokenOddnessCheckRule.cpp)
>Checking oddness by x % 2 == 1won’t work for negative numbers. Use x & 1 == 1,or x % 2 != 0 instead.

>简单解释：对数字奇数性进行检查（x % 2 == 1），如果是负数，会有异常，应使用`x & 1 == 1`或者` x % 2 != 0`

```
	void example() {
	    if (x % 2 == 1)         // violation
	    {
	    }
	    if (foo() % 2 == 1)     // violation
	    {
	    }
	}
```

> <font color=red>PS：</font>在Objective-C 上的测试结果上看，并没有什么大的异常。应该主要的问题是负数进行取模区的结果不会出现正值。
比如（x % 2）的结果是`-1`或者`0`，

####5、collapsible if statements     
 &#160; &#160; &#160;  `Since: 0.6`  [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/CollapsibleIfStatementsRule.cpp)
>This rule detects instances where the conditions of two consecutive if statements can be combined into one in order to increase code cleanness and readability.

>简单解释：两个连续`if`条件可以合并的应该合并，可以提高可读性和代码整洁。

```
	void example(bool x, bool y){
	    if (x)              // these two if statements can be
	    {
	        if (y)          // combined to if (x && y)
	        {
	            foo();
	        }
	    }
	}
```

####6、constant conditional operator  
&#160; &#160; &#160;  `Since: 0.6`  [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/ConstantConditionalOperatorRule.cpp)
>conditional operator whose conditionals are always true or always false are confusing.

>简单解释：检查始终为`true`或者`false`的操作，会使人疑惑。

```
	void example() {
	    int a = 1 == 1 ? 1 : 0;     // 1 == 1 is actually always true
	}
```

####7、constant if expression       
&#160; &#160; &#160;  `Since: 0.2` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/ConstantIfExpressionRule.cpp)
>if statements whose conditionals are always true or always false are confusing.

>简单解释：`if`始终为`true`或者`false`的操作，会使人疑惑。

```
	void example(){
	    if (true) {      // always true
	        foo();
	    }
	    if (1 == 0) {    // always false
	        bar();
	    }
	}
```

####8、 dead code       
&#160; &#160; &#160;  `Since: 0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/DeadCodeRule.cpp)
>Code after return, break, continue, and throw statements is unreachable and will never be executed.

>简单解释：在 `return`、 `break`、 `continue` and `throw` 之后的代码都是无效的

```
	void example(id collection) {
	    for (id it in collection) {
	        continue;
	        int i1;                 // dead code
	    }
	    return;
	    int i2;                     // dead code
	}
```

####9、double negative  
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/DoubleNegativeRule.cpp)
> There is no point in using a double negative, it is always positive.

>简单解释：代码中双重否定没有意义

```
	void example() {
	    int b1 = !!1;
	    int b2 = ~~1;
	}
```

####10、for loop should be while loop     
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/ForLoopShouldBeWhileLoopRule.cpp)
> Under certain circumstances, some for loops can be simplified to while loops to make code more concise.

>简单解释：在有些情况下使用`while`比使用`for`更简洁

```
	void example(int a) {
	    for (; a < 100;) {
	        foo(a);
	    }
	}
```

####11、 goto statement        
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/GotoStatementRule.cpp)
> [Go To Statement Considered Harmful](http://www.cs.utexas.edu/users/EWD/ewd02xx/EWD215.PDF)

>简单解释：[`go to`被认为有害的 ](http://www.cs.utexas.edu/users/EWD/ewd02xx/EWD215.PDF)

```
	void example() {
	    A:
	        a();
	    goto A;     // Considered Harmful
	}
```

####12、jumbled incrementer        
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/JumbledIncrementerRule.cpp)
> Jumbled incrementers are usually typos. If it’s done on purpose, it’s very confusing for code readers.

>简单解释：混乱的迭代器通常会出现错误，可读性不好。

```
	void aMethod(int a) {
	    for (int i = 0; i < a; i++) {
	        for (int j = 0; j < a; i++) {     // references both 'i' and 'j'
	        }
	    }
	}
```

####13、misplaced null check        
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/MisplacedNullCheckRule.cpp)
> The null check is misplaced. In C and C++, sending a message to a null pointer could crash the program. When null is misplaced, either the check is useless or it’s incorrect.

>简单解释：`C `和`C++`在条件判断的时候注意`null`判断放在前面，`逻辑短路`。

```
	void m(A *a, B *b) {
	    if (a->bar(b) && a != NULL) { // violation 
	    }
	    if (a->bar(b) || !a) {        // violation
	    }
	}
```

####14、misplaced nil check    
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/MisplacedNullCheckRule.cpp)
>  The nil check is misplaced. In Objective-C, sending a message to a nil pointer simply does nothing. But code readers may be confused about the misplaced nil check.

>简单解释：`Objective-C`在条件判断的时候注意`nil`判断放在前面，`逻辑短路`。

```
	+ (void)compare:(A *)obj1 withOther:(A *)obj2 {
	    if ([obj1 isEqualTo:obj2] && obj1) {
	    }
	    if (![obj1 isEqualTo:obj2] || obj1 == nil)   {
	    }
	}
```

####15、 multiple unary operator         
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/MultipleUnaryOperatorRule.cpp)
> Multiple unary operator can always be confusing and should be simplified.

>简单解释：多重的运算符不易理解，应该简化。

```
	void example() {
	    int b = -(+(!(~1)));
	}
```

####16、return from finally block           
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/basic/ReturnFromFinallyBlockRule.cpp)
> Returning from a finally block is not recommended.

>简单解释：不建议在finally中返回

```
	void example() {
	    @try {
	        foo();
	    }
	    @catch(id ex) {
	        bar();
	    }
	    @finally {
	        return;         // this can discard exceptions.
	    }
	}
```

####17、throw exception from finally block               
&#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](--)
> Throwing exceptions within a finally block may mask other exceptions or code defects.

>简单解释：从finally块抛出异常，可能忽略其他的错误

```
	void example() {
	    @try {;}
	    @catch(id ex) {;}
	    @finally {
	        id ex1;
	        @throw ex1;                              // this throws an exception
	        NSException *ex2 = [NSException new];
	        [ex2 raise];                             // this throws an exception, too
	    }
	}
```


