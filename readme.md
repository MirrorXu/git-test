## git 学习笔记

>	Git is a distributed version control system.
>	Git is free software.


## some err

- 	`git add <filename>`  (添加文件)
- 	`git add .` / `git add *`  (添加所有文件)
- 	`git status`  (查看当前状态)
-	`git commit -m < 描述内容 >` (提交并添加描述)
-	`git log`  (显示从最近到最远的提交日志)
- 	`git log --pretty=oneline` (以单行显示从最近到最远的提交日志)

-	`git reset --hard head`    (强制恢复到当前版本的初始状态)
-	`git reset --hard head^`    (强制回退至上一版本)
-	`git reset --hard head~100` (强制回退100个历史版本)
-	`git reset --hard commit_id` (强制回退到指定版本)
-	`git reflog `  (查看历史命令)

## 撤销修改

> 撤销修改就是将修改恢复到 上一次的 暂存区状态 ， 用于版本在没有commit 之前  ，如果已经commit ，恢复的方法只能是通过 `git reset --hard xxx`

-	`git checkout -- <filename>`

>	当我们将一些错误的修改提交到暂存区，但并没有commit , 此时我们可以通过`git reset head <file>` 来撤销暂存区的修改（清空了暂存区！）， 但是你在文件中作出的修改并没有被删除，可以通过修改不合适的 地方，然后重新add到暂存区。

- `git reset head <file>` 将暂存区清空，并保留了用户的add到暂存区之前的修改来让用户进一步修改。

**小结：**
- 	场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

- 	场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

-	场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，可以通过版本回退实现，不过前提是没有推送到远程库。


## 添加远程库

-	删除了github 中的公钥 ， 此时还能够能够push到github上吗？
-	删除了github远程仓的 ssh公钥，无法push. 错误提示如下：
	```
	$ git push origin master
	Permission denied (publickey).
	fatal: Could not read from remote repository.

	Please make sure you have the correct access rights
	and the repository exists.
	```
- **小结：**

	-	要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

	-	关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

	-	此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；


	


##  从远程仓克隆

-	`git clone <https/ssh>`

-	要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

-	Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
-	ssh 和 https区别
	-	https 协议比 ssh 协议慢
	-	https 协议克隆的仓库，每次提交都需要输入账号和密码
	-	ssh需要在客户端生成相应文件，然后将公钥添加至github的ssh中，这样就能够免密push代码了




## 创建与合并分支

-	首先创建`dec`分支，然后切换到`dev`分支

	```
	$ git checkout -b dev
	Switched to a new branch 'dev'
	```
	git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：

	```
	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'
	```

	用git branch命令查看当前分支：

	```
	$ git branch
		* dev
  		  master
	```
	git branch命令会列出所有分支，当前分支前面会标一个*号。


	在`dev`分支修改内容并正常提交。

	`$ git checkout master` 切换回 `master`分支。

	`git log ` 发现master 分支上的文件并没有被修改。

	`git merge dev`  将`dev` 分支合并到当前 `master` 分支，`git log --oneline ` 发现 最新的一条提交记录为 	`b6b63a6 (HEAD -> master, dev) edit readme.md in dev branch`  (dev 分支与 master 分支合并)，并且在`dev` 分支上的修改已经合并到`master` 分支上。

	`git branch -d dev` 删除 `dev` 分支。

	`git branch` 查看分支。 只剩下 `* master` 分支。

- **小结：**
	-	查看分支：`git branch`

	-	创建分支：`git branch <name>`

	-	切换分支：`git checkout <name>`

	-	创建+切换分支：`git checkout -b <name>`

	-	合并某分支到当前分支：`git merge <name>`

	-	删除分支：`git branch -d <name>`


## 解决冲突

	// 添加并跳转至 dev 分支

	$ git checkout -b dev	
	
	// 添加并跳转至 feature1 分支

	$ git checkout -b feature1

	// feature1 分支上 ， 修改readme.md

	// 在 feature1 分支上提交
	
	$ git commit -a -m "commit new notes in readme.md "

	// 切换到master分支 , Git还会自动提示我们当前master分支比远程的master分支要超前1个提交。
		
	$ git checkout master
	Switched to branch 'master'
	Your branch is ahead of 'origin/master' by 1 commit.

	// 修改readme.md 然后提交

	$ git commit -a -m "update readme.md at branch master"
	
	//将feature1 分支合并是 master 分支

	$ git merge feature1
	Auto-merging readme.md
	CONFLICT (content): Merge conflict in readme.md
	Automatic merge failed; fix conflicts and then commit the result.


	// git status 查看冲突
	
	$ git status
	On branch master
	Your branch is ahead of 'origin/master' by 2 commits.
  		(use "git push" to publish your local commits)

	You have unmerged paths.
  		(fix conflicts and run "git commit")
  		(use "git merge --abort" to abort the merge)

	Unmerged paths:
  		(use "git add <file>..." to mark resolution)

        		both modified:   readme.md


	// 直接查看冲突的文件 readme.md ， Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存

	// 根据自身的需求对 readme.md 进行修改 , 提交
		
	$ git add readme.md	
	$ git commit -m "conflict fixed"
	
	// 用带参数的git log也可以看到分支的合并情况

	$  git log --graph --pretty=oneline --abbrev-commit
	*   e648320 (HEAD -> master) fixed conflict
	|\
	| * 4b39867 (feature1) update notes in readme.md
	* | 706a25f update readme.md in branch master
	|/
	* aa9cf3a (dev) 开始学习解决分支之间的冲突
	* 9a13418 (origin/master) readme.md  新增小结
	* 7b21023 add some new notes  to readme.md
	*...


	```

	


