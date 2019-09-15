# Learn Git

[廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)

零散的命令：

1. 显示当前目录——*pwd*
2. 显示隐藏目录——*ls -ah*
3. 查看文件 —— *cat + <文件名>*

#### 创建版本库

1. 创建空目录，并进入该目录
2. 通过***git init***命令把这个目录变成Git可以管理的仓库

#### 把文件添加到版本库

1. 用命令***git add***把文件添加到仓库

2. 用命令***git commit***把文件提交到库

   - -m 后面输入的使本次提交的说明

   可以多次***add***不同文件，在**commit**一次性上传多个文件

   ```
   $git add readme.txt
   $git add file1.txt file2.txt
   $git commit -m "add 3 files"
   ```

   

### 时光机穿梭

1. 运行***git status***命令查看仓库当前的状态，文件是否修改，是否添加入库否

2. 使用***dit diff*** ***+ 文件名*** 命令查看文件修改

   ***git diff*** 查看到的是工作区和暂存区的不同

   ***git diff --cached*** 查看到的是暂存区和HEAD的不同

#### 版本回退

1. 使用***git log***命令显示从最近到最远的提交日志，查看提交历史，以便确定回退到哪个版本

   *--pretty=oneline* 输出信息简洁化, 输出的一段神奇的数字是*commit id*(版本号)

2. 使用***git reset --hard HEAD^***（几个^，往上回退几个版本）回退（n）个版本

   也可以写成***git reset --hard HEAD~n***(个数)

3. 使用 ***git reset --hard <版本号>*** 在各个版本穿梭

4. 使用***git reflog*** 查看命令历史，以便要回到未来的哪个版本

#### 工作区和暂存区

工作区（Working Directoty）: 电脑能看到的目录

版本库（Repository）: 工作区有一个隐藏目录*.git*，是*Git*的版本库

***git add*** 命令实际上是把要提交的所有修改放在暂存区（*Stage*）,然后执行***git commit*** 可以一次性把暂存区的所有修改提交到分支(*master*)

![git-stage](https://www.liaoxuefeng.com/files/attachments/919020074026336/0)

![git-stage-after-commit](https://www.liaoxuefeng.com/files/attachments/919020100829536/0)



#### 管理修改

使用***git diff HEAD -- <文件名>*** 查看工作区和版本库里最新版本的区别

每次修改，如果不用***git add*** 到暂存区，那就不会加入到***commit***中。



#### 撤销修改

1. 使用 ***git checkout --<文件名>*** 可以丢弃工作区的修改

2. 使用 ***git reset HEAD <文件名>*** 可以把暂存区的修改撤销*（unstage）*,重新放回工作区 

3. 使用 ***git reset HEAD^ <文件名>*** 回退到上个版本

   如果提交到远程库就没办法了



#### 删除文件

1. 从版本库删除该文件
   - 使用 ***git rm <文件名>*** 删除该文件，并且 ***git commit***
2. 恢复本地误删文件
   - 本地误删后，使用 ***git checkout -- <文件名>*** 从版本库恢复误删文件



### 远程仓库

[本步骤见教程](https://www.liaoxuefeng.com/wiki/896043488029600/896954117292416)

本地Git仓库和gitHub仓库建立连接

1. 创建SSH key

   ```
   ssh-keygen -t rsa -C "youremail@example. com"
   ```

   一路回车，使用默认值，但是记得设置***passphrase***密码，这是后面要用的！

2. 登录GitHub,打开***setting***界面，然后打开***SSH and GPG keys***界面。

   点击***New SSH key***, title随意， key框中粘贴 用户主目录/.ssh目录/***id_rsa.pub***文件中的内容

#### 添加远程库

1. 登录Github， 创建新的仓库（Create a new repository）
2. 把本地仓库的内容推送到GitHub仓库
   
   - 本地仓库与gitHub建立的仓库相连接
   
      ```
      $ git remote add origin git@github.com:<自己的用户名>/<仓库名>.git 
      ```

	​	origin是远程库的名字，是Git默认的叫法

   - 把本地库的所有内容推送到远程库上（这里可能需要输入passphrase密码）
   
      ```
      $ git push <可填-u> origin master
      ```
   
     ***-u***  如果远程库是空的，我们第一次推送master分支时，加上了***-u***参数，Git不但会把本地的*master*分支内容推送的远程新的*master*分支，还会把本地的*master*分支和远程的*master*分支关联起来，在以后的推送或者拉取时就可以简化命令。
     
     

#### 从远程库克隆

​	使用命令***git clone*** 克隆一个远程库

```
#git clone git@github.com:<用户名>/<库名>.git
```



### 分支管理

分支就是科幻电影里面的平行宇宙，当你正在电脑前努力学习Git的时候，另一个你正在另一个平行宇宙里努力学习SVN。

如果两个平行宇宙互不干扰，那对现在的你也没啥影响。不过，在某个时间点，两个平行宇宙合并了，结果，你既学会了Git又学会了SVN！（还是廖老师的例子好）

#### 创建与合并分支

1. 查看当前分支 —— ***git branch***

2. 创建分支 

   - ***git branch <分支名称>***

   - ***git switch -c <分支名称>***

     ***-c*** 表示创建并切换

   - ***git checkout -b 分支名称***

     ***-b*** 表示创建并切换，相当于以下两条命令

     ```
     $ git branch dev
     $ git checkout dev
     ```

3. 切换分支

   - ***git checkout <分支名称>***
   - ***git switch <分支名称>***
4. 将XXX分支合并到当前分支上 —— ***git merge XXX***

5. 删除分支 —— ***get branch -d <分支名称>***

#### 解决冲突

对相同文件，不同分支各自都有新的提交，这种情况下，Git无法执行“快速合并”，必须手动解决冲突后再提交。

使用***git merge <分支名>*** 后，存在冲突的文件内容会出出现“ <<<<<<<，=======，>>>>>>> ”。Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容。

使用***git status*** 可以知道产生冲突的文件。

对冲突的文件进行修改，修改之后重新提交，产生冲突的两个分支合并。

使用***git log -- graph***命令可以**查看分支的合并情况**，输入q退出

```
$ git log --graph --pretty=oneline --abbrev-commit
```

  

#### 分支管理策略

通常，合并分支时，如果可能，Git会用*Fast forward*模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用*Fast forward*模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

禁用*Fast forward*模式合并分支，须添加 ***--no-ff*** 参数，且合并要创建一个新的commit，所以加上***-m***参数，把commit描述写进去。

```
$ git merge --no-ff -m "描述" 分支名称
```

 合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。（这个我没明白，求解答（＞人＜；））



#### Bug分支

保存工作现场 —— ***git stash***

查看工作现场 —— ***git stash list***

恢复工作现场

1. ***git stash apply*** 恢复后，stash内容并不删除，需要用 ***git stash drop*** 来删除

2. ***git stash pop*** , 恢复的同时把stash内容也删除了

修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

当手头工作没有完成时，先把工作现场***git stash***一下，然后去修复bug，修复后，再***git stash pop***，回到工作现场；

在master分支上修复的bug，想要合并到当前dev分支，可以用***git cherry-pick <commit>***命令，把bug提交的修改“复制”到当前分支，避免重复劳动。

总的来说，就是，在分支下进行的工作，如果不commit的话，回到master，就会显示出你在分支下你添加的工作。这个时候，你在master下修改完bug提交后，正在分支进行的工作也会提交了。为了避免这个情况，你就在分支下，git stash将工作隐藏，这个时候，切换到master时候，修改了bug，提交。分支的内容不会被提交上去。（这是下面的评论，但是说得很明确）