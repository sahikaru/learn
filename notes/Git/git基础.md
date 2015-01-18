### Git基础

##### 相关命令
*	``git init``			初始化仓库
*	``git add ``			暂存文件
*	``git commit "``		提交跟新
*	``git clone ``			克隆仓库，文件夹名字为仓库名
*	``git clone xxx mygit``克隆仓库，文件夹名字为mygit
*	``git status``			查看工作目录文件的状态
*	``git diff``			比较暂存文件和未暂存文件
*	``git diff --cached``	比较暂存文件和上次提交时候的文件
*	``git rm``				从暂存区删除文件，
*	``git rm --cached ``	从git仓库中删除
*	``git mv``				移动文件
*	``git log``				打印git的历史提交信息
*	``gitk``				图形化工具
*	``git remote``			列出远程仓库简短名
*	``git remote -v``		列出远程仓库和对应的地址
*	``git fetch``			从远程仓库拉取数据
*	``git push``			把数据推送到远程仓库
*	``git remote show``		远程仓库的具体信息
*	``git remote rename``	在本地给远程仓库简短名
*	``git tag``					打标签（暂时不看）
*	``git show``				最后提交的信息
*	``git unstage``				其实是下面的别名，可以自己设置
*	``git reset HEAD filename`` 把文件从暂存状态到未暂存			
*	``git last``			最后一次提交信息


##### 在工作目录中初始化新仓库

``git init``

初始化之后，当前目录下会出现.git目录，所有git需要的数据和资源都在个目录里面，还没有跟踪目录中的任何文件


##### 跟踪文件
``git add *.c``

``git add README``

``git commit -a -m "init project version"``

这句等于

``git add *``

``git commit -m "init project version"``

##### 从现有的仓库

``git clone git://github.com/schacon/grit/.git``

这个命令会在当前目录下创建一个grit的目录，其中包括了.git目录，用于保存在来的所有版本记录。

如果希望在拷贝仓库的时候重命名文件夹

``git clone git://github.com/schacon/grit/.git mygrit``

__协议__：git支持多种数据传输协议，之前的例子使用的是git://协议，也可以使用https://或者user@server:/path.git表示SSH传输协议


##### 检查当前文件状态

``git status``
	
	➜  FE git:(master) git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	nothing to commit, working directory clean

提示信息包括：目前的分支，当前没有出现任何未跟踪的新文件

用vim修改README.md文件后在查看

	➜  FE git:(master) vi README
	➜  FE git:(master) ✗ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Untracked files:
    (use "git add <file>..." to include in what will be committed)

	README

	no changes added to commit (use "git add" and/or "git commit -a")
	
这里的提示信息表示README文件未跟踪，表示git之前的快照中没有这个文件，提示中现实如果需要在提交前跟踪文件，使用``git add <filename>``命令。

##### 跟踪新文件

``git add README``

再运行``git status``

	➜  FE git:(master) ✗ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)

		new file:   README
		
只有在changes to be committed下的，说明是暂存状态。


##### 暂存已经修改的文件
``vi README.md``
修改了README.md文件

	  FE git:(master) ✗ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)

		new file:   README

	Changes not staged for commit:
  	(use "git add <file>..." to update what will be committed)
  	(use "git checkout -- <file>..." to discard changes in working directory)

		modified:   README.md

``git add README.md``

``git status``
这个时候，把README.md加入暂存文件

	➜  FE git:(master) ✗ gst
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)

		new file:   README
		modified:   README.md

2个文件都暂存，如果commit就提交记录到仓库了。
且慢,这个时候，在修改README.md文件

	➜  FE git:(master) ✗ vi README.md
	➜  FE git:(master) ✗ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)

		new file:   README
		modified:   README.md

	Changes not staged for commit:
  	(use "git add <file>..." to update what will be committed)
  	(use "git checkout -- <file>..." to discard changes in working directory)

		modified:   README.md
		
如果这个时候提交，就提交add之后的READ.md



##### 忽略某些文件

创建一个.gitignore的文件

格式规范如下：

*	所有空行或者注释号#开头的行都被Git忽略
*	可以使用标志Glob模式匹配(shell所使用的正则表达式)
*	匹配模式最后跟反斜杠(\)说明要忽略的是目录
*	要忽略指定模式以外的文件或者目录，可以在模式上加惊叹号(!)取反

一个例子

	# 此为注释 -- 将被Git忽略
	# 忽略所有.a结尾的文件
	*.a
	# 但lib.a 除外
	!lib.a
	# 仅忽略项目根目录下的TODO文件，不包括 subdir/TODO
	/TODO
	# 忽略build /目录下的所有文件
	build/
	# 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
	doc/*.txt
	# ignore all .txt files in the doc/ directory
	doc/**/*.txt
	
##### 查看以暂存和未暂存的更新

``git status``

	➜  FE git:(master) ✗ git status
	On branch master
	Your branch is up-to-date with 'origin/master'.

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)

		new file:   README
		modified:   README.md

	Changes not staged for commit:
  	(use "git add <file>..." to update what will be committed)
  	(use "git checkout -- <file>..." to discard changes in working directory)

		modified:   README.md
		
要查看尚未暂存的文件更新了哪些部分``git diff``
	
	diff --git a/README.md b/README.md
	index 237339a..89116ef 100644
	--- a/README.md
	+++ b/README.md
	@@ -8,4 +8,3 @@ include
 	*   charts
 	*   bootstrap
 	*   FU project
	-
	(END)
	
此命令是比较工作目录中当前文件和暂存区快照之间的差异，也就是修改后没有暂存起来的变化的地方

``git diff --cached``

	Changes to be committed:
  	(use "git reset HEAD <file>..." to unstage)
	diff --git a/README b/README
	new file mode 100644
	index 0000000..6a48407
	--- /dev/null
	+++ b/README
	@@ -0,0 +1 @@
	+test for git
	diff --git a/README.md b/README.md
	index 89116ef..237339a 100644
	--- a/README.md
	+++ b/README.md
	@@ -8,3 +8,4 @@ include
 	*   charts
 	*   bootstrap
 	*   FU project
	+
	(END)
	
这个命令是比较暂存起来的文件和上次提交时的快照比较



##### 提交跟新
``git commit``

这种方式会启动默认的编辑器输入对本次提交的说明

	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	# On branch master
	# Your branch is up-to-date with 'origin/master'.
	#
	# Changes to be committed:
	#   new file:   README
	#   modified:   README.md
	#
	# Changes not staged for commit:
	#   modified:   README.md
	#

默认提交的消息包括最后一次运行``git status``的输出，放在注释里，开头第一行是给写提交说明的

可以使用``git commit``来提交

``git commit -m "tanli02: +add file README -modify file README.md. all for git test"``

	➜  FE git:(master) ✗ git commit -m "sahikaru: +add file README -modify file 	README.md. all for git test"
	[master 9e16fb0] sahikaru: +add file README -modify file README.md. all for git test
	 2 files changed, 2 insertions(+)
	 create mode 100644 README

##### 跳过使用暂存区

``git commit -a -m "" ``

##### 移除文件

``git rm``
从暂存区删除文件

	➜  FE git:(master) ✗ git rm README
	rm 'README'

这个时候，在提交，在git上就不会对这个文件放进版本管理系统中

``rm filename``		只是在工作目录删除文件，而不是git的缓存区
	
``git checkout -- file``得到上次提交的文件

``git rm --cached README.md``	在git的仓库中删除

##### 移动文件

``git mv file_form file_to`` git不会跟踪文件的移动操作，但是如果git中重命名了某个文件，仓库中的元数据并不会体现这是一次改名操作，但是git非常充满，它会推断出究竟发生了什么

其实``git mv README.txt README``等于

``mv README.txt README``

``git rm README.txt``

``git add README``


##### 查看提交历史

``git log``  默认不用任何参数的话，git log会按照提交时间列出所有的更新，最近的跟新排在最上面,每次更新都有一个SHA-1校验和、作者名字和邮件地址、提交时间，提交说明

``git log -p -2`` -p现实每次提交的内容差异,-2是现实最近2次

	commit e4f1e1294c954ca5ecd83bac93f30fccf2a0c674
Author: tanli02 <tanli02@baidu.com>
Date:   Sun Jan 18 20:52:51 2015 +0800

    sahikaru:delete README

 	diff --git a/README b/README
	deleted file mode 100644
	index 6a48407..0000000
	--- a/README
	+++ /dev/null
	@@ -1 +0,0 @@
	-test for git

	commit 9e16fb044e58ce65e62f1520f0b2bd9457edb6b7
	Author: tanli02 <tanli02@baidu.com>
	Date:   Sun Jan 18 01:59:06 2015 +0800

    	sahikaru: +add file README -modify file README.md. all for git test

	diff --git a/README b/README
	new file mode 100644
	index 0000000..6a48407
	--- /dev/null
	+++ b/README
	@@ -0,0 +1 @@
	+test for git
	diff --git a/README.md b/README.md
	index 89116ef..237339a 100644
	--- a/README.md
	+++ b/README.md
	@@ -8,3 +8,4 @@ include
	 *   charts
	 *   bootstrap
	 *   FU project
	+
	(END)
		
这里会有diff的信息

``git log -U1 --word-diff``  --word-diff 单词对比，-U1上下文只留一行

``git log --stat``			统计每次提交修改的文件的行数变化

``git log --pretty=oneline``	每个提交显示一行

	e4f1e1294c954ca5ecd83bac93f30fccf2a0c674 sahikaru:delete README
	9e16fb044e58ce65e62f1520f0b2bd9457edb6b7 sahikaru: +add file README -modify file README.md. all for git test
	b1bcd7767760e71e708f517b524399ba2263f44f sahikaru: +add nvd3 chart'file,some of the charts example code
	494c54d2976fc7c069765ff37f4a5f8b665e08d9 sahikaru: +add new folder for bootstrap style demo,all file download from 	getbootstrap.com
	6b681d2bd0ee821c3eb54c121e428ffdbb6a55a9 sahikaru: -modify the explaining of this project
	9b0fa5eda40d97757ff520818eca50317a1745a8 sahikaru: -modify README.md
	eb51bfe66d5c2dcc7084727262185db94495193e Initial commit
	
