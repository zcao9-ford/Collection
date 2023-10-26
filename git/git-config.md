# git config

git的配置文件采用**INI**文件格式。
git config 用于读取和更改INI配置文件中的内容

## 作用范围
分为**system，global，local**
```
git config -e --system         
# 会在/etc/gitconfig系统级配置文件中编辑

git config -e --global         
# 会在用户主目录下编辑.gitconfig文件，进行全局配置

git config -e --local          
# 当前git仓库的.git/config文件中编辑
```

优先级: local >  global > system

## git config语法
```
读取
git config <section>.<key>

设置
git config <section>.<key>  <value>

移除
git config --unset <section>.<key>
```

### 示例
```

-> % git config --global core.filemode
true

-> % git config --global user.name apple
-> % git config --global user.email apple@google.com
-> % cat config

[user]
    name = apple
    email = apple@google.com

-> % git config --global --unset user.name
-> % git config --global --unset user.email

```


### 其他
```
git config https.proxy internet.proxy.com
git config url."git@github.com:".insteadOf "https://github.com" 

git config pull.rebase false  # merge
git config pull.rebase true   # rebase
git config pull.ff only       # fast-forward only
```
