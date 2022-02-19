### Git使用说明文档

#### 1、创建仓库(Repository)

* #### git init 

```shell
mkdir LearningGit #创建一个目录
cd LearningGit # 进入创建的目录
git init # 初始化该目录为一个repository，并在该目录下生成一个.git文件，.git就是管理LearningGit仓库版本的文件
```

#### 2、添加文件到仓库

* #### git add file

* #### git commit -m  "message"

```shell
touch readme.md # 创建一个Markdown文件
vim readme.md # 打开文件输入内容，比如“This is a Git sample file. Welcome to join the Git world.”
git add readme.md # 添加readme.md文件到LearningGit仓库，也可以一次提交多个文件,比如git add file1 file2 ... 或者采用通配符 git add file_dir/*
git commit -m "add readme file" # 将文件提交到仓库
```

#### 3、版本管理

* #### git log

* #### git reset --hard commit_id

* #### git reflog

* #### git diff 

* #### git checkout -- targetFile

* #### git reset HEAD targetFile

* #### git rm targetFile

> `Git`中`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100` ；

```shell
git log # 显示最近到最远的提交日志
#git log --pretty=online # 如果感觉输出信息太多，可以加上--pretty=oneline参数
git reset --hard HEAD^ # 版本回退到上一个提交的版本
#git reset --hard commit_id # 或者采用commit_id的方式回退到指定的版本，每次commit的时候都会有一个commit_id来记录提交的版本号
git reflog # 查看你的git命令历史，可以查找回到未来版本的commit_id
```



> 工作区是本地电脑中创建的文件夹；
>
> 版本库(Repository)是工作区中的隐藏目录`.git`文件；版本库中最重要的就是暂存区(Stage)，以及Git自动创建的分支`master`，以及指向`master`的一个指针`HEAD`；
>
> `git add file`的过程就是将工作区的文件添加到版本库中的暂存区；
>
> `git commit -m "commit description"`的过程就是将暂存区的文件提交到分支；



> `git`只记录修改，而非文件；
>
> `git diff HEAD -- <targetFile>`可以查看工作区和版本库里面的最新版本关于`targetFile`的区别；



> `git checkout -- <targetFile>`可以撤销**工作区**中关于`targetFile`的全部修改，让文件恢复到最近一次`git commit`或者`git add`时的状态；
>
> **此处需要强调的是chekout后面的 `--` 一定要加上，不然就是切换分支的命令了**
>
> `git checkout --`其实是用**版本库**里的版本*替换* **工作区**的版本，无论工作区是修改还是删除，都可**一键还原**；
>
> `git reset HEAD <targetFile>`可以将**暂存区**中关于`targetFile`的**修改**撤销掉，重新放回**工作区**；
>
> `git rm <targetFile>`用于删除**版本库**中的`targetFile`，并使用`git commit -m "修改说明"`提交修改说明；

#### 4、远程仓库

> 由于远程仓库刚创建是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令；
>
> 后续只要本地修改做了提交，就可以采用`git push origin master`命令将 本地`master`分支的最新修改推送至GitHub ；

```shell
git remote add origin git@github.com:YOUR_GITHUB_USER_NAME/YOUR_REPOSITORY.git # 将本地的仓库与远程的仓库的origin进行关联，前提需要在GitHub上的账户需要添加本地仓库所在设备的SSH Keys
git push -u origin master # git push命令将本地仓库的master分支推送到远程仓库，远程仓库默认名字是origin，第一次推送分支加上-u参数
# 后续本地master分支所有新的修改就可以由以下命令提交至Github上了
git push origin master # 将本地master分支的最新修改提交至GitHub上
```



> 如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm `命令。使用前，建议先用`git remote -v`查看远程库信息；
>
> 此处的**“删除”**其实是**解除了本地和远程的绑定关系**，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。 

```shell
git remote -v # 查看远程库的信息
git remote rm origin # 删除与远程的origin建立的绑定关系
```



> 克隆远程仓库至本地；

```shell
git clone https://github.com/YOUR_GITHUB_USER_NAME/LearningGit.git
```

#### 5、分支管理

```shell
# 创建分支develop并切换至该分支
git checkout -b develop # checkout -b branchName相当于git branch develop+git checkout develop

git branch # 查看分支
git checkout branchName # 切换分支至branchName
git merge branchName # 合并指定分支branchName到当前分支
git branch -d branchName # 删除分支branchName
```

>  当合并分支遇到冲突时，比如同一个文件被两个分支都修改过，此时需要手动修改两个分支修改的差异，使得两个分支的修改一致，修改后需要提交，提交后才能进行合并；
>
>  `git`用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容 ；

```shell
git status # 可以查看有冲突的文件
git log --graph # 可以查看分支的合并图
```



>  `stash`，可以把未完成的当前工作现场“储藏”起来，等以后恢复现场后继续工作；
>
>  在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit_id> `命令，把bug提交的修改“复制”到当前分支，避免重复劳动；

```shell
git stash # 存储当前工作现场，等其他任务完成后恢复现场继续工作
git stash list # 查看保存的工作现场列表
# 恢复工作现场方法一
git stash apply stashName # 恢复工作现场
git stash drop stashName # 删除stash内容
# 恢复工作现场方法二
git stash pop # 恢复现场的同时把stash内容也删除了
```



> **`feature`分支**：添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。 

```shell
git branch -D <branchName> # 强行删除一个没有被合并过的分支
```



> **本地分支与远程相对应的分支常用的管理规则：**
>
> - `master`分支是主分支，因此要时刻与远程同步；
> - `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
> - bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
> - feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
>
> 
>
> 本地的dev分支推送至远程失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后在本地合并，解决冲突，再推送；
>
>  **多人协作的工作模式通常是这样：** 
>
> 1. 首先，可以试图用`git push origin <branchName> `推送自己的修改；
> 2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
> 3. 如果合并有冲突，则解决冲突，并在本地提交；
> 4. 没有冲突或者解决掉冲突后，再用`git push origin <barnchName> `推送就能成功！

```shell
git clone https://github.com/YOUR_GITHUB_USER_NAME/LearningGit.git # 拉取远程仓库时有默认分支，此处假设是master分支
# 假设你需要在dev分支上开发，需要拉取远程的dev分支到本地
git checkout -b dev origin/dev # 在本地创建和远程分支对应的分支
git pull # 拉取最新提交的origin/dev分支，然后在本地合并，解决冲突，再推送至远程分支
git branch --set-upstream-to=origin/<branchName> <branchName> # 指定本地branchName与远程origin/branchName分支的链接
```

