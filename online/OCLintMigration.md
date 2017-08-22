##<center>OCLint的部分规则（Migration 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：



####1、use boxed expression
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/migration/ObjCBoxedExpressionsRule.cpp)
> This rule locates the places that can be migrated to the new Objective-C literals with boxed expressions.

>简单解释：建议使用新方法，快速创建（`numberWithInt`和`stringWithUTF8String:`）。

```
	void aMethod() {
	    NSNumber *fortyTwo = [NSNumber numberWithInt:(43 - 1)];
	    // NSNumber *fortyTwo = @(43 - 1);
	    NSString *env = [NSString stringWithUTF8String:getenv("PATH")];
	    // NSString *env = @(getenv("PATH"));
	}
```

####2、 use container literal
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/migration/ObjCContainerLiteralsRule.cpp)
> This rule locates the places that can be migrated to the new Objective-C literals with container literals.

>简单解释：建议使用新方法，快速创建（`arrayWithObjects`和`dictionaryWithObjects`）。

```
	void aMethod()
	{
	    NSArray *a = [NSArray arrayWithObjects:@1, @2, @3, nil];
	    // NSArray *a = @[ @1, @2, @3 ];
	    NSDictionary *d = [NSDictionary dictionaryWithObjects:@[@2,@4] forKeys:@[@1,@3]];
	    // NSDictionary *d = @{ @1 : @2, @3 : @4 };
	}
```

####3、use number literal
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/migration/ObjCNSNumberLiteralsRule.cpp)
> This rule locates the places that can be migrated to the new Objective-C literals with number literals.

>简单解释：建议使用新方法，快速创建（`numberWithInt`和`numberWithBOOL`）。

```
	void aMethod() {
	    NSNumber *fortyTwo = [NSNumber numberWithInt:42];
	    // NSNumber *fortyTwo = @42;
	    NSNumber *yesBool = [NSNumber numberWithBool:YES];
	    // NSNumber *yesBool = @YES;
	}
```

####4、use object subscripting
&#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/migration/ObjCObjectSubscriptingRule.cpp)
> This rule locates the places that can be migrated to the new Objective-C literals with object subscripting.

>简单解释：建议使用新方法，快速创建（`objectAtIndex`和`objectForKey`）。

```
	void aMethod(NSArray *a, NSDictionary *d) {
	    id item = [a objectAtIndex:0];
	    // id item = a[0];
	    id item = [d objectForKey:@1];
	    // id item = d[@1];
	}
```