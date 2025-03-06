---
title: Power Platform での仮想ネットワーク データ ゲートウェイについて
date: 2024-01-20 00:00:00
tags:
  - PowerPlatform
  - VNetGateway
  - Network
categories:
  - [Power Platform]
---

# Power Platform での仮想ネットワーク データ ゲートウェイについて
<!-- ここに 導入部分 -->
こんにちは、Power Platform サポートチームの藤田です。
今回は、最近お問い合わせの増えている仮想ネットワーク ゲートウェイについて、サポートされる機能をご紹介します。

※2024-04-12 追記：仮想ネットワーク ゲートウェイに代わりご利用いただける、Power Platform の仮想ネットワーク サポートがパブリック プレビューとなりました。
[Power Platform 向け Virtual Network のサポートの概要 (プレビュー)](https://learn.microsoft.com/ja-jp/power-platform/admin/vnet-support-overview)
[仮想ネットワーク データ ゲートウェイと Power Platform の Azure Virtual Network サポートの違いは何ですか?](https://learn.microsoft.com/en-in/power-platform/admin/vnet-support-overview#whats-the-difference-between-a-virtual-network-data-gateway-and-azure-virtual-network-support-for-power-platform)
[Announcing public preview of virtual network support for Power Platform Dataverse plug-ins and Connectors](https://powerapps.microsoft.com/en-us/blog/announcing-public-preview-of-virtual-network-support-for-power-platform-dataverse-plug-ins-and-connectors/)

<!-- more -->

<!-- ここに Read more 以降の文章 -->
> [!IMPORTANT] 
> 本記事は弊社公式ドキュメントの公開情報を元に構成しておりますが、 本記事編集時点と実際の機能に相違がある場合がございます。 
> 最新情報につきましては、参考情報として記載しておりますドキュメントをご確認ください

---
## 仮想ネットワーク データ ゲートウェイとは？

仮想ネットワーク データ ゲートウェイを使用すると、お使いの VNet 内の Azure サービスと Power Platform のサービスを安全に接続することが出来ます。
詳細は以下の公開情報をご参照ください。
[仮想ネットワーク (VNet) データ ゲートウェイ (プレビュー) とは](https://learn.microsoft.com/ja-jp/data-integration/vnet/overview)
[仮想ネットワーク データ ゲートウェイのアーキテクチャ](https://learn.microsoft.com/ja-jp/data-integration/vnet/data-gateway-architecture)

※ 2024 年 1 月現在、上記の仮想ネットワーク データ ゲートウェイはプレビュー中の機能となっています。

## 仮想ネットワーク データ ゲートウェイが使える機能

現在、仮想ネットワーク データ ゲートウェイが使える機能は以下の通りです。
- Fabric Dataflow Gen2
- Power BI データセット
- Power Platform データフロー
- Power BI のページ分割されたレポート

[仮想ネットワーク (VNet) データ ゲートウェイ (プレビュー) とは - 制限事項](https://learn.microsoft.com/ja-jp/data-integration/vnet/overview#limitations)

## Power Automate のカスタム コネクタと仮想ネットワーク データ ゲートウェイ

カスタム コネクタから仮想ネットワーク データ ゲートウェイを使用し、Azure OpenAI Service 等の Azure サービスに接続したいというお問い合わせが増えております。
この機能については、2022 年 4 月頃にプレビューとして登場し、2023 年 8 月末頃にプレビューを取り下げた機能です。現在はサポートされておらず、公開情報等もございません。
一方で、本記事執筆時点では機能自体は削除されていない状態であり、依然使用できる状態となっております。

プレビュー期間中に既にお試しいただいていた方や、使用してはいなかったが Microsoft 社外サイト等で機能を知った方等、現在も利用したい・試してみたいという状況はあるかと思います。
その場合は、**本機能がプレビュー未満の状態であり、予告なく変更または削除される可能性がある点をご了承いただいた上で**お使いいただくようお願いいたします。

## プレビュー機能とは？

プレビュー機能には、いくつかの免責事項があります。※
- 個別の [追加使用条件](https://dynamics.microsoft.com/ja-jp/legaldocs/supp-dynamics365-preview/) に依存しています。
- 運用環境での使用を目的としていません。
- Microsoft サポートによる運用環境での使用はサポートされていません。 ただし、Microsoft サポートは、プレビュー機能に関するフィードバックをお伺いし、場合によってはベスト エフォート サポートを提供する場合があります。
- 機能が制限される可能性があります。
- 地域によっては利用できない場合があります。

上記の通り、プレビュー機能のご利用で問題が発生し、お問い合わせいただいた場合、プレビュー機能を使用中止いただくようお願いすることとなってしまう場合がございます。
ビジネス クリティカルなシナリオでは、プレビュー機能のご利用を避けていただくようお願いいたします。

※ [プレビュー機能とは](https://learn.microsoft.com/ja-jp/dynamics365/sales/sales-previews-in-trial) より引用
