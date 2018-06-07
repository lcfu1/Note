# Git命令总结

目录：

- [命令总结](#命令总结)
- [参考](#参考)


## 命令总结

>$ git config --global user.name "Your Name"
>- 设置全局用户名。

>$ git config --global user.name
>- 查询用户名。

> $ git config --global user.email "Your Email"
>- 设置全局邮箱。

>$ git config --global user.email
>- 查询邮箱。

>$ git config --list
>- 检查已有的配置信息。

>$ git config --global alias.st status
>- 用st表示status。

>$ mkdir gitReporitory
>- mkdir命令是用来创建文件夹的，这里创建一个叫gitreporitory的文件夹。

>$ cd gitReporitory
>- 进入gitReporitory文件夹。

>$ pwd
>- 显示当前目录。

>$ ls
>- 查看当前项目的所有文件。

>$ git init
>- 把这个目录变为Git可以管理的仓库。

>$ git add hello.txt
>- 添加文件到git仓库上，其中hello.txt就是要添加的文件。

>$ cat hello.txt
>- 查看文件的内容，这里查看hello.txt的内容。

>$ cat .git/config
>- 查看Git配置文件。

>$ git add .
>- 添加所有文件到git仓库上。

>$ git commit -m "first commit"
>- 添加完文件还需要提交到git仓库上，-m后面输入的是本次提交的说明。

>$ git status
>- 用于检查本地项目的状态，遇到问题就试试它，会有提示。

>$ git diff
>- 可以查看哪里有改动过。

>$ git rm hello.txt
>- 删除版本库中的hello.txt文件。

>$ rm hello.txt
>- 把工作区中的hello.txt文件删除掉，但版本库中的并没有删除掉。

>$ git checkout -- hello.txt
>- 假设我们修改了hello.txt文件，但还没git add到暂存区，现在想返回到修改前，可以使用$ git checkout -- hello.txt返回到没修改的时候。

>$ git reset HEAD hello.txt
>- 假设我们修改了hello.txt文件，并git add到了暂存区，但现在我们发现有错误，需要修改，我们可以使用该命令把添加到暂存区的修改撤销回工作区。

>$ git reset HEAD hello.txt加$ git checkout -- hello.txt
>- 删除版本库中的hello.txt文件后，没提交前我们还可以通过$ git reset HEAD hello.txt加$ git checkout -- hello.txt撤消删除。

>$ git log
>- 查看修改记录。

>$ git log -1
>- 显示最后一次提交信息。

>$ git reflog
>- 查看你的每一次命令。

>$ git reset --hard
>- 查看当前版本。

>$ git reset --hard HEAD^
>- 回退到上一版本。HEAD表示当前版本，HEAD^表示上一版本，HEAD^^表示上上一个版本，HEAD~100则为上100个版本。

>$ git reset --hard b6db134
>- 其中b6db134是我刚刚回退前版本的commit id的前七位，Git使用SHA-1算法计算数据的校验和，通过对文件的内容或目录的结构计算出一个SHA-1哈希值，作为指纹字符串，由40个十六进制字符组成，但这里我们用前7位就好了，Git会自动寻找出来的。

>$ git help
>- 查看帮助文档。

>$ git help add
>-查看add命令的使用。

>$ git branch
>- 查看当前项目所有分支。

>$ git branch one
>- 创建一个one分支。

>$ git checkout one
>- 切换到one分支。

>$ git checkout -b two
>- 创建并切换到two分支。

>$ git checkout -b one origin/one
>- 创建远程origin的one分支到本地。

>$ git branch --set-upstream-to=origin/branch-name branch-name
>- 创建本地分支和远程分支的链接关系。

>$ git checkout master
>- 切换回master分支。

>$ git branch -d one
>- 删除one分支。

>$ git merge one
>- 合并one分支到master分支上。

>$ git merge --no-ff -m "merge with no-ff" one
>- 合并one分支，--no-ff参数用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

>$ git log --graph --pretty=oneline --abbrev-commit
>- 用带参数的git log查看分支的合并情况。

>$ git stash
>- 保存当前仓库已改变的状态。

>$ git stash list
>- 查看保存到哪。

>$ git stash apply或$ git stash apply stash@{0}
>- 恢复已保存的状态。

>$ git stash drop
>- 删除stash内容。

>$ git stash pop
>- 恢复的同时把stash内容也删除。

>$ git tag
>- 查看当前所有版本。

>$ git tag -a v1.0 -m "这里填相关信息"
>- 为项目版本打个标签。

>$ git show v1.0
>- 查看对应版本的相关信息。

>$ git clone https://github.com/lcfu1/lcfu1.git
>- 这里是以https的形式下载的，也可以以SSH的格式，如$ git clone git@github.com:lcfu1/lcfu1.git。

>$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
>- SSH key的配置，your_email@example.com是你注册github的邮箱。

>$ ssh -T git@github.com
>- 查看是否添加成功。

>$ git remote add origin git@github.com:lcfu1/Git.git
>-向本地仓库添加一个名为origin，地址为git@github.com:lcfu1/Git.git的远程仓库。

>$ git remote -v
>- 查看该项目的远程仓库。

>$ git remote rm origin
>- 删除已有的远程仓库。

>$ git remote add github git@github.com:lcfu1/Git.git
>- 关联GitHub的远程库。

>$ git push origin master:master
>- 推送到远程仓库，origin是上面我们在本地仓库添加的一个名为origin，地址为git@github.com:lcfu1/Git.git的远程仓库。第一个master是本地的master分支，第二是远程的master分支，如果两个一个则可以简化为$ git push origin master。

>$ git push origin master
>- 推送到远程仓库的master分支。

>$ git push origin one
>- 推送到远程仓库的one分支。

>$ git push github master
>- 推送到GitHub。

>$ git pull
>- 更新本地仓库。

>$ git push one:master
>- 将本地的one分支推送到远程仓库。

>$ git push master:one
>- 删除远程仓库的one分支。

## 参考

- [版本控制工具Git的使用](https://lcfu1.github.io/git/2018/03/05/git_001.html)

- [廖雪峰Git教程](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

- 《Android开发进阶从小工到专家》--何红辉