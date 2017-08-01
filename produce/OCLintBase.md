#<center>OCLint-iOS-OC项目几种简单使用</center>

<font color=red size=15> 未完待续</font>
####OCLint简介
>OCLint is a static code analysis tool for improving quality and reducing defects by inspecting C, C++ and Objective-C code and looking for potential problems like possible bugs, unused code, complicated code, redundant code, code smells, bad practices, and so on.

安装
> $ brew tap oclint/formulae

>$ brew install oclint

###Using OCLint with xcodebuild
`xcodebuild -target DemoProject -configuration Debug -scheme DemoProject`
###Using OCLint with xctool
安装
> $ brew install xctool --HEAD

`xctool -reporter json-compilation-database:compile_commands.json build`
###Using OCLint with xcpretty
安装
>$ brew install xcpretty

`xcodebuild [flags] | xcpretty -r json-compilation-database -o compile_commands.json`
###Using OCLint in Xcode
*  Add a new target in the project, and choose Aggregate as the template.
<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_2_git_undo_01.png" width = "500" alt="图片名称" align=center />
</center>

*  Name the new target, here we simply call it “OCLint”, you could have more than one targets that focus on different aspects of the code analysis.

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_2_git_undo_02.png" width = "500" alt="图片名称" align=center />
</center>

*  Add a new build phase in the target we just created. Choose Add Run Script for the phase type.

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_2_git_undo_03.png" width = "500" alt="图片名称" align=center />
</center>

* Choose the correct build scheme, here we choose OCLint.Click to build, or use the shortcut Command+B.
* Click to build, or use the shortcut Command+B.
* When the progress bar scrolls to the very right, the analysis is done, then we can check out the analysis results same as compile warnings. 

<center>
	 <img src="http://osz3uubsl.bkt.clouddn.com/blog_8_2_git_undo_04.png" width = "500" alt="图片名称" align=center />
</center>