---
publication_name: "typebase_dev"
title: "Cloudflare Registrarで.devと.appドメインが取得できるようになった"
emoji: "🙆"
type: "tech"
topics: ["cloudflare", "domain"]
published: true
---

先日、[Cloudflare Registrar](https://www.cloudflare.com/ja-jp/products/registrar/) で `.dev` と `.app` ドメインが取得できるようになりました。

https://twitter.com/CloudflareDev/status/1686812617153593355

これまではこの 2 つのドメインを取得しようとすると、おそらく [Google Domains](https://domains.google/intl/ja_jp/) を利用する方が多かったかなと思います。しかし Google Domains は 2023 年後半にサービスが終了し、Squarespace 社に事業譲渡されることが発表されています。

これまでは Google Domains からの移管先として Cloudflare Registrar を選びたくても、`.dev` や `.app` を利用しているからできない･･･！と困っている方も多かったかなと思います（自分もその一人でした）が、これでようやく Cloudflare Registrar へ移管ができるようになりました🎉

## `.dev` と `.app` の料金

- `.dev`
  - Cloudflare Registrar: 約 1,450 円（$10.18）
  - 参考：Google Domains: 1,400 円
- `.app`
  - Cloudflare Registrar: 約 1,736 円（$12.18）
  - 参考：Google Domains: 1,600 円

Cloudflare Registrar は移管手数料（$0.18）も含んでいます。レートは 2023 年 8 月 4 日現在のもので、1 ドル = 142.52 円として計算しています。

## Cloudflare Registrarへの移管の流れ

https://blog.cloudflare.com/ja-jp/a-step-by-step-guide-to-transferring-domains-to-cloudflare-ja-jp/

お名前.com:
https://zenn.dev/muchoco/articles/9039762136e15c

Google Domains:
https://zenn.dev/dojineko/articles/google-domains-to-cloudflare-registrar

自分は Google Domains からの移管だったので、上記記事を参考にさせていただきました。

## おわりに

個人的には Google Domains が終了するまでに、Cloudflare Registrar で `.dev` と `.app` ドメインを取得できるようになってよかったです。

今後はドメインの取得先の選択肢として、Cloudflare Registrar を選ぶ方がより増えていきそうですね。
