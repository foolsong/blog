# EasyCharts

<!--![](http://osz3uubsl.bkt.clouddn.com/ec180@3x.png?imageMogr2/thumbnail/!70p) -->

<center> ![](http://osz3uubsl.bkt.clouddn.com/ec_blog_9_14.png?imageMogr2/thumbnail/!70p) </center>


一个简单可方便快捷画出折线图、饼图、进度条、柱状图、雷达图的iOS库（Objective-C版本）。新库刚开始维护，希望大家多多支持，可[issue](https://github.com/foolsong/EasyCharts/issues)、[`pull request`](https://github.com/foolsong/EasyCharts)、[`find bug`](https://github.com/foolsong/EasyCharts/issues)、[`feature request`](https://github.com/foolsong/EasyCharts/issues) 。[GitHub地址](https://github.com/foolsong/EasyCharts)。

<font size=3 color=red>还有别忘了 star</font>  <font size=4 >:-)</font> 

## Features
 -  BrokenLineChart (折线图)
 -  PieChart （圆饼图）
 -  ProgressChart  （进度条）
 -  BarGraph  （柱状图->开发中）
 -  RadarMap （雷达图->开发中）

**持续更行中……**

## Installation

目前

* 下载源代码，将`EasyCharts`copy进项目

<!-- EasyCharts supports multiple methods for installing the library in a project.
* using CocoaPods
* using Carthage -->



<!--## How to use-->

## Usage

### 导入头文件 
`#import "EasyCharts.h"`

下面介绍一下几种图的基本使用

### BrokenLineChart 

   BrokenLineChart目前有两种类型`BrokenLineTypeCenterPoint`和`BrokenLineTypeNormal`。
   >  `BrokenLineTypeNormal`是普通的折线图 \
  >  `BrokenLineTypeCenterPoint`选中的点始终居中
  
   创建时除了`BrokenLineType`还有两个参数，一个是`frame`，另一个是`ECBrokenLineConfig`(参数如下)对象。
   
   > `ECBrokenLineConfig`可配置折线图的属性，属性都有默认值。当然可以传`nil`,全部被使用默认值
   
```Objective-c
@property (nonatomic, strong) UIColor *brokenLineColor;
@property (nonatomic, strong) UIColor *backVeiwLineColor;
@property (nonatomic, strong) UIColor *backVeiwTextColor;
@property (nonatomic, strong) UIColor *backVeiwBackGroupColor;
@property (nonatomic, strong) UIColor *brokenAbscissaColor;

@property (nonatomic, assign) CGFloat minValue;  //default 0
@property (nonatomic, assign) CGFloat maxValue;  //default 100

@property (nonatomic, assign) CGFloat numberOfIntervalLines; //default 5
@property (nonatomic, assign) BrokenLineType brokenLineType;
```
   
   delegate,在当前折线图上的点被点击时调用：
   
```Objective-c
- (void)brokenLineView:(ECBrokenLineView *)brokenLineView
   selectedAtIndexPath:(NSIndexPath *)indexPath;
```

初始化：

```Objective-C
 ECBrokenLineView *brokenLineView = [ECBrokenLineView lineViewWithFrame:frame
                                                      withBrokenLineConfig:nil
                                                            brokenLineType:BrokenLineTypeNormal];
    brokenLineView.delegate = self;
    [self.view addSubview:brokenLineView];
```

```Objective-C
 ECBrokenLineView *brokenLineView = [ECBrokenLineView lineViewWithFrame:frame
                                                      withBrokenLineConfig:nil
                                                            brokenLineType:BrokenLineTypeCenterPoint];
    brokenLineView.delegate = self;
    [self.view addSubview:brokenLineView];
```

填充数据（也可刷新页面数据）（**数值小于最小值，则值显示为最小值，点两边为虚线。值大于最大值，则显示最大值，点两边为虚线**）：

```Objective-C
[self.brokenLineView reloadLineViewDataWithPointValveList:self.pointValveList
                                                    titleText:self.pointTextList];
```													

**BrokenLineTypeNormal 效果图:**

<center>
<img src="http://osz3uubsl.bkt.clouddn.com/EC_lineNormal_gif.gif"  width=300 alt="EC_lineNormal" />
</center>

**BrokenLineTypeCenterPoint 效果图:**

<center>
<img src="http://osz3uubsl.bkt.clouddn.com/EC_lineCenterView_gif.gif"  width=300 alt="EC_lineCenterView" />
</center>



### ProgressView

初始化只需要传入`frame`

```Objective-c
ECProgressChartView *progressView = [ECProgressChartView progressChartViewWithFrame:frame];
    [self.view addSubview:progressView];
```

填充数据（也可刷新页面数据）；

```Objective-c
[self.progressView resetProgress:[self createProgress]];
```
**效果图:**
<center>
<img src="http://osz3uubsl.bkt.clouddn.com/EC_ ProgressView_gif.gif"  width=300 alt="EC_lineCenterView" />
</center>

### BrokenLineChart && ProgressView
简单看一下应用场景：
<center>
<img src="http://osz3uubsl.bkt.clouddn.com/EC_BrokenLineChart_ProgressView.gif"  width=300 alt="EC_lineCenterView" />
</center>

### PieChart

初始化只需要传入`frame`

```Objective-c
 ECPieChartView *pieView = [ECPieChartView pieChartViewWithFrame:CGRectMake(0, 100, ECScreenW, 200)];
    [self.view addSubview:pieView];
```

填充数据（也可刷新页面数据）；三个参数，分别是百分比列表、颜色列表、文案列表

```Objective-c
[pieView drawPieChartWithPercentList:self.percentList
                               colorList:self.colorList
                             arcTextList:self.arcTextList];
```

**效果图:**
<center>
<img src="http://osz3uubsl.bkt.clouddn.com/EC_ PieChart_gif.gif"  width=300 alt="EC_lineCenterView" />
</center>



## Communication

* 如果在使用过程中遇到BUG，希望你能Issues我（最好提供复现步骤），谢谢
* 如果在使用过程中发现功能不够用，希望你能Issues我
* 如果你想为`EasyCharts`输出代码，请拼命`Pull Requests`我