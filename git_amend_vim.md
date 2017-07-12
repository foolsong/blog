#<center>git commit --amend error</center>
<font  size=3 face="黑体">
在项目中通过`git commit -m'modify message'`提交代码，发现提交的`modify message`不太准确，想要修改。使用`git commit --amend`命令修改`modify message`，在`vim`中编辑完`message`之后，`w+q`退出的时候报错且`message`保存失败，

错误信息如下：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_git_commit_error.png" width = "500" alt="图片名称" align=center />
</center>

<center>
`error: There was a problem with the editor 'vi'.
Please supply the message using either -m or -F option.`
</center>

解决方案：

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_git_commit.png" width = "500" alt="图片名称" align=center />
</center>

<center>
`git config --global core.editor /usr/bin/vim`
</center>

在次执行`git commit --amend`就OK了。
</font>