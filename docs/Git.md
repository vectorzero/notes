## .gitignore不生效

.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。

解决方法就是先把本地缓存删除（改变成未track状态），然后再提交:

git rm -r --cached .

git add .

git commit -m 'update .gitignore'


## 配置别名

```cmd
// 第一步：设置全局别名lg
git config --global alias.hello "log"
// 第二步：输入设置别名的命令
git hello
```

## 创建标签

```cmd
// 1. 创建标签，默认是打在最后一次提交的内容上的 (<tagname> 代表你要输入的标签名字)
git tag <tagname> 
// 2.查看标签
git tag
// 3.在历史版本上打上标签 (num 代表提交时的hash值)
git tag num 
// 删除标签
git tag -d <tagname> 
// 添加描述信息(-a 后面指定标签名 -m 后面指定说明文字)
git tag -a v0.1 -m "version 0.1 released"  
```

## cherry-pick

`git cherry-pick`可以选择某一个分支中的一个或几个commit(s)来进行操作。

例如，假设我们有个稳定版本的分支，叫v2.0，另外还有个开发版本的分支v3.0，我们不能直接把两个分支合并，这样会导致稳定版本混乱，但是又想增加一个v3.0中的功能到v2.0中，这里就可以使用cherry-pick了,其实也就是对已经存在的commit 进行再次提交.



`git cherry-pick <commit id>` 单独合并一个提交

注意：当执行完 cherry-pick 以后，将会生成一个新的提交；这个新的提交的哈希值和原来的不同，但标识名一样；

`git cherry-pick  -x <commit id>` 同上，不同点：保留原提交者信息。

Git从1.7.2版本开始支持批量cherry-pick，就是一次可以cherry-pick一个区间的commit。

```
git cherry-pick <start-commit-id>..<end-commit-id>
git cherry-pick <start-commit-id>^..<end-commit-id>
```

前者表示把`<start-commit-id>`到`<end-commit-id>`之间(左开右闭，不包含start-commit-id)的提交cherry-pick到当前分支；

后者有"^"标志的表示把`<start-commit-id>`到`<end-commit-id>`之间(闭区间，包含start-commit-id)的提交cherry-pick到当前分支。

其中，`<start-commit-id>`到`<end-commit-id>`只需要commit-id的前6位即可，并且`<start-commit-id>`在时间上必须早于`<end-commit-id>`

注：以上合并，需要手动push代码。
