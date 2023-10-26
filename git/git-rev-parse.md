# git rev-parse

在 Git 中，git rev-parse 命令用于解析 Git 引用（如分支、标签和提交 ID），并将其转换为对应的 SHA-1 值。该命令可以用于获取 Git 对象的 SHA-1 值，以及检查 Git 引用是否存在、是否是合法的等。

## 解析tag
```
git rev-parse 1.0
7e9a18e020c93b058b2e7d0f42560ff03fd76e54

git cat-file -t 7e9a18e020c93b058b2e7d0f42560ff03fd76e54
tag
```
对tag的这个sha值，检查类型，可能是tag，也可能是commit

## 解析branch
```
git rev-parse main
```
将返回main分支的最新提交的SHA-1 值


## 判断某个引用是否存在
```
if git rev-parse --verify <ref_name> >/dev/null 2>&1
then
    echo "<ref_name> exists"
else
    echo "<ref_name> does not exist"
fi


if git show-ref --verify --quiet refs/remotes/origin/release-1.0; then
    echo "repo have branch release-1.0"
else
    echo "error:repo do not have branch release-1.0"
    exit -1
fi
```

## 显示路径
```
显示工作区根目录。
git rev-parse --show-toplevel
/path/to/my/workspace/demo

显示版本库.git目录所在的位置
git rev-parse --git-dir
/path/to/my/workspace/demo/.git
```