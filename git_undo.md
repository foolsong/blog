#<center>git 撤销操作</center>

###前言
   在使用git过程当中经常会用到撤销一些已经完成的操作，经常会用到`git checkout`、`git reset`、`git  revert`、`git commit -- amend`、`git rebase`。在用法上也有不同之处，简单的分析下这几种方法的不同。
  
  首先简单看一下git的一个简单流程(简单的流程，可以结合下面的图看一下):
  
  
  *     在工作区进行修改、操作，文件一般处于`modified`或`Untracked`
  *    `git add`   就是把文件修改添加到暂存区
  *    `git commit`  就是把暂存区的所有内容提交到当前分支。
  
  <center>
  <img src="http://osz3uubsl.bkt.clouddn.com/gitflow.png"  alt="" width=500 />
  </center>
  
###git commit -- amend
这个命令主要是修改最近一次的提交的`modify message`。比如我们在通过`git commit -m'modify message'`提交修改信息。之后发现修改信息有误可以通过`git commit --amend`修改已经提交的信息:
<div style="color:#fff;background:#000;width:500px;padding-left:15px;padding-top:15px;padding-bottom:15px"> <div padding-left:15px>
	➜  blog git:(master) ✗ git commit -m'modify messgae'</br>
	 [master c75d298] modify messgae</br>
	 2 files changed, 20 insertions(+), 4 deletions(-)</br>
	 rewrite git_undo.md (85%)</br>
	 create mode 100644 runtime.md</br>
	➜  blog git:(master) git commit --amend</br>
</div></div>
使用`vim`编辑需要的信息:
<div style="color:#fff;background:#000;width:500px;padding-left:15px;padding-top:15px;padding-bottom:15px"> <div padding-left:15px>
	add new file runtime.md &&   modify messgae git_undo.md</br>
	 # Please enter the commit message for your changes. Lines starting</br>
	 # with '#' will be ignored, and an empty message aborts the commit.</br>
</div></div>
`w+q`保存退出

<div style="color:#fff;background:#000;width:500px;padding-left:15px;padding-top:15px;padding-bottom:15px"> <div padding-left:15px>
	➜  blog git:(master) ✗ git commit --amend</br>
	[master 7698fae] add new file runtime.md &&   modify messgae git_undo.md ：</br>
	 Date: Thu Jul 13 17:29:53 2017 +0800</br>
	 2 files changed, 20 insertions(+), 4 deletions(-)</br>
	 rewrite git_undo.md (85%)</br>
	 create mode 100644 runtime.md</br>
</div></div>

 `git commit -- amend` 默认的编辑器不是`vim`可能会遇到一个bug：<font color="red">There was a problem with the editor ‘vi’   [bug修复传送门～点击](http://blog.csdn.net/foolsong/article/details/75042751)</font>

###git checkout
`git checkout`一些常用的用法：

*   git checkout [-q] [<commit>] [--] <paths>... 

http://www.cnblogs.com/craftor/archive/2012/11/04/2754147.html