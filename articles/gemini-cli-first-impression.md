---
publication_name: "typebase_dev"
title: "最速でGemini CLIを試す"
emoji: "🚀"
type: "tech"
topics: ["gemini", "cli", "ai", "geminicli"]
published: true
---

日本時間の 2025 年 6 月 25 日の午後 10:08 に、正式に Gemini CLI が発表されました。

https://x.com/GoogleCloudTech/status/1937860467843625124

公式の X によると

> Access Gemini 2.5 Pro with 1M token context window, 60 requests/min, and 1,000 requests/day—at no cost with a free Gemini Code Assist license → https://goo.gle/3HW0jL0

とあり、しっかり無料枠が用意されておりかなり太っ腹ですね。

Gemini CLI の詳細は以下の記事から確認できます。

https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/

## Gemini CLIをインストールする

それではさっそく、Gemini CLI をインストールして使ってみましょう。
インストールの手順は以下のリポジトリから確認できます。

※Gemini CLI を使うためには Node.js の v18 以上が必要です。

https://github.com/google-gemini/gemini-cli/

自分は以下のコマンドでインストールしました。

```sh
npm install -g @google/gemini-cli
```

そして、`gemini` を実行します。

```sh
gemini
```

すると以下のような画面が表示され、まずはテーマの選択を求められます。
選択できるテーマが結構多いですね👀
私は GitHub を選びました。

![](/images/gemini-cli-first-impression/select-theme.png)

続いて、認証に進みます。
Login with Google を選択すると、ブラウザが開いて Google アカウントの認証が求められます。

![](/images/gemini-cli-first-impression/auth.png)

これで Gemini CLI が使えるようになりました！

とりあえず `/help` を開いてみます。

![](/images/gemini-cli-first-impression/help.png)

めちゃくちゃ Claude Code っぽいですね。

一旦終了してみます。

![](/images/gemini-cli-first-impression/quit.png)

## 感想

CLI の操作感がめちゃくちゃ Claude Code っぽいですね。

ただ、実際の挙動などはもう少しじっくり触ってみないとわからないと思うので、さらに検証を続けていきたいと思います。
