## git 学习笔记

>	Git is a distributed version control system.
>	Git is free software.


## 常用命令

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

	
	// 到 feature1 分支，发现该分支上并没有因为master分支的合并而改变
	$ git checkout feature1

	// ????  此时是否能够通过命令将feature1分支与master分支同步一下？

	// 调回master分支 ， 暂时删除 feature1 分支
	$ git checkout master
	$ git branch -d  feature1



	```

## 分支管理策略

> 通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
> 下面实战一下--no-ff方式的git merge：

	-	首先，仍然创建并切换dev分支
	```
	git checkout -b dev
	``` 	
	-	修改readme.md 并提交新的commit
	`git commit -a -m "add merge" `

-	切换回master,准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
```
$ git checkout master

$ git merge --no-ff -m "merge with no-ff" dev
```
-	合并后，我们用`git log --graph --pretty=oneline --abbrev-commit`看看分支历史：

```
$ git log --graph --oneline --abbrev-commit

*   4341b5b (HEAD -> master) merge with no-ff
|\
| * 3d48cf6 (dev) add merge
|/
* fe16e23 再次commit master
* d20dad7  ddd
* 44873b8 解决冲突学习完成
* 48eb6c3 update readme.md
*   e648320 fixed conflict
|\
| * 4b39867 update notes in readme.md
* | 706a25f update readme.md in branch master
|/
* aa9cf3a 开始学习解决分支之间的冲突
* 9a13418 (origin/master) readme.md  新增小结
* 7b21023 add some new notes  to readme.md
* a494646 在dev 分支上readme.md 文件中新增添了一些内容
* b6b63a6 edit readme.md in dev branch

...

```

### 分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。




## Bug 分支 

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支issue-101来修复它，但是，等等，当前正在dev上进行的工作还没有提交：

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```js
$ git status
On branch xu
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.md


$ git stash
Saved working directory and index state WIP on xu: 0e1ddb0 merge from xu


$ git status
On branch xu
nothing to commit, working tree clean

```


首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：

```
$ git checkout dev

// 创建新的fix分支
$ git checkout -b fix


// 在fix分支上修复bug，然后commit

$ git commit -a -m "fixed  bug"

// 切换到dev分支，合并，删除fix分支

$ git checkout dev

$ git merge --no-ff -m "merged bug fix " fix

$ git branch -d fix

```

回到dev分支继续自己的开发

```js

$ git checkout dev

$ git status 
On branch dev
nothing to commit, working tree clean


// 查看隐藏的工作现场
$  git stash list 
stash@{0}: WIP on xu: f52c633 add merge

// 恢复工作现场
$ git stash apply stash@{0}

// 删除隐藏
$ git stash drop

```

`git stash pop`，恢复的同时把stash内容也删除.



## add some new notes

## now is at aaa branch

## 11111

## 22222

## 444 dev
