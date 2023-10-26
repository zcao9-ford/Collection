# git push

将本地分支的更新，推动到远程

```
git push <远程主机名> <本地分支名>:<远程分支名>

```
这里的 : 前后是必须没有空格的。
```
git push
git push origin
git push origin master

git push origin :branch   删除远程仓库的分支
git push origin --tags    把 tag 推送到远端仓库。
```

## 快进式推送
但是推送在多人合作时，存在一些问题。

假设，现在用户user1和user2的工作区是相同的。思考一个问题：如果两人各自在本地版本库中进行独立的提交，然后再分别向共享版本库推送，会互相覆盖么？

假设 user1 优先提交了,更新了“远程”共享版本库
```
$ git log --oneline --graph
* b4f3ae0 user1's profile.
* 5174bf3 initial commit.
```
如果用户user2在不知道用户user1所做的上述操作的前提下，在基于“远程”版本库旧的数据同步过来的本地版本库中进行改动。
```
$ cd /path/to/user2/workspace/project/
$ mkdir team
$ echo "I'm user1?" > team/user2.txt
$ git add team
$ git commit -m "user2's profile."
[master 8409e4c] user2's profile.
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 team/user2.txt
```
用户user2将本地提交推送到服务器时出错。
```
$ git push
To file:///path/to/repos/shared.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'file:///path/to/repos/shared.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.
```

Git通过对推送操作是否是“快进式”操作进行检查，从而保证用户的提交不会相互覆盖。一般情况下，推送只允许“快进式”推送。

==所谓**快进式推送**，就是要推送的本地版本库的提交是建立在远程版本库相应分支的现有提交基础上的，即远程版本库相应分支的最新提交是本地版本库最新提交的祖先提交。==

### 非快进式推送强制更新
在推送命令的后面使用-f参数可以进行强制推送。
但是这样用户user1向版本库推送的提交由于用户user2的强制推送被覆盖了。实际上在这种情况下user1也可以强制的推送，从而用自己（user1）的提交再去覆盖用户user2的提交。

==所以，使用非快进式推送强制更新版本库，实际上是危险和错误的==

### 合并后推送
如果在向服务器推送过程中遇到了非快进式推送的警告，应该进行如下操作才更为理性：执行**git pull**获取服务器端最新的提交并和本地提交进行合并，合并成功后再向服务器提交。

合并之后，看看版本库的提交关系图。
合并之后远程服务器中的最新提交6b1a7a0成为当前最新提交（合并提交）的父提交。如果再推送，则不再是非快进式的了。
```
$ git log --graph --oneline
*   bccc620 Merge branch 'master' of file:///path/to/repos/shared
|\
| * 6b1a7a0 user2's profile.
* | b4f3ae0 user1's profile.
|/
* 5174bf3 initial commit.
```

### 禁止非快进式推送
将版本库的参数receive.denyNonFastForwards设置为true可以禁止任何用户进行非快进式推送
```
$ git config receive.denyNonFastForwards true
```
此时，用户user1使用强制推送也会失败