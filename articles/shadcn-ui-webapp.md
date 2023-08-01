---
publication_name: "typebase_dev"
title: "shadcn/uiを使ってWebアプリ開発したらとても快適だった"
emoji: "🤸"
type: "tech"
topics: ["shadcnui", "tailwindcss", "nextjs", "radix", "javascript"]
published: true
---

少し前に T3 Stack で Web アプリケーションの開発を行ってみました。

https://create.t3.gg/

T3 Stack は主に以下の技術スタックで構成されています。このうち、UI の部分は Tailwind CSS となっていますが、これに追加して今回は shadcn/ui を使ってみました。

- Next.js
- TypeScript
- tRPC
- Prisma
- Tailwind CSS
- NextAuth.js（Auth.js）

https://ui.shadcn.com/

shadcn/ui を使うのは初めてでしたが、使ってみるととても快適だったので、この記事ではその概要と実際に使ってみた感想を紹介します。

## shadcn/uiの概要

shadcn/ui は Radix UI をベースとして作られたコンポーネント集です。

https://www.radix-ui.com/

Radix UI はいわゆるヘッドレス UI コンポーネントで、デザインはあまりされていません。そのため、必要に応じてデザインは自分で作る必要があります。その点、shadcn/ui ではデザイン部分が Tailwind CSS ですでに実装済みであり、自分でデザインを作る必要がなく非常に便利でした。

また shadcn/ui はこれまでの一般的な UI フレームワークとは異なり、自分で必要なコンポーネントだけをプロジェクトに追加して使えるようになっています。追加されるコンポーネントは直接編集できるためカスタマイズ性が高く、細かい箇所に手を入れやすかったです。

https://ui.shadcn.com/docs/installation/next

shadcn/ui については以下の記事が詳しいです。

https://zenn.dev/mottox2/articles/react-shadcn-ui

## shadcn/uiを用いて実装した画面の例

以下は、今回作成した Web アプリケーションの設定画面です。この画面では主にフォームの実装を shadcn/ui で行っています。

![](https://storage.googleapis.com/zenn-user-upload/8017321725fd-20230729.png)

shadcn/ui のフォームは React Hook Form との組み合わせが容易で、公式ドキュメントを参考にサッと実装できました。

https://ui.shadcn.com/docs/components/form

また、shadcn/ui はライトモード/ダークモードの仕組みも備えています。現在は最初から shadcn/ui 側で用意されたカラーパレットを使っていますが、ある程度それっぽいテーマの切り替えが実現できており、実装の工数が減って助かりました。

https://ui.shadcn.com/docs/dark-mode

![](https://storage.googleapis.com/zenn-user-upload/1405872257b1-20230729.png)

以下はダークモードを有効にしたときの画面です。

![](https://storage.googleapis.com/zenn-user-upload/72786cd21dec-20230729.png)

## おわりに

shadcn/ui を使えば、ある程度見た目が整った Web アプリケーションを短時間で、かつコンポーネントのカスタマイズ性を損なうことなく実装できることがわかりました。

個人的には Tailwind CSS を使ってデザインできるのが嬉しいので、今後も積極的に使っていきたいですね。

---

今回記事中で紹介したサイトはこちら：[Bluebird](https://bluebird.blue/)
