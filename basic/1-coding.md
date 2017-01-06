---
title: coding 操作
---

### 一些操作
```bash
git remote add origin git@git.coding.net:yewenxiang/yewenxiang.git
```

修改 `.git/confing` 文件中的属性，添加仓库的地址

```
git push -u origin maste
```

推送仓库当前版本到主分支之上
```
ssh-keygen
```
生成一个公钥和私钥 在 `cd ~/.ss` 文件夹下
```
git branch
```
查看本地的分支
```
git checkout -b coding-pages
```
在本地创建一个 `coding-pages` 分支
```
git push -u origin coding-pages
```
首次执行时，在远端创建 `coding-pages` 分支，并且把本地的代码上传到这个分支之上
```

>
>后续推送代码只需执行 `git push` 即可
