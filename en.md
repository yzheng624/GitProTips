# Git Pro Tips

This is still under development.

This is **NOT** for newbies. It's for 适合了解 Git 的基本使用，知道 commit、push、pull，希望掌握 Git 更多功能的人阅读。你可以以任何顺序阅读，或者作为一个手册，在遇到问题的时候查询。如果你遇到没有涉及的问题，欢迎 [新建 issue](https://github.com/xhacker/GitProTips/issues) 反馈。我会持续扩充本文。

## Commit

### How to set username and email?
```shell
git config --global user.name "Dongyuan Liu"
git config --global user.email "liu.dongyuan@gmail.com"
```

### How to switch between user identities?
```shell
git commit --author "Xhacker <liu.dongyuan@gmail.com>"
```

Or

```shell
GIT_COMMITTER_NAME="Xhacker" GIT_COMMITTER_EMAIL="liu.dongyuan@gmail.com" git commit --author "Xhacker <liu.dongyuan@gmail.com>"
```
What's the difference? In face, there are two informations about author in Git，`committer` and `author`. 第一条命令将使用 ``Xhacker <liu.dongyuan@gmail.com>`` 作为 commit 的 `author`，第二条命令则同时设置 `committer` 和 `author`。

关于 `committer` 和 `author` 的区别，[Pro Git 2.3 章](http://git-scm.com/book/zh/v1/Git-基础-查看提交历史) 中提到：

> 你一定奇怪作者（author）和提交者（committer）之间究竟有何差别，其实作者指的是实际作出修改的人，提交者指的是最后将此工作成果提交到仓库的人。所以，当你为某个项目发布补丁，然后某个核心成员将你的补丁并入项目时，你就是作者，而那个核心成员就是提交者。我们会在第五章再详细介绍两者之间的细微差别。

### How to write commit message?
Commit message 应简短、清晰地描述这个 commit 中做了什么。如果所有协作者都能阅读中文，则可以使用中文。

Example:

> Fixed table view cell text overflow

<!-- -->
> 修复了内存泄漏的问题

Negative example：

> fix some bugs

<!-- -->
> 修改文件

同时，如果你使用 GitHub 管理代码，还可以通过 commit message 关闭 issue，详见 /* TODO: 链接 */。

### How to append changes to last commit？
假设你发现刚刚完成的 commit 中有一处错误，你一定希望把修改追加到上一个 commit 中，而不是创建一个新的 commit。很简单，只需要用 `--amend`：

```shell
git commit --amend
```

在 amend 之后，commit 的时间是不会变的。如果你想更新一下 commit 时间，可以用

```shell
git commit --amend --reset-author
```

需要注意的是，amend 实际上修改了上一个 commit。所以如果已经 push 了上一个 commit，请尽量不要 amend。

如果一定要 amend 已经 push 了的 commit，**请确保这个 commit 所在的 branch 只有你一个人使用**（否则会给其他人带来灾难），然后在 amend 之后使用 `git push --force`。

### How to change a part of commit files?
```shell
git commit -p
```

### What is stage？

### 如何查看修改了哪些文件？
```shell
git status
git diff
git diff --staged
```

### 如何查看某个特定的 commit 修改了哪些文件？
```shell
git show
git show HEAD~1
git show 9790eff
```

### How long is Commit's hash?

### 如何 push 一部分 commit？
```shell
git push <remotename> <commit SHA>:<remotebranchname>
git push origin 9790eff:master
```

### How to commit an empty directory?
```shell
git add output/.keep
```

### How to ignore files and directories?
.gitignore

### What should be commited，and what not？

### How to ignore files locally?

## Branch

### How to create branch？
```shell
git branch <branchname>
git checkout -b <branchname>
```

### How to delete branch？
```shell
git checkout -d <branchname>
```

### How to switch to last branch？
```shell
git checkout <branchname>
git checkout -
```

### 如何应用其他 branch 的某个 commit？
```shell
git cherry-pick 5130058
```

### merge 和 rebase 有什么区别？

### 如何重置到某个 commit？
```shell
git reset HEAD~1
```

### 如何查看已经 merge 的 branch？
```shell
git branch --merged
git branch --no-merged
git branch --merged | xargs git branch -d
```

## Time Machine

### 如何重置某个文件到未修改的状态？
```shell
git checkout -- <filepath>
```

### 如何重置所有文件到未修改的状态？
```shell
git reset --hard
```

### 如何删除所有新增的文件？

### 如何还原某个 commit？
```shell
git revert
```

### 如何修改最后一个 commit？
```shell
git reset --soft
```

### 如何删除某个 commit？如何修改某个 commit 的 message？如何变换两个 commit 的顺序？如何把一个 commit 拆成多个？如何追加内容到以前的 commit？
删除和还原有什么区别？
```shell
git rebase -i master
git rebase -i 22e21f2
git rebase -i HEAD~3
```
[more explaination]

already pushed?
mention git push -f

### 我把一个 commit 彻底删掉了，还能恢复吗？
```shell
git reflog
git log HEAD@{6}\n: 1415211713:0;git log HEAD@{6}
git reflog HEAD@{6}
git reset --hard HEAD@{6}
```

## Collaboration with GitHub

### 别人 push 了修改，我无法 push 了，怎么办？
```shell
git fetch
git rebase origin/master
```

### 如何用 commit 关闭 GitHub issue？

### 如何给开源项目提交 pull request？

### 如何跟进开源项目上游的更改？

## Dark magic

### 发现了一个 bug，如何知道是哪个 commit 导致的？
```shell
git biselect
```

### How to shorten Git command？
.gitconfig
.zshrc

### How to use Git in shell preferably？

## Appendix

### Better Git Client

### .gitconfig

```ini
[alias]
    co = checkout
    st = status
    lg = log --pretty=format:\"%h %ad | %s%d [%an]\" --graph --date=short
    remove-untracked-files = clean -f -d
    ignore = update-index --assume-unchanged
    unignore = update-index --no-assume-unchanged
    ignored = !git ls-files -v | grep "^[[:lower:]]"
[diff]
    algorithm = patience
[color]
    ui = on
```
