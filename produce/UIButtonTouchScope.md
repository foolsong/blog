##<center>UIButton小技巧----点击事件的范围</center>

###起因
&#160; &#160; &#160; 在开发过程中对于UIbutton的点击事件，有时按钮太小不能被轻易点击到，希望放大点击的范围。

###解决方案
&#160; &#160; &#160;通过添加Category，重写`pointInside: withEvent:`方法，通过判断点击的位置是否在希望响应事件所在范围，在期望的范围内就响应事件，不再就不响应。

###代码

```Objective-C
#import <UIKit/UIKit.h>

@interface UIButton (TouchScope)

- (void)expandTouchScope:(UIEdgeInsets)edgeInsets;

@end
```

```Objective-C
#import "UIButton+TouchScope.h"
#import <objc/runtime.h>

static NSString *expandRectXKey = @"expandRectX";
static NSString *expandRectYKey = @"expandRectY";
static NSString *expandRectWidthKey = @"expandRectWidthKey";
static NSString *expandRectHeightKey = @"expandRectHeightKey";

@implementation UIButton (TouchScope)

- (void)expandTouchScope:(UIEdgeInsets)edgeInsets {
    objc_setAssociatedObject(self,
                             &expandRectYKey,
                             [NSNumber numberWithFloat:self.bounds.origin.y - edgeInsets.top],
                             OBJC_ASSOCIATION_COPY_NONATOMIC);
    objc_setAssociatedObject(self,
                             &expandRectXKey,
                             [NSNumber numberWithFloat:self.bounds.origin.x - edgeInsets.left],
                             OBJC_ASSOCIATION_COPY_NONATOMIC);
    objc_setAssociatedObject(self,
                             &expandRectWidthKey,
                             [NSNumber numberWithFloat:self.frame.size.width + edgeInsets.left + edgeInsets.right],
                             OBJC_ASSOCIATION_COPY_NONATOMIC);
    objc_setAssociatedObject(self,
                             &expandRectHeightKey,
                             [NSNumber numberWithFloat:self.frame.size.height + edgeInsets.top + edgeInsets.bottom],
                             OBJC_ASSOCIATION_COPY_NONATOMIC);
}

- (CGRect)expandRect {
    NSNumber *expandRectX = objc_getAssociatedObject(self, &expandRectXKey);
    NSNumber *expandRectY = objc_getAssociatedObject(self, &expandRectYKey);
    NSNumber *expandRectWidth = objc_getAssociatedObject(self, &expandRectWidthKey);
    NSNumber *expandRectHeight = objc_getAssociatedObject(self, &expandRectHeightKey);
    return CGRectMake(expandRectX.floatValue,
                      expandRectY.floatValue,
                      expandRectWidth.floatValue,
                      expandRectHeight.floatValue);
}

// 响应用户的点击事件
- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event {
    CGRect buttonRect = [self expandRect];
    if (CGRectEqualToRect(buttonRect, self.bounds)) {
        return [super pointInside:point withEvent:event];
    }
    return CGRectContainsPoint(buttonRect, point) ? YES : NO;
}

@end

```

###应用

```Objective-C
 [button expandTouchScope:UIEdgeInsetsMake(20, 20, 20, 20)];
```


![](http://osz3uubsl.bkt.clouddn.com/1508322671.png?imageMogr2/thumbnail/!70p)


红色区域为Button，蓝色区域是向外拓展的20的点击范围。设置完成在蓝色、红色范围内都可以响应事件。