---
title: "新しい技術を学習しながらNotionでメモをとり、Zennで効率よく記事化するフロー"
emoji: "✏️"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["notion", "zenn", "情報収集", "学習", "勉強"]
published: true
---

新しい技術を学習するときに調べ物をしながらNotionに開発メモ（という名のリンク集）をつくり、開発が終わったらそのメモを基に記事化する、というフローがいい感じに型にはまって、1年くらい運用できたので紹介します。

調べ物をしながら開発を進めていくスタイルの人にはおすすめの方法なので、是非参考にしてください。

# 対象読者

- 新しい技術を調べ物をしながら開発を進めることが多い
- ブラウザがタブでいっぱいになりがちでどうにかしたい
- 参考にしたページをよく見失って困る
- 学習したことを記事化したいけどめんどくさくて諦めてしまう

# 1. Notionでメモ用ページを作る

[Notion – The all-in-one workspace for your notes, tasks, wikis, and databases.](https://www.notion.so/product)

> Notionは、タスク、Wiki、およびデータベースを統合するマークダウンサポートを備えたメモアプリケーション及びサービス。 同社はこのアプリを、メモ作成、プロジェクト管理、タスク管理のためのオールインワンワークスペースと説明している。
> [Notion (ソフトウェア) - Wikipedia](https://ja.wikipedia.org/wiki/Notion_(%E3%82%BD%E3%83%95%E3%83%88%E3%82%A6%E3%82%A7%E3%82%A2))

Notionは最近話題のサービスです。詳しくはたくさん記事が出てるので調べてみてください。開発メモをとるのに、Notionを使います。僕がNotionを使う理由は、以下のとおりです。

- 自分の使いやすい形にリストをカスタマイズできる
- マークダウンで書ける（コピペできる）
- リンクプレビューが見やすくて便利

メモを作るサービスは自分好みのがあれば何でも良いと思います。僕の場合はNotionに、以下のようなメモ用のデータベースを作っています。（公開用に一部編集しています。）

![](https://storage.googleapis.com/zenn-user-upload/j2whrrd8w5q0puilq688qk7t2qjo)


ページが作れたらNotionの準備は完了です。

# 2. 参考にしたページはタブに残し、閉じる前にNotionに記録する

新しい技術を学習するとき、基本的にはウェブを徘徊して参考になるページを探すと思います。そして、参考になる記事が見つかったらしばらくはタブに残すことが多いと思います。

そしてここが一番大事なところです。その参考になった記事のタブを消す前、あるいはタブの数が多くなってきたら、先程作ったNotionのメモ用データベースにページを作って、そこにリンクをメモしていきます。

つまり、新しい技術の学習で参考になった記事のリンク集をNotionのページに作っていくような感じです。NotionはURLを貼り付けて`Create bookmark`すると、OGPなどを読み取っていい感じのプレビューを作ってくれるので、URLさえコピペしていけばリンク集が簡単に作れます。

自分はそれ加えて、簡単なメモを書いたりタイトルを付けてリンクのグループ分けをしたりします。画像は「OpenAPI(Swagger)」を勉強したときに作った実際のNotionページになります。

![](https://storage.googleapis.com/zenn-user-upload/0y3916f3dc7yyxgp4ztz2fwxcqop)

この時点では自分しか見ないので、自分が分かりやすいように整理できていれば良いです。これを作っておけば、ブラウザのタブを閉じても参考になった記事を再参照しやすくなりますし、時間を空けて再学習するときにも役立ちます。

# 3. 完全に理解したらNotionを基に情報を整理して記事化する

Notionに記録するところまでで止めてしまっても問題ないですが、せっかくなので記事化します。少し前まではQiitaで記事化していましたが、最近はZennで書くのが楽しいのでZennで記事化しています。

Notionでリンク集を作っておくと、コピーしてエディタに貼り付けるだけでMarkdown形式で貼り付けられます。リンクプレビュー部分もただのリンク記述としてコピーされてくるので、記事化が捗ります。

以下は画像で紹介した「OpenAPI(Swagger)」のNotionページを一部コピペした例です。

```markdown
[Redocly/redoc](https://github.com/Redocly/redoc)

redocのサンプル見るのが一番わかりやすい

[ReDoc Interactive Demo](https://redocly.github.io/redoc/)

[How to reference array item examples in OpenAPI 3?](https://stackoverflow.com/questions/49839121/how-to-reference-array-item-examples-in-openapi-3)

### OneOf, AllOf, AnyOf
[oneOf, anyOf, allOf, not](https://swagger.io/docs/specification/data-models/oneof-anyof-allof-not/)


## 設計

[スキーマファースト開発のためのOpenAPI（Swagger）設計規約](https://future-architect.github.io/articles/20200409/)

[Swaggerの記法まとめ - Qiita](https://qiita.com/rllllho/items/53a0023b32f4c0f8eabb)

[Swagger ではない OpenAPI Specification 3.0 による API サーバー開発](https://www.slideshare.net/techblogyahoo/swagger-openapi-specification-30-api)
```

あとはこれを基に記事として整形します。この「OpenAPI(Swagger)」を整形して公開したのがこちらです。

[OpenAPIでAPIドキュメントを初めて作るときに読む資料](https://zenn.dev/d_forest/articles/bec25d3a1b111ed37a09)

自分が書いた記事で、リンク集っぽい記事はだいたいこの方法で書いたものです。効率的に学習しながらアウトプットもできてなかなか良いと思いませんか？

他にもいい方法があったら是非教えて下さい。
