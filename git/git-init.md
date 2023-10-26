# git init

git init 命令用来初始化一个空的 git 本地仓库。执行完 git init 命令，当前目录下会自动生成 .git 隐藏文件夹，该隐藏文件夹就是 git 版本库。

## .git目录
目录结构如下
```
-rw-r--r-- HEAD
-rw-r--r-- config
-rw-r--r-- description
drwxr-xr-x hooks
drwxr-xr-x info
drwxr-xr-x objects
drwxr-xr-x refs
drwxr-xr-x logs
```

### HEAD
记录当前激活的分支，其内容指向具体的分支。或者是一个具体的commit-id值

### config
本地仓库配置，通过git config --local查看或者设置

### hooks/
目录存放的是一些hook脚本

### logs/
存储引用的历史变更记录。

### info/
仓库描述信息目录。一般只有exclude这个文件,描述哪些文件排除在版本控制之外，和 .gitignore 文件功能类似

### refs/
git 引用，该目录下存储 git 的所有引用。包含tag，branch

### index
二进制文件。实际上就是一个包含文件索引的目录树，像是一个虚拟的工作区。在这个虚拟工作区的目录树中，记录了文件名、文件的状态信息（时间戳、文件长度等）。文件的内容并不存储其中，而是保存在Git对象库.git/objects目录中，文件索引建立了文件和对象库中对象实体之间的对应

### objects/
松散对象。新创建的对象都是存在这些目录中，对象的文件名称时对象内容的sha1值，取sha1值的前2个字符作为子目录
