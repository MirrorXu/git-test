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

-	`git reset --hard head^`    (回退至上一版本)
-	`git reset --hard head~100` (回退100个历史版本)
-	`git reset --hard commit_id` (回退到指定版本)
-	`git reflog `  (查看历史命令)


some new

add  new message 

edit git config  	

` git config --global user.name"Mirror" ` 

`git config --global user.email"xuxulee@163.com"`

set git global config user.name  by other peoject


now , at this project i set git config user.name is "xupenglai"  but  global  config user.name is 'Mirror'



