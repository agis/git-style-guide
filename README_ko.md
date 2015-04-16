# Git Style Guide

[*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches)에서 영감을 받은 Git 스타일 가이드입니다.,
[Git 공식문서](http://git-scm.com/doc)와 유명한 커뮤니티에서 경험한 것을 다룹니다.
among the community.

이 가이드는 아래 언어로 번역되어 있습니다. 

* [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)

함께 하고 싶으시면, 자유롭게 하세요. Fork 하시고 pull request 보내주세요. 

# 목차

1. [Branches](#branches)
2. [Commits](#commits)
  1. [Messages](#messages)
3. [Merging](#merging)
4. [Misc.](#misc)

## Branches (브랜치)

* Choose *short* and *descriptive* names:
* *간결하고* 잘 *설명될 수 있는* 이름을 선택하세요.

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - 너무 애매함
  $ git checkout -b login_fix
  ```

* 외부 서비스(예를 들어 깃헙)의 티켓(할일)에 해당하는 이름도 브랜치 이름으로 쓰는데 좋은 예시 중 하나입니다. 

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* 단어를 분리하기 위해 *이음표(-)* 를 사용하세요.

* 몇명의 사람들이 *같은* 기능들을 구현한다면, *개인적* 기능 브랜치와 *팀* 기능 브랜치를 가지고 작업하는데 편리 할 수 있습니다. 아래와 같은 명명 규칙을 사용해 보세요.

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria  # Maria's personal branch
  $ git checkout -b feature-a/nick   # Nick's personal branch
  ```

  개인이 작업한 브랜치는 계속해서 팀이 구현하는 브랜치로 합쳐지고 나중 "master"와 합쳐질 것입니다. 
  (see ["Merging"](#merging)).

* 브랜치가 머지된 이후에는 (남겨둬야 할 특별한 이유가 없다면) 리파지토리에서 삭제합니다. 

  Tip: "master"브랜치에 합쳐진 브랜치를 보려면 아래와 같은 명령을 실행하세요.

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits (커밋)

* 각각의 커밋은 하나의 *논리적 변화*를 나타내야 합니다. 여러개의 *논리적 변화*를 한번에 커밋하지 마세요.
  예를 들어 하나의 문제점을 수정하고 기능의 성능을 향상 시켰다면, 두 개의 커밋으로 분리해야 합니다. 

* 한개의 *논리적 변화*를 여러개의 커밋으로 나누지 마세요. 예를 들어
  하나의 구현된 기능과 그것의 테스트 코드는 한 커밋이어야 합니다. 

* 커밋은 *빨리* 그리고 *자주* 하세요. 독립적인 커밋은 무언가 잘못됬을때, 
  이해하고 되돌리기 쉽습니다. 

* 커밋은 *논리적*으로 순서를 가져야 합니다. 예를 들어 *커밋 X*는 *커밋 Y*의 변화에 의존한다면 
  *커밋 Y*는 *커밋 X* 보다 먼저 일어나야 합니다. 

### Messages (메시지)

* 커밋 메시지를 작성할때, 터미널 보다는 에디터를 사용하세요.

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  터미널을 통한 커밋은 한줄에 모든것을 써야할 것처럼 느껴집니다. 그것은 항상 정보가 없고,
  모호한 커밋을 작성하게 됩니다. 

* 요약하는 문장(예를 들어, 메시지의 첫출)은 *간단명료한* *설명*이 되어야 합니다. 
  *50글자*를 넘지 않는 것이 바람직합니다. 대문자로 시작하는 명령형 현재시제를 사용해야 합니다. 
  커밋의 실질 적인 *제목*이 되기 때문에 기간(시간을 나타내는 말)으로 끝나는 것은 바람직하지 않습니다. 

  ```shell
  # good - 50글자를 넘지않고 대문자로 시작하는 명령형 현재시제 문장 
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* 이후에 자세한 설명을 위해 이어지는 문장 앞에 빈 줄이 와야 합니다. 
  *72글자*정도에서 줄바꿈이 되는 *왜* 변화가 필요했는지, *어떻게* 변경했는지에 대한 설명이 필요하고 
  혹시나 있을지 모르는 *side-effects*에 대해서 기술해야 합니다. 

  또한 관련된 리소스을 바로 접근할 수 있는 경로를 제공하는 것이 좋습니다. 
  ( 예를 들어 버그 트래커에서 관련된 이슈로 바로 가는 링크 )
  
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

  궁극적으로, 커밋 메시지를 작성할때, 지금부터 1년 후에 커밋을 거슬러 갈때 무엇이
  필요한지를 생각해야 합니다. 
  
* 만약 *커밋 A*가 다른 *커밋 B*에 의존한다면, *커밋 A* 메시지의 시작은 의존성을 언급해야 합니다. 
  커밋을 언급할때는 커밋 해쉬를 사용하면 됩니다. 

  비슷하게, *커밋 B*에서 발생된 버그를 *커밋 A*에서 풀었다면 *커밋 A*에서 
  그것을 언급해야 합니다. 

* 만약   
* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Use the `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*

## Merging (머지)

* **Do not rewrite published history.** The repository's history is valuable in
  its own right and it is very important to be able to tell *what actually
  happened*. Altering published history is a common source of problems for
  anyone working on the project.

* However, there are cases where rewriting history is legitimate. These are
  when:

  * You are the only one working on the branch and it is not being reviewed.

  * You want to tidy up your branch (eg. squash commits) and/or rebase it onto
    the "master" in order to merge it later.

  That said, *never rewrite the history of the "master" branch* or any other
  special branches (ie. used by production or CI servers).

* Keep the history *clean* and *simple*. *Just before you merge* your branch:

    1. Make sure it conforms to the style guide and perform any needed actions
       if it doesn't (squash/reorder commits, reword messages etc.)

    2. Rebase it onto the branch it's going to be merged to:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # then merge
       ```

       This results in a branch that can be applied directly to the end of the
       "master" branch and results in a very simple history.

       *(Note: This strategy is better suited for projects with short-running
       branches. Otherwise it might be better to occassionally merge the
       "master" branch instead of rebasing onto it.)*

* If your branch includes more than one commit, do not merge with a
  fast-forward:

  ```shell
  # good - ensures that a merge commit is created
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc. (기타)

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) for
  marking releases or other important points in the history.

  Prefer [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags) for personal use, such as to bookmark commits
  for future reference.

* Keep your repositories at a good shape by performing maintenance tasks
  occasionally, in your local *and* remote repositories:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

This work is licensed under a Creative Commons Attribution 4.0
International license.

# Credits

chris / [@agisanast](https://twitter.com/agisanast) / http://agis.io
