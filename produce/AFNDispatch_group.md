##<center>dispatch_group实践，AFN3.0多个网络请求</center>

&#160; &#160; &#160; 在实际开发过程中经常会有在一个页面有多个网络请求，页面UI需要在所有网络请求全部回来的情况下更新。先简单看一个具体的问题：

> 页面有三个网络请求a、b、c。三个网络请求都成功的情况下刷新UI，有一个失败就认为请求失败，不进行UI刷新。

目前想到的方法

 *  添加`标识`进行判断是否所有的网络请求都是成功的
 * 通过`信号量`判断
 * 直接使用`dispatch_group`（当然也可以结合`信号量`）

这里就简单使用`dispatch_group`来实践一下：

###先简单了解一下必须的东西
<br/> <br/>

####常用方法

>  **1、`dispatch_group_create`  创建一个调度任务组**

>  **2、`dispatch_group_async` 把一个任务异步提交到任务组里**

>  **3、`dispatch_group_enter/dispatch_group_leave` 这种方式用在不使用dispatch_group_async来提交任务，且必须配合使用**

>  **4、`dispatch_group_notify` 用来监听任务组事件的执行完毕**

>  **5、`dispatch_group_wait` 设置等待时间**


<br/> <br/>

####实现一个`dispatch_group_async` 简单的例子
&#160; &#160; &#160; 基本需求和文章前面的需求差不多，`task 1`、`task 2`、`task 3`、`task 4`四个任务，`task 4`要在`task 1`、`task 2`、`task 3`执行完成之后在执行。
先看一下代码：

```Objective-C

	- (void)testDisGroup {
	    dispatch_queue_t demoQuene = dispatch_queue_create("demoQuene", DISPATCH_QUEUE_CONCURRENT);
	    dispatch_group_t group = dispatch_group_create();
    
	    NSLog(@"\n\nmain thread %@\n\n",[NSThread currentThread]);
    
	    dispatch_group_async(group, demoQuene, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 1  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_async(group, demoQuene, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 2  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_async(group, demoQuene, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 3  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
	        NSLog(@"task 4  %@",[NSThread currentThread]);
	    });
	}
```

看一下结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_AFN_GCD_8_24_01.jpg" width = "600" alt="图片名称" align=center />
</center>

当然`task 1`、`task 2`、`task 3`完成的顺序不是固定，最后是`task 4`，需要看系统怎么分配资源。

<br/> <br/>

####升级一下`dispatch_group_async` 的例子
在上面任务的基础上添加一个需求，就是`task 2`、`task 3`要有一个顺序，比如`task 2`要在`task 3`执行完成之后才能执行。
看一下代码（当然实现的方案很多，这只是个例子）：

```Objective-C

	- (void)testDisGroup {
	    dispatch_queue_t demoQueneCONCURRENT = dispatch_queue_create("demoQuene", DISPATCH_QUEUE_CONCURRENT);
	    dispatch_queue_t demoQueneSERIAL = dispatch_queue_create("demoQuene", DISPATCH_QUEUE_SERIAL);
	    dispatch_group_t group = dispatch_group_create();
    
	    NSLog(@"\n\nmain thread %@\n\n",[NSThread currentThread]);
    
	    dispatch_group_async(group, demoQueneCONCURRENT, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 1  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_async(group, demoQueneSERIAL, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 3  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_async(group, demoQueneSERIAL, ^{
	        sleep(arc4random_uniform(5));
	        NSLog(@"task 2  %@",[NSThread currentThread]);
	    });
    
	    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
	        NSLog(@"task 4  %@",[NSThread currentThread]);
	    });
	}
```

执行结果:

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_AFN_GCD_8_24_02.jpg" width = "600" alt="图片名称" align=center />
</center>

另一个结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_AFN_GCD_8_24_03.jpg" width = "600" alt="图片名称" align=center />
</center>

当然这个结果也存在多种情况，不变的是`task 4`要在`task 1`、`task 2`、`task 3`执行完成之后在执行，`task 2`要在`task 3`执行完成之后才能执行。其他的顺序是不确定的。

<br/><br/>
####使用`dispatch_group_enter/dispatch_group_leave` 复写一下第一个例子

```Objective-C

	-(void)testDisGroupEnterAndLeave {
	    dispatch_queue_t demoQueneCONCURRENT = dispatch_queue_create("demoQuene2", DISPATCH_QUEUE_CONCURRENT);
	    dispatch_group_t group = dispatch_group_create();
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneCONCURRENT, ^{
	        NSLog(@"task 1  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneCONCURRENT, ^{
	        NSLog(@"task 2  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneCONCURRENT, ^{
	        NSLog(@"task 3  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
	        NSLog(@"task 4  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	    });
	}
```

运行结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_AFN_GCD_8_24_04.jpg" width = "600" alt="图片名称" align=center />
</center>

另一结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/image/blogblog_AFN_GCD_8_24_05.jpg" width = "600" alt="图片名称" align=center />
</center>

<br/><br/>
####使用`dispatch_group_enter/dispatch_group_leave` 复写一下第二个例子
上代码：

```Objective-C

	-(void)testDisGroupEnterAndLeave {
	    dispatch_queue_t demoQueneCONCURRENT = dispatch_queue_create("demoQuene2", DISPATCH_QUEUE_CONCURRENT);
	    dispatch_queue_t demoQueneSERIAL = dispatch_queue_create("demoQuene3", DISPATCH_QUEUE_SERIAL);
	    dispatch_group_t group = dispatch_group_create();
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneCONCURRENT, ^{
	        NSLog(@"task 1  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneSERIAL, ^{
	        NSLog(@"task 3  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_enter(group);
	    dispatch_async(demoQueneSERIAL, ^{
	        NSLog(@"task 2  %@",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	        dispatch_group_leave(group);
	    });
    
	    dispatch_group_notify(group, dispatch_get_main_queue(), ^{
	        NSLog(@"task 4  %@ \n\n\n.",[NSThread currentThread]);
	        sleep(arc4random_uniform(5));
	    });
	}
```

结果（两次结果写在一起了）：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/image/blog/blog_AFN_GCD_8_24_06.jpg" width = "600" alt="图片名称" align=center />
</center>



<br/><br/>
###开始实战
&#160; &#160; &#160; 还是文章开始的那道题现在看一下
> 页面有三个网络请求a、b、c。三个网络请求都成功的情况下刷新UI，有一个失败就认为请求失败，不进行UI刷新。

```Objective-C

	@interface TestThread () {
	    dispatch_group_t _group;
	}

	@end

	@implementation TestThread

	- (void)p_loadBannerData {
	    dispatch_group_enter(_group);
	   AdvBannerApi *bannerApi = [[AdvBannerApi alloc] initWithAdvType:AdvTypeBanner];
	    [bannerApi startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 1 success");
	        dispatch_group_leave(_group);
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 1 failure");
	        dispatch_group_leave(_group);
	    }];
	}

	- (void)p_loadCourseListData {
	    dispatch_group_enter(_group);
	    QualityCourseApi *courseApi = [[QualityCourseApi alloc] initWithStart:@"0"
	                                                                    count:@"20"];
	    [courseApi startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 2 success");
	        dispatch_group_leave(_group);
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 2 failure");
	        dispatch_group_leave(_group);
	    }];
	}

	- (void)p_loadLivingLessonData {
	    dispatch_group_enter(_group);
	    LessonForecastApi *api = [[LessonForecastApi alloc] init];
	    [api startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 3 success");
	        dispatch_group_leave(_group);
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 3 failure");
	        dispatch_group_leave(_group);
	    }];
	}

	- (void)reloadUI {
	    NSLog(@"task 4");
	}

	-(void)disGroupEnterAndLeave {
	    _group = dispatch_group_create();

	    [self p_loadBannerData];
	    [self p_loadCourseListData];
	    [self p_loadLivingLessonData];
    
	    dispatch_group_notify(_group, dispatch_get_main_queue(), ^{
	        NSLog(@"run task 4 %@",[NSThread currentThread]);
	        [self reloadUI];
	    });
	}

	@end
```

看一下结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/image/blog/blog_AFN_GCD_8_24_07.jpg" width = "600" alt="图片名称" align=center />
</center>

<br/>
**其他的写法就一一实现了。**现在看一下<font color=red size=5>错误</font>的写法

```Objective-C

	@interface TestThread () {
	    dispatch_group_t _group;
	}

	@end

	@implementation TestThread

	- (void)p_loadBannerData {
	   AdvBannerApi *bannerApi = [[AdvBannerApi alloc] initWithAdvType:AdvTypeBanner];
	    [bannerApi startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 1 success");
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 1 failure");
	    }];
	}

	- (void)p_loadCourseListData {
	    QualityCourseApi *courseApi = [[QualityCourseApi alloc] initWithStart:@"0"
	                                                                    count:@"20"];
	    [courseApi startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 2 success");
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 2 failure");
	    }];
	}

	- (void)p_loadLivingLessonData {
	    LessonForecastApi *api = [[LessonForecastApi alloc] init];
	    [api startWithSuccessBlock:^(ApiResult * _Nullable result) {
	        NSLog(@"task 3 success");
	    } failureBlock:^(NSError * _Nullable error) {
	        NSLog(@"task 3 failure");
	    }];
	}

	- (void)reloadUI {
	    NSLog(@"task 4");
	}

	-(void)disGroup {
	    dispatch_queue_t demoQueneCONCURRENT = dispatch_queue_create("demoQuene", DISPATCH_QUEUE_CONCURRENT);
	    _group = dispatch_group_create();

	    dispatch_group_async(_group, demoQueneCONCURRENT, ^{
	        [self p_loadBannerData];
	    });
    
	    dispatch_group_async(_group, demoQueneCONCURRENT, ^{
	        [self p_loadCourseListData];
	    });
    
	    dispatch_group_async(_group, demoQueneCONCURRENT, ^{
	        [self p_loadLivingLessonData];
	    });
    
	    dispatch_group_notify(_group, dispatch_get_main_queue(), ^{
	        NSLog(@"run task 4 %@",[NSThread currentThread]);
	        [self reloadUI];
	    });
	}

	@end
```

看一下结果：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/image/blog/blog_AFN_GCD_8_24_08.jpg" width = "600" alt="图片名称" align=center />
</center>
<br/>
&#160; &#160; &#160; 很显然结果是错误的,`task 4`最先得出了结果，主要因为

```Objective-C

    dispatch_group_async(_group, demoQueneCONCURRENT, ^{
    });
```
开启了一个新线程，然后

```Objective-C
		        [self p_loadBannerData];
```

网络请求又开启了一个线程。

```Objective-C

    dispatch_group_async(_group, demoQueneCONCURRENT, ^{
    });
```

开启的3个新线程执行完之后执行`task 4`。网络耗时的操作才执行。然后就………………错了。





