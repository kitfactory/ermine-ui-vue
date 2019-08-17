# Web(Single Page Application)開発でフロントエンド開発

# 開発のベースとなるパッケージ管理システム：npm

(Pythonでのpipのように)JavaScriptの世界では大きく２つの手法でライブラリが管理されています。一つがnpmであり、もう一つがyarnと呼ばれるコマンドです。npmの方が古くからあるツールです。サーバーサイドのプログラム実行Node.js(server-side javascript)と呼ばれるプログラムに同梱しています。

まずはNode.jsからインストールしてみましょう。



## 4つのインストールを覚えよう

インストール済みパッケージはpackage.jsonに保存されます。

* 開発物の中で利用するライブラリをインストールする。

```
> npm install foobar --save

```

* プロジェクトのビルドなどに使用するツールチェインなど、プロジェクトの開発環境にインストールする。

```
> npm install foobar --save-dev

```

ここまでのコマンドでライブラリはnode_modulesと呼ばれるフォルダにインストールされます。
また、package.jsonと呼ばれるファイルに必要なライブラリが依存性も含め記録されます。
ですので、Gitなどからソースコードを取得した場合は、自分のプロジェクトにはこれらのライブラリは含まれていないので、以下でpackage.jsonに従って依存ライブラリをインストールします。

```
> npm install

```

また、開発環境のコマンドラインにコマンドが必要になるツールがあります。
それらは、以下のようにインストールします。

* PC全体にインストール（コマンドの追加など）

```
> npm install -g foobar

```

## 開発準備：Vue-CLI

Vue.jsを使用するにあたっては、Vue-CLIというコマンドラインツールを使用するとプロジェクトの雛形作成が簡単にできます。ですので、今回はこのツールを使って、最初のホームページを作成してみましょう。

vue-cliのインストール

```
> npm install -g @vue/cli

```

* 雛形を作成します。

```
> vue create myapp(アプリ名)

```

* 実行

```
> cd myapp
> npm run serve

```

(※osxではローカルホストを解決できる必要があります。)

