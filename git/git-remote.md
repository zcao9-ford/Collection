# git remote

## 添加远程仓库
```
指定本地仓库的远程仓库
git remote add [shortname] [url]

shortname   远程仓库的别名
url         远程仓库的地址

```

## 查看远程仓库

```
查看远程仓库地址
git remote -v

查看远程仓库信息。比git remote -v更加详细
git remote show [shortname]

```

## 移除远程仓库

```
移除本地仓库的远程仓库

git remote remove [shortname]
```

## 重命名远程仓库
```
git remote rename old new
```