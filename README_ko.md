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

* 만약 다른 커밋에 밀어넣으려면 `--squash`를 사용하고, 
  의도를 확실하게 표시하고자 한다면 `--fixup`을 사용하세요.

  ```shell
  $ git commit --squash f387cab2
  ```
  
  *(Tip: 리베이스 할때 `--autosquash`를 사용하세요. 표시된 커밋은 자동으로 밀어 넣어집니다.)*

## Merging (머지)

* **배포된 이력(History)를 재 기록하지 마세요** 리파지토리의 이력은 제대로 되어 있을때
  가치있습니다. 그리고 *어떤 일이 실제로 일어난는가*를 말하는 것이 매우 중요합니다. 
  배포된 이력을 수정한다는 것은 프로젝트에서 일하는 모두에게 문제가 됩니다. 

* 그러나, 이력을 다시 기록해도 되는 경우도 있습니다. 이런 경우는 
  
  * 아직 리뷰되지 않았고 혼자서만 개발하는 브랜치인 경우. 
  
  * 브랜치를 깔끔하게 정리하고 (예를 들어 squash commit같은) 나중에 머지하기 위해 
    "master"를 리베이스 하는 경우. 
  
  이말은 *절대 "master"브랜치나 다른 특별한 브랜치의 이력을 재기록하지 말라*는 것입니다.
  (예를 들면, 운영하는데 사용되거나, CI 서버에서 사용하는 )

* 이력을 "깔끔"하고 "단순"하게 유지하세요. 브랜치를 "머지하기 바로 직전에"  

    1. 이력이 관습을 따르고 있는지, 필요하지 않은 행위는 없는지 확인하세요.
       (squash/reorder commits, reword messages etc.) 
    
    2. 머지될 브랜치를 리베이스 하세요.
    
       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # then merge
       ```
       
       이 결과는 "master"브랜치의 맨뒤에 이력을 붙여서 매우 간단한 
       이력을 보여주게 됩니다. 
       
       *(노트: 이러한 전략은 짧은동안 브랜치를 유지하는 프로젝트에 적합합니다. 
       한편으로 리베이스하는 것보다 "master"브랜치에 머지하는 것이 나은 경우도 종종 있습니다.
       
* 만약 브랜치에 하나 이상의 커밋이 있다면, fast-forward 머지는 하지 마세요.

  ```shell
  # good - 머지 커밋이 반드시 일어납니다. 
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc. (기타)

* 다양한 workflow들이 존재하고 각각이 강점과 약점을 가지고 있습니다. 
  어떤 workflow가 현재 상황에 적합할지는, 팀, 프로젝트 그리고 개발 절차에 
  따라 정해져야 합니다. 

  어떤 workflow를 *선택*하고 계속할지는 매우 중요합니다. 

* *동일한 규칙을 사용하세요.* 이것은 workflow와 연관되있을 뿐 아니라 커밋 메시지를 
  작성하는 것, 브랜치 명이나 태그를 달때도 확장할 수 있습니다. 리파지토리 전체에 
  동일한 규칙을 가지는 것은 커및 메시지또는 로그를 찾을 때, 이해하기 쉽게 만들어 줍니다. 

* *푸시하기 전에 테스트 하세요.* 완료되지 않은 일을 푸시하지 마세요. 

* 
* 릴리즈나 중요한 이력을 기록하기 위해 [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)를 사용하세요.

  개인적일 때는 [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  즉, 나중 참조를 위한 북마크 커밋 같은것이 바람직합니다. 

* 종종 성능 유지 작업을 통해 리파지토리가 좋은 모습을 가지도록 유지하세요. 
  그것이 로컬 리파지토리이건, 원격 리파지토리건 간에 

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# License

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

이 문서는 Creative Commons Attribution 4.0 International license를 따릅니다.

# Credits

chris / [@agisanast](https://twitter.com/agisanast) / http://agis.io