ここまでできたら　[http://localhost:8080/](http://localhost:8080/)にアクセスしてみましょう。

また、ローカル環境での開発が終わった場合、JavaScriptなどをパッケージにします。
このときにビルド作業を以下のコマンドで行います。このコマンドでできたdist以下を持っていくと考えてください。

* プロダクションビルドの作成

```

> npm run build

```


##　開発準備：API開発用ダミー

サーバーとの通信をしばらくの間はダミーで開発することになります。
json-serverでダミーのRESTサーバーを構築することができます。

```
> npm install -g json-server

```

用意するAPIの返答ファイル：db.jsonファイル
```
{
  "movies": [
    {"id": 1, "name": "The Godfather", "director":"Francis Ford Coppola", "rating": 9.1},
    {"id": 2, "name": "Casablanca", "director": "Michael Curtiz", "rating": 8.8}
  ]
}
```

```
> json-server --watch db.json

```

このように実行すると、ローカルホストの3000番ポートでダミーのAPIを返却するAPIサーバーが実行されます。
以下で確認してみましょう。

```
> curl -X GET "http://localhost:3000/movies"

```

開発サーバーからAPIのHTTPにリバースプロキシを設定する必要があります。
以下のようにすることでapi/*を3000番ポートのダミーサーバーにリダイレクトできます。

```
// vue.config.js
module.exports = {
    devServer: {
        proxy: {
            "/api/*": {
                target: "http://localhost:3000",
                secure: false
            }
        }
    }
};
```


# Vue.js　- Webアプリケーションフレームワーク -

Vue.jsはJavaScriptによるフロントエンド用のアプリケーションフレームワークです。最近のWebフレームワークに共通の考え方を取り入れ、かつ、簡単に開発できることで人気のフレームワークです。

## フロントエンドは、どのように動くのか

* コンポーネント
画面の部分の表示を管理するコンポーネントを作成し、そのコンポーネントが表示を受け持つようにします。

* バインディング
コンポーネントの持つ値を表示する方向が決まっており、値が変わったら表示に自動的に反映されます。

## Vue.jsでのコンポーネントの作り方

Vue.jsではコンポーネントをvueファイルに記載して作成します。
vueファイルは以下の２つの要素からできています。

```
<template>
    <!--HTML要素の構造を示す->
</template>
<script>
    <!--ロジックの内容を示す->
</script>
```

それでは、新しくコンポーネントを作成することにします。
まずはコンポーネントの持つ値を表示する単純なコンポーネント、MyViewコンポーネントを作成してみましょう。

### コンポーネントの考え方：バインディング

まずはtemplate内を記載しましょう。コンポーネントが持つプロパティ値を表示する際に口ひげバインディングを使用します。まずはmsgプロパティ値が表示されるようにバインディングします。

__MyView.vue__

```
<template>
    <h1>{{msg}}</h1>
</template>
```

次にscriptを記述します。script内では、コンポーネントの内容（どんなプロパティを持つか、どんなメソッドを持つ）を記載し、それを何コンポーネントとして登録するかを決めます。

また、コンポーネントの値の保持方法には、２種類存在します。dataとprops(プロパティ)です。コンポーネントの中だけで閉じている場合はdataを使います。一方で上位コンポーネントから値を引き継ぐ場合はプロパティを使用します。

今回はmsgのdataのみを有し、コンポーネントの処理を実行する関数が存在しないコンポーネントを、MyViewコンポーネントとして作成してみます。

__MyView.vue__

```
<template>
    <h1>{{msg}}</h1>
</template>
<script>
export default {
  name: 'MyView',
  data: function(){
      return {
          "msg": "Hello MyView!!"
      }
  },
  methods: {
  }
}
</script>
```

この値をVue-CLIで作成されたアプリに組み込んでみましょう。App.vueファイルを開き、HelloWorld.vueの代わりにMyViewを組み込みます。

```
<script>
import MyView from './components/MyView.vue'
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
//    HelloWorld,
    MyView
  }
}
</script>
```

## イベントとコンポーネントのプロパティ操作

Webフレームワークでの良い点はアクションに対して、何かをするというJavaScript部分の処理を決まった枠組みで処理することができるようになることです。MyViewテンプレートでbutton要素を押したらなにかするといった形式にしてみます。

テンプレート部分にクリック時にメソッドを呼ぶ操作を加えてみます。

```
<template>
    <h1>{{msg}}</h1>
    <button v-on:click="greet()">Button</button>
</template>
```

これでボタンが押されたときにscriptに記載したgreetメソッドを呼び出すことになります。
それでは、script部分にgreetメソッドを追加します。

```
<template>
    <h1>{{msg}}</h1>
    <button v-on:click="greet()">Button</button>
</template>
<script>
export default {
  name: 'MyView',
  data: function(){
      return {
          "msg": "Hello MyView!!"
      }
  },
  methods: {
      greet: function(){
          this.msg = "Pushed!!"
      }
  }
}
</script>
```

### コンポーネントの表示を豊かにする。 v-for / v-if / v-show

繰り返し要素の表示は大変ですね。そのようなリストを表示する場合は、v-forを使用することで自動的にリスト要素を展開して、表示することができます。まずはdataにlist_dataを追加してみましょう。

```
<script>
export default {
  name: 'MyView',
  data: function(){
      return {
          "msg": "Hello MyView!!"
          "list_data": ["foo","bar","baz"]
      }
  },
  methods: {
      greet: function(){
          this.msg = "Pushed!!"
      }
  }
}
</script>

```

次にテンプレート部分を編集してみましょう。

```
<template>
    <h1>{{msg}}</h1>
    <button v-on:click="greet()">Button</button>
    <ul>
        <li v-for item in data_list>
            {{item}}
        </li>
    </ul>
</template>
```

また、データの状態によって表示を変えたいという場合がありますね、そのような場合には、v-if/v-showを使用することができます。


## 高度なコンポーネントの使用方法

### コンポーネントに親子関係を作る

親子関係をもったコンポーネントを作成することは簡単です。App.vueとMyView.vueですでに親子関係ができています。親子関係を持つコンポーネントはテンプレートとスクリプト側で、それぞれ対応が必要です。

テンプレート側ではコンポーネントのタグを配置することが必要です。

```
<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <MyView />
  </div>
</template>

script側では子になるコンポーネントをimportし、componentsに登録しておくことが必要です。

```
<script>
import MyView from './components/MyView.vue'

export default {
  name: 'app',
  components: {
    MyView
  }
}
</script>

```

## 親子の連携イベント

* インスタンス間の通信

親から子のプロパティを渡すには、Vue で親子間の通信の一般的なパターンは、props/data を使用することができます。子にコールバックとして親メソッドを渡しています。

* 子から親のメソッドを呼び出す

this.$emit('イベント名',引数)でイベントを創出することができます。
上位では、そのイベントを取得できるように、v-on:イベント名=""でメソッドを起動することができます。


## ロジックの共通処理 -> Mixin

ロジックの共通処理にはMixinを使います。AngularではServiceといい別の方法になりますので、必ずしも共通に使える概念ではないのですが、同様な機能を別の方法で実現する必要があるということは理解しましょう。

Mixinはコンポーネントからtemplateを除いたものと言えます。コンポーネントはvueファイルで作成しましたが、テンプレートがないため、jsファイルで作成することができます。

```

export default {
    // ..
    data : function(){
        ...
    },
    methods: {
        ...
    }
    
}

```

なお、Mixinはグローバルにすることで、よりサービスらしく使用することができます。

# Vue Router によるルーティング

画面の切り替えを行いたい場合、Vue Routerを使います。ルーティングを行うことで、routerに割り当てたコンポーネントの中をURLに応じて切り替えることが可能になります。vue cliではプロジェクトを作成する際に、routerを含めることができます。



# デザインを加える
