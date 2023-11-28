---
title: "WSL2とVSCodeを用いてローカル環境でのzenn執筆環境を整える"
emoji: "🙌"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [VSCode,zenn]
published: false
Author: yuuma
---

# はじめに

__本記事の対象となる方__

■ WSL2とVSCodeを用いてZennの執筆環境を整えたい人


この記事では､以下の3点をご紹介します｡
__(1) WSL2のディストリビューション（Ubuntu）にZenn CLIをインストールする__
__(2) VSCodeの拡張機能Zenn Editorを使ってCWLを編集する環境を整える__

&nbsp;

# 前提
- WSL2（Ubuntu22.04）をインストールしている
- GitHubにリポジトリをpushできる

# 1. Ubuntuにnode.jsとnpmを入れる
初めに、Ubuntuにnode.jsとnpmを入れます。npmとはNode.jsのパッケージを管理するシステムのことです。これを用いてZenn CLIをインストールします。__[【2023年4月版】Ubuntu に node.js と npm を入れたい（バージョン管理も）](https://qiita.com/nouernet/items/d6ad4d5f4f08857644de)__ の手順をそのまま掲載しています。

### aptのパッケージを更新
まずは、aptのパッケージを最新にします。`sudo apt update` でaptが管理しているパッケージリストを更新します。次に、 `sudo apt upgrade -y` でパッケージをアップデートします。 

```bash=
sudo apt update
sudo apt upgrade -y
```


### aptを使ってとりあえずのnode.jsとnpmをインストール
次に､node.jsとnpmをaptでインストールします。
```bash=
sudo apt install -y nodejs npm
```

### n（バージョン管理システム）をインストール
```bash=
sudo npm install n -g
```

### 最新のnode.jsとnpmをインストール
```bash=
sudo n stable
```

### 最初に入れたnode.jsとnpmをアンインストール
```bash=
sudo apt purge -y nodejs npm
sudo apt autoremove -y
```


&nbsp;

# 2. Zenn CLIを導入して、ローカルで記事を編集する
npmをインストールしたら、Zenn CLIを導入します。Zenn CLIを用いると、ローカルの好きなエディターで投稿コンテンツの作成・編集ができるようになります。
参考：[Zenn CLIをインストールする](https://zenn.dev/zenn/articles/install-zenn-cli)


### CLIをインストールする
Zennのコンテンツを管理したいディレクトリで、以下のコマンドを実行します。
```bash:workspace下で実行
npm init --yes # プロジェクトをデフォルト設定で初期化
npm install zenn-cli # zenn-cliを導入
```
これでディレクトリにCLIがインストールされます。

### Zenn用のセットアップを行う
続いて以下の`npx`コマンドを実行します。
```bash:workspace下で実行
npx zenn init
```
以上のコマンドを順に実行すると、ディレクトリ構成が以下のようになります。
```bash=
workspace/
├── node_modules/
├── articles/
├── books/
├── package-lock.json
├── package.json
└── README.md
```
`articles/` ディレクトリに`.md` ファイルを配置すると、記事を作成することができます。

### CLIをアップデートする
Zenn CLIの表示がzenn.devと異なるときやCLI利用時に更新通知が表示されたときは下記のコマンドでアップデートを行ってください。
```bash:workspace下で実行
npx install zenn-cli@latest
```

### ローカルで記事を閲覧する
```bash:workspace下で実行
npx zenn preview
```
で記事を閲覧することができます。コマンドを実行すると、

```
👀 Preview: http://localhost:8000
```
と出てきます。webブラウザで`http://localhost:8000`にアクセスすると、記事を閲覧することができます。


# 3. VSCodeの拡張機能Zenn Editorを使ってCWLを編集する環境を整える
[Zennの執筆を支援するVSCode拡張Zenn Editor](https://zenn.dev/negokaz/articles/aa4e12b76d516597a00e)に詳しく掲載されています。これを用いると、VSCode上でZennの記事をプレビューすることができます。こちらのプレビュー機能は、おそらくZennのサイトで公開するときと同じように表示されます。

![zenn preview](/images/20231127/20231127_01.png =1000x)

:::message
Zenn Editorをアクティブにするためには、CLIをインストールしたディレクトリ（先ほどの`workspace\`）を開いている必要があります。
:::