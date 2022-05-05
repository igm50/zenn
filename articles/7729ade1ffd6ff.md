---
title: "Zennに移行する前に: Zenn Getting Started"
emoji: "✋"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Zenn]
published: false
---

思うところあって、メインの技術ブログを慣れ親しんだQiitaからZennに移行することにしました。
で、Zennで執筆するためには……と調べていたところ、Zennではいろいろ(エンジニア的に)お手軽な方法が用意されているようです。

ただ、Zenn側ではいわゆる「Getting Started」がなく、Zenn公式アカウントの記事として各機能について説明したものが用意されているようです。
軽くググってもそれらしいものがパッと見当たらなかったため、「無いなら自分で作るか」ということで本記事を書かせていただきます。

## 前提条件

- Zennにアカウント登録できていること
- GitHubにアカウント登録できていること

## 1. Zenn CLI環境構築

参考記事①: [Zenn CLIをインストールする](https://zenn.dev/zenn/articles/install-zenn-cli)
参考記事②: [Zenn CLIで記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)

Zennでは記事の執筆・プレビュー手段として`Zenn CLI`が用意されているようです。
`Zenn CLI`はシンプルなnpmライブラリのようなので、ある程度JavaScriptの知識があれば容易に扱えるかと思います。
本項では`Zenn CLI`を使うための環境を構築していきます。

好きなところに作業ディレクトリを作成し、作業ディレクトリでターミナルを開きます。

```terminal:ターミナルで全部やる場合
$ cd ${MY_WORK_DIR}
$ mkdir myZennDir
$ cd myZennDir
```

作業ディレクトリでnpmを初期化し、`Zenn CLI`を導入してセットアップしましょう。

```console
$ npm init --yes
$ npm i zenn-cli
$ npx zenn init
```

導入後、正常に動作するか確認しましょう。
下記コマンドを実行し、適当なブラウザで`http://localhost:8000/`にアクセスして見てください。

```terminal
$ npx zenn preview
```

`Zenn Editor`が表示されればOKです！

### npxではなくnpmでやりたい場合

Zenn側の記事では`npx zenn ***`コマンドで実行する形式で記述されています。
でも私はnpm scriptでやりたい。是が非でも`npm run xxx`でやりたい。npxってタイプし慣れてないから。

そんなわけで、npm scriptでやる方法を記述しておきます。
`package.json`に以下のように記述するだけです。

```json:package.json
{
  (略),
  "scripts": {
+     "p": "zenn preview",
+     "n:a": "zenn new:article",
+     "n:b": "zenn new:book",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  (略)
}
```

`npx zenn ***`を`"xxx": "zenn ***"`と記述しただけですね。
これで`Zenn Editor`を開きたい場合は`npm run p`でできるようになりました。やったね！

## 2. GitHub連携

参考資料: [GitHubリポジトリでZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)

Zennはなんと素晴らしいことにGitHubリポジトリと連携できるようです。
指定したブランチを更新するだけで勝手に同期(デプロイ)してくれます。
本項ではリポジトリの作成と同期設定を行なっていきます。



