# assemble生活

[@Takazudo](https://twitter.com/Takazudo)

----

## 私とassemble

* 最初: 大して興味ない
* CodeGrid記事書くのに調べた（調べすぎた）
* Gruntで諸々困ってたことが色々解決できたかも
* なんか結構いいんじゃない？（イマココ）

----

## これはイイって思った点

あらゆるファイルはコンパイルして作るものとして扱う

* JS: minify / coffee / jshint...
* CSS: Sass / Less / Compass...
* img: imagemin
* HTML: assemble

というかそうしたほうが色々便利であることに気付いた  
（別に必ずしもassembleする必要無いのだけれど）

今日の内容の詳細はCodeGridのassemble最終回参照

----

## 基本的なディレクトリ

とりあえずCSS、JSはそのまま使う想定

```bash
├── Gruntfile.js
├── dest # wwwルートになるところ（空っぽ）
└── src
    ├── assemble # assembleのパーツ諸々
    └── assets # いわゆるcommon的なやつら
        ├── css
        ├── imgs
        └── js
```

----

## 基本的な考え方

2つのビルドパターン

* `build:dev`で開発用のビルド
* `build:production`でリリース用のビルド

----

## dev版 - step1: clean

```bash
├── Gruntfile.js
├── dest # 削除
└── src
    ├── assemble
    └── assets
        ├── css
        ├── imgs
        └── js
```

前のを削除

---

## dev版 - step2: assemble

```bash
├── Gruntfile.js
├── dest # 2.ここに展開
└── src
    ├── assemble # 1.assembleの諸々を
    └── assets
        ├── css
        ├── imgs
        └── js
```

HTMLできた

---

## dev版 - step3: copy

```bash
├── Gruntfile.js
├── dest # 2.ここにコピー
│  └── assets
│      ├── css
│      ├── imgs
│      └── js
└── src
    ├── assemble
    └── assets # 1.assetsの諸々を
        ├── css
        ├── imgs
        └── js
```

コピーされた。基本的にはこれだけで見れる。  
SassなりCoffeeなりする場合はソレ

---

## dev版 - step4: watch

```bash
├── Gruntfile.js
├── dest # 1-2.assembleしてここに展開
│  └── assets
│      ├── css # 3-2.cssをここにコピー
│      ├── imgs # 4-2.画像をここにコピー
│      └── js # 5-2.jsをここにコピー
└── src
    ├── assemble # 1-1.assembleの変更で
    └── assets
        ├── css # 3-1.cssの変更で
        ├── imgs # 4-1.画像の変更で
        └── js # 5-1.jsの変更で
```

watchしてコピー  

---

## dev版 まとめ

* やることなくて軽い
* 開発中はそれで十分
* watchは開発中専用

----

## production版 - step1: clean

```bash
├── Gruntfile.js
├── dest # 削除
└── src
    ├── assemble
    └── assets
        ├── css
        ├── imgs
        └── js
```

前のを削除

---

## production版 - step2: assemble

```bash
├── Gruntfile.js
├── dest # 2.ここに展開
└── src
    ├── assemble # 1.assembleの諸々を
    └── assets
        ├── css
        ├── imgs
        └── js
```

同じ

---

## production版 - step3: copy

```bash
├── Gruntfile.js
├── dest # 2.ここにコピー
│  └── assets
│      ├── css
│      ├── imgs
│      └── js
└── src
    ├── assemble
    └── assets # 1.assetsの諸々を
        ├── css
        ├── imgs
        └── js
```

一応コピーしとく。使わないけど。

---

## production版 - step4: 各種minify

```bash
├── Gruntfile.js
├── dest # htmlmin
│  └── assets
│      ├── css # cssmin
│      ├── imgs # imagemin
│      └── js # uglify
└── src
    ├── assemble
    └── assets
        ├── css
        ├── imgs
        └── js
```

minifyらを色々する。htmlminもできる。

---

## production版 - JS, CSS読み込み

<pre class="html"><code>{{#if production}}
  &lt;link rel=&quot;stylesheet&quot; href=&quot;{{assets}}/css/base.min.css&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;{{assets}}/css/modules.min.css&quot;&gt;
  &lt;script src=&quot;{{assets}}/js/library-a.min.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;{{assets}}/js/library-b.min.js&quot;&gt;&lt;/script&gt;
{{/if}}
{{#if dev}}
  &lt;link rel=&quot;stylesheet&quot; href=&quot;{{assets}}/css/base.css&quot;&gt;
  &lt;link rel=&quot;stylesheet&quot; href=&quot;{{assets}}/css/modules.css&quot;&gt;
  &lt;script src=&quot;{{assets}}/js/library-a.js&quot;&gt;&lt;/script&gt;
  &lt;script src=&quot;{{assets}}/js/library-b.js&quot;&gt;&lt;/script&gt;
{{/if}}</code></pre>

こういうので書き分けられる

---

## production版 まとめ

* 色々重い処理する
* でもリリース時のみ
* watchは開発中専用

ということで普段はサクサク開発  
リリース用には色々やっちゃえる

----

## 結構めんどいこと

どこまでやるかによるけど

* Handlebarsの学習コストがそれなりにある
* ヘルパーの概念
* Handlebarsのテンプレートエンジン的な思想
* assembleとHandlebarsの関係

とかの理解がなかなか大変だった。  
でもHandlebars他でも使えるし許せるかも？

----

## まとめ

全てGruntでコントロール出来て  
結構素敵な感じがする

マッチする案件では試してみたい感
