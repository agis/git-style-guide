# Git スタイルガイド

このスタイルガイドは [*How to Get Your Change Into the Linux
Kernel*](https://www.kernel.org/doc/Documentation/SubmittingPatches) や [git man pages](http://git-scm.com/doc) 、ソフトウエェア開発コミュニティでの様々な Git の使い方に触発されて書かれました。

以下の言語で、このスタイルガイドの翻訳を読むことが出来ます:

* [Chinese (Simplified)](https://github.com/aseaday/git-style-guide)
* [Chinese (Traditional)](https://github.com/JuanitoFatas/git-style-guide)
* [Portuguese](https://github.com/guylhermetabosa/git-style-guide)

貴方がこのガイドをよりよい物にしたいと思ったなら、是非このプロジェクトをフォークしてプルリクエストを送って下さい。

# 目次

1. [ブランチ](#ブランチ)
2. [コミット](#コミット)
  1. [コミットメッセージ](#コミットメッセージ)
3. [マージ](#マージ)
4. [その他](#その他)

## ブランチ

* *短く* *説明的な* 名前を選びましょう:

  ```shell
  # 良い
  $ git checkout -b oauth-migration

  # 悪い - 曖昧すぎる
  $ git checkout -b login_fix
  ```

* 外部のサービスが使うチケット番号など（例: GitHub の issue）はブランチ名の良い候補です。例えば:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* 単語の区切りには 「-」を使いましょう。

* 複数人が一つのフィーチャーに対して作業する場合、以下のような命名規則で *個人用* フィーチャーブランチと *チーム用* フィーチャーブランチを作ると便利かもしれません:

  ```shell
  $ git checkout -b feature-a/master # チーム用フィーチャーブランチ
  $ git checkout -b feature-a/maria  # Maria の個人用フィーチャーブランチ
  $ git checkout -b feature-a/nick   # Nick の個人用フィーチャーブランチ
  ```

  個人用フィーチャーブランチをチーム用のフィーチャーブランチにマージします (see ["マージ"](#マージ))。
  チーム用フィーチャーブランチは最終的に "master" にマージされる事になるでしょう。

* 特別な理由がない限り、上流のリポジトリに自分のブランチがマージされた後に自分のブランチを削除しましょう。

  Tip: master ブランチに居る状態で以下のコマンドを使うとマージされたブランチの一覧を見る事が出来ます:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## コミット

* 箇々のコミットは *論理的に* 1つの変更を表すべきです。複数の *論理的な* 変更を 1つのコミットでするべきではありません。
  ある 1つのパッチがバグフィックスと最適化を両方行っているとしたら、その変更は 2つのコミットに分けるべきです。

* *論理的に* 1つの変更を複数のコミットに分けてはいけません。例えば、ある機能の実装と関連するテストは 1つのコミットになっているべきです。

* 出来るだけ *早い段階で* *頻繁に* コミットしましょう。
  小さく、自己完結的なコミットは理解しやすく、何か問題が起きた場合の復旧も早いからです。

* コミットは *論理的順番* に並べましょう。
  *コミット X* が *コミット Y* での変更に依存しているのなら、 *コミット Y* は *コミット X* の前に来るようにすべきです。

### コミットメッセージ

* コミットメッセージを書く際にはコマンドライン引数で与えるのではなくエディターを使いましょう:

  ```shell
  # 良い
  $ git commit

  # 悪い
  $ git commit -m "Quick fix"
  ```

  コマンドライン引数でコミットメッセージを与えると、どうしても 1行に収まるようにしてしまいがちです。
  それは役に立たない曖昧なコミットメッセージを生む原因になってしまいます。

* サマリー行（コミットメッセージの 1行目）は *説明的で* 且つ *簡潔な* ものにしましょう。
  理想的には *50文字以下* の長さで、キャピタライズされた命令形の現在時制であるべきです。
  コミットの *タイトル* として使える様に、末尾にピリオドを付けてはいけません:

  ```shell
  # good - imperative present tense, capitalized, fewer than 50 characters
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* サマリー行の後は 1行の空行を挟んで、より完全な説明が書かれるべきです。
  この節は *なぜ* この変更が必要で、 *どうやって* 問題に対処したのか、そして、どんな *副作用* があるのかを *72文字* 折り返しで記述します。

  また、関連する事項へのポインタ（例: 関連する issue への bug tracker 上でのリンク）があるのなら、ここに書かれるべきです:

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

  コミットメッセージを書く場合、1年後に参照される事迄考えた上で書きましょう

* *コミット A* が他の *コミット B* に依存する場合、その依存関係はコミットメッセージの中に明示されるべきです。
  参照するにはコミットハッシュを使うのが良いでしょう

  また *コミット A* が *コミット B* で入り込んだバグを解決した場合、 *コミット A* のコミットメッセージで *コミット B* に言及すべきです

* コミットが他のコミットに縮約される場合、コミットの意図を明確にするために `--squish` と `--fixup` が適切に使われるべきです:

  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: リベースの際に `--autosquash` を使うとマークされたコミットを自動で縮約できます)*

## マージ

* **公開された履歴を書き換えてはいけません。** リポジトリの履歴自体が価値のあるものであり、 *何が実際に起きたのか* を知ることの出来る非常に重要な物です。公開された履歴の書き換えは諸問題の根源になり、プロジェクトに関わる全ての人の混乱の元になります。

* しかしながら履歴の書き換えが妥当な場合もあります:

  * 自分 1人で作業をしているブランチで、まだ誰もレビューをしていない

  * 後で "master" へマージするために、リベースして履歴を掃除する（例: コミット履歴の縮約）

  *"master" ブランチの履歴は絶対に書き換えてはいけません* その他の特別なブランチ（実際にプロダクション環境で使用されるとか CI サーバーが使うとか）も同様に履歴を書き換えてはいけません。

* 自分のブランチを *マージする前に* 履歴を *綺麗に* かつ *単純に* しておきましょう:

    1. スタイルガイドに沿っているかの確認、そして必要なら履歴の縮約、並べ替え、コミットメッセージの編集などを適用しておきましょう

    2. マージ先のブランチへリベースします:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # then merge
       ```

       これで自分のブランチは "master" の先端に直接繋げる事が出来、非常に単純な履歴にする事が出来ます。

       *(Note: このやり方は生存期間の短いブランチを使うプロジェクト向きです。そうでないのなら、"master" ブランチを随時マージする方がリベースをするよりも適しているでしょう)*

* ブランチが 2つ以上のコミットを含むなら fast-forward マージをすべきではありません:

  ```shell
  # 良い - 必ずマージコミットが生成されるようにする
  $ git merge --no-ff my-branch

  # 悪い
  $ git merge my-branch
  ```

## その他

* 様々なワークフローが存在し、それぞれ強み・弱みがあります。どのワークフローが適切かはチーム、プロジェクト、開発技法によって変わります。

  適切なワークフローを *選び* それを遵守することが重要です。

* *一貫性を保ちましょう* ワークフローとも関係しますが、コミットメッセージ、ブランチ名、タグ名の与え方に一貫性を持たせましょう。
  一貫性を保つことで、ログやコミットメッセージ等を見ただけで何が起きているのかを短時間に理解出来るように出来ます。

* *プッシュする前にテストをしましょう* 未完成のものをプッシュしてはいけません。

* [annotated tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Annotated-Tags) をリリース時等の重要な履歴を記録するために使いましょう。

  [lightweight tags](http://git-scm.com/book/en/v2/Git-Basics-Tagging#Lightweight-Tags) は個人的な用途（将来参照するであろう履歴の記録）に使いましょう。

* リポジトリを健全な状態に保つ為、時々以下のコマンドを使ってリポジトリのメンテナンスをしましょう:

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
* Masashi Fujita / [@objectxplosive](https://twitter.com/objectxplosive)
