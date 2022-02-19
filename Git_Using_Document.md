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