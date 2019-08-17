# Vue.js

## バインディング


## コンポーネント


## イベント

## ルーティング

## ロジックの共通化 -> Mixin


# npm

Pythonのようなパッケージシステム管理、プログラムの実行などができる。
Node.js(server-side javascript)と呼ばれるプログラムに同梱しています。

## 4つのインストール

インストール済みパッケージはpackage.jsonに保存されます。

* 開発物の中でインストール

```
> npm install --save

```


* プロジェクトの開発環境にインストール

```
> npm install foobar --save-dev

```

* PC全体にインストール（コマンドの追加など）

```
> npm install -g foobar

```



## Vue-CLI

* 雛形を作成します。

```
> vue create アプリ名

```

* 実行

```
> npm run serve

```

* プロダクションビルドの作成

```

> npm run build

```


### API開発用ダミー

```
> npm install -g json-server

```

db.jsonファイル
```
{
  "movies": [
    {"id": 1, "name": "The Godfather", "director":"Francis Ford Coppola", "rating": 9.1},
    {"id": 2, "name": "Casablanca", "director": "Michael Curtiz", "rating": 8.8}
  ]
}
```

```
json-server --watch db.json

```
