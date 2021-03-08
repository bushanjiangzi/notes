## 同时提交到 gitee 和 GitHub

1. 先删除已关联的名为 origin 的远程库

`git remote rm origin`

2. 分别关联 GitHub 和码云的远程库

```
git remote add github https://github.com/bushanjiangzi/notes.git
git remote add gitee https://gitee.com/bushanjiangzi/notes.git
```

3. 修改[remote "origin"]中`origin`分别为 github 和 gitee

```
[remote "github"]
	url = https://github.com/bushanjiangzi/notes.git
	fetch = +refs/heads/*:refs/remotes/github/*
[remote "gitee"]
	url = https://gitee.com/bushanjiangzi/notes.git
	fetch = +refs/heads/*:refs/remotes/gitee/*
```

4. 用`git remote -v`查看远程库信息

```
gitee   https://gitee.com/bushanjiangzi/notes.git (fetch)
gitee   https://gitee.com/bushanjiangzi/notes.git (push)
github  https://github.com/bushanjiangzi/notes.git (fetch)
github  https://github.com/bushanjiangzi/notes.git (push)
```

5. 上传代码

```
git add .
git commit -m "update"

# 提交到github
git push github master

# 提交到码云
git push gitee master
```

6. 拉取代码

```
# 从github拉取更新
git pull github

# 从gitee拉取更新
git pull gitee
```
