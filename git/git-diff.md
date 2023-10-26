# git diff

git diff 命令用于比较两个文件之间的不同，git diff 命令可以用来比较工作区、暂存区、工作目录以及两个分支之间的差异。

Git对差异比较进行了扩展。

## 比较工作区和暂存区的差异

假设当前暂存区有文件first.txt
```
tstone10@MGC12GX28JQ05N[main]
-> % git status -s
tstone10@MGC12GX28JQ05N[main]
-> % cat first.txt
abcd%
```
然后，我们修改first.txt，新增一行内容
```
tstone10@MGC12GX28JQ05N [main *]
-> % git status -s
 M first.txt
tstone10@MGC12GX28JQ05N [main *]
-> % cat first.txt
abcd
zxcv%
```
此时，我们用`git diff`命令查看差异变化
```
diff --git a/first.txt b/first.txt
index 85df507..dfb64b0 100644
--- a/first.txt
+++ b/first.txt
@@ -1 +1,2 @@
-abcd
\ No newline at end of file
+abcd
+zxcv
\ No newline at end of file
(END)
```

## 比较暂存区和里程碑

加入--cached可以比较暂存区和里程碑的差别。 
```
git commit -m 'commit first.txt'

echo "new content" >> first.txt
git add first.txt

git diff --cached first.txt
```

## 其他比较

-   比较里程碑B和里程碑A，用命令：**git diff B A**
-   比较工作区和里程碑A，用命令：**git diff A**
-   比较暂存区和里程碑A，用命令：**git diff –cached A**
-   比较工作区和暂存区，用命令：**git diff**
-   比较暂存区和HEAD，用命令：**git diff –cached**
-   比较工作区和HEAD，用命令：**git diff HEAD**