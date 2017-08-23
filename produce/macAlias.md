##<center>Mac下配置alias，zsh终端命令别名</center>
&#160; &#160; &#160;经常使用命令行进行一些操作，一些常用的命令一遍遍的敲比较浪费时间，想通过别名的方式简化操作。

1、就是编辑`~/.bash_profile`,比如添加<font color=red>PS:`=`两边没有空格</font>：

```
	alias st="git status"
	alias pull='git pull'
	alias push='git push'
	alias add='git add .'
	alias ci='git commit -m'
	alias co='git checkout'
	alias myip="ifconfig | grep '192'"
	alias fetch=' git fetch upstream'
	alias merge='git merge'
```

<font color=red>PS:</font>如果没有`.bash_profile`文件，那就直接创建一个`.bash_profile`，然后进行编辑。

```
	touch .bash_profile
	vim .bash_profile
```

2、按照正常的逻辑直接执行（或者重启一下终端）。

```
	source ~/.bash_profile
```

就可以了。

&#160; &#160; &#160;但是我的电脑没有成功。主要原因我使用的是`zsh`,相应配置了`oh-my-zsh`。那么以上的管理配置会发现无效，因为配置了zsh之后，打开新的终端不会按照`bash`的方式走 .bash_profile，`source ~/.bash_aliases`没有执行，因此发现就没有起作用。而是走了.zshrc文件。

&#160; &#160; &#160;解决方案较多,可以直接在`.zshr`当中可以设置`aliases` 。但是这样感觉不太好，怪怪的，但是`~/.bash_profile`中的设置又不会生效。

&#160; &#160; &#160;<font color=red>所以：</font>推荐在在`.zshrc`文件的开头添加这样一行语句

```
	test -f ~/.bash_profile  && source ~/.bash_profile
```

然后重启一下`zsh`