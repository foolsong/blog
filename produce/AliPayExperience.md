## <center> 大话 AliPay踩的坑 </center>
做三方集成的时候按照官方的步骤进行集成基本不会出现太多问题。最近在做App集成支付，主要就是微信支付、支付宝。在集成支付宝支付的时候还是遇到一些坑，简单分享一下：

###RSA和RSA2签名算法
开始对这两个签名算法不是很了解，混淆了两种算法。Alipay提供的工具没有进行区分RSA和RSA2，我误认为密钥长度2048的是RSA2。导致密钥使用错误。
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_12_git_AiliPay_01.png" width = "400" alt="图片名称" align=center />
</center>

现在简单介绍一下：

**关于签名算法的区别：**

|开放平台签名算法名称|标准签名算法名称|备注|
|:----:|:-----:|:-----:|
|RSA2|SHA256WithRSA|强制要求RSA密钥的长度至少为2048|
|RSA|SHA1WithRSA|对RSA密钥的长度不限制，推荐使用2048位以上是|

**如下是使用OpenSSL工具生成的密钥（MAC下应该有内置的OpenSSL）：**

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_17_ali_pay_01.png" width = "600" alt="图片名称" align=center />
</center>

	OpenSSL> genrsa -out app_private_key.pem   2048  #生成私钥
	OpenSSL> pkcs8 -topk8 -inform PEM -in app_private_key.pem -outform PEM -nocrypt -out app_private_key_pkcs8.pem #Java开发者需要将私钥转换成PKCS8格式
	OpenSSL> rsa -in app_private_key.pem -pubout -out app_public_key.pem #生成公钥
	OpenSSL> exit #退出OpenSSL程序

在Finder中查看：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_17_ali_pay_02.jpg" width = "300" alt="图片名称" align=center />
</center>

目前生成RSA2的方法貌似只能通过代码PHP或者JAVA。

<font size=5 color=red> 注意：</font>Alipay提供的工具产生的密钥是`txt`格式的，使用`OpenSSL`生成的是`pem`格式的（据了解之前版本的工具生成的也是`pem`格式）。官方建议使用提供的工具生成，但是从实践上看还是直接生成`pem`格式的文件（我们后台使用的是PHP）。当然目前提供的方案还是只能使用`OpenSSL`生成。

关于数字签名推荐两篇文章:
 [What is a Digital Signature?](http://www.youdzone.com/signature.html).
 中文翻译的可以参考：[数字签名是什么？](http://www.ruanyifeng.com/blog/2011/08/what_is_a_digital_signature.html)
 
###AlipayDemo 密钥使用错误
&#160; &#160; &#160;在账号配置完成之后，为了测试账号是否可以正常的支付，使用官方Demo进行账号测试。官方的Demo只要设置`appID`和`privateKey`(**真实App里，privateKey等数据严禁放在客户端，加签过程务必要放在服务端完成；防止商户私密数据泄露，造成不必要的资金损失，及面临各种安全风险；**)就可以使用。

&#160; &#160; &#160; 在Alipay提供的工具和`OpenSSL`生成中都可以看到`PKCS8`格式（[wiiki-PKCS8](https://en.wikipedia.org/wiki/PKCS_8)、[wiiki-PKCS](https://en.wikipedia.org/wiki/PKCS)）。JAVA需`PKCS8`格式，其他语言直接使用生成的私钥就可以了。

<font color=red size=5>但是，</font>在Demo中直接使用生成的密钥，签名时签名结果为`nil`。但是换成`PKCS8`时可以成功。但是后台（PHP）还是需要使用直接生成的密钥。在Demo上可以签名成功并成功支付之后，就可以通过后台签名，基本就可以跑通了，但是又遇到坑了…………

###签名数据出错
&#160; &#160; &#160;为了快速验证后台加签是否正确，和后台的同学商议，可以先拿到签名结果，在App里直接进行测试，通过之后直接就可以走接口了。结果还是出了问题：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_17_ali_pay_03.jpg" width = "300" alt="图片名称" align=center />
</center>
显示：`系统繁忙，请稍后再试。（ALI40247）`。

最后的最后发现发现了问题。我们的后台可以直接使用浏览器进行接口的数据请求，为了快速验证加签结果，就直接通过浏览器请求从页面上copy出加签结果进行调试。<font color=red>问题是：</font>浏览器把加签后的数据进行了部分转义，导致的结果就是App最后拿到的结果是错误的。

一个道理；别懒～

###PHP、SDK版本问题
&#160; &#160; &#160;惊喜的是在调试通过之后，放到仿真环境上的时候又有问题了。。。。。。<font color=red>后台验签失败</font>

&#160; &#160; &#160;后台童鞋的PHP版本是PHP7，仿真环境的PHP使用的PHP5.6版本。
在本地test环境调试时，Alipay的公钥直接在Alipay的网站上直接copy到`pem`文件中。PHP7可以直接读的到。但是PHP5.6不支持复制生成的pem公钥格式，导致验签失败。
基于目前的后台开发环境，PHP端的异步回调必须使用ali-php-sdk中自带的阿里公钥，不能使用开放平台中获取的公钥。


## <center> 微信支付 踩的坑 </center>


&#160; &#160; &#160;在集成微信支付的过程中处理申请阶段比较繁琐，在集成的时候还好，没有踩到太多的坑。
###端口问题
唯一的坑是关于端口。在和服务器同学本地连调的时候，支付完成回到我们的App的时候，拿不到支付成功的信息，原因还是<font color=red>后台验签失败</font>。
微信支付用于安全问题，仅会想80端口的服务器发送回调信息，基于ngrok之类的内网穿透工具搭建的本地服务均无效。
&#160; &#160; &#160;最后大神指点，微信在回调的要走<font color=red>80端口</font>。本地走的不是`80端口`，在上到仿真环境的时候就不会有问题了。

###基本配置
还有个小问题就是在支付的时候报个错：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_18_wx_pay_01.jpg" width = "300" alt="图片名称" align=center />
</center>

其实这个问题还是很简单的就是在配置的时候出现了问题，有个关键的key没有配对。

