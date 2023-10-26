# git clone

git clone用来从远程主机克隆一个版本库。

语法
```
git clone [option] url [dir]
```

## option
### --origin name
可以指定命名上流仓库的名字，替换默认的origin

### --branch name
检出到指定分支

### --depth
浅拷贝，只会克隆指定数目的commit历史信息。
## url
git clone 命令支持多种协议，比如：HTTP(s)、SSH、Git、本地文件协议。
```
    ssh://[user@]host.xz[:port]/path/to/repo.git/ 
    
    git://host.xz[:port]/path/to/repo.git/

    http[s]://host.xz[:port]/path/to/repo.git/

    ftp[s]://host.xz[:port]/path/to/repo.git/
```

对于需要权限才能访问的仓库u，可以使用token，如下
```
git clone https://oauth2:token@github.com/username/repo.git

git clone https://username:token@github.com/username/repo.git
```