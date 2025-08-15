---
title: キャンバスアプリ / Power Automate 利用時の通信要件
date: 2024-11-10 09:00:00
tags:
  - Power Platform
  - Power Apps
  - キャンバスアプリ
  - Power Automate
  - Network
  - Governance
categories:
  - [Power Automate, Cloud flow]
  - [Power Automate, Desktop flow]
  - [Power Apps, Canvas app]
toc:
  enabled: true
  min_depth: 1
  max_depth: 3
  list_number: false

---

# キャンバスアプリ / Power Automate  利用時の通信要件

こんにちは、Power Platform サポートチームの網野、大友です。  
今回は キャンバスアプリ / Power Automate  利用時の通信要件についてご案内します。  
Microsoft 365 や Azure などの製品とは別に、Power Platform 独自で通信要件を定義しており、別途対応が必要となりますのでご留意ください。

> [!IMPORTANT]
> 通信に利用するドメインや IP アドレスは定期的に変更されるため  
> 必ず [最新の公開情報](https://learn.microsoft.com/en-us/power-platform/admin/online-requirements) を参照し設定を行ってください。  
> 公開情報は翻訳までに一定の時間がかかりますので、本ブログでは英語の公開情報へリンクを貼っています。



<!-- more -->

## 通信要件とは

キャンバスアプリ / Power Automate  はクラウド上に展開されたサービスであり、クラウドサービスへアクセスする、またはクラウドサービスからアクセスされる際に、特定のドメインや IP アドレスへ片方向または双方向の通信が発生します。  
Power Platform では通信に必要な要件を定義し、通信要件として Power Platform の公開情報に記載しています。  

プロキシサーバー等を用いてネットワークに制限をかけている場合、製品利用に必要な通信がブロックされると問題が生じる恐れがあるため、必要な通信許可の設定を行ってください。

通信の種類には 2 種類があります。  
<b>①クライアントとの通信</b>  
クライアント端末とマイクロソフトクラウドサーバーとの通信です。  
クライアント端末から Power Automate ポータルにアクセスするときや、キャンバスアプリを実行するときなどに利用されます。

<b>②接続先との通信</b>  
マイクロソフトクラウドサーバーとコネクタ接続先との通信です。  
フローやアプリにてコネクタを通じてデータに接続するときに利用されます。  
  
![](./ip-range-and-domain/network.png)  

① クライアントとの通信に必要な要件は  [キャンバスアプリ](#anchor-canvasapp)、[Power Automate](#anchor-powerautomate) および [オンプレミスデータゲートウェイ](#anchor-onpremisedatagateway) を参照してください。  
② 接続先との通信に必要な要件は [コネクタ](#anchor-connector) を参照してください。  

## クライアントとの通信
<a id='anchor-canvasapp'></a>
### キャンバスアプリ

1. [必要なサービス](https://learn.microsoft.com/en-us/power-apps/limits-and-config#required-services)に記載されているすべてのドメインを許可してください。<br>
  ![](./ip-range-and-domain/powerapps-domain.png)  

<a id='anchor-powerautomate'></a>
### Power Automate
1. [IP アドレスの構成](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration) に記載されているドメインを許可してください。  
   モバイルアプリやデスクトップフローなど利用するサービスに応じて設定してください。  
   ![](./ip-range-and-domain/powerautomate-domain.png)

<a id='anchor-onpremisedatagateway'></a>
### オンプレミス データ ゲートウェイ

1. [ゲートウェイが機能するために必要なポート](https://learn.microsoft.com/en-us/data-integration/gateway/service-gateway-communication#required-ports-for-the-gateway-to-function)に記載されているすべてのドメインを許可してください。  
![](./ip-range-and-domain/onpremisedatagateway.png)  

   > [!NOTE]
   > ドメインに加え、ポートも指定されています。  
   > オンプレミス データ ゲートウェイではお客様からマイクロソフト向きのリクエストにより通信を確立します。  
   > そのため、お客様のコンピューターをインターネット上に公開していただく必要はございません。  

   端末にインストール済のオンプレミス データ ゲートウェイアプリにて組織アカウントでのサインインに成功している場合には、診断メニューからネットワーク ポートのテストを実施頂く事で、問題なく通信が成功するか確認頂く事も可能です。  
   ![](./ip-range-and-domain/onpremisedatagateway2.png)  
   ![](./ip-range-and-domain/onpremisedatagateway3.png)  

## 接続先との通信

<a id='anchor-connector'></a>
### コネクタ
1. [コネクターの送信 IP アドレス](https://learn.microsoft.com/en-us/connectors/common/outbound-ip-addresses#power-platform) の Power Platform セクションに記載されているサービスタグおよびすべての IP アドレスを許可してください。  
   各サービスタグには IP アドレスがグルーピングされているので、サービスタグを指定できない場合は、サービスタグに紐づく IP アドレスを許可してください。  
   サービスタグに紐づく IP アドレスは、[PowerShell](https://learn.microsoft.com/en-us/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api) または [ダウンロードしたJSON ファイル](https://learn.microsoft.com/en-us/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) から確認できます。  

   多くのコネクタはこちらの IP アドレスから通信を行います。
   ![](./ip-range-and-domain/connector-outbound.png)

<a id='anchor-builtin'></a>
### 組込アクション・トリガー
組込アクションやトリガーは Power Automate サービスから直接通信を行うため、前述のコネクタとは異なる許可を構成頂く必要がございます。  
要求を送信するアクションと要求を受信して動作するトリガーで異なるネットワーク設定が必要です。
![](./ip-range-and-domain/builtin-action.png)  

1. HTTP、'HTTP + Swagger' 等、対向サービスへ要求を送信する組込アクション
   [Power Automate の IP アドレスの構成](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration#allowlist-http-and-http--swagger-calls-to-your-services) の **サービスへの 'HTTP' よび 'HTTP + Swagger' 呼び出しを許可リストに載せる** セクションに記載されているサービスタグを許可してください。
   
   ![](./ip-range-and-domain/builtin-outbound.png)  
  
1. HTTP 要求受信時トリガー等、対向サービスからの要求を受信する組込トリガー
   サービスタグのご用意はありません。
   [Power Automate の IP アドレスの構成](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration#allow-users-on-your-network-to-use-when-an-http-request-is-received-trigger) の **ネットワーク上のユーザーが "HTTP 要求が受け取った時" トリガーを使用するのを許可する** セクションに記載されているドメインを許可してください。

   ![](./ip-range-and-domain/builtin-inbound.png)  

> [!NOTE]
> 許可する IP アドレスまたはサービスタグ、ドメインは、ご利用の Power Platfrom 環境の地域に対応するものをご選択頂く事が可能です。  

<a id='anchor-summary'></a>
## まとめ

| サービス | 許可手段 | サービスタグ | 通信方向 | マイクロソフト視点 |
| :- | :- | :- | :- | :- |
| キャンバスアプリ | [ドメイン](https://learn.microsoft.com/en-us/power-apps/limits-and-config#required-services)  | - | クライアント ⇔ マイクロソフト | 送信/受信 |
| Power Automate | [ドメイン](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration)  | - | クライアント ⇔ マイクロソフト | 送信/受信 |
| オンプレミス<br>データ ゲートウェイ | [ドメイン/ポート](https://learn.microsoft.com/en-us/data-integration/gateway/service-gateway-communication#required-ports-for-the-gateway-to-function) | - | クライアント ⇒ マイクロソフト | 受信 |
| Power Platform コネクタ | [IP アドレス/サービス タグ](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration#allow-connector-calls-to-your-services) | AzureConnectors | 接続先サービス ⇔ マイクロソフト | 送信/受信 |
| 組込アクション | [IP アドレス/サービス タグ](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration#allowlist-http-and-http--swagger-calls-to-your-services) | LogicApps<br />PowerPlatfromPlex | マイクロソフト ⇒ 接続先サービス | 送信 |
| 組込トリガー | [ドメイン](https://learn.microsoft.com/en-us/power-automate/ip-address-configuration#allow-users-on-your-network-to-use-when-an-http-request-is-received-trigger)  | - | 接続先サービス ⇒ マイクロソフト | 受信 |


## よくある質問

### どの地域を選べばいいですか
フローやアプリを作成している Power Platform 環境の地域を選んでください。  
  ![](./ip-range-and-domain/region.png)

### Microsoft 365 や Azure とは別に IP アドレスやドメインを指定する必要がありますか
はい、別で指定する必要があります。  
公開情報も分かれていますので、Power Platform を利用する場合は Power Platform の公開情報を元に設定を行ってください。

### 社内のネットワークとコネクタの接続先の通信を許可する必要はありますか
コネクタを利用する場合は、マイクロソフトのクラウドサーバーを経由して通信するため、社内ネットワークとコネクタ接続先の通信を許可する必要はありません。

### 利用するコネクタが限られているため、特定のコネクタが利用する通信だけ許可することはできますか
恐れ入りますが、現時点では特定のコネクタの通信のみ許可する方法はございません。  
利用すると記載のある IP アドレスすべてを許可してください。

### IP アドレスやドメインの変更はメッセージセンター等で通知されますか
影響範囲の大きい変更がある場合は通知されることもありますが、基本的には通知されませんのでお客様にて定期的な監視をお願いしています。  
IP アドレスについてはサービスタグや[パブリック IP アドレス](https://learn.microsoft.com/en-us/power-platform/admin/online-requirements#ip-addresses-required) ファイルを提供していますので、こちらをご利用ください。ドメインについてはお手数ですが定期的に公開情報にて増減がないか確認をお願いいたします。今後、確認作業の簡易化について取り組んでいく予定です。

### どれくらいの頻度で確認したほうがいいですか
最低でも月に 1 度はご確認をお願いいたします。  

### IP アドレスやドメインをホワイトリストに追加しない状態で使うことができていますが、追加しなくてもいいですか
いいえ、ホワイトリストに追加し、接続できるようにしてください。  
未利用の機能で使われているため新しく機能を使うときにエラーが出る可能性があります。  
また、機能変更で新しい接続先を使うようになり、急にエラーとなる可能性があります。

### ドメイン指定時に * を指定しない方法はありますか
いいえ、FQDNs は公開されていないため * を指定せずにドメインを許可する方法はありません。

---
