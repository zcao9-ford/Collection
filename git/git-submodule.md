# 子模块

需要在.gitsubmodule文件中配置子模块的信息
```
[submodule "submodule-name"]
	path = submodule-path
	url = submodule-url
    branch = main
```

## 初始化
```
git submodule update --init --recursive --remote
```
执行完命令后，会在项目的.git目录下创建modules目录。然后会在工程中获取对应代码

## 删除子模块
移除modules的代码，但是不会删除在.git目录下生成modules目录
```
git submodule deinit -f submodule-name
rm -rf .git/modules/submodule-name
```