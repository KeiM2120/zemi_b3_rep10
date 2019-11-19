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

+++?image=img/7-05.png&size=contain



+++

#### 投稿されるメッセージを出力
```ejs
<% for(var i in collection) { %>
<%- include('data_item', {val:collection[i]}) %>
<% } %>
```

+++?code=src/7-04.ejs&size=contain

#### データ構造の定義　data_item.ejs

+++

### ユーザのホームページを作成する

home.ejs

+++?image=img/7-06.png&size=contain

+++

### ユーザ新規作成ページの作成

add.ejs

+++?image=img/7-07.png&size=contain


+++

### スタイルシートの用意

style.css

+++?image=img/7-08.png&size=contain

+++
### メインのアプリケーション部分を作成

app.js

+++?image=img/7-09.png&size=contain

+++
### トップページの動作を作成

rouer/index.js

+++?image=img/7-10-1.png&size=contain
+++?image=img/7-10-2.png&size=contain


+++
### ユーザ管理の動作を作成

user.js

+++?image=img/7-11-1.png&size=contain
+++?image=img/7-11-2.png&size=contain

+++

### ユーザホームページ部分の動作を作成

home.js

+++?image=img/7-12-1.png&size=contain
+++?image=img/7-12-2.png&size=contain

+++

### 実際に使ってみる

+++?image=img/bord01.png&size=contain
+++?image=img/bord02.png&size=contain
+++?image=img/bord03.png&size=contain
+++?image=img/bord04.png&size=contain
+++?image=img/bord05.png&size=contain


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

## 7-2 Markdownデータ管理ツール