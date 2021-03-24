#### Fukuoka.rb 200回 LT大会 (#202)


- - -

### Rails のデフォルトブランチが
###  main になって CI がぶっ壊れた話

---

#### 自己紹介
- - -

* 名前：osyo
* Twitter : [@pink_bangbi](https://twitter.com/pink_bangbi)
* github  : [osyo-manga](https://github.com/osyo-manga)
* ブログ  : [Secret Garden(Instrumental)](http://secret-garden.hatenablog.com)
* [趣味で気になった bugs.ruby のチケットをブログにまとめてる](https://secret-garden.hatenablog.com/archive/category/bugs.ruby)               <!-- .element: class="fragment" -->
* 最近はるりまにめっちゃ PR 投げてる                   <!-- .element: class="fragment" -->
    * [ここ1ヶ月で 15個ぐらい PR 投げてる](https://github.com/rurema/doctree/pulls?q=is%3Apr+author%3Aosyo-manga)
* Kaigi on Rails                 <!-- .element: class="fragment" -->
    * [ActiveRecord の歩み方](https://speakerdeck.com/osyo/activerecord-falsebu-mifang)
* 銀座Rails                   <!-- .element: class="fragment" -->
    * [これからの Ruby と今の Ruby について](https://speakerdeck.com/osyo/korekarafalse-ruby-tojin-false-ruby-nituite)
    * [12月25日にリリースされる Ruby 3.0 に備えよう！](https://speakerdeck.com/osyo/12yue-25ri-niririsusareru-ruby-3-dot-0-nibei-eyou)
* BuriKaigi2021                 <!-- .element: class="fragment" -->
    * [Ruby 2.0 から Ruby 3.0 を駆け足で振り返る](https://speakerdeck.com/osyo/ruby-2-dot-0-kara-ruby-3-dot-0-woqu-kezu-dezhen-rifan-ru)

---

## 今日話すこと
<div class="fragment">
<h3>Rails のデフォルトブランチが</h>
<h3>main になって CI がぶっ壊れた話</h>
</div>

---

## ※諸注意※
#### デフォルトブランチが main になることを批判したり推奨したるする話ではない            <!-- .element: class="fragment" -->
#### あくまでも『Rails のデフォルトブランチが変わってこういうことがありました』という話            <!-- .element: class="fragment" -->
#### デフォルトブランチを変える場合はちゃんと注意しましょうね！！！            <!-- .element: class="fragment" -->


---


#### はじめに
- - -

* お仕事で activerecord-bitemporal という gem を開発している                    <!-- .element: class="fragment" -->
    * [気になる人はこの資料を読んでね！](https://speakerdeck.com/f440/implementing-command-history-and-temporal-access)
* これは ActiveRecord に強く依存している                    <!-- .element: class="fragment" -->
    * ActiveRecord の内部 API を使ったり無理やり挙動を書き換えたりしている
    * ActiveRecord の挙動が変更されるたびにぶっ壊れたりしてる…
* なので Rails の master ブランチも含めて複数のバージョンで CI を回して『いた』        <!-- .element: class="fragment" -->
    * [appraisal を使って複数の Rails 環境をテストしてる](https://github.com/kufu/activerecord-bitemporal/blob/8a409076917ff9a1c84ee6e2b27ce0107864bf4e/Appraisals)
    * それを使って [CircleCI でテストを回している](https://github.com/kufu/activerecord-bitemporal/blob/8a409076917ff9a1c84ee6e2b27ce0107864bf4e/.circleci/config.yml)

---

# そんなある日…

---

# 何もしてないのに
# 突然CIがぶっ壊れた

---

# 何が起きたのか…

---

### [エラーログ](https://app.circleci.com/pipelines/github/kufu/activerecord-bitemporal/532/workflows/a20498cc-6afb-4b27-bdc3-0fce87fb52dc/jobs/3264)

```
>> BUNDLE_GEMFILE=/root/project/gemfiles/rails_master.gemfile bundle install
Fetching https://github.com/rails/rails.git
fatal: Needed a single revision
Git error: command `git rev-parse --verify master` in directory /root/project
has failed.
Revision master does not exist in the repository
https://github.com/rails/rails.git. Maybe you misspelled it?
If this error persists you could try removing the cache directory
```

<div class="fragment">
<h2>master ブランチが</h>
<h2>見つからない？？？🤔</h>
</div>

---

<blockquote class="twitter-tweet" align="center"><p lang="en" dir="ltr">Default branch for Rails is now main instead of master ✌️ <a href="https://t.co/wdtupbyp1E">pic.twitter.com/wdtupbyp1E</a></p>&mdash; DHH (@dhh) <a href="https://twitter.com/dhh/status/1350091751789375490?ref_src=twsrc%5Etfw">January 15, 2021</a></blockquote>

<div class="fragment">
<h3>Rails のデフォルトブランチが</h>
<h3>master から main に変わってた😇</h>
</div>

---

## あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛あ゛
# orz

---

# 対処するぞい…

---

#### 対処方法
- - -

* テキスト的な意味で master -> main に変える            <!-- .element: class="fragment" -->
* Gemfile で参照する場合は branch: "main" を指定する            <!-- .element: class="fragment" -->
* こちらが PR になります            <!-- .element: class="fragment" -->
    * https://github.com/kufu/activerecord-bitemporal/pull/69/files

---

## まとめ

---

#### まとめ
- - -

* デフォルトブランチが master -> main に変わると普通に壊れる       <!-- .element: class="fragment" -->
    * 対岸の火事だと思っていたが自分に実害が降り注いできた…
* Rails に限らず master を参照している gem は注意しよう       <!-- .element: class="fragment" -->
* デフォルトブランチを変更する場合はこういう弊害があることを覚えておこう       <!-- .element: class="fragment" -->
* Rails のバグレポート用のテンプレートも変わっているのでローカルにコピーしている人は対応しておこう       <!-- .element: class="fragment" -->
    * https://github.com/rails/rails/tree/main/guides/bug_report_templates


---


### ご清聴
### ありがとうございました

