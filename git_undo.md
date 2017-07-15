#<center>git 撤销操作</center>

##一、前言
   在使用git过程当中经常会用到撤销一些已经完成的操作，经常会用到`git checkout`、`git reset`、`git  revert`、`git commit -- amend`。在用法上也有不同之处，简单的分析下这几种方法的不同。
  
  首先简单看一下git的一个简单流程(简单的流程，可以结合下面的图看一下):
  
  
  *     在工作区进行修改、操作，文件一般处于`modified`或`Untracked`
  *    `git add`   就是把文件修改添加到暂存区
  *    `git commit`  就是把暂存区的所有内容提交到当前分支。
  
  <center>
  <img src="http://osz3uubsl.bkt.clouddn.com/gitflow.png"  alt="" width=500 />
  </center>
  
##二、git commit -- amend
这个命令主要是修改最近一次的提交的`modify message`。比如我们在通过`git commit -m'modify message'`提交修改信息。之后发现修改信息有误可以通过`git commit --amend`修改已经提交的信息:

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blogblog_7_14_git_undo_01.png" width = "500" alt="图片名称" align=center />
</center>

使用`vim`编辑需要的信息:
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blogblog_7_14_git_undo_03.png" width = "500" alt="图片名称" align=center />
</center>

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_04.png" width = "500" alt="图片名称" align=center />
</center>

`w+q`保存退出

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blogblog_7_14_git_undo_02.png" width = "500" alt="图片名称" align=center />
</center>

这样就OK了。

 第一次使用`git commit -- amend` 默认的编辑器可能不是`vim`可能会遇到一个bug：<font color="red">There was a problem with the editor ‘vi’   [bug修复传送门～点击](http://blog.csdn.net/foolsong/article/details/75042751)</font>

##三、git checkout
`git checkout`一些常用的用法：

>  git checkout [-q] [< commit >] [--] \< paths >... 

>  git checkout [< branch >]

>  git checkout [-m] [[-b]--orphan] < new_branch >] [< commit__id >]

今天主要讨论一下`git checkout`撤销的一些操作

1.   ` git checkout`  ||  ` git checkout HEAD` || ` git checkout [< commit__id >]`
2.   `git checkout -- filename`
3.   `git checkout branch -- filename`
4.   `git checkout -- .  `   ||    `  git checkout .`


| 参数        | 作用                  | 
| :---------------: |:-------------|
| ` git checkout`  \|\|  ` git checkout HEAD`     | 显示工作区、暂存区与HEAD的差异|
|`git checnkout -[< commit__id >]`|切换至指定节点的代码库（git节点都含有一个完整的代码仓库），如 `git checnkout -[3e07fd8]` |
| `git checkout -- filename`      | 用暂存区中filename文件来覆盖工作区中的filename文件。相当于取消自上次执行`git add/commit `以来（如果执行过）的本地修改      |
| `git checkout branch -- filename` | HEAD不变。用branch所指向的提交中filename替换暂存区和工作区中相应的文件。注意会将暂存区和工作区中的filename文件直接覆盖。     | 
| `git checkout -- .  `   \|\|    `  git checkout .` | 相当于用暂存区的所有文件直接覆盖本地文件，<font color="red">危险操作⚠️</font>|


##四、git reset
简单说一下`git reflog`:
>`git reflog`
>
显示整个本地仓储的`commit`, 包括所有`branch`的`commit`, 甚至包括已经撤销的`commit`, 只要HEAD发生了变化, 就会在`reflog`里面看得到. `git log`只包括当前分支的`commit`.

`git reflog`查询结果为下：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_05.png" width = "500" alt="图片名称" align=center />
</center>

|参数|作用|
| :---------------: |:-------------|
|`git reset HEAD`  |任何事情都不会发生，这是因为我们告诉GIT重置这个分支到HEAD，而这个正是它现在所在的位置(上图的3e07fd8位置)。|
|`git reset HEAD~1`|则意味着将HEAD从顶端的commit往下移动一个更早的commit|
|`git reset HEAD~2`|则意味着将HEAD从顶端的commit往下移动两个更早的commit|

在看一下各个参数：

|参数|作用|
| :---------------: |:-------------|
| soft |--soft参数使git重置HEAD到另外一个commit。意味着工作区、暂存区都不会做任何变化，所有的在 HEAD和重置到的那个commit之间的所有变更集仍然在暂存区(index)区域中。|
| hard |--hard参数将会blow out everything。它将重置HEAD到另外一个commit，重置缓存区以便反映HEAD的变化，并且重置工作区也使得其完全匹配起来。<font color="red">这是一个比较危险的操作⚠️</font>，具有破坏性！|
|mixed(default）|--mixed是reset的默认参数。它将重置HEAD到另外一个commit,并且重置缓存区。工作区不会被更改。所以所有从HEAD到你重置到的那个commit之间的所有变更仍然保存在工作区中，被标示为已变更|
举个简单的例子： `git reset --mixed [版本号]`

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_06.png" width = "500" alt="图片名称" align=center />
</center>
1.先`git commit`提交一部分内容，在通过`git reset --mixed 62aba96`，撤销最后一次的提交。

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_07.png" width = "500" alt="图片名称" align=center />
</center>
2.在通过`git diff`查看修改过的内容。会发现`[62aba96]`提交的内容，回到了工作区
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_08.png" width = "500" alt="图片名称" align=center />
</center>
3.这个时候通过`git reflog`查看最上面两条信息，一条是`git commit`的信息，一条是`git reset`的信息。

##五、git revert
如果简单的说一下`git revert`的作用，通过创建一条新的`commit`，覆盖之前的某一次`commit`所提交的内容，但是不会影响之前的提交信息。
>Given one or more existing commits, revert the changes that the related patches introduce, and record some new commits that record them. This requires your working tree to be clean (no modifications from the HEAD commit).

|参数|作用|
| :---------------: |:-------------|
|git revert HEAD| 撤销上一次的`commit`|
|git revert HEAD^|撤销上上一次的`commit`|
|git revert [commit-id]|撤销到指定的某一次的`commit`|

举个简单的例子：
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_09.png" width = "500" alt="图片名称" align=center />
</center>

1. 先`commit`提交一条内容。
2. 在通过`git revert HEAD`
3. 在通过`git reflog`（下图）和`git log -p`（下图）查看会发现，有条`commmit`覆盖了之前`commit`的操作。

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_10.png" width = "500" alt="图片名称" align=center />
</center>

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_7_14_git_undo_12.png" width = "500" alt="图片名称" align=center />
</center>
##六、参考
[http://www.cnblogs.com/craftor/archive/2012/11/04/2754147.html](http://www.cnblogs.com/craftor/archive/2012/11/04/2754147.html)

[http://www.cnblogs.com/kidsitcn/p/4513297.html](http://www.cnblogs.com/kidsitcn/p/4513297.html)