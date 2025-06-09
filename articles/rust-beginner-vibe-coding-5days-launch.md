---
title: "Rust未経験でも5日でサービス公開！Vibe Codingの威力がすごかった話"
emoji: "💥"
type: "tech"
topics: ["rust", "nextjs", "vibecoding", "oss", "個人開発"]
published: true
---

## 本記事の概要

本日、[**みんなの話題**](https://minwada.com) というサービスを公開しました！  
このサービスは Vibe Coding により **Rust 未経験** の状態から約 5 日[^5days]で開発したものです。  
コードは OSS になっているため、ぜひサービスとあわせてご覧ください。

[^5days]: Initial commit からの経過日数。途中、ほぼ開発していない日もあるため、実際の開発時間はもう少し短いです。

https://minwada.com

https://github.com/y-temp4/minwada

## サービス開発のきっかけ

以前から Rust に興味があり、多少学習を進めていました。

具体的にはいくつか書籍を読んだり、少しだけ LeetCode の問題に取り組むなどしていた感じです。ただ、やはり実際にがっつりとコードを書く機会がないとあまり身につかないなと感じていました。

そこで今流行りの Vibe Coding を使って、実際に何か開発してみることにしました。開発する対象は普段から馴染みのある Web サービスにしようと思い、API 側を Rust で開発することにしました。

## 技術スタック

📖 詳細は [README](https://github.com/y-temp4/minwada) もご覧ください。

### バックエンド

- **Language**: [Rust](https://www.rust-lang.org/) 🦀
- **Framework**: [axum](https://github.com/tokio-rs/axum) ⚡
- **Database**: [PostgreSQL](https://www.postgresql.org/) 🐘
- **ORM**: [sqlx](https://github.com/launchbadge/sqlx) 📊
- **認証**: JWT + OAuth2 (Google、予定) 🔐
- **API 仕様**: [OpenAPI](https://www.openapis.org/) ([utoipa](https://github.com/juhaku/utoipa)) 📚

axum、sqlx、utopia などの Rust のライブラリは、Vibe Coding を通じて今回初めて触れました。

### フロントエンド

- **Framework**: [Next.js](https://nextjs.org/) 15+ (App Router) ⚛️
- **UI**: [shadcn/ui](https://ui.shadcn.com/) + [Tailwind CSS](https://tailwindcss.com/) 🎨
- **API クライアント**: [TanStack Query](https://tanstack.com/query) + [Orval](https://orval.dev/) 🔄
- **フォーム**: [React Hook Form](https://react-hook-form.com/) + [Zod](https://zod.dev/) 📋

フロント側は普段から慣れている技術を使いました。

## Vibe Coding でも意外と勉強になった

コーディングを始めた際は、思いの外 AI がコードを生成してくれたため、「これは全然勉強にならないのでは？」と思いました。

一方で、開発を進めていくと AI がなかなか解決してくれないコードも出てきたため、自分でコードを読んだり、解決方法を模索したりする必要性が出てきました。

結果的に、Vibe Coding を通じて Rust のライブラリの使い方を（多少ではありますが）学ぶことができました✨

### 開発時につまづいたポイント

- Rust の挙動
  - 再帰的な処理をする関数を書く際に、なかなか AI では正しいコードを生成してくれないことがありました
    - [該当の関数](https://github.com/y-temp4/minwada/blob/e30ab1f75f2513db4b114e8a8913da56ab1085eb/backend/src/handlers/comments/utils.rs)
- sqlx のテスト
  - sqlx を使っているにもかかわらず、テストコードが `tokio::test` で書かれていました
  - [sqlx ではテスト用のDBを作成してくれる](https://zenn.dev/collabostyle/articles/b8b13870d5ee5b) はずなのに挙動がおかしいな、と思っていたら、よくコードを読んでみると `#[tokio::test]` になっていました

この辺は実際に自分でコードを読んだり、挙動をしっかりと確認する必要がありました。

また、フロントエンド側のコードを書く際も生成された API クライアントを使ってくれないなどの問題があり、Vibe Coding はまだまだ万能ではないと感じました。

## まとめ

今回始めて、サービス開発においてほとんどのコードを AI に生成してもらいました。

学習観点ではあまり効果がないかなと最初は思っていましたが、結果的には自分でコードを読む機会もあり、とても勉強になりました👍

今後は本サービスを運用しながら、継続的に Rust や関連するライブラリの使い方を学んでいきたいと思います。

また、AI による開発で学んだ知見などは今後も定期的に記事にしていく予定です。

もしこの記事が参考になりましたら、ぜひサービスの利用や GitHub でのスターをお願いします！⭐️


- サービス URL: [https://minwada.com](https://minwada.com)
- GitHub (OSS): [https://github.com/y-temp4/minwada](https://github.com/y-temp4/minwada)

最後まで記事を読んでいただき、ありがとうございました！
