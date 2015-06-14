# คู่มือสไตล์การเขียน Git

คู่มือสไตล์การเขียน Git นี้ได้รับแรงบันดาลใจมาจาก [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches),
[git man pages](http://git-scm.com/doc) และ ข้อที่ควรปฏิบัติที่เป็นนิยมในชุมชน

คู่มือที่มีการผ่านการแปลภาษามีภาษาดังต่อไปนี้ :

* [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [French](https://github.com/pierreroth64/git-style-guide)
* [Japanese](https://github.com/objectx/git-style-guide)
* [Korean](https://github.com/ikaruce/git-style-guide)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)
* [Thai](https://github.com/zondezatera/git-style-guide)
* [Ukrainian](https://github.com/denysdovhan/git-style-guide)

ถ้าคุณรู้สึกว่าอยากจะมีส่วนร่วมกับเรา โปรดทำแบบนี้! Fork โปรเจ็คนี้ และเปิด pull request

# ตารางสารบัญ

1. [Branches](#branches)
2. [Commits](#commits)
  1. [Messages](#messages)
3. [Merging](#merging)
4. [Misc.](#misc)

## Branches

* ควรเลือกชื่อที่ *สั้น* และ *บอกรายละเอียดเฉพาะตัวได้* :

  ```shell
  # ควรใช้
  $ git checkout -b oauth-migration

  # ไม่ควรใช้ - เพราะกำกวมเกินไป
  $ git checkout -b login_fix
  ```

* ระบุถึง tickets ที่ตรงกันกับใน service ภายนอก (ตัวอย่างเช่น GitHub
  issue) นอกจากนี้ยังมีรูปแบบที่เหมาะ สำหรับการใช้งานใน branch
  ยกตัวอย่างเช่น:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* ควรใช้ *ขีดกลาง ( - )* เพื่อแยกคำออกจากกัน

* ในขณะที่มีคนหลายๆคนทำงานอยู่บนฟีเจอร์ที่*เหมือนกัน* มันอาจจะมีแนวทางที่ง่ายและสะดวกสำหรับ branches ส่วน ฟีเจอร์ *สำหรับบุคคล* และ *สำหรับทีม*
  ซึ่งควรใช้การตั้งชื่อแบบนี้สำหรับแบ่งแยก:

  ```shell
  $ git checkout -b feature-a/master # team-wide branch
  $ git checkout -b feature-a/maria  # Maria's personal branch
  $ git checkout -b feature-a/nick   # Nick's personal branch
  ```

  Merge ส่วน branches แต่ละบุคคล ไปยังส่วน branch สำหรับทีม (ดูส่วน ["Merging"](#merging))
  และสุดท้าย  branch สำหรับทีม จะถูก merged เข้าไปรวมกับ "master"

* ลบ branch ออกจาก upstream repository หลังจาก ที่มัน merged เสร็จเรียบร้อย (ยกเว้นแต่จะมีเหตุผล)

  ข้อแนะนำ: ใช้คำสั่งต่อไปเมื่ออยู่บน "master" เพื่อแสดงรายการ merged
  branches:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commits

* ทุกๆ commit ควรจะเป็น *การเปลี่ยนแปลงโลจิกแบบเจาะจง*. อย่ารวมเอา
  *หลายๆการเปลี่ยนเปลี่ยนโลจิก* ไว้ใน commit เดียว ตัวอย่างเช่น ถ้าสมมุติ มี patch ในแก้ไข bug และ optimizes ประสิทธิภาพของ feature ควรแบ่งมันออกเป็น 2 commits

* อย่าแบ่งการเปลียนแปง *แบบเจาะจง* ออกเป็นหลายๆ commits. ตัวอย่างเช่น
  ในการ implementation ของ feature และ การทดสอบเป้นส่วนๆ ควรจะอยู่ใน commit เดียวกัน

* ควรจะมี commit *เนิ่นๆ* และ *บ่อยๆ* ในจุดเล็กๆน้อย ในตัวของมันเองเพื่อให้ง่ายต่อการเข้าใจ และ สามารถย้อนกลับได้เมื่อมีอะไรผิดพลาด

* ในการ Commits ควรจะมีการเรียงลำดับ *อย่างมีเหตุผล*. ตัวอย่างเช่น ถ้า *commit X* มีการเปลียนแปลงในส่วน *commit Y* ดังนั้น *commit Y* ควรจะมาก่อน *commit X*.

### Messages

* ควรใช้พวก editor ต่างๆ ในการเขียนข้อความสำหรับ commit ไม่แนะนำให้ใช้ terminal :

  ```shell
  # ควรใช้
  $ git commit

  # ไม่ควรใช้
  $ git commit -m "Quick fix"
  ```

  เมื่อมีการ commit จาก terminal จะส่งผลให้เกิดแนวคิดที่ว่าให้ทุกสิ่งอย่างอยู่ใน บรรทัดเดียว ทำให้อาจจะเกิดผลกระทบเกิดความไม่ชัดเจนของข้อความที่ส่งไป

* บรรทัดสรุป (อาทิเช่น บรรทัดแรกของข้อความ) ควรจะมี
  *การบรรยาย* แต่อย่าง *รวบรัด*. มันจะเป็นดี ถ้ามันไม่ควรที่จะยาวเกินไปกว่า
  *50 ตัวอักษร*. It should be capitalized and written in imperative present
  tense. It should not end with a period since it is effectively the commit
  *title*:

  ```shell
  # ควรใช้ - ตามความจำเป็นในขณะนั้น ไม่เกินกว่า 50 ตัวอีกษร
  Mark huge records as obsolete when clearing hinting faults

  # ไม่ควรใช้
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* หลังจากนั้นควรมาพร้อมบรรทัดว่างตามด้วยอธิบายเพิ่มเติมอย่างละเอียด
  มันควรจะประกอบไปด้วย *72 ตัวอักษร* และ การอธิบายเหตุผลว่า *ทำไม* การเปลี่ยนนี้ถึงจำเป็น และ
  *ทำอย่างไร* ตามจุดประสงค์ และ*ผลกระทบ* ที่อาจจะมี

  นอกจากนี้ยังควรให้คำแนะนำต่างๆให้ไปแหล่งข้อมูลอื่นๆที่เกี่ยวข้อง ( อาทิเช่น การอ้างอิงไปยังปัญหาที่เกี่ยวเนื่องกันในจุดบกพร่อง ):

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

  ในท้ายที่สุดแล้ว เมื่อ เขียนข้อความสำหรับ commit ต้องคิดเกี่ยวกับสิ่งที่ต้องการถ้ารู้ว่า 
  คุณต้องทำงานใน commit ในหนึ่งปีจากนี้ไป

* ถ้า *commit A* ขึ้นอยกับ *commit B* ควรจะต้องถูกแจ้งไว้กับข้อความของ *commit A*
  Use the commit's hash when referring to commits.

  Similarly, if *commit A* solves a bug introduced by *commit B*, it should
  be stated in the message of *commit A*.

* If a commit is going to be squashed to another commit use the `--squash` and
  `--fixup` flags respectively, in order to make the intention clear:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(ข้อแนะนำ: ควรใช้ `--autosquash` flag when rebasing. The marked commits will be
  squashed automatically.)*


## Merging

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
  # ควรใช้ - เพื่อให้แน่ใจว่าการ merge จะถูกสร้างขึ้น
  $ git merge --no-ff my-branch

  # ไม่ควรใช้
  $ git merge my-branch
  ```

## Misc.

* There are various workflows and each one has its strengths and weaknesses.
  Whether a workflow fits your case, depends on the team, the project and your
  development procedures.

  That said, it is important to actually *choose* a workflow and stick with it.

* *Be consistent.* This is related to the workflow but also expands to things
  like commit messages, branch names and tags. Having a consistent style
  throughout the repository makes it easy to understand what is going on by
  looking at the log, a commit message etc.

* *Test before you push.* Do not push half-done work.

* Use [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags)
  for marking releases or other important points in the history. Prefer
  [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags)
  for personal use, such as to bookmark commits for future reference.

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

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io

# Translator

Kitti Pariyaakkarakun 

