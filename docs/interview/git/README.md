## 在 Git 中，你如何还原已经 push 并公开的提交

- 删除或修复新提交中的错误文件，并将其推送到远程存储库。这是修复错误的最自然方式。对文件进行必要的修改后，将其提交到我将使用的远程存储库
  ```js
  git commit -m "commit message"
  ```
- 创建一个新的提交，撤消在错误提交中所做的所有更改。可以使用命令：
  ```js
  git revert <name of bad commit>
  ```

## git pull 和 git fetch
* git pull 命令从中央存储库中提取特定分支的新更改或提交，并更新本地存储库中的目标分支。

* git fetch 也用于相同的目的，但它的工作方式略有不同。当你执行 git fetch 时，它会从所需的分支中提取所有新提交，并将其存储在本地存储库中的新分支中。如果要在目标分支中反映这些更改，必须在 git fetch 之后执行git merge。只有在对目标分支和获取的分支进行合并后才会更新目标分支。
## 创建、删除分支
```js
  git branch //查看分支
  git checkout // 切换分支
  git checkout -b //切换并创建分支
  git branch -d // 删除分支
  git branch -D //强行删除
```
## git stash

  stash 会将你的工作目录，即修改后的跟踪文件和暂存的更改保存在一堆未完成的更改中，你可以随时重新应用这些更改。

## git在pull后想要回滚到之前版本
git reflog master  (查看本地master分支历史变动纪录)

使用git reset --hard <COMMIT_ID> （恢复到之前位置）
或git reset --hard master@{1}
## git clone指定分支
`git clone -b {分支名} url`
## git merge和git rebase
都是用来合并分支的。
`git merge b`， 将b分支合并到当前分支，

同样 `git rebase b`，也是把 b分支合并到当前分支
### marge:
自动创建一个新的`commit`,如果合并的时候遇到冲突，仅需要修改后重新`commit`.
- 优点：记录了真实的`commit`情况，包括每个分支的详情。
- 缺点：因为每次merge会自动产生一个`merge commit`，所以在使用一些git 的GUI tools，特别是commit比较频繁时，看到分支很杂乱。
### rebase:
会合并之前的commit历史
- 优点：得到更简洁的项目历史，去掉了`merge commit`
- 缺点：如果合并出现代码问题不容易定位，因为re-write了history