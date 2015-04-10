# Git 风格指南
这是一个 Git 风格指南， 收到了来自 [*How to Get Your Change Into the Linux Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
[git man pages](http://git-scm.com/doc)
和大量社区欢迎的实践的启发。

# 目录

1. [分支](#branches)
2. [提交](#commits)
  1. [消息](#messages)
3. [合并](#merging)
4. [杂项](#misc)

## Branches

* 选择*短的*和*描述性*的名字来命名分支

  ```shell
  # 好！
  $ git checkout -b oauth-migration

  # 错误，太模糊了！
  $ git checkout -b login_fix
  ```

* 来自外部的标识符也是很好的用作分支的名字，例如来自 Github 的 Issue 的
  序号。

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* 用破折号去分割单词

* 当不同的人在同一个特性上工作的时候，这也许很方便去拥有个人的特性分支和一个面向全队的特性分支。
  在这种情况下，将斜划线作为分支名的后缀，并接着一个人的名字来代表这个的个人分支，并用 master
  表示面向团队的分支。

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria # Maria's branch
  $ git checkout -b feature-a/nick # Nick's branch
  ```

  在你变基后(rebase), 随意[合并](#merging) 你的特性分支到团队的特性分支。最终，
  整个团队的分支会被合并到 master 分支。

* 当这个分支不再需要的时候，通常也就是Merge过后，删除这个特性分支。除非你有必须的原因。
  Tip: 用下面的命令，清理干净分支。

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* 每个提交应当只包含一个简单地的逻辑上的改变，不要在一个提交里改变多件事情。比如，如果一个
  Patch 修复了一个Bug，又改变了一个功能的效率， 把它分开吧。

* 不要将一个改变分成多个提交。 例如一个功能的实现和他对应的测试应当在一个提交里提交。

* 快速和实时提交，小的，独立的提交更容易理解和撤销当出错的时候。

* 提交应当*逻辑上*排序的得当。例如，如果 X 提交依赖于 Y，那么 Y 提交应该在 X 前面。

### Messages

* 使用编辑器, 而不是终端去编写你的提交消息

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  来自终端的提交造成了一种不好的模式： 我必须包含所有事情在一行的空间里，这通常让人不知道你到底
  在这个提交信息里到底想说什么，

  概要行，通常是第一行应当是描述性而简明的，理想情况下，他应当不超过50个字符，用大写字母开头并且
  有着迫切的表现欲。不应当以句号结尾, 这是一个*标题*

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* 在那之后，应当有一空行，然后跟着详细的描述，应当小于*72 字符* 和解释
  *为什么* 需要需要变化, *如何*解决了这个 issue 和它拥有什么副作用。

  应当提供尽可能的帮助和相关资源，例如连接到对应的 issue 的 bug tracer

  ```shell
  Short (50 chars or fewer) summary of changes

  More detailed explanatory text, if necessary. Wrap it to
  72 characters. In some contexts, the first
  line is treated as the subject of an email and the rest of
  the text as the body.  The blank line separating the
  summary from the body is critical (unless you omit the body
  entirely); tools like rebase can get confused if you run
  the two together.

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Use a hyphen or an asterisk for the bullet,
    followed by a single space, with blank lines in
    between

  Source http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  最终，当你写完了一个 commit 的 message, 考虑一下你会遇到什么问题，在漫长的一
  年之后。

* 如果 *commit A* 依赖于另一个 *commit B* ，依赖应当在你的提交里提及到，使用对应
  的 hash 值来表示。

  相似的，如果 *commit A* 解决了一个 bug 被 *commit B* 引入的，这应当也被在
  *commit A* 提及。

* 如果一个commit 将要被 squash 到另一个 commit 处，各自使用 `--squash` 和 `--fixup`
  使得这些 commit 的目的更明显:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

## Merging

* **不要篡改提交历史** 仓库的历史本身就很宝贵并且它是十分重要的能够告诉
  *实际发生了什么*。 修改历史是万恶之源对任何参与这个工程的人。

* However, there are cases where rewriting history is legitimate. These are
  when:
* 尽管如此，有些时候还是可以重写历史的：

  * 你一个人孤军奋战，而且你的代码不会被人看到。

  * 你想清洁一点的你的分支，例如压缩commit 或者 rebase 他们到主分支为了更好的
    的合并

  最重要的，*不要重写你的 master 分支* 或者任何有特殊意义的分支(例如， 用来发布
  的或者持续集成的)

* 保持你的提交历史*干净* 和 *简单*，*在你 merget* 你的分支之前：

  1. 确保它符合 style guide 并且执行任何必要的操作如果他不符合的话，比如 squash
     commmits, 重写你的 messages.

  2. Rebase 这个分支到他将要合并的分支上。

     ```shell
     [my-branch] $ git fetch
     [my-branch] $ git rebase origin/master
     # then merge
     ```
  这样有助于快速合并并且保持了一个干净的提交历史。

  *(备注：这个策略更适合较短生命周期的分支，否则还是最好保持经常性的合并习惯
  而不是 rebase)*

* 如果你的分支包含多过一个的 commmit , 不要使用快进模式。

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* 有大量的工作流，每一个都有好有坏。一个工作流是否符合你的案例，取决于你的团队,
  项目，和你的开发规律。

  也就是说，实事求是的 *选择* 合适的工作流并且坚持他。

* *保持连续性*， 这涉及到从工作流到你的 commit 消息，分支名还有 tag. 在你的整个
  仓库里保持一样的习惯有助于理解你在项目里到底做了什么。

* *push 前测试*， 不要提交未完成的工作。

* 使用 [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) 标记
  release 版本或者其他重要的时间点。

  Prefer [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags) for personal use, such as to bookmark commits
  for future reference.

* 保持你的仓库有一个好的状态，便于随时执行维护任务。不仅在你的本地
  还有远程的仓库。

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by-nc/3.0/88x31.png)

This work is licensed under a Creative Commons Attribution-NonCommercial 4.0
International license.

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io

# Translators
Qi Chen / aseaday@gmail.com
