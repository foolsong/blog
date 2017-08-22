##<center>OCLint的部分规则（Design 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：



####1、avoid default arguments on virtual methods
 
&#160; &#160; &#160;  `Since:0.10.1` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/design/AvoidDefaultArgumentsOnVirtualMethodsRule.cpp)
> Giving virtual functions default argument initializers tends to defeat polymorphism and introduce unnecessary com- plexity into a class hierarchy.

>简单解释：避免给虚函数设置默认参数，给虚函数设置默认参数会破坏多样性和引起不必要的层次结构发杂性。

```
	class Foo
	{
	public:
	    virtual ~Foo();
	    virtual void a(int b = 3);
	    // ...
	};
	class Bar : public Foo
	{
	public:
	    void a(int b);
	    // ...
	};
	Bar *bar = new Bar;
	Foo *foo = bar;
	foo->a();   // default of 3
	bar->a();   // compile time error!
```

####2、avoid private static members 

&#160; &#160; &#160;  `Since:0.10.1` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/design/AvoidPrivateStaticMembersRule.cpp)
> Having static members is easier to harm encapsulation.

>简单解释：避免使用私有静态成员，静态成员很容易破换封装性

```
	class Foo {
	    static int a;       // static field
	};
	class Bar {
	    static int b();     // static method
	}
```
