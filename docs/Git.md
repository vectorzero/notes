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