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
这个命令主要是修改最近一次的提交的`modify message`。比如我们在通过`git commit -m'modify message'`提交修改信息

`git commit -- amend` 可能会遇到一个bug：[There was a problem with the editor ‘vi’](http://blog.csdn.net/foolsong/article/details/75042751)
