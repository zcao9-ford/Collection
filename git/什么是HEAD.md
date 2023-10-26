首先，查看.git目录下HEAD文件
```
$ cat .git/HEAD
ref: refs/heads/master
```
可以看到，HEAD: 指向一个引用：refs/heads/master。
这个引用在文件.git/refs/heads/master中
```
$ cat .git/refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
```
用**git cat-file**命令进行查看，可以看到他是一个commit id
```
$ git cat-file -t e695606
commit

$ git cat-file -p e695606fc5e31b2ff9038a48a3d363f4c21a3d86
tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

which version checked in?

```

**目录.git/refs是保存引用的命名空间，其中.git/refs/heads目录下的引用又称为分支。对于分支既可以使用正规的长格式的表示法，如refs/heads/master，也可以去掉前面的两级目录用master来表示。**

Git 有一个底层命令**git rev-parse**可以用于显示引用对应的提交ID。
```
$ git rev-parse master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
$ git rev-parse refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
$ git rev-parse HEAD
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
```