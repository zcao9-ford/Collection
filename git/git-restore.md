# git restore

现在，我们想放弃本地工作区的修改，我们可以使用 git restore 命令。

假设我们修改了文件A，想要放弃修改,则使用
```
git restore A
```

假设我们已经将文件A放到了暂存区。此时我们想要完全放弃修改
```
git restore --staged A
git restore A
```