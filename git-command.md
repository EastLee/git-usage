# Git远程操作详解

本文介绍几个git命令供查阅，如下：

* git clone
* git remote
* git fetch
* git pull
* git push

![git command](images/bg2014061202.jpg)

---

## git clone

从github上下载一个版本库

```git
$ git clone <版本库的网址>
```

比如获取此本版库

```git
$ git clone https://github.com/EastLee/git-usage
```

通常会在本地产生一个与远程版本库相同的目录，如果想改变本地版本库的名字，可以添加第二个参数

```git
$ git clone <版本库的网址> <本地目录名>
```

## git remote

列出所有远程主机名：

```git
$ git remote
```

使用-v选项，可以查看远程主机名网址信息

```git
$ git remote -v
```

克隆版本库时，远程主机名自动被命名为origin，如果想用其他主机名，可以使用-o选项

```git
$ git clone -o <主机名> <版本库网址>
```

查看主机名详细信息

```git
$ git remote show <主机名>
```

添加远程主机名

```git
$ git remote add <主机名> <版本库网址>
```

删除远程主机名

```git
$ git remote rm <主机名>
```

重命名主机名

```git
$ git remote rename <原主机名> <新主机名>
```

## git fetch

远程版本库有了更新，可以使用此命令将更新拉到本地

```git
$ git fetch <远程主机名>
```

默认取回所有分支的，如果想取回特定分支需要加一个分支名选项

```git
$ git fetch <远程主机名> <分支名>
```

比如

```git
$ git fetch origin master
```

所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。
git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。

```git
$ git branch -r
origin/master

$ git branch -a
* master
  remotes/origin/master
```

在去更新的基础，建立一个新分支

```git
$ git checkout -b newbranch origin/master
```

此外，也可以使用git merge命令或者git rebase命令，在本地分支上合并远程分支。

```git
$ git merge origin/master
# 或者
$ git rebase origin/master
```

## git pull

取回远程主机某个分支的更新，再与本地的指定分支合并

```git
$ git pull <远程主机名> <远程分支名>:<本地分支名>
```

比如拉取origin上的master分支与本地master分支合并

```git
$ git pull origin master:master
```

如果与当前分支合并，可以省略本地分支名

```git
$ git pull origin master
```

等同于

```git
$ git fetch origin
$ git merge origin/master
```

本地分支与远程分支也可以存在追踪关系，比如clone的时候，所有本地分支默认与远程主机的同名分支，建立追踪关系，也就是说，本地的master分支自动"追踪"origin/master分支。

也可以手动建立追踪关系

```git
git branch --set-upstream master origin/next
```

上面命令表明master分支追踪origin/next分支。

如果存在追踪关系

```git
$ git pull origin
```

上面命令表示本地分支与origin上存在追踪关系的分支合并

如果当前分支只有一个追踪分支，连远程主机名都可以省略。

```git
$ git pull
```

如果合并需要采用rebase模式，可以使用--rebase选项。

```git
$ git pull --rebase <远程主机名> <远程分支名>:<本地分支名>
```

如果远程主机删除了某个分支，默认情况下，git pull 不会在拉取远程分支的时候，删除对应的本地分支。这是为了防止，由于其他人操作了远程主机，导致git pull不知不觉删除了本地分支。
但是，你可以改变这个行为，加上参数 -p 就会在本地删除远程已经删除的分支。

```git
$ git pull -p
# 等同于下面的命令
$ git fetch --prune origin 
$ git fetch -p
```

## git push

将本地内容推送到远程版本库

```git
$ git push <远程主机名> <本地分支名>:<远程分支名>
```

如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。

```git
$ git push <远程主机名> <本地分支名>
```

如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

```git
$ git push origin :master
# 等同于
$ git push origin --delete master
```

如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

```git
$ git push origin
```

如果当前分支只有一个追踪分支，那么主机名都可以省略。

```git
$ git push
```

如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。

```git
$ git push -u origin master
```

不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。

```git
$ git config --global push.default matching
# 或者
$ git config --global push.default simple
```

还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用--all选项。

```git
$ git push --all origin
```

如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用--force选项。

```git
$ git push --force origin 
```

最后，git push不会推送标签（tag），除非使用--tags选项。

```git
$ git push origin --tags
```
