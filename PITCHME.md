## Node.js 超入門

### ECL 輪読発表
#### 蒔田圭輔


---

## OUTLINE

- ### 7-1 DB版ミニ伝言板
- ### 7-2 Markdownデータ管理ツール

---

## 7-1 DB版ミニ伝言板

+++

### 本節での目的
6章までで作った伝言板アプリに対して
DB連携をしてアプリケーションを作ろう

+++

### Express Generatorでひな形作成

```shell
$ express -e mini_board_2
```

+++

### 必要なパッケージのインストール

```shell
$ npm install --save express-sessions
$ npm install --save express-validator
$ npm install --save mysql
$ npm install --save knex
$ npm install --save bookshelf
```

+++

### テーブルの定義をする(1)

#### ユーザの情報を管理するテーブル user

- id: INT
  -  PRIMARY_KEY, AUTO_INC : 識別用番号 
- name : VARCHAR(255)
  - ユーザの名前
- password: VARCHAR(255)
  - ユーザのパスワード
- comment: VARCHAR(255)
  - コメント


+++

#### SQL文にすると

```sql
CREATE TABLE `users` (
        `id`        INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
        `name`        TEXT NOT NULL,
        `password`        TEXT NOT NULL,
        `comment`        TEXT
);
```

+++

### テーブルの定義をする(2)

#### 掲示板のメッセージを管理するテーブル messages

- id: INT
  -  PRIMARY_KEY, AUTO_INC : 識別用番号 
- user_id: INT
  -  投稿したユーザの識別番号
- message : TEXT
  - 投稿内容
- created_at: DATETIME
  - 投稿日時
- updated_at: DATETIME
  - 更新日時

+++

```sql
CREATE TABLE `messages` (
        `id`        INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT UNIQUE,
        `user_id`        INTEGER NOT NULL,
        `message`        TEXT NOT NULL,
        `created_at`        REAL,
        `updated_at`        REAL
);
```

+++

### トップページをつくる


index.ejs

+++?image=img/7-05.png&size=auto 90%



+++

#### 投稿されるメッセージを出力
```ejs
<% for(var i in collection) { %>
<%- include('data_item', {val:collection[i]}) %>
<% } %>
```

+++?code=src/7-04.ejs&size=auto 90%

#### データ構造の定義　data_item.ejs

+++

### ユーザのホームページを作成する

home.ejs

+++?image=img/7-06.png&size=auto 90%

+++

### ユーザ新規作成ページの作成

add.ejs

+++?image=img/7-07.png&size=auto 90%


+++

### スタイルシートの用意

style.css

+++?image=img/7-08.png&size=auto 90%

+++
### メインのアプリケーション部分を作成

app.js

+++?image=img/7-09.png&size=auto 90%

+++
### トップページの動作を作成

routes/index.js

+++?image=img/7-10-1.png&size=auto 90%
+++?image=img/7-10-2.png&size=auto 90%


+++
### ユーザ管理の動作を作成

user.js

+++?image=img/7-11-1.png&size=auto 90%
+++?image=img/7-11-2.png&size=auto 90%

+++

### ユーザホームページ部分の動作を作成

home.js

+++?image=img/7-12-1.png&size=auto 90%
+++?image=img/7-12-2.png&size=auto 90%

+++

### 実際に使ってみる

+++?image=img/bord01.png&size=auto 90%
+++?image=img/bord02.png&size=auto 90%
+++?image=img/bord03.png&size=auto 90%
+++?image=img/bord04.png&size=auto 90%
+++?image=img/bord05.png&size=auto 90%


+++

### ここまでのまとめ

- アプリケーションの大枠は
  - Express Generator  |
- node.jsで様々なパッケージを管理する機能
  - npm(Node Package Manager)|
- データベースとして利用したサービス
  - MySQL|
- データベースでの操作を簡単にするツール
  - BookShelf|


---

## 7-2 Markdown<br/>データ管理ツール

+++

### 本節での目的
Markdownで作成した文書を管理、検索する機能のアプリケーション

+++

そもそもMarkdownって...?

- このスライドのために嫌になるほど書いたので割愛|

+++

### 下準備

```shell
$ express -e mark_app

$ npm install --save express-sessions
$ npm install --save express-validator
$ npm install --save mysql
$ npm install --save knex
$ npm install --save bookshelf
```

+++

### テーブルの定義をする(1)

#### ユーザの情報を管理するテーブル user

前節にて作成してるのでそれを再利用

+++

### テーブルの定義をする(2)
#### markdownの情報を管理するテーブル markdata

- id: INT
  - PRIMARY_KEY, AUTO_INC : 識別用番号 
- user_id: INT
  - 投稿したユーザの識別番号
- title: VARCHAR(255)
  - タイトル
- content: TEXT
  - 実際の中身
- created_at: DATETIME
  - 投稿日時
- updated_at: DATETIME
  - 更新日時

+++

## view部分の作成
### 文書検索ページ

index.ejs

+++?image=img/7-15.png

+++

### 文書検索フォーム

```ejs
<form action="/" method="post">
    <input type="text" name="find" size="40" value="<%= form.find %>">
    <input type="submit" value="検索">
</form>
```

+++

#### 検索結果の表示部分

```ejs
<% for (var i in content) { %>
    <% if (content[i].attributes.user_id != login.id) { continue; } %>
        <li>
            <a href="/mark/<%=content[i].id %>">
                <%=content[i].attributes.title %>
            </a>
        </li>
<% } %>
```

+++

### ログイン画面の作成

login.ejs

割愛|

+++

### markdownの記入と表示を行う

mark.ejs

+++?image=img/7-18.png

+++

### スタイルシートの用意

style.css

+++?image=img/7-19.png

+++
### アプリケーション部分の作成
app.js

+++?image=img/7-20-1.png
+++?image=img/7-20-2.png

+++

### トップページの動作を作成
routes/inde.js

+++?image=img/7-21-1.png
+++?image=img/7-21-2.png

+++

### ログイン動作の作成

routes/login.js

+++?image=img/7-22-1.png
+++?image=img/7-22-2.png

+++

### markdownデータの追加を行う動作の作成

routes/add.js

+++?image=img/7-23-1.png
+++?image=img/7-23-2.png


### markdownデータの表示を行う動作の作成

routes/mark.js

+++?image=img/7-24-1.png
+++?image=img/7-24-2.png
