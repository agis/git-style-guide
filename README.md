# Git 风格指南

这份风格指南受到 [*How to Get Your Change Into the Linux Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches)，[git man pages](http://git-scm.com/doc) 和大量社区通用实践的启发。

# 目录

1. [分支](#branches)
2. [提交](#commits)
  1. [消息](#messages)
3. [合并](#merging)
4. [杂项](#misc)

## Branches

* 选择*简短*和*具有描述性*的名字来命名分支：

  ```shell
  # 好
  $ git checkout -b oauth-migration

  # 不好，过于模糊
  $ git checkout -b login_fix
  ```

* 来自外部的标识符也适合用作分支的名字，例如来自 Github 的 Issue 序号。

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* 用破折号分割单词。

* 当不同的人围绕同一个特性开发时，维护整个团队的特性分支与每个人的独立分支是比较方便的做法。使用如下的命名方式：

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria # Maria's branch
  $ git checkout -b feature-a/nick # Nick's branch
  ```

  合并时，由每个人的独立分支向全队的功能分支合并，最后合并到主分支。见[合并](#merging) 。

* 合并之后，除非有特殊原因，从上游仓库中删除你的分支。使用如下命令查看已合并的分支：

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* 每个提交应当只包含一个简单的逻辑改动，不要在一个提交里包含多个逻辑改动。比如，如果一个补丁修复了一个 Bug，又优化了一个特性的性能，就将其拆分。
* 不要将一个逻辑改动拆分提交。例如一个功能的实现及其对应的测试应当一并提交。
* 尽早、尽快提交。出问题时，短小、完整的提交更容易发现并修正。
* 提交应当依*逻辑*排序。例如，如果 X 提交依赖于 Y，那么 Y 提交应该在 X 前面。

### Messages

* 使用编辑器编写提交信息，而非命令行。

  ```shell
  # 好
  $ git commit

  # 不好
  $ git commit -m "Quick fix"
  ```

  使用命令行会鼓励试图用一行概括提交內容的风气，而这会令提交信息难以理解。

* 概要行（即第一行）应当简明扼要。它最好不超过 50 个字符，首字母大写，使用现在时祈使语气。不要以句号结尾, 因为它相当于*标题*。

  ```shell
  # 好
  Mark huge records as obsolete when clearing hinting faults

  # 不好
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* 在那之后空一行，然后填写详细描述。每行不超过 *72 字符*，解释*为什么*需要改动, *如何*解决了这个 issue 以及它有什么*副作用*。

  最好提供相关资源的链接，例如 bug tracker 的 issue 编号：
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

  最后，编写提交信息时，设想一下你一年以后再看这段提交信息时，希望获取什么信息。

* 如果 *提交 A* 依赖于另一个 *提交 B* ，在前者的 commit message 中应当指明。援引对应提交的 Hash。

  同理，如果 *提交 A* 解决了 *提交 B* 引入的 bug，这应当也被在 *提交 A* 提及。
* 如果将一个提交 squash 到另一个提交，分别使用 `--squash` 和 `--fixup` 来强调目的。
  ```shell
  $ git commit --squash f387cab2
  ```
  *（Rebase 时使用 `--autosquash` 参数，标记的提交就会自动 squash。）*

## Merging

* **不要篡改提交历史。**仓库的历史本身就很宝贵，重要的是它能够还原*实际发生了什么*。对任何参与项目的人来说，修改历史是万恶之源。
* 尽管如此，有些时候还是可以重写历史，例如：
  * 你一个人孤军奋战，而且你的代码不会被人看到。
  * 你希望整理分支（例如使用 squash），以便日后合并。
  最重要的，*不要重写你的 master 分支历史* 或者任何有特殊意义的分支（例如发布分支或 CI 分支）。
* 保持你的提交历史*干净*、*简单*。*在你 merge* 你的分支之前：
  1. 确保它符合风格指南，如果不符合就执行相应操作，比如 squash 或重写提交信息。
  2. 将其 rebase 到目标分支：
     ```shell
     [my-branch] $ git fetch
     [my-branch] $ git rebase origin/master
     # then merge
     ```
  这样会在 master 后直接添加一个新版本，令提交历史更简洁。

  *（这个策略更适合较短生命周期的分支，否则还是最好经常合并而不是 rebase。）*

* 如果你的分支包含多个 commmit , 不要使用快进模式。
  ```shell
  # 好；注意添加合并信息
  $ git merge --no-ff my-branch

  # 不好
  $ git merge my-branch
  ```

## Misc.

* 有许多工作流，每一个都有好有坏。一个工作流是否符合你的情况，取决于你的团队，项目，和你的开发规律。

  也就是说，重要的是认真 *选择* 合适的工作流并且坚持。
* *保持统一*， 这涉及到从工作流到你的提交信息，分支名还有标签。 在整个 Repository 中保持统一的命名风格有助于辨认工作进度。
* *push 前测试*， 不要提交未完成的工作。
* 使用 [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) 标记发布版本或者其他重要的时间点。

  个人开发可以使用 [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)，例如为以后参考做标记。
* 定期维护，保证你的仓库状态良好，包括本地还有远程的仓库。

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

* Qi Chen / aseaday@gmail.com
* chadluo / i@yuki.rocks
