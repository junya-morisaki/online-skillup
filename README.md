

## 環境
+ node: 10.15.0
+ npm: 6.2.0以上

## パッケージのインストール
`$ npm install`

## ローカルでの開発
ローカルで開発するときは以下の2つのコマンドを2つのターミナルを使って実行する。

### フロントエンドサーバーの立ち上げ
`$ npm start`  
[http://localhost:3000](http://localhost:3000)にアクセスしてフロントエンドの開発をする。  

### websocketサーバーの立ち上げ
別のターミナルを開いて、以下を実行する。  
`$ npm run server`  
実行すると[http://localhost:4000](http://localhost:4000)にsocketサーバーが立ち上がる。  
ここへの接続は既に設定されているため特に設定する必要はない。
開発中はここにアクセスしてもエラーになるが、フロントエンドのコードをビルドするとビルドされたファイルが表示される。  

## ディレクトリ構成
```
.
├── server/          # サーバーを設定するディレクトリ
│   ├── public/      # ビルドしたときに生成先(特に触りません)
│   └── server.js    # サーバーの起動を設定するファイル
│
├── src/                  # フロントエンドのコードを管理するディレクトリ
│   ├── css/              # cssディレクトリ
│   │   ├── resources/    # 変数などの設定ファイル置き場
│   │   └── base.scss     # bodyタグやulタグなど、タグに対してスタイルを設定するファイル
│   ├── html/             # htmlを生成するディレクトリ(特に触りません)
│   ├── images/           # 画像ファイルを配置するディレクトリ
│   └── javascripts/      # javascriptディレクトリ
│       ├── components/   # componentを管理するディレクトリ
│       ├── constants/    # 定数を管理するディレクトリ
│       ├── utils/        # 汎用的なモジュールを管理するディレクトリ
│       ├── App.vue       # Vue.jsのエントリーポイント
│       └── entry.js      # JavaScriptのエントリーポイント
│
│ (これ以降のファイルは基本的に触る必要はない)
├── .babelrc                       # 古いブラウザにもサポートする為に使用するbabelの設定ファイル
├── .editorconfig                  # インデントの数や改行コードなどを統一するための設定ファイル
├── .eslintrc.yml                  # JavaScriptのコードをチェックするルールが書かれている設定ファイル
├── .gitignore                     # gitリポジトリに登録して欲しくない内容を記述する設定ファイル
├── .stylelintrc.yml               # CSSのコードをチェックするルールが書かれている設定ファイル
├── package-lock.json              # npm installでバージョンがずれてしまわないようにするlockファイル
├── package.json                   # npm packageや実行スクリプトを管理する設定ファイル
├── Procfile                       # Herokuにデプロイした後に実行するコードを書くファイル
├── README.md                      # github上に表示される最初に読んで欲しい内容を書くファイル
├── webpack.config.base.js         # webpackでビルドするときの基本設定ファイル
├── webpack.config.development.js  # 開発時に使用されるwebpackの設定ファイル
└── webpack.config.production.js   # productionビルド時に使用されるwebpackの設定ファイル
```

## その他のコマンド
### フロントエンドのコードのビルド
`$ npm run build`  
server/public以下に生成物を配置する。  
生成物が配置されたことで、`$ npm run server`を実行している時は[http://localhost:4000](http://localhost:4000)にアクセスするとビルドしたファイルを見ることができる。


## この課題を通して学んだこと  
vue.js,node.js,webpack,websocket,eslinter など、初めて触れる技術が多く、フロントエンドの開発で使われる技術について幅広く学ぶことができたと思います。  
また、cssは苦手意識がありこれまでbootstrapなどで簡易的なデザインをして済ますことが多かったのですが、今回は素のcssで書いてみたのでcssの学習のよい機会にもなりました。

## 工夫した点  
+ フォームをページ上部に固定
+ 参加者リストをページ左に固定
+ LINE風のデザイン(自分が送ったメッセージは右側に表示、他人からのメッセージは左側に表示)
+ 自分より以前に接続したユーザも参加者リストに表示
+ 参加者リストで、切断したユーザの表示は半透明に
+ ユーザ名の変更をメッセージ上部で表示している名前の方にも反映
## 苦労した点  
vue.js,websocketの学習
