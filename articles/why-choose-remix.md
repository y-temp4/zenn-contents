---
publication_name: "typebase_dev"
title: "2024年上期におけるRemix潮流の兆し―なぜRemixが選ばれるのか？"
emoji: "🎶"
type: "tech"
topics: ["remix", "nextjs", "react", "frontend"]
published: true
---

## はじめに

ここ最近、[Remix](https://remix.run/) に関する話題や採用事例が増えてきているように感じます。

- [一休レストランで Next.js App Router から Remix に乗り換えた話 - 一休.com Developers Blog](https://user-first.ikyu.co.jp/entry/2023/12/15/093427)
- [RemixでWeb標準を学んだ1年間 / First year with Remix - Speaker Deck](https://speakerdeck.com/nkzn/first-year-with-remix)
- [SPAの歴史とRemix SPAモードという到達点 / the SPA's chronicle reaches to remix - Speaker Deck](https://speakerdeck.com/nkzn/the-spas-chronicle-reaches-to-remix)
- [検索システムのフロントを SSR・Remix で作り直した - Unyablog.](https://nonylene.hatenablog.jp/entry/2024/02/29/032149)
- [Remixを使い始めた話 | Money Forward Kessai TECH BLOG](https://tech.mfkessai.co.jp/2024/03/remix/)

そこで本記事ではここ最近 Remix が採用されがちな理由を**筆者の独断と偏見**で考察し、一部 Next.js とも比較しながらその内容をまとめてみます。

## おことわり

私は Next.js を用いた開発の経験はありますが、Remix に関してはほとんど開発の経験がありません。そのため、Remix に対して浅い理解をしている可能性があることはご了承ください。

また、本記事は特定のフレームワークやライブラリを否定する意図はありません。

最後に、本記事は先にあげたような資料や、その他 Remix に関する情報を元に私が感じたことをまとめたものです。そのため、すでに先に挙げたような資料を読んでおり、Remix について十分に理解している方にとっては新しく得られる情報はないかもしれません。

## Remixが選ばれる理由

ざっくり、Remix の以下のような特徴が評価されていると感じています。

- Web 標準を尊重している
- フレームワークの安定感
- キャッシュの挙動に悩まされにくい

### Web 標準を尊重している

Remix は Web の標準的な API を尊重した設計になっています。例えばフォームの送信については [FormData](https://developer.mozilla.org/ja/docs/Web/API/FormData) が使えたり、[`Link`](https://remix.run/docs/en/main/components/link) コンポーネントや [`useNavigate`](https://remix.run/docs/en/main/hooks/use-navigate) フックが [History API](https://developer.mozilla.org/ja/docs/Web/API/History_API) の薄いラッパーになっていたり、など。

そのため学んだ知識が陳腐化しにくく、今後も応用が効きやすいのではないか、という期待が各所から感じられます。

> ### v1のPhilosophyに惚れてしまった
> - https://remix.run/docs/en/1.19.3/pages/philosophy
> - Web標準には結構いいパーツが揃ってきているので活用することにしたし、フレームワークによる隠蔽・抽象化も最小限にする
>   - ex. 通信周りはlib.dom.d.tsの型定義をそのまま使うことが多い
> - せっかく学習コストを払うならWeb標準を学んだほうが潰しが効く
>
> https://speakerdeck.com/nkzn/first-year-with-remix?slide=13 より引用

Next.js は App Router の登場後、その仕様の大幅な変更[^app-router]からキャッチアップが難しいと感じる人も多いような印象があります（自分もその一人です）。

[^app-router]: 追加という方が正しいかもしれない🤔

### フレームワークの安定感

Remix はパッケージのバージョンアップ時の互換性を尊重している印象があります。例えば [Upgrading to v2 | Remix](https://remix.run/docs/en/main/start/v2) にあるように、v1 からの移行は `v2_` フラグを使いながら進められます。

参考事例：[Remix v2へアップデートした時のメモ | Web Scratch](https://efcl.info/2023/10/14/remix-v2/)

これと比較して Next.js は、特に v14 系あたりから小さなバージョンアップ時にも良きせぬ不具合に遭遇しがちな印象があります。

> ### 継続的なアップデートに懸念を覚えた
> Next.js のパッチバージョンを上げたときに production build でだけ 500 エラーが発生するという問題に幾度か苦しめられました。
>
> https://user-first.ikyu.co.jp/entry/2023/12/15/093427 より引用

### キャッシュの挙動に悩まされにくい

Next.js と比較した際に、Remix はキャッシュの挙動に悩まされにくいイメージがあります。

Next.js の App Router はデフォルトでゴリゴリにキャッシュが有効化されており、その挙動を理解していないと思わぬ挙動に悩まされることがあります。

- [Next.js の fetchCache を考える](https://zenn.dev/takepepe/articles/nextjs-app-router-fetch-cache)
- [Next.jsの4つのキャッシュ](https://zenn.dev/frontendflat/articles/nextjs-cache)

この挙動に悩まされるくらいなら･･･という理由で、Remix を採用している人も多いのではないでしょうか。

## Remixの課題

一方で、Remix には以下のような課題があると感じています。

- ファイルルーティングがやや扱いにくい
- 型安全なルートを公式で扱う予定がなさそう

### ファイルルーティングがやや扱いにくい

Remix は v2 で Flat Routes を採用しています。

- [Route File Naming | Remix](https://remix.run/docs/en/main/file-conventions/routes#route-file-naming)
- [Remix v2 のルーティングにおける flat-routes と Dot Delimiters](https://zenn.dev/t_tonyo_maru/articles/3408dcdcfd0c8f)

個人的に Remix のファイルルーティングは Next.js と比べるとややとっつきにくいと感じています。その現れが、[🗺️ Flat Routes · remix-run/remix · Discussion #4482](https://github.com/remix-run/remix/discussions/4482) での反応からも見て取れる気がします。

一応、現在（2024 年 3 月）の公式ドキュメントでは [remix-flat-routes](https://github.com/kiliman/remix-flat-routes) というライブラリが紹介されています。

> While we like this file convention, we recognize that at a certain scale many organizations won't like it. You can always define your routes programmatically in [`vite.config.ts`](https://remix.run/docs/en/main/file-conventions/vite-config#routes).
>
> There's also the [`remix-flat-routes`](https://github.com/kiliman/remix-flat-routes) third-party package with configurable options beyond the defaults in Remix.
> https://remix.run/docs/en/main/file-conventions/routes#more-flexibility より引用

`remix-flat-routes` を使うと、フラットではないファイルベースのルーティングが可能になります。

しかし公式に紹介されているライブラリとはいえ、サードパーティ製のライブラリにファイルルーティングの制御をまかせるのは少し不安が残る気もします[^remix-flat-routes]。

[^remix-flat-routes]: ちなみに、Remix の開発者である Kent C. Dodds 氏は自身のリポジトリで `remix-flat-routes` をがっつり使っています（2024 年 3 月現在）。 [kentcdodds.com/package.json at 217ae3411dd626410fa7150038b9dae073003e15 · kentcdodds/kentcdodds.com](https://github.com/kentcdodds/kentcdodds.com/blob/217ae3411dd626410fa7150038b9dae073003e15/package.json#L210)


### 型安全なルートを公式で扱う予定がなさそう

Next.js では [`experimental.typedRoutes`](https://nextjs.org/docs/app/building-your-application/configuring/typescript#statically-typed-links) を有効にすると `next/link` の `Link` コンポーネントの利用時に遷移先を型安全に指定できます[^tanstack]。

```tsx
import Link from 'next/link'

// /about が存在しない場合はエラーになる
<Link href="/about" />
```

[^tanstack]: 最近では [TanStack Router](https://tanstack.com/router/latest) が話題になりましたね。参考: [TanStack Router（& Query）はSPA開発で求めていたものだった✨【Reactのルーティングとデータ取得】](https://zenn.dev/aishift/articles/ad1744836509dd)

一方で、Remix では公式に型安全なルートを扱う方法が提供されておらず、かつ、今後公式でこれが提供されそうな雰囲気も感じられません。

というのも、Remix でも [remix-routes](https://github.com/yesmeck/remix-routes) を使うことによって型安全なルートを扱うことができるのですが、これは公式のドキュメントには紹介されていません。

また、Remix 公式における型安全なルーティングについても、あまり積極的に議論されていないように見えます。

参考：[[Feature]: Typed routes · Issue #9791 · remix-run/react-router](https://github.com/remix-run/react-router/issues/9791)

もちろん、将来的にはどうなるかはわかりませんが、少なくとも直近では上記のようなサードパーティ製のライブラリを使うことになりそうです[^remix-routes]。

[^remix-routes]: ちなみに `remix-routes` と `remix-flat-routes` を組み合わせて使うことも不可能ではなさそうですが、`remix-routes` が `remix-flat-routes` の変更にどれだけ追従する意思があるかは現時点では不明です。`remix-routes` と `remix-flat-routes` を組み合わせた実装例：[coji/remix-spa-example: Remix SPA mode + Firebase で「しずかなインターネット」の真似サイトを作った。](https://github.com/coji/remix-spa-example)

## おわりに

本記事では一部 Next.js と比較しつつ、Remix のメリットとデメリットについて整理してみました。

---

[React プロジェクトを始める – React](https://ja.react.dev/learn/start-a-new-react-project) にあるように、React を使う際はフレームワークの利用が推奨されている雰囲気を感じます。

> フレームワークなしで React を使うことも可能ですが、ほとんどのアプリやサイトにおいては、コード分割、ルーティング、データ取得、HTML 生成といった問題に対処するための開発が必要であることが分かっています。これらは React に限らずあらゆる UI ライブラリに共通の問題です。
>
> フレームワークを使ってスタートすることで React での開発を素早く立ち上げ、後で実質的に独自フレームワークのようなものを作ってしまわずに済むようになるでしょう。

作るのアプリケーションの要件によって最適なフレームワークは適宜変わります。そのため、ただ単にいま流行っているから、という理由だけで技術選定をせずに、要件にあわせて適切なフレームワークを選べるようになりたいなと改めて思いました。
