shell 是指一个面向用户的命令接口，表现形式就是一个可以由用户录入的界面，这个界面也可以反馈运行信息

## shell脚本
用户事先编写一个 sh 脚本文件，内含 shell 脚本，而后使用 shell 程序执行该脚本，这种方式，我们习惯称为 shell 编程。

shell 脚本中包含一些编程元素：
* if…else 选择结构，case…in 开关语句，for、while、until 循环；
* 变量、数组、字符串、注释、加减乘除、逻辑运算等概念；
* 函数，包括用户自定义的函数和内置函数

## 查看当前shell
在linux机器上，默认都是使用bash。而在mac机器上，默认的是zsh。

查看支持的shell
```
-> % cat /etc/shells
/bin/bash
/bin/csh
/bin/dash
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh

```
查看默认的shell
```
-> % echo $SHELL
/bin/zsh
```

## login shell与非login Shell
### login shell

包含两种情况：
1. 直接通过终端输入帐号密码登录,或者ssh连接
2. 使用 `su - username` 切换的用户

登录式shell的配置文件以 `/etc/profile` 为入口，然后嵌套引入其他配置文件。
**常见的加载顺序**通常为
 1. /etc/profile
 2. /etc/bash.bashrc
 3. ~/.bash_profile
 4. ~/.bashrc

#### 验证
在mac机器上准备两个文件，然后远程ssh登陆
```
File1: .zprofile:
export http_proxy=zprofile
export AGE=20

File2: .zshrc:
export http_proxy=internet.ford.com
```
执行:
```
tstone10@MGC12GX28JQ05N[~]
-> % echo $0
-zsh    # -  表示是login shell
tstone10@MGC12GX28JQ05N[~]
-> % echo $AGE
20
tstone10@MGC12GX28JQ05N[~]
-> % echo $http_proxy
http://internet.ford.com
```


### 非登陆式shell

包含这几种情况：
1.  su username
2.  图形界面下打开的终端
3.  执行脚本
4.  其他任何bash实例

常见的加载顺序为:
```
~/.bashrc -> /etc/bashrc -> /etc/profile.d/*.sh
```

## 父shell与子shell
当在执行一个脚本时，父Shell会根据脚本程序的第一行 `#!` 之后指定的解释器开启一个 子Shell 环境，然后在子Shell中执行此脚本。
但是一旦，子Shell 中的脚本执行完毕，此子Shell随即结束，回到父Shell中，不会影响父Shell原本的环境。
也就是说export一个变量后，父shell是不会读取到该变量


### 如何判断父shell与子shell?
通过进程ID，执行ps -f

```
tstone10@MGC12GX28JQ05N[~]
-> % ps -f
  UID   PID  PPID   C STIME   TTY           TIME CMD
  501 14251 14250   0  3:25下午 ttys000    0:00.53 -zsh
tstone10@MGC12GX28JQ05N[~]
-> % zsh                 # 可以通过这种方式开启一个子shell
tstone10@MGC12GX28JQ05N[~]
-> % ps -f
  UID   PID  PPID   C STIME   TTY           TIME CMD
  501 14251 14250   0  3:25下午 ttys000    0:00.53 -zsh
  501 14448 14251   0  3:32下午 ttys000    0:00.20 zsh


> PS 命令:
> UID 程序被该UID所拥有
> PID 代表这个程序的ID
> PPID 代表其上级父程序的ID
```
由上可以看到子shell还是一个非login shell
