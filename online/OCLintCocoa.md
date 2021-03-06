##<center>OCLint的部分规则（CoCoa 部分）</center>
对OCLint的部分规则进行简单翻译解释，有部分进行了验证以及进一步分析、测试。OCLint其他相关内容如下：

####1、missing hash method        
 &#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/cocoa/ObjCVerifyIsEqualHashRule.cpp)
> When isEqual method is overridden, hash method must be overridden, too.

>简单解释：`isEqual` 方法被重写, `hash` 方法也应该被重写.

```
	@implementation BaseObject
	- (BOOL)isEqual:(id)obj {
	    return YES;
	}
	/*
	- (int)hash is missing; If you override isEqual you must override hash too.
	*/
	@end
```

####2、missing call to base method        
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/cocoa/ObjCVerifyMustCallSuperRule.cpp)
> When a method is declared with __attribute__((annotate("oclint:enforce[base method]"))) annotation, all of its implementations (including its own and its sub classes) must call the method implementation in super class.

>简单解释：当使用 `__attribute__((annotate("oclint:enforce[must call super]")))` 注解时, 他的所有实现（包括他自己和子类）都必须调用超类的实现

```
	@interface UIView (OCLintStaticChecks)
	- (void)layoutSubviews __attribute__((annotate("oclint:enforce[must call super]")));
	@end
	@interface CustomView : UIView
	@end
	@implementation CustomView
	- (void)layoutSubviews {
	    // [super layoutSubviews]; is enforced here
	}
	@end
```

####3、calling prohibited method     
&#160; &#160; &#160;  `Since:0.10.1` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/cocoa/ObjCVerifyProhibitedCallRule.cpp)
>  hen a method is declared with __attribute__((annotate("oclint:enforce[prohibited method]"))) annotation, all of its usages will be prohibited.

>简单解释：当一个方法标记` __attribute__((annotate("oclint:enforce[prohibited call]"))) `注解,所有的引用都将被禁止。

```
	@interface A : NSObject
	- (void)foo __attribute__((annotate("oclint:enforce[prohibited call]")));
	@end
	@implementation A
	- (void)foo {
	}
	- (void)bar {
	    [self foo]; // calling method `foo` is prohibited.
	}
	@end
```

####4、calling protected method 
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/cocoa/ObjCVerifyProtectedMethodRule.cpp)
> Even though there is no `protected` in Objective-C language level, in a design’s perspective, we sometimes hope to enforce a method only be used inside the class itself or by its subclass. This rule mimics the protected behavior, and alerts developers when a method is called outside its access scope.

>简单解释：在`Objective-C ` 中虽然没有`protected`这个概念。但是在设计的角度，我们有时希望一个方法只希望被它自己或者它的子类调用。这个方法可以模仿`protected`在有人调用的时候给一个警告。

```
	@interface A : NSObject
	- (void)foo __attribute__((annotate("oclint:enforce[protected method]")));
	@end
	@interface B : NSObject
	@property (strong, nonatomic) A* a;
	@end
	 //
	@implementation B
	- (void)bar {
	    [self.a foo]; // calling protected method foo from outside A and its subclasses
	}
	@end
```

####5、missing abstract method implementation         
&#160; &#160; &#160;  `Since:0.8` [定义类传送门～点击](https://github.com/oclint/oclint/blob/master/oclint-rules/rules/cocoa/ObjCVerifySubclassMustImplementRule.cpp)
> Due to the Objective-C language tries to postpone the decision makings to the runtime as much as possible, an abstract method is okay to be declared but without implementations. This rule tries to verify the subclass implement the correct abstract method.

>简单解释：由于`Objective`的`runtime`特性，抽象方法可以被声明，可以不实现，该规则验证子类是否正确实现抽象方法。

```
	@interface Parent
	//
	- (void)anAbstractMethod __attribute__((annotate("oclint:enforce[subclass must implement]")));
	//
	@end
	@interface Child : Parent
	@end
	//
	@implementation Child
	/*
	// Child, as a subclass of Parent, must implement anAbstractMethod
	- (void)anAbstractMethod {}
	*/
	@end
```
