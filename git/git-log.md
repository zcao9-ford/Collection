# git log

用于查看我们提交的 git 历史记录信息，同时，git log 命令可以通过多种不同的参数，定制显示的样式。

## 显示指定数量的信息
语法：
```
git log -n <number>
git log -<number>
```
案例：
```
git log -2
git log -n 3
```

## 查看某个文件的提交信息
```
git log file
```

## 显示某个作者的提交信息
```
git log --author <name>
```

## 限定指定时间段段信息
```
git log --before 
git log --after 
```
案例
```
git log --before "2022-05-11 21:21"
```

## 加入增删改查信息
```
git log --stat # 输出每个 commit增删改查的文件
git log -p   #输出每个 commit 具体修改的内容，输出的形式以 diff 的形式给出。
```

## log信息格式

```
git log --oneline  #用单行的信息展示
git log --graph  #以简单的图形方式列出提交记录。
```

## 自定义展示格式
```
git log --pretty=format:"format"

格式化format字符串:
%H  提交对象（commit）的完整哈希字串。
%h  提交对象的简短哈希字串。
%T  树对象（tree）的完整哈希字串。
%t  树对象的简短哈希字串。
%P  父对象（parent）的完整哈希字串。
%p  父对象的简短哈希字串。
%an 作者（author）的名字。
%ae 作者的电子邮件地址。
%ad 作者修订日期
%cn 提交者(committer)的名字。
%ce 提交者的电子邮件地址。
%cd 提交日期。
%s  提交说明。
```

案例
```
git log --pretty=format:"%H"
git log --pretty=format:"%an %ae %ad %cn %ce %cd %cr %s"
```

