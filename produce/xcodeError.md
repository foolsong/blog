##iOS & Xcode 常见问题整理(持续更新……)

使用iOS & Xcode的时候经常遇到奇奇怪怪的问题，在这整理一下方便查询。


**1. This application's application-identifier entitlement does not match that of the installed application. These values must match for an upgrade to be allowed.**

<center>![](http://osz3uubsl.bkt.clouddn.com/blog_xcode_9_14.jpg?imageMogr2/thumbnail/!70p)</center>

解决方案：
> 1、Xcode－Window->Devices

> 2、选中你的设备，在右边的installed Apps中删除这个App

> 3、重新编绎即可

<center>![](http://osz3uubsl.bkt.clouddn.com/blog_xcode_9_14_02.png?imageMogr2/thumbnail/!70p)</center>

**2. Xcode has encountered an unexpected error (0xC01C)**

<center>![](http://osz3uubsl.bkt.clouddn.com/blog_xcode_9_14_03.jpg?imageMogr2/thumbnail/!70p)</center>

解决方案：

>  重启Xcode

> 拔线重插

> clean之后运行  

**3.  Unable to simultaneously satisfy constraints.**

```
    [LayoutConstraints] Unable to simultaneously satisfy constraints.
   	Probably at least one of the constraints in the following list is one you don't want. 
   	Try this: 
   		(1) look at each constraint and try to figure out which you don't expect; 
   		(2) find the code that added the unwanted constraint or constraints and fix it.
```

```
	Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger.
	The methods in the UIConstraintBasedLayoutDebugging category on UIView listed in <UIKit/UIView.h> may also be helpful.
	2017-09-14 19:07:39.448008+0800 StudentLive_Sim[28985:4843290] [LayoutConstraints] Unable to simultaneously satisfy constraints.
		Probably at least one of the constraints in the following list is one you don't want. 
		Try this: 
			(1) look at each constraint and try to figure out which you don't expect; 
			(2) find the code that added the unwanted constraint or constraints and fix it.
```


解决方案： 
> 降低相关约束的优先级

例如(`Masonry`)：

```Objective-c
    [lineView mas_makeConstraints:^(MASConstraintMaker *make) {
           make.left.equalTo(@(MARGIN_COMMON));
           make.right.equalTo(@(- MARGIN_COMMON));
           make.bottom.equalTo(@(-ParameterFor(44, 45)));
           make.top.equalTo(self.dateLabel.mas_bottom).offset(ParameterFor(80, 85)).with.priority(999);
           make.height.equalTo(@(SINGLE_LINE_WIDTH)).with.priority(999);
       }];
```

**4.The use of Swift 3 @objc inference in Swift 4 mode is deprecated.**

> The use of Swift 3 @objc inference in Swift 4 mode is deprecated. Please address deprecated @objc inference warnings, test your code with “Use of deprecated Swift 3 @objc inference” logging enabled, and then disable inference by changing the "Swift 3 @objc Inference" build setting to "Default" for the "EasyChartsSwift" target.

<center>
	![](http://osz3uubsl.bkt.clouddn.com/blog_xcodeerror_9_21_01.jpg?imageMogr2/thumbnail/!70p)
</center>
