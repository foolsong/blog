最近在做一点Jenkins的探索，想把OCLint的结果解析出来用钉钉的形式发送到钉钉群（[参考文档](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.B33DOI&treeId=257&articleId=105733&docType=1)），以便大家接受及时消息。

简单说一下大概的步骤：

## 创建一个机器人

<center>
	![](http://osz3uubsl.bkt.clouddn.com/blog_98_JenkinsR.png?imageMogr2/thumbnail/!70p)

</center>

按照下一步下一步就行了。
最后：

<center>
![](http://osz3uubsl.bkt.clouddn.com/blog_98_JenkinsR__02.jpg?imageMogr2/thumbnail/!70p)
</center>

获得到一个<font color=red  size=5> `webhook`</font>

## 发送消息

其实发送消息十分简单，官网写的比较详细了（[官方文档](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.karFPe&treeId=257&articleId=105735&docType=1)）。
但是官方的文档只有Java和PHP的。

但是。。。。。Java好久不用，PHP不会。。。。。。。

只能用`python`想办法，没有啥样的实例只能自己动手了：

```Python
	#!/usr/bin/python
	#coding=utf-8
	import urllib
	import urllib2
	import json
	import sys
	import socket

	reload(sys)
	sys.setdefaultencoding('utf8')

	# 获取钉钉消息
	def extractionMessage() :
	    #拼接需要发送的消息
	    return "##### <font color=orange> 钉钉message </font>"

	#发送钉钉消息
	def sendDingDingMessage(url, data):
	    req = urllib2.Request(url)
	    req.add_header("Content-Type", "application/json; charset=utf-8")
	    opener = urllib2.build_opener(urllib2.HTTPCookieProcessor())
	    response = opener.open(req, json.dumps(data))
	    return response.read()

	#主函数
	def main():
	    posturl = "https://oapi.dingtalk.com/robot/send?access_token=????????????????????????????"
	    data = {"msgtype": "markdown", "markdown": {"text": extractionMessage(),"title":"Jenkins","isAtAll": "false"}}
	    sendDingDingMessage(posturl, data)

	main()
```

具体解析OCLint的结果XML的代码和解析log的代码就不贴了，别忘了<font color=red  size=5> 把main()中的posturl换成自己的webhook地址 </font>就OK了。

附上一个结果,这里用的是`markdown`格式，其他格式参考官方文档：（[官方文档](https://open-doc.dingtalk.com/docs/doc.htm?spm=a219a.7629140.0.0.karFPe&treeId=257&articleId=105735&docType=1)）。

<center>
![](http://osz3uubsl.bkt.clouddn.com/blog_98_JenkinsR__03.png?imageMogr2/thumbnail/!70p)

</center>

