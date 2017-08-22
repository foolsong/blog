##<center>OCLint的部分规则（Convention 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：


####1、avoid branching statement as last in loop        
 &#160; &#160; &#160;  `Since:0.7` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/AvoidBranchingStatementAsLastInLoopRule.cpp)
> Having branching statement as the last statement inside a loop is very confusing, and could largely be forgetting of something and turning into a bug.

>简单解释：不要再循环最后加入分支，负责理解起来比较困难，很大程度上会遗忘一些事情，导致一些错误。

```
	void example() {
	    for (int i = 0; i < 10; i++) {
	        if (foo(i)) {
	            continue;
	        }
	        break;      // this break is confusing
	    }
	}
```

####2、base class destructor should be virtual or protected         
&#160; &#160; &#160;  `Since:0.10.2` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/BaseClassDestructorShouldBeVirtualOrProtectedRule.cpp)
> Make base class destructor public and virtual, or protected and nonvirtual

>简单解释：基类的析构函数需要是`public`、`virtual`或者`protected``nonvirtual`。

```
	class Base {
	public:
	    ~Base(); // this should be either protected or virtual
	}
	class C : public Base {
	    virtual ~C();
	}
```
Sutter & Alexandrescu (November 2004). [`“C++ Coding Standards: 101 Rules, Guidelines, and Best Practices”`(http://gotw.ca/publications/c++cs.htm)]. Addison-Wesley Professional

####3、unnecessary default statement in covered switch statement         
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/CoveredSwitchStatementsDontNeedDefaultRule.cpp)
> When a switch statement covers all possible cases, a default label is not needed and should be removed. If the switch is not fully covered, the SwitchStatements Should Have Default rule will report.

>简单解释：如果`switch`覆盖了所有的条件，`default`是不需要的应该被移除。如果不是`default`还是需要的。

```
	typedef enum {
	    value1 = 0,
	    value2 = 1
	} eValues;
	//
	void aMethod(eValues a)
	{
	    switch(a)
	    {
	        case value1:
	            break;
	        case value2:
	            break;
	        default:          // this break is obsolete because all
	            break;        // values of variable a are already covered.
	    }
	}
```

####4、 ill-placed default label in switch statement
 &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/DefaultLabelNotLastInSwitchStatementRule.cpp)
> It is very confusing when default label is not the last label in a switch statement.

>简单解释：`default`应该在`switch`的最后，负责会很难理解。

```
	void example(int a) {
	    switch (a) {
	        case 1:
	            break;
	        default:  // the default case should be last
	            break;
	        case 2:
	            break;
	    }
	}
```

####5、destructor of virtual class          
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/DestructorOfVirtualClassRule.cpp)
> This rule enforces the destructor of a virtual class must be virtual.

>简单解释：这个规则是虚拟类的析构函数必须是虚拟的。

```
	class Base { // class Base should have a virtual destructor ~Base()
	    public: virtual void f();
	};
	class Child : public Base {
	    public: ~Child();  // destructor ~Child() should be virtual
	};
```

####6、inverted logic           
 &#160; &#160; &#160;  `Since:0.4` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/InvertedLogicRule.cpp)
> An inverted logic is hard to understand.

>简单解释：倒置逻辑不易理解。

```
	int example(int a) {
	    int i;
	    if (a != 0)             // if (a == 0)
	    {                       // {
	        i = 1;              //      i = 0;
	    }                       // }
	    else                    // else
	    {                       // {
	        i = 0;              //      i = 1;
	    }                       // }
	    return !i ? -1 : 1;     // return i ? 1 : -1;
	}
```
> <font color=red>PS：</font>在做判断的时候，应该先做的`是`判断，在做`非`的判断。做个一个测试如果只有非的测试是不会有警告的。

```
	int example(int condition) {
	    int temp;
	    if (acondition!= 0)            //不会有警告
	    {                       
	        temp = 1;            
	    }                  
	}
```

####7、missing break in switch statement           
 &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/MissingBreakInSwitchStatementRule.cpp)
> A switch statement without a break statement has a very large chance to contribute a bug.

>简单解释：在`switch`语句中缺失了`break`，很有可能引发`bug`。

```
	void example(int a) {
	    switch (a) {
	        case 1:
	            break;
	        case 2:
	            // do something
	        default:
	            break;
	    }
	}
```

####8、non case label in switch statement              &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/NonCaseLabelInSwitchStatementRule.cpp)
> It is very confusing when label becomes part of the switch statement.

>简单解释：`label`出现在`switch`条件中不易理解。

```
	void example(int a) {
	    switch (a) {
	        case 1:
	            break;
	        label1:     // label in a switch statement in really confusing
	            break;
	        default:
	            break;
	    }
	}
```

####9、 ivar assignment outside accessors or init      &#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/ObjCAssignIvarOutsideAccessorsRule.cpp)
> This rule prevents assigning an ivar outside of getters, setters, and init method.

>简单解释：检查某些成员的初始化不再`getters`、` setters` and` init method`中。

```
	@interface Foo : NSObject {
	    int _bar;
	}
	@property (assign, nonatomic) int bar;
	@end
	@implementation Foo
	@synthesize bar = _bar;
	- (void)doSomething {
	    _bar = 3; // access _bar outside its getter, setter or init
	}
	@end
```

> <font color=red>PS：</font>简单和小伙伴讨论了下，感觉iOS开发并不是很适合。

####10、parameter reassignment             &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/ParameterReassignmentRule.cpp)
> Reassigning values to parameters is very problematic in most cases.

>简单解释：对参数进行重新赋值，在很多情况下是有问题的。

```
	void example(int a) {
	    if (a < 0) {
	        a = 0; // reassign parameter a to 0
	    }
	}
```

> <font color=red>PS：</font>简单测试了传值时使用指针，经测试不会有警告。也就是说不会对指针类型的参数做检查。

####11、prefer early exits and continue       &#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/PreferEarlyExitRule.cpp)
> Early exits can reduce the indentation of a block of code, so that reader do not have to remember all the previous decisions, therefore, makes it easier to understand the code.

>简单解释：在有退出语句 的时候，应该让退出语句靠前，这样阅读代码使代码可以很好的被理解。

```
	int *doSomething(int a) {
	  if (!foo(a) && bar(a) && doOtherThing(a)) {
	    // ... some really long code ....
	  }
	  return 0;
	}
	// is preferred as
	int *doSomething(int a) {
	  if (foo(a)) {
	    return 0;
	  }
	  if (!bar(a)) {
	    return 0;
	  }
	  if (!doOtherThing(a)) {
	    return 0;
	  }
	  // ... some long code ....
	}
```

####12、 missing default in switch statements           &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/SwitchStatementsShouldHaveDefaultRule.cpp)
> Switch statements should have a default statement.

>简单解释：检查 `Switch`中`default`缺失的情况。

```
	void example(int a) {
	    switch (a) {
	        case 1:
	            break;
	        case 2:
	            break;
	        // should have a default
	    }
	}
```

####13、 too few branches in switch statement             &#160; &#160; &#160;  `Since:0.6` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/convention/TooFewBranchesInSwitchStatementRule.cpp)
> To increase code readability, when a switch consists of only a few branches, it’s much better to use an if statement instead.

>简单解释：如果`switch`语句条件很少，可以一用`if else` 代替。

```
	void example(int a) {
	    switch (a) {
	        case 1:
	            break;
	        default:
	            break;
	    } // Better to use an if statement and check if variable a equals 1.
	}
```
>Thresholds:
MINIMUM_CASES_IN_SWITCH The reporting threshold for count of case statements in a switch statement, de-
fault value is 3.

