##iOS手机开热点适配问题

###热点问题
在手机打开热点(打电话)时状态栏高度由20变为40。导致页面整体下移，页面底部不做处理会导致部分页面内容不显示、遮挡。
###处理方案
* 方案1<br>
	使用自动布局，下边距距离固定。
  
* 方案2<br>
使用系统通知使用系统通知`UIApplicationWillChangeStatusBarFrameNotification`<br>
 可以获取状态栏frame变化通知
<code>CGRect newStatusBarFrame = [(NSValue*)[notification.userInfo objectForKey:UIApplicationStatusBarFrameUserInfoKey] CGRectValue];</code>

* 方案3<br>
个人热点后 状态栏会向下弹出多余的20个像素点，这是系统会调用VC 的
   `- (void)viewWillLayoutSubviews{}`<br> 
   函数`viewWillLayoutSubviews` 会在很多情况下调用,使用时应注意。
   
###注意
有两种情况

* 进入页面后状态栏发生变化
* 在进入页面之前状态栏已经发生变化


###其他
另外在Common.h中添加几个宏，以供大家使用



	// 标准系统状态栏高度
	#define SYS_STATUSBAR_HEIGHT          20
	// 热点栏高度
	#define HOTSPOT_STATUSBAR_HEIGHT          20
	// APP_STATUSBAR_HEIGHT=SYS_STATUSBAR_HEIGHT+[HOTSPOT_STATUSBAR_HEIGHT]
	#define APP_STATUSBAR_HEIGHT  (CGRectGetHeight([UIApplication sharedApplication].statusBarFrame))
	// 根据APP_STATUSBAR_HEIGHT判断是否存在热点栏
	#define IS_HOTSPOT_CONNECTED  (APP_STATUSBAR_HEIGHT==(SYS_STATUSBAR_HEIGHT+HOTSPOT_STATUSBAR_HEIGHT)?YES:NO)


