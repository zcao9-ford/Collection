# git commit

git commit 命令主要是将暂存区里的改动提交到本地的版本库。每次使用 git commit 命令我们都会在本地版本库生成一个 40 位的哈希值，这个哈希值也叫 commit-id。

## 常用语句
```
# 提交暂存区到仓库区
$ git commit -m [message]

# 提交暂存区的指定文件到仓库区
$ git commit [file1] [file2] ... -m [message]

# 使用一次新的commit，替代上一次提交
# 如果代码没有任何新变化，则用来改写上一次commit的提交信息
$ git commit --amend -m [message]
```

## git对象库揭秘
使用git log 命令后，同时添加`--pretty=raw`参数以便显示每个提交对象的parent属性
```
$ git log -1 --pretty=raw
commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

    which version checked in?
```
一个提交中居然包含了三个SHA1哈希值表示的对象ID。

### git cat-file
用来展示对象库大小，内容等信息
```
-t   Instead of the content, show the object type identified by <object>

-p   Pretty-print the contents of <object> based on its type
```

然后，使用**git cat-file -t**命令，来研究这3个对象ID
```
$ git cat-file -t e695606
commit
$ git cat-file -t f58d
tree
$ git cat-file -t a0c6
commit
```
接着，用**git cat-file -p**命令，查看这几个对象的内容
```
$ git cat-file -p e695606
tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

which version checked in?


$ git cat-file -p f58da9a
100644 blob fd3c069c1de4f4bc9b15940f490aeb48852f3c42    welcome.txt
```
对于commit对象，保存的是该commit对应的log信息。

而在上面目录树（tree）对象中看到了一个新的类型的对象：blob对象。
```
$ git cat-file -t fd3c069c1de4f4bc9b15940f490aeb48852f3c42
blob

$ git cat-file -p fd3c069c1de4f4bc9b15940f490aeb48852f3c42
Hello.
Nice to meet you.
```
这个blob对象保存着文件welcome.txt的内容。

这些所有的对象都保存在Git库中的objects目录下（ID的前两位作为目录名，后38位作为文件名）。