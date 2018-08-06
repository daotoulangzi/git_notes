#### 记录常用的 git 应用场景及 git 指令
---
- 1、统计代码量，日期+作者

```
git log --author="wangjian" --since ==2018-06-01 --until=2018-06-30 --pretty=tformat: --numstat | gawk '{ add += $1 ; subs += $2 ; loc += $1 } END { printf "added lines: %s removed lines : %s total lines: %s\n",add,subs,loc }' -
```

- 2、git 查看提交以及改动的文件

`git log -n 10 --stat`

- 3、git 的 pull 和 fetch 区别，以及设置自动 rebase
```
1、git pull
git pull 是将远程仓库拉取到本地仓库，如果本地仓库的提交均为远程分支的历史提交，则直接更新本地仓库的分支；
如果本地分支有新的提交，如果没有冲突，则直接跳到 merge 界面，如果有冲突，提示修改冲突。

2、git fetch
git fetch 是将远程仓库分支 commits 拉取到本地仓库分支，但是它不是直接将 commits 接在本地分支的最后面，而是从最后一次 push 的 commit 节点，
再拉取一个新的分支，此时使用 git merge 即 git fetch + git merge = git pull。但是此时图谱将不再是一条直线。

3、使用 git fetch + git rebase 可以将图谱变成一条直线。

4、git merge 粗暴，直接将两个分支各自拉出一条线，连在一起；git rebase 比较细心，会把当前分支跟你要合并的分支不同的 commits 取消掉，临时保存起来；
然后在要合并的分支上再把保存起来的 commits patch 上去，编程新的 commits 。这样就是一条直线了。

# git pull 设置自动 rebase，此时 git pull = git fetch + git rebase
git config branch.dev.rebase true   # 对已建好的分支，如dev
git config --global branch.autosetuprebase always    # 对于所有的新建分支
```

- 4、git 将其他分支的 commit 提交到当前分支
```
git checkout dev
git cherry-pick commit_id    # commit_id 为其他分支的 commit
```

- 5、git 将其他分支的多个连续的 commits 提交到当前分支
```
# 如当前分支：dev,其他分支：feature
# 基于 feature 创建一个新的分支，并且分支指向要合并的结束 commit
git checkout -b temp_branch last_commit_id

# rebase 这个分支的 commit 到 dev 分支（76cada为需要提交的起始 commit 号）
git rebase --onto dev 76cada^
```
