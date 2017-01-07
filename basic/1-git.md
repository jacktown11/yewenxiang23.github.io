---
title: git的一些命令操作
---

### 安装git

```bash
sudo apt-get update
sudo apt-get install git
sudo npm install git
```

> `apt-get` 是 `ubuntu` 系统（deepin其实就是ubuntu的一个变种）的软件安装命令
>

```bash
git --version
获取git的版本号
```

### git本地化的步骤

- 第一步：在项目文件夹中运行 `git init` 来初始化一个仓库,会建立一个.git 的隐藏文件夹。
- 第二步：`git add -A` 添加修改后的版本到 `.git` 文件夹中， `-A`是添加所有文件的意思
- 第三步: `git commit -m"留言内容"` 做成一个git本地的一个版本，`-m` 是 `message` 留言的意思，这个是必须的。

>这一步对于新装的git用户来说还要告诉git 用户名和邮箱。
>

```bash
git config --global user.name "yewenxiang"
git config --global user.email "yewenxiang23@gmail.com"
```
### 使用git上传本地仓库到github上

- 第一步：


### git 其他的一些命令

```bash
git log -p
```
`log` 是日志的意思, `-p`是patch (补丁,就是修改内容)的缩写

使用 `brew install tig` 安装tig包后，查看版本日志方便了很多，选中其中一个版本按 `D` 可以查看详细信息 按 `Q` 可以退出

### git 回滚的操作

- 修改后没做版本 回到上个 `git commit -m"..."` 的版本可以使用

```bash
git reset --hard HEAD
```

- 修改后做了版本 回到上个版本可以使用

```bash
git reset --hard HEAD~
```

- 在用户的主目录文件夹下有一个 `.gitconfig` 的隐藏文件，可以修改里面的属性来配置git.

```json
[alias]
throw = reset --hard HEAD
throwh = reset --hard HEAD~
```

>添加上述代码后 每次回滚操作只需输入 git throw 或者 git throwh 就可以了
>

### git 取消回滚的操作

`git log` 可以查看项目各个版本 也就是 `git commit -m"..."` 做的本地版本，有时候我们做了 *回滚* 的操作（此时，输入`git log`，由于回滚到了上一个版本，所以回滚前的版本不见了。 ），但是又需要改变为 *回滚前* 的状态使用以下步骤。

- `git reflog` 来查看记录的HEAD历史，当做reset,checkout等操作时候，这些操作会被记录在 `reflog` 中,就可以查看 *回滚前* 操作的版本的哈希值，取前7位

- `git reset --hard 7位哈希值` 就可以被找回reset操作的那个版本

>如果你因为reset等操作丢失一个提交的时候，你总是可以把它找回，除非你的操作已经被git当做垃圾处理了，一般是在30天后执行。
>

### git clone 命令

要想把 github 上的一个项目代码下载到本地有两种方式，一种就是普通下载（ download ）。但是，开发者 基本上会选择另外一种方式，就是 clone 。
```
git clone git@github.com:happypeter/digicity.git
```
clone 的特点就是不仅仅可以得到最新代码，而且可以得到整个改版历史。而普通下载只能得到最新版本。

### git 各个命令的作用

- `git push` 把本地仓库中有，而远端对应仓库中没有的版本推送到远端

- `git pull` 把远端仓库中有，而本地对应仓库中没有的版本拉到本地

- `git clone` 把远端仓库，克隆到本地

---

### 学习git资源

- [Atom 爱上 JS](http://haoqicat.com/atom-love-js)
- [驾驭命令行怪兽](http://haoqicat.com/ride-cli-monster)
- [Git北京](http://haoqicat.com/gitbeijing)
