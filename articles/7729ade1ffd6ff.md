---
title: "Zennに移行する: Zenn Getting Started"
emoji: "✋"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: [Zenn, JavaScript, GitHub]
published: true
---

思うところあって、メインの技術ブログをZennに移行することにしました。
で、Zennで執筆するためには……と調べていたところ、Zennでは(エンジニア的に)お手軽な方法が用意されているようです。

ただ、Zenn側ではいわゆる「Getting Started」がなく、Zenn公式アカウントの記事として各機能について説明したものが用意されているようです。
軽くググってもそれらしいものがパッと見当たらなかったため、「無いなら自分で作るか」ということで本記事を書かせていただきます。

## 本記事でできるようになること

- ローカル環境で記事のプレビューが行えるようになる
- コマンドで設定Yamlを含む記事ファイルが作成できるようになる
- GitHubのリポジトリと連携し、プッシュするだけで記事を投稿できるようになる

## 前提条件

- Zennにアカウント登録できていること
- GitHubにアカウント登録できていること

## Zenn CLI環境構築

参考記事①: [Zenn CLIをインストールする](https://zenn.dev/zenn/articles/install-zenn-cli)
参考記事②: [Zenn CLIで記事・本を管理する方法](https://zenn.dev/zenn/articles/zenn-cli-guide)

Zennでは記事の執筆・プレビュー手段として`Zenn CLI`が用意されているようです。
`Zenn CLI`はnpmライブラリで、ローカル環境に記事ファイル作成やプレビューを行える環境が構築できます。
オンラインエディターも存在しますが、後述のGitHub連携のこともあり個人的にはローカル環境での執筆をオススメします。
本項では`Zenn CLI`を使うための環境を構築していきます。

好きなところに作業ディレクトリを作成し、作業ディレクトリでターミナルを開きます。

```terminal:ターミナルでやる場合
$ mkdir {作業ディレクトリ名}
$ cd {作業ディレクトリ名}
```

作業ディレクトリでnpmを初期化し、`Zenn CLI`を導入してセットアップしましょう。

```terminal
$ npm init --yes
$ npm i zenn-cli
$ npx zenn init
```

導入後、正常に動作するか確認しましょう。

```terminal
$ npx zenn preview
```

適当なブラウザで`http://localhost:8000/`にアクセスしてみてください。
`Zenn Editor`が表示されればOKです！

![](/images/1/2022-05-05_3.png)

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

## GitHub連携

参考資料: [GitHubリポジトリでZennのコンテンツを管理する](https://zenn.dev/zenn/articles/connect-to-github)

Zennはなんと素晴らしいことにGitHubリポジトリと連携できるようです。
指定したブランチを更新するだけで勝手に同期(デプロイ)してくれます。
本項ではGitの初期化、GitHubリポジトリの作成と同期設定を行なっていきます。

先ほど環境構築した作業ディレクトリでGitを初期化し、コミットしましょう。

```terminal
$ git init
$ git add -A
$ git commit -m "最初のコミット"
```

次はGitHub側の設定を行なっていきます。
パブリックでもプライベートでもいいので、適当にリポジトリを作成します。
作成できたら初期ページの指示に従って作業ディレクトリをプッシュしましょう。

```terminal
$ git remote add origin https://github.com/{GitHubアカウント名}/{リポジトリ名}.git
$ git branch -M main
$ git push -u origin main
```

次はGitHubとZennの連携を行うのですが、この時点では`main`ブランチしか存在しませんね。
Zennの同期設定はブランチ名を指定することができるので、`main`ブランチ以外を利用したい場合は作っておくといいでしょう。
(後で変更できるので、後回しでも大丈夫です)

```terminal
$ git branch {ブランチ名}
$ git push origin {ブランチ名}
```

リポジトリができたらZennの[デプロイ管理](https://zenn.dev/dashboard/deploys)から連携していきます。
こちらは[Zenn公式アカウントの記事](https://zenn.dev/zenn/articles/connect-to-github)を見ながら行うといいでしょう。

設定が完了したらちゃんと連携できているか確認しましょう。
`npx zenn new:article`で記事ファイルを作成し、適当に何か書きます。
このとき、先述の`Zenn Editor`を使うとイケイケな気持ちになれます。

![](/images/1/2022-05-05_2.png)

自動作成した記事は下書き設定になっているため、Zennと連携しても公開されないので安心です。
(記事ファイル6行目くらいに記述されている`published: false`のこと)

記事が書けたら先ほど指定したブランチにコミットし、リポジトリにプッシュしましょう。
あるいは別ブランチでプッシュし、GitHubでプルリクエストを出してマージしてもいいです。

指定したブランチが更新された場合、Zennは変更を検知して同期してくれます。
非公開記事は普通に見ることはできないため、[デプロイ管理](https://zenn.dev/dashboard/deploys)から確認しましょう。
うまく連携できていれば、「最近のデプロイ」として先ほど追加した記事を閲覧できるはずです。

![](/images/1/2022-05-05_1.png)

## おわりに

GitHub連携してくれるのは非常にありがたいですね。
プレビュー機能が用意されているのもグッド。
これでローカルのエディターからコピペすることなくスムーズに記事が書けます。嬉しい。
