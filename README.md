# git 常用知识总结
### 1、用户信息配置

     git config --global user.name “olifer"
     git config --global user.email olifer@example.com

   如果用了 --global 选项，那么更改的配置文件就是位于你用户主目录下的那个，以后你所有的项目都会默认使用这里配置的用户信息。如果要在某个特定的项目中使用其他名字或 者电邮，只要去掉 --global 选项重新配置即可，新的设定保存在当前项目的 .git/config 文件里。

   查看配置信息

      git config --list

### 2、取得项目的 Git 仓库

#### 2.1  在现存的目录下,通过导入所有文件来创建 新的 Git 仓库

    git init
    git add .       //.代表添加全部文件

  瞬间Git就把仓库建好后，当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

#### 2.2  从已有的 Git 仓库克隆出一个新的镜像仓库来

  克隆仓库的命令格式为 git clone [url]。比如，要克隆 Ruby 语言的 Git 代码仓库 Grit，可以用下面的命令：

    git clone git://github.com/schacon/grit.git

  如果希望在克隆的时候，自己定义要新建的项目目录名称，可以在上面的命令最后指定：

    git clone git://github.com/schacon/grit.git mygit

### 3、本地分支

#### 3.1 创建分支

    git branch [分支名称]

#### 3.2 切换分支

    git checkout [分支名称]      //简写：gco [分支名称]

##### 注：创建并切换可以合并为一条命令

    git checkout -b [分支名称]

#### 3.3 合并分支

    git checkout master
    git merge [要合并的分支名称]

#### 3.4 删除分支

    git branch -d [分支名称]

#### 3.5 冲突合并

    git merge [分支名称]
    git status

查看git状态后，为合并的文件会以unmerged状态出现。

    [master*]$ git status

     index.html: needs merge

    #  On branch master
    #  Changed but not updated:
    #     (use "git add <file>..." to update what will be committed)
    #     (use "git checkout -- <file>..." to discard changes in working directory)
    #  unmerged: index.html

#### 3.6 查看目前分支清单

    $ git branch

    iss53
    * master
    testing

注意看 master 分支前的 * 字符:它表示当前所在的分支。也就是说,如果现在提交 更新,master 分支将随着开发进度前移。若要查看各个分支最后一次 commit 信息,运行 git branch -v:

    $ git branch -v
    iss53 93b412c fix javascript issue
    * master 7a98805 Merge branch 'iss53'
    testing 782fd34 add scott to the author list in the readmes

要从该清单中筛选出你已经(或尚未)与当前分支合并的分支,可以用 --merge 和 -- no-merged 。比如 git branch -merge 查看哪些分支已被并 入当前分支:

    $ git branch --merged
    iss53
    * master

之前我们已经合并了 iss53,所以在这里会看到它。一般来说,列表中没有 * 的分支通常都可以用 git branch -d 来删掉。原因很简单,既然已经把它们所包含的工作整合到了 其他分支,删掉也不会损失什么。

我们会看到其余还未合并的分支。因为其中还包含未合并的工作,用 git branch -d 删 除该分支会导致失败:

    git branch -d [分支名称]

不过,如果你坚信你要删除它,可以用大写的删除选项 -D 强制执行,例如 git branch -D testing。


### 4、远程分支

#### 4.1 查看当前的远程仓库

    git remote

也可以加上 -v （-verbose的简写）选项，显示对应得克隆地址：

#### 4.2 添加远程仓库

    git remote add [shortname] [url]

    例如：git remote add ziran git@github.com:olifer655/sakura.git

现在可以用ziran 指代对应的仓库地址了。

    git fetch ziran
    及 git fetch [remote-name]

#### 4.3 推送数据到远程仓库

    git push [remote-name] [branch-name]

如果使用了git commit --amend 提交commit，会存在信息被覆盖的情况，所以需要 -f 来强制推送。

#### 4.4 查看远程仓库信息

    git remote show [remote-name]

git remote show 不指定分支名称的情况，会将整条分支的内容显示出来

#### 4.5 远程仓库的删除和重命名

    git remote [remote-name]      //删除
    git remote rename A B

A为当前的名称，B 为将要修改后的名称

### 5、合衍（rebase）

    git rebase --onto master server client

这基本上等于在说“检出 client 分支,找出 client 分支和 server 分支的共同祖先之 后的变化,然后把它们在 master 上重演一遍”。

等价于

    git checkout master
    git merge client

##### 注：永远不要rebase那些已经推送到master上的分支

若要合并commit

    git rebase -i HEAD~2

### 6、查看本地与远程仓库分支的差异

    git diff

若要看已经暂存起来的文件和上次提交时的快照之间的差异,可以用 git diff --cached 命令。请注意,单单 git diff 不过是显示还没有暂存起来的改动,而不是这次工作和上次提交 之间的差异。

### 7、移除文件

    git rm [分支名称]

我们想把文件从 Git 仓库中删除(亦即从暂存区域移除),但仍然希 望保留在当前工作目录中。换句话说,仅是从跟踪清单中删除。比如一些大型日志文件或者 一堆 .a 编译文件,不小心纳入仓库后,要移除跟踪但不删除文件,以便稍后在 .gitignore 文件中补上,用 --cached 选项即可:

    git rm --cached readme.txt

### 8、修改文件名称

      git mv '旧' from '新' to

### 9、退到某个版本reset

    git reset –mixed：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
    git reset –soft：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
    git reset –hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容

### 10、在本地版本与线上版本之间解决冲突

 * 获取线上master的最新代码

  git fetch origin master

 * 然后进行rebase,并解决冲突

  git rebase origin/master

  git status

 * 解决冲突之后，继续rebase

  git add  

  git rebase --continue

  git push

### 11、更新本地分支，使其与远程分支同步

    git fetch origin master
    
所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。

使用git merge命令或者git rebase命令，在本地分支上合并

    $ git merge origin/master
    # 或者
    $ git rebase origin/master