``git log --pretty=format: "%h - %an, %ar : %s"``  自定义格式化输出格式
	
	e4f1e12 - tanli02, 23 minutes ago: sahikaru:delete README
	9e16fb0 - tanli02, 19 hours ago: sahikaru: +add file README -modify file README.md. all for git test
	b1bcd77 - sahikaru, 12 days ago: sahikaru: +add nvd3 chart'file,some of the charts example code
	494c54d - sahikaru, 12 days ago: sahikaru: +add new folder for bootstrap style demo,all file download from getbootstrap.com
	6b681d2 - sahikaru, 12 days ago: sahikaru: -modify the explaining of this project
	9b0fa5e - sahikaru, 12 days ago: sahikaru: -modify README.md
	eb51bfe - sahikaru, 8 months ago: Initial commit、、
	
每种占位符的意义

*	``%H``	commit对象的完整哈希值
*	``%h``	commit对象的简短哈希值
*	``%T``	树对象的完整哈希字符串
*	``%t``	树对象的简短哈希字符串
*	``%P``	父对象的完整哈希字符串
*	``%p``	父对象的简短哈希字符串
*	``%an``	作者的名字
*	``%ae`` 作者的电子邮件
*	``%ad``	作者修订日期	-data＝选定格式
*	``%ar`` 作者修定日期，按照多久前来现实
*	``%cn``	提交者的名字
*	``%ce`` 提交者的电子邮件
*	``%cd`` 提交日期
*	``%cr`` 提交奥日期，按照多久前显示
*	``%s``	提交说明

git log的选项说明

*	``-p``	补丁格式显示每个更新指尖的差异
*	``--word-diff``	按照work diff格式显示差异
*	``--stat``		显示每次更新的文件的统计信息
*	``--shortstat``	只显示 --stat中最后行数修改的统计
*	``--name-only`` 仅在提交信息后显示已修改的文件清单
*	``--name-status``	显示新增、修改、删除的文件清单
*	``--abbrev-date``	仅显示SHA-1的前几个字符、而非所有的40个字符
*	``--relative-date`` 使用较短的相对时间来显示 比如2 weeks ago
*	``graph1``			图形表示的分支合并历史
*	``pretty``			使用其他格式显示历史提交信息，可用选项包括oneline,short,full,fuller,format
*	``--oneline``		``--pretty=oneline --abbrev-commit``的简化用法


##### 简化输出长度

``git log --since=2.weeks``		最近2天的提交

选项说明

*	``-n``						最近n次提交
*	``--since``，``--after``	仅显示指定时间之后的提交
*	``--until, --before``		仅显示指定时间之前的提交
*	``--author``				仅显示指定作者的提交
*	``--committer``				仅显示指定提交者相关的提交


##### 图形化工具

``gitk``

``sourcetree``


##### 撤销操作

修改最后一次提交

``git commit --amend`` 这个命令使用当前缓存区域的快照提交。如果上次提交没有什么修改的话，就是修改提交时候的信息。


这里可以把第二次提交放到第一次,这个时候再看log的话，只是有第一次提交的信息
``git commit -m "initial commit"``

``git add forgotten_file``

``fit commit --amend``


##### 取消已经暂存的文件

``git reset HEAD filename``

##### 取消对文件的修改

``git checkout -- filename`` 	

任何提交到Git的都可以恢复。即便在已经删除的分支中的提交。


##### 使用远程仓库

远程仓库是指托管在网络上的项目仓库，可能会有好多个。

###### 查看当前的远程仓库

``git remote`` 列出每个远程仓库的简短名，在复制了一个项目后，至少看到一个名为origin的远程库，Git默认使用了这个名字来标志你所克隆的原始仓库。

``git remote -v``	显示对应仓库的地址

	➜  FE git:(master) ✗ git remote -v
	origin	https://sahikaru@github.com/sahikaru/FE.git (fetch)
	origin	https://sahikaru@github.com/sahikaru/FE.git (push)
	

``git remote add pb git://github.com/paulboone/ticgit.git``添加一个叫pb的远程仓库


###### 从远程仓库拉取数据
``git fetch ``	从远程仓库拉取数据

此命令会到远程仓库中拉取所有你的本地没有的仓库数据。运行完之后就可以在本地访问远程仓库的所有分支了

如果是克隆了一个仓库，此命令会自动将远程仓库归与origin名下。

 ``只是拉取，并不会自动合并，只有当你准备好了，才能手工合并``
 如果用某个分支跟踪远程仓库的分自黑，可以使用git pull拉取，然后将远端分支自动合并倒本地仓库中。
 

###### 推送数据到远程仓库

``git push origin master``把本地的master分支推送倒origin服务器上

###### 查看远程仓库信息

``git remote show origin``	

###### 远程仓库的删除和重名

``git remote rename``	修改某个远程仓库在本地的简称

``git remote rm paul``	删除本地对应的远程仓库





	



