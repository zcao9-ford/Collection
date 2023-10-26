# git rm 移除文件

git rm 命令用于从索引中删除文件或者同时从工作区和索引中删除文件。

主要是git rm，git rm --cached和rm的区别
```
git rm  file
同时从工作区和索引中删除文件，即本地的文件也被删除了。

git rm --cached file
从索引中删除文件，但是本地文件还存在， 只是不希望这个文件被版本控制。

rm file
删除文件，仅仅是删除了物理文件，没有将其从 git 的记录中剔除。
```