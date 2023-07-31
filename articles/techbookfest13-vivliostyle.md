---
publication_name: "typebase_dev"
title: "技術書典13の執筆環境にVivliostyleを採用しました"
emoji: "📚"
type: "tech"
topics: ["vivliostyle", "markdown", "vfm", "css組版", "技術書典"]
published: true
---

こんにちは。[株式会社Typebase](https://typebase.dev/) の代表でエンジニアの寺嶋（[@y_temp4](https://twitter.com/y_temp4)）です。

2022 年 9 月 10 日から開催されている [技術書典13](https://techbookfest.org/event/tbf13/market) で [Next.js, Prisma, GraphQL Code Generator で作るフルスタックWebアプリケーション](https://techbookfest.org/product/dZ2G3HZrREypJHMuAvHDCM) という本を公開しました。

その際に、執筆環境として [Vivliostyle](https://vivliostyle.org/ja/) を採用したため、この記事では初めて Vivliostyle を使ってみた感想や、ハマりポイントについて解説していきます。

## Vivliostyle とは？

![](/images/techbookfest13-vivliostyle/vivliostyle.png)

[Vivliostyle](https://vivliostyle.org/ja/) は Web の技術を駆使して組版システムを作るためのオープンソース・プロジェクトです。執筆環境は npm で構築でき、スタイリングは CSS で行えます。

```sh
$ npm create book <directory>
```

参考：[Create Book で同人誌を作ろう！ | Vivliostyle](https://vivliostyle.org/ja/make-books-with-create-book/)

原稿はマークダウンで作成していきます。マークダウンは [Vivliostyle Flavored Markdown(VFM)](https://vivliostyle.github.io/vfm/#/vfm) で記述していくことになりますが、自分はマークダウンの互換性を考慮してそこまで凝った記法は使っていません。

## Vivliostyle を選んだ理由

技術書を書くなら最初の選択肢に挙がるのが [Re:VIEW](https://reviewml.org/ja/) ではないでしょうか。自分も最初は Re:VIEW で書こうと思ったのですが、Re:VIEW 独自の記法や設定などを学ぶのが面倒に感じてしまい、別の選択肢を取ることにしました。

そこで色々と調べていて見つけた Vivliostyle は、マークダウンベースで記述しやすく、環境も Node.js さえ入っていればすぐに構築できそうで個人的には好みでした。

ただし、マークダウンで記述する関係から表現力が劣る部分や、Vivliostyle 特有の問題もいくつか残っているのも事実です。今回のケースではそこまで問題になりませんでしたが、より細かい設定をしたい場合などは、おとなしく Re:VIEW を選んでおいたほうが良いと思います。

## Vivliostyle のハマりポイント

Vivliostyle を使って原稿を書いていて、いくつか困ったことがあったためその解決方法とともに紹介していきます。

### 1. 目次がいい感じに生成できない

[チュートリアル⑦目次の作成 | Vivliostyle](https://vivliostyle.org/ja/tutorials/create-table-of-contents/) にあるように、Vivliostyle は目次の自動生成ができるのですが、この機能で生成した目次には章のタイトルしか表示されません。

自分は節も表示したかったため、独自に目次を生成するスクリプトを書いて対応しました。

```ts:generate-toc.ts
// @ts-expect-error
import config from "../vivliostyle.config";
import fs from "fs";
import { marked } from "marked";

const entry: string[] = config.entry.filter(
  (e: string | Record<string, unknown>) => typeof e === "string"
);
const tocFromEntry = entry
  .map((content) => {
    const s = fs.readFileSync(content, { encoding: "utf8" });
    const tokens = marked.lexer(s);
    const headings = tokens.filter(
      (t): t is marked.Tokens.Heading => t.type === "heading"
    );
    const listItem = headings.reduce((prev, crr) => {
      return (prev += `${"    ".repeat(crr.depth - 1)}1. [${crr.text}](${content
        .replace(".md", ".html")
        .replaceAll(" ", "%20")}${
        crr.depth === 1
          ? ""
          : `#${crr.text
              .replaceAll(" ", "-")
              .replaceAll(".", "")
              .toLocaleLowerCase()}`
      })\n`);
    }, "");
    return listItem;
  })
  .join("");

const toc = `<nav id="toc" role="doc-toc">\n\n## 目次\n\n${tocFromEntry}\n</nav>`;

fs.writeFileSync("toc.md", toc, { encoding: "utf8" });

console.log("✨ toc.md generated.\n");
console.log(toc);
```

実行すると、以下のような感じで出力されます。

```sh
$ yarn generate-toc
yarn run v1.22.19
$ ts-node scripts/generate-toc.ts
✨ toc.md generated.

<nav id="toc" role="doc-toc">

## 目次

1. [Next.jsのセットアップ](contents/Next.jsのセットアップ.html)
    1. [各種設定の追加](contents/Next.jsのセットアップ.html#各種設定の追加)
        1. [拡張機能の有効化](contents/Next.jsのセットアップ.html#拡張機能の有効化)
        1. [Node.jsのバージョン指定](contents/Next.jsのセットアップ.html#nodejsのバージョン指定)
        1. [ESLintとPrettierの設定](contents/Next.jsのセットアップ.html#eslintとprettierの設定)
        1. [ディレクトリ構造の変更](contents/Next.jsのセットアップ.html#ディレクトリ構造の変更)
        1. [Tailwind CSSの追加](contents/Next.jsのセットアップ.html#tailwind-cssの追加)
1. [Prismaのセットアップ](contents/Prismaのセットアップ.html)
    1. [Prismaの概要](contents/Prismaのセットアップ.html#prismaの概要)
    1. [ライブラリのインストール](contents/Prismaのセットアップ.html#ライブラリのインストール)
    1. [スキーマファイルの生成](contents/Prismaのセットアップ.html#スキーマファイルの生成)
    1. [NextAuth.jsの導入](contents/Prismaのセットアップ.html#nextauthjsの導入)
        1. [各種ライブラリのインストール](contents/Prismaのセットアップ.html#各種ライブラリのインストール)
        1. [プロバイダーの設定](contents/Prismaのセットアップ.html#プロバイダーの設定)
        1. [DBスキーマの追加](contents/Prismaのセットアップ.html#dbスキーマの追加)
        1. [Dockerを用いたローカル環境の構築](contents/Prismaのセットアップ.html#dockerを用いたローカル環境の構築)
        1. [メールアドレスログインの実装](contents/Prismaのセットアップ.html#メールアドレスログインの実装)
1. [GraphQL Code Generatorのセットアップ](contents/GraphQL%20Code%20Generatorのセットアップ.html)
    1. [GraphQL Code Generatorの概要](contents/GraphQL%20Code%20Generatorのセットアップ.html#graphql-code-generatorの概要)
    1. [GraphQL Code Generatorの動作確認](contents/GraphQL%20Code%20Generatorのセットアップ.html#graphql-code-generatorの動作確認)
    1. [リゾルバーの作成](contents/GraphQL%20Code%20Generatorのセットアップ.html#リゾルバーの作成)
    1. [GraphQL エンドポイントの作成](contents/GraphQL%20Code%20Generatorのセットアップ.html#graphql-エンドポイントの作成)
    1. [クライアントの作成](contents/GraphQL%20Code%20Generatorのセットアップ.html#クライアントの作成)
    1. [コンポーネントの作成](contents/GraphQL%20Code%20Generatorのセットアップ.html#コンポーネントの作成)
1. [テストコードの追加](contents/テストコードの追加.html)
    1. [VitestとReact Testing Libraryの概要](contents/テストコードの追加.html#vitestとreact-testing-libraryの概要)
    1. [各種ライブラリのインストール](contents/テストコードの追加.html#各種ライブラリのインストール)
    1. [設定ファイルの追加](contents/テストコードの追加.html#設定ファイルの追加)
    1. [テストコードの追加](contents/テストコードの追加.html#テストコードの追加)
    1. [CIの追加](contents/テストコードの追加.html#ciの追加)
1. [本番環境へのデプロイ](contents/本番環境へのデプロイ.html)
    1. [メール配信サービスの設定](contents/本番環境へのデプロイ.html#メール配信サービスの設定)
    1. [データベースの設定](contents/本番環境へのデプロイ.html#データベースの設定)
    1. [Webサーバーの設定](contents/本番環境へのデプロイ.html#webサーバーの設定)
1. [次のステップ](contents/次のステップ.html)

</nav>
✨  Done in 0.93s.
```

雑な箇所もありますが、自分の環境では動作するためよしとします。見出しにスペースやピリオドなどが含まれる際に、想定しない挙動になるためその対応をする必要があって若干苦戦しました。

### 2. タイトルに章番号をいい感じに付与できない

「1 章」などの章のタイトルは、CSS の `content` プロパティを用いて付与します。その際に、章の番号を管理するために `counter-reset` や `counter-increment` を使うのですが、以下にあるように `@page` と組み合わせていい感じに番号を管理するのに苦労しました。

参考：[チュートリアル④用紙と文字のスタイル | Vivliostyle](https://vivliostyle.org/ja/tutorials/configure-page-text/#%E7%94%A8%E7%B4%99%E3%81%AE%E3%82%B9%E3%82%BF%E3%82%A4%E3%83%AB)

今回は `表紙 → はじめに → 対象読者 → 目次 → 本文`、といった形で原稿を書いた関係で、`@page` を用いたカウンターのリセットをうまく調整しにくかったのが原因です。結果的に、自分は以下のように `counter-reset` で初期値を `-4` にして乗り切りました。

```css
@page :first {
  /* FIXME: はじめに、目次のページ数によって初期値を調整 */
  counter-reset: chapter -4;
}

@page :nth(1) {
  counter-increment: chapter;
}
```

ただし、この方法だと上記コードのコメントにもあるように最初のページの数が変わるたびに、この箇所のカウンターのリセット値も手動で調整する必要があり、あんまりスマートではありません。良い対処法をご存知の方がいれば、コメントで教えていただけますと幸いです🙏

## おわりに

技術書は本文を執筆するだけでも大変なので、その前段階の執筆環境の整備で手間取ると厳しいところがあります。Vivliostyle は技術書の執筆に慣れていない人でも簡単に執筆環境を整えられるポテンシャルがあると思っているので、今後のさらなる進化に期待したいですね。

## 宣伝

[株式会社Typebase](https://typebase.dev/) では TypeScript を用いた新規の開発案件を募集中です！開発に関するご相談がありましたら、お気軽にお声がけください。

[お問い合わせはこちら](https://forms.gle/gjjhAcAKU328qE4eA)
