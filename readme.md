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
	-	







some new

add  new message 

edit git config  	

` git config --global user.name"Mirror" ` 

`git config --global user.email"xuxulee@163.com"`

set git global config user.name  by other peoject


now , at this project i set git config user.name is "xupenglai"  but  global  config user.name is 'Mirror'


add a new paragraph






