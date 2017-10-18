##<center>UIButton小技巧----点击事件时间间隔</center>

###起因
&#160; &#160; &#160; 在开发过程中对于UIbutton的点击事件，如果进行频繁的点击，可能会造成事件的不必要的重复执行事件，甚至造成不必要的错误。

###解决方案
&#160; &#160; &#160;通过添加Category，重写`sendAction:to:forEvent:`方法。(通过`runtime`交换系统的`sendAction:to:forEvent:` 和 自定义的`customSendAction:to:forEvent:`方法，在定义方法中进行时间判断是否符合时间间隔需求，不符合直接`return`否则在调用系统的`sendAction:to:forEvent:`)。

###代码

```Objective-c
#import <UIKit/UIKit.h>

@interface UIButton (TimeInterval)

@property NSTimeInterval repeatEventInterval;

@end
```

```Objective-c
#import "UIButton+TimeInterval.h"
#import <objc/runtime.h>

const char *repeatEventIntervalKey  = "repeatEventIntervalKey";
const char *previousClickTimeKey = "previousClickTimeKey";

@implementation UIButton (TimeInterval)

+ (void)load {
    Method sendAction = class_getInstanceMethod([self class], @selector(sendAction:to:forEvent:));
    Method customSendAction = class_getInstanceMethod([self class], @selector(customSendAction:to:forEvent:));
    
    method_exchangeImplementations(sendAction, customSendAction);
}

- (void)sendAction:(SEL)action to:(id)target forEvent:(UIEvent *)event {
    [super sendAction:action to:target forEvent:event];
}

- (void)setRepeatEventInterval:(NSTimeInterval)repeatEventInterval {
    objc_setAssociatedObject(self, repeatEventIntervalKey, @(repeatEventInterval), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

- (NSTimeInterval)repeatEventInterval {
    return (NSTimeInterval)[objc_getAssociatedObject(self, repeatEventIntervalKey) doubleValue];
}

- (void)setPreviousClickTime:(NSTimeInterval)previousClickTime {
    objc_setAssociatedObject(self, previousClickTimeKey, @(previousClickTime), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

- (NSTimeInterval)previousClickTime {
    return [objc_getAssociatedObject(self, previousClickTimeKey) doubleValue];
}

- (void)customSendAction:(SEL)action to:(id)target forEvent:(UIEvent *)event {
    if ( NSDate.date.timeIntervalSince1970 - self.previousClickTime < self.repeatEventInterval ) {
        return;
    }
    
    if (self.repeatEventInterval > 0) {
        self.previousClickTime = NSDate.date.timeIntervalSince1970 ;
    }
    
    [self customSendAction:action to:target forEvent:event];
}

@end
```

###应用

```Objective-c
#import "UIButton+TimeInterval.h"

button.repeatEventInterval = 2;
```