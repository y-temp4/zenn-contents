---
title: "TanStack Startを試してみたけど、もうNext.jsには戻れない"
emoji: "🏝️"
type: "tech"
topics: ["nextjs", "tanstackstart", "tanstack", "react", "typescript"]
published: true
---

英語圏の X で Next.js と TanStack Start を比較する投稿が話題になっていました。

投稿の冒頭には「TanStack Start を試してみたけど、もう Next.js には戻れない」とあり、投稿主は TanStack Start を強く推奨しているように見えます。

この投稿に対し、多くのコメントが寄せられていました。

興味深い議論だと感じたので、この記事では元の投稿と寄せられた意見、そして最後に自分の考えをまとめます。

## おことわり

本記事は、X 上で見かけた議論を紹介しつつ、自分の理解を整理したものです。

特定の投稿者への反論や指摘を目的としたものではなく、あくまで議論の要点をまとめたうえで、自分の主観や着眼点を記録するための記事です。

解釈には個人差があると思うので、その点はあらかじめご了承ください。

## TanStack Startを試してみたけど、もうNext.jsには戻れない？

元の投稿はこちらです。

https://x.com/melvynxdev/status/1992477735927885964

少し長いので、要約してみました↓

https://x.com/y_temp4/status/1993820821543113149

この投稿に対して、多くの意見が寄せられていました。

## Next.jsとTanStack Start、それぞれの良し悪しについて寄せられた意見

元の投稿に寄せられていた意見をいくつかのカテゴリに分類し、まとめてみました[^補足]。

[^補足]: 自分が投稿を確認した時点で引用、返信されていたほぼすべての投稿を対象にしています。ただし、目視で確認しているため抜け漏れがあるかもしれません。

### 1. TanStack Startの支持とNext.jsのブラックボックスな実装への懸念

- Next.js にはブラックボックスになっている魔法や慣習が多すぎますが、TanStack Start にはそれがありません。Next.js の問題はアプリが大規模になるにつれて発生します。TanStack Query は素晴らしく、Vite も快適です。それだけで十分です [^1]
- Next.js は完全な型安全でありません。時間が経つにつれてコードベースがどれほど保守しにくくなることか…。TanStack Start を使えば、AI エージェントが失敗することも減るでしょう [^2]
- Vite のエコシステムが使えるというだけで、ベンダーロックインの三角形（Vercel/Next.js 等の囲い込み）から抜け出す十分な理由になります。いつものように Vite に賭けましょう [^3]
- Next.js は今の魔法のせいで逆に複雑だと感じます。TanStack の冗長さは書くのに時間がかかるかもしれませんが、メンタルモデルが明確なため、デバッグにかかる時間ははるかに短くて済みます [^4]
- 「魔法」の裏側に隠れない代償として記述量が増えるのは仕方ないことです [^5]
- TanStack Form はデフォルトではインストールされませんよ。記述量はほんの数行増えるだけで、Next.js を含む他のフレームワークにはない「完全な型安全性」を実現するための必要最小限のものです。データロードとクライアントの対話のために大量のファイルを分けなくて済むので、トントンだと思います [^6]
- なぜ Next を使うのか。TanStack のエコシステムを使う方がいいです [^27]

多くのユーザーが Next.js の隠蔽されたロジックやベンダーロックインを嫌い、TanStack Start の明示性や Vite エコシステムを支持しているようです。


### 2. 「Next.jsの方がシンプルで速い」という主張への異論

- 「Next.js の方が早くリリースできる」という議論は、「エンタープライズには TypeScript が良いが、早くリリースしたいなら素の JavaScript を使う」と言っているのと同じ気がします。私は TanStack 等の Composer を使って 1 時間でアプリをリリースしましたよ [^7]
- 「Next.js の方がシンプル」だって？[^8]
- Next.js の方がシンプルですって？？？？[^9]
- 具体的に Next.js の何が TanStack より早くリリースできる要因なんですか？純粋な疑問です [^10]

元の投稿者が「Next.js の方がシンプルで開発スピードは速い」とした点に対し、多くの反論が寄せられました。

個人的にも、特に App Router の導入以後あたりから Next.js はシンプルなフレームワークとは言いにくくなってきたと感じています。

### 3. コードの記述量・ボイラープレート・使い勝手への批判

<!-- textlint-disable -->

- Svelte の視点からすると、書くのも保守するのもボイラープレート（定型コード）が多すぎるように見えます [^11]
- `createfileroute` 😂😂 このクソなライブラリを使うのはやめてくれ [^12]
- ファイルルーティングが劣化しているし、データフェッチも直感的ではありません [^13]
- どこで「大量のコード行」が追加されるんですか？[^14]

<!-- textlint-enable -->

TanStack Start の記述量の多さや、特定の仕様（ファイルベースルーティング）に対する否定的な意見もありました。


### 4. チーム規模と採用に関する議論

- 公平な意見ですね。ソロ開発者や小規模チームにとっては、明示的なパターンよりも開発速度の方がはるかに重要です。「魔法 vs 明示的」の議論は、通常、チームの規模や新しい人をどのくらいの頻度でオンボーディングするかに行き着きます [^15]
- まったく同意ですが、TanStack Start は小規模チームでも機能すると思いますよ [^16]
- 大企業で好まれる「明示的」というのは、大抵「誰かが書いた大量のボイラープレート」を意味しますが、まあ少なくとも一貫性はありますね [^17]
- そのロジックは少し奇妙ですね。Next.js の採用数とエコシステムの方がはるかに巨大です。スタートアップよりも、むしろエンタープライズの選択肢になっています [^18]

「小規模チームなら Next.js、大規模なら TanStack Start」という元の投稿で言及されていた点に対しては、賛成意見がありつつ、反論も見られました。

個人的にも、TanStack Start は小規模チームでも十分に使えると考えています。

### 5. 他のフレームワークやアプローチの提案

<!-- textlint-disable -->

- React Router は試しましたか？　TanStack ほど冗長ではありませんが、使い勝手は似ていますよ [^19]
- ほとんどのアプリでは、SPA にルーターと TanStack Query があれば十分で、Next.js や Start は必要ありません [^20]
- プロジェクトによりますが、私は Astro、Next.js、TanStack、React Router 7 のいずれかを使います。Astro 以外なら React を使いますが、フレームワーク戦争の意味がわかりません。自分が快適なものか、お金になるものを使って作ればいいだけです [^21]
- 私は TanStack スタック自体は大好きです。エンタープライズ・アプリケーションには TanStack Router を使っています。認証が必要なアプリなら SSR は不要ですから。SPA としてシンプルかつ高性能に保つ方がいいです [^22]
- Express のことをみんな忘れ続けている… [^23]
- なぜ React と Node.js じゃだめなの？[^24]
- SolidJS でも TanStack Start は動くし、もう React には疲れました [^25]
- 以前は TanStack の主なユースケースは「どこでも動く」ことでしたが、Next.js は Vercel でしか動きませんでした。個人的には Next.js v10 は最高でしたが、今は複雑すぎます。Remix は何がしたいのか分からないフレームワークです [^28]

<!-- textlint-enable -->


TanStack Start や Next.js 以外の選択肢や、そもそもフレームワーク不要論などの意見です。

個人的に Remix については、2024 年に記事を書いた時点ではかなり有力な選択肢になると思っていました。一方で現在は React に依存しない Remix v3 が出たこともあり、単純な比較はしにくくなりつつあるなと感じています。

https://zenn.dev/typebase_dev/articles/why-choose-remix

https://azukiazusa.dev/blog/try-remix-v3/


### 6. その他の意見（AIのサポート）

- Next.js は AI によく知られており、MCP さえ持っています。それだけでも使い続ける非常に確固たる理由になります [^26]

確かに AI のサポートという面では、現時点では TanStack Start は情報量が足りないので Next.js が圧勝だと思います。

## Next.jsとTanStack Startに関する個人的な見解

元々、本記事を書く前に自分は「直近の個人開発のプロジェクトで Next.js ではなく TanStack Start を選択した」という内容の記事を書く予定でした。

そのため、自分も元の投稿に寄せられた意見の大半と同じく、今では TanStack Start 寄りの立場にいます。

https://x.com/y_temp4/status/1993833516585648535?conversation=none

判断材料として、自分が重視したポイントは以下の通りです。

### 1. `"use client"` を書きたくない

シンプルに自分の理解不足/不慣れなことが原因ではあるのですが、Next.js を使っていて use client ディレクティブを書きたくないな〜と思うことが多かったです。

そもそも、SSR が不要でクライアントメインで開発することが多いという個人的な要因もありますが⋯。

TanStack Start であれば（いまのところ）use client なしで開発できます。

---

若干話が逸れますが、以下の記事も参考になりました。

https://zenn.dev/tsuboi/articles/3f00d532bcb4dc

### 2. Turbopack より Vite を使いたい

App Router になってから、Turbopack を有効にしていないとビルド時間が長くなったな〜と思うことが増えました。また、少し前までは Turbopack を有効にしている場合に Sentry が完全に動作しないなど、少し実用面で不安が残る挙動をしていました。

現在の Next.js v16 以降では Turbopack がデフォルトで有効になっており、先述の Sentry の不具合も改善されているはずですが、それでも少しバンドラーとして挙動に不安が残るかも、と思っています。

自分は Vite を使っていて不満に感じたことがほぼないので、Vite のほうがバンドラーとしての安心感・信頼度が高い印象です。

### 3. TanStack 系のライブラリの DX（開発者体験）が高い

シンプルに TanStack 系のライブラリの DX が高く、その上に構成されている TanStack Start への期待値が高いです。

特に TanStack Router は関連する React のルーティングライブラリの中では群を抜いて高い DX を実現している印象。

まだ完全に安定しているとはいえず、マイナーバージョンアップでやや不安定な挙動になることもありますが、それでも満足できる挙動です。

※補足：TanStack Start は主に TanStack Router の上に構成されたフレームワークで、TanStack Router だけでは実現できない API Routes などのさまざまな機能が搭載されているフレームワークです。

また、以下の記事の内容も大変参考になりました。

https://zenn.dev/chiji/articles/a8eaa9ed1d042c#2.-tanstack-start

## おわりに

寄せられた意見の 1 つにあるように、そもそもフレームワークは開発対象や用途、チームの特性などによって最適解が変わるものです。

そのため、Next.js と TanStack Start のどちらが絶対的に優れているかという議論はあまり意味がないと考えています。

一方で、Next.js と TanStack Start はフレームワークの性質上（React ベースのフルスタックなフレームワークであること）、どうしても比較の対象になりやすいのも事実です。

そして国内ではまだ TanStack Start の採用事例が少ないこともあり、今回のような議論は非常に興味深く感じました。

みなさんは今回の議論について、どのように感じましたか？ぜひ記事へのコメントや [X](https://x.com/y_temp4/status/1995318461648793857) 等で教えてください。

[^1]: [https://x.com/VLTHellolin/status/1993302410178314487](https://x.com/VLTHellolin/status/1993302410178314487)
[^2]: [https://x.com/\_7obaid\_/status/1992707426232287620](https://x.com/_7obaid_/status/1992707426232287620)
[^3]: [https://x.com/hugokorte_/status/1992745680834859205](https://x.com/hugokorte_/status/1992745680834859205)
[^4]: [https://x.com/KaousNadirHatem/status/1992878092298461266](https://x.com/KaousNadirHatem/status/1992878092298461266)
[^5]: [https://x.com/ddsuhaimi_/status/1992844742653337899](https://x.com/ddsuhaimi_/status/1992844742653337899)
[^6]: [https://x.com/tannerlinsley/status/1993777276547420423](https://x.com/tannerlinsley/status/1993777276547420423)
[^7]: [https://x.com/fedeeeeeev/status/1992742920546553873](https://x.com/fedeeeeeev/status/1992742920546553873)
[^8]: [https://x.com/radimhfer/status/1992840599217856734](https://x.com/radimhfer/status/1992840599217856734)
[^9]: [https://x.com/laduniestu/status/1992875607487828390](https://x.com/laduniestu/status/1992875607487828390)
[^10]: [https://x.com/cirejr_/status/1992957640784441678](https://x.com/cirejr_/status/1992957640784441678)
[^11]: [https://x.com/vamshi_nenu/status/1992944245418946719](https://x.com/vamshi_nenu/status/1992944245418946719)
[^12]: [https://x.com/nextgenai_fr/status/1992605821499551960](https://x.com/nextgenai_fr/status/1992605821499551960)
[^13]: [https://x.com/popuguy_/status/1992899077655110062](https://x.com/popuguy_/status/1992899077655110062)
[^14]: [https://x.com/schanuelmiller/status/1992542369510465820](https://x.com/schanuelmiller/status/1992542369510465820)
[^15]: [https://x.com/its_tianyi/status/1992941520815878231](https://x.com/its_tianyi/status/1992941520815878231)
[^16]: [https://x.com/alexandretrotel/status/1992862436492542277](https://x.com/alexandretrotel/status/1992862436492542277)
[^17]: [https://x.com/leodoan_/status/1993282044491944295](https://x.com/leodoan_/status/1993282044491944295)
[^18]: [https://x.com/garyfung/status/1992869292397011370](https://x.com/garyfung/status/1992869292397011370)
[^19]: [https://x.com/larsgraubner/status/1992709986770882655](https://x.com/larsgraubner/status/1992709986770882655)
[^20]: [https://x.com/imaks_dev/status/1992847367448736200](https://x.com/imaks_dev/status/1992847367448736200)
[^21]: [https://x.com/codingthirty/status/1992902494687789164](https://x.com/codingthirty/status/1992902494687789164)
[^22]: [https://x.com/GeoffreyOlson7/status/1992701595935731858](https://x.com/GeoffreyOlson7/status/1992701595935731858)
[^23]: [https://x.com/maylivesforever/status/1993107300400373826](https://x.com/maylivesforever/status/1993107300400373826)
[^24]: [https://x.com/ilgizilgizilgiz/status/1992819539994903006](https://x.com/ilgizilgizilgiz/status/1992819539994903006)
[^25]: [https://x.com/electroheadfx/status/1992525722318168152](https://x.com/electroheadfx/status/1992525722318168152)
[^26]: [https://x.com/yes_iamenes/status/1992946360711352678](https://x.com/yes_iamenes/status/1992946360711352678)
[^27]: [https://x.com/ichikiwhere/status/1992927997813457378](https://x.com/ichikiwhere/status/1992927997813457378)
[^28]: [https://x.com/echoes2099/status/1992984416235499604](https://x.com/echoes2099/status/1992984416235499604)

