---
title: 外部アプリから Dataverse の TDS endpoint に接続できないときの確認ポイント
date: 2026-03-16 18:00:00
tags:
  - Power Platform
  - Dataverse
  - Power BI
  - TDS endpoint
  - Network
  - Troubleshooting
categories:
  - [Dataverse]
  - [Power BI]
toc:
  enabled: true
  min_depth: 1
  max_depth: 3
  list_number: false
---

# 外部アプリから Dataverse の TDS endpoint に接続できないときの確認ポイント

こんにちは、Power Platform サポートチームの王です。

Power BI Desktop、SQL Server Management Studio (SSMS)、Excel などの外部アプリケーションから Dataverse に接続しようとした際に、次のようなエラーが表示されることがあります。

- A connection was successfully established with the server, but an error occurred during the handshake before you sign in
- Unable to connect (provider Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)
- クラウド サービス側 (Power BI Service 等) では利用できるが、クライアント アプリからは接続できない
- 環境一覧は表示されるが、テーブル一覧を展開すると authentication error や missing privilege error になる

これらの事象は認証だけでなく、Dataverse の TDS endpoint の設定やネットワーク経路が関係している場合があります。
本記事では、公開情報をもとに、よく見られる原因と確認ポイントを順を追ってご紹介いたします。

> [!NOTE]
> 特に Power BI Desktop からの接続に関するお問い合わせが多いため、本記事では Power BI Desktop を主な例として記載していますが、TDS endpoint を利用する他のアプリケーション (SSMS、Excel、Azure Data Factory など) にも共通する内容です。

<!-- more -->

> [!IMPORTANT]
> 通信要件や IP アドレスは変更される可能性があります。実際の設定にあたっては、記事末尾の公式ドキュメントもあわせてご確認ください。

## 目次

- [まず押さえたいポイント](#anchor-key-points)
- [確認ポイントと切り分け](#anchor-checks)
- [過去の事例で多く見られた原因](#anchor-past-cases)
- [それでも解決しない場合に整理したい情報](#anchor-support-info)
- [まとめ](#anchor-summary)
- [参考情報](#anchor-references)

<a id='anchor-key-points'></a>

## まず押さえたいポイント

Dataverse の TDS endpoint は、Power BI Desktop、SSMS、Excel などの外部アプリケーションが SQL ベースのクエリで Dataverse のデータにアクセスするためのインターフェイスです。
これらの接続は HTTPS 443 だけでなく TDS プロトコル (TCP 1433 / 5558) を使用するため、Web ブラウザーで Dataverse にアクセスできることと、外部アプリから TDS endpoint で接続できることは、必ずしも同じ意味ではありません。

特に確認しておきたい点は次のとおりです。

- TDS endpoint への接続には TCP 1433 または 5558 が必要
- 1433 が使えない場合は 5558 を利用できるが、接続先 URL に ,5558 を付与する必要がある
- 多くのプロキシ サーバーは TDS プロトコルを適切に処理できない
- TDS 通信では 1433/5558 から 443 へのポート リダイレクトが発生し、SSL inspection が干渉することがある
- TDS endpoint のユーザーレベル制御が有効な場合、権限不足で authentication error や missing privilege error になることがある


<a id='anchor-checks'></a>

## 確認ポイントと切り分け

### 確認 1. 接続先 URL と接続方法を見直す

手動で URL を入力する場合は、環境 URL の書式に少し注意が必要です。

- https:// は付けない
- 末尾の / は付けない
- 5558 を使う場合は末尾に ,5558 を付ける

例:

- 正しい例: orgname.crm7.dynamics.com
- 5558 を使う例: orgname.crm7.dynamics.com,5558
- 避けたい例: https://orgname.crm7.dynamics.com/

### 確認 2. TDS endpoint 設定を確認する

Power Platform 管理センターで、対象環境の TDS endpoint 設定をご確認ください。

1. Power Platform 管理センターを開きます。
2. 対象環境を選択します。
3. 設定 > 製品 > 機能 を開きます。
4. 次の項目を確認します。

- Enable TDS endpoint
- Enable user level access control for TDS endpoint

既定では、Enable TDS endpoint はオン、Enable user level access control for TDS endpoint はオフです。

ユーザーレベル制御をオンにしている場合は、対象ユーザーに Allow user to access TDS endpoint 権限を持つセキュリティ ロールが必要になります。
この設定が不足していると、Power BI Desktop のテーブル展開時や SSMS での接続時に authentication error や missing privilege error が発生することがあります。

### 確認 3. サインイン アカウントと資格情報キャッシュを確認する

Dataverse の TDS endpoint へのアクセスには、Microsoft Entra ID (旧 Azure AD) による認証が前提となります。
Power Apps や Dataverse に接続しているものと同じ組織アカウントでサインインしているかどうか、一度ご確認ください。

また、アプリに古い資格情報が残っていると、誤ったテナントやアカウントで接続を試みて失敗することがあります。

- Power BI Desktop の場合: File > Options and settings > Data source settings から資格情報をクリアして再サインイン
- SSMS の場合: Azure Active Directory 認証を選択し、正しいアカウントで接続

### 確認 4. TCP 1433 / 5558 の疎通を確認する

まずはクライアント端末から Dataverse endpoint へ TCP 接続できるかどうかをご確認ください。

```powershell
Test-NetConnection -ComputerName <environment>.crm7.dynamics.com -Port 1433
Test-NetConnection -ComputerName <environment>.crm7.dynamics.com -Port 5558
```

TcpTestSucceeded が False の場合は、クライアント側でポートがブロックされている可能性があります。

あわせて、名前解決結果と IP アドレス直接指定での接続もご確認いただくと、切り分けがしやすくなります。

```powershell
Resolve-DnsName <environment>.crm7.dynamics.com -Type A
Resolve-DnsName <environment>.crm7.dynamics.com -Type AAAA

Test-NetConnection -ComputerName <RemoteAddress> -Port 1433
Test-NetConnection -ComputerName <RemoteAddress> -Port 5558
```

> [!NOTE]
> Test-NetConnection で成功しても、TDS プロトコル全体の通信が最後まで成功するとは限りません。
> 後述する SSL inspection やプロキシの影響で、TCP 接続はできてもアプリからの接続がうまくいかない場合があります。

### 確認 5. プロキシや SSL inspection の影響を切り分ける

公開情報では、Dataverse の TDS endpoint で利用する TDS プロトコルについて、次の点が案内されています。

- 多くのプロキシ サーバーは TDS プロトコルを処理できない
- 1433/5558 から 443 へのポート リダイレクトが発生する
- SSL inspection がこのリダイレクトをブロックすることがある
- ホスト名ベースの許可だけでは不十分で、IP ベースの許可が必要になることがある

そのため、社内ネットワーク配下でのみ再現する場合は、次の点をご確認いただくと切り分けに役立ちます。

1. モバイル テザリングなど、社内プロキシを経由しない経路で再現するか確認する
2. ネットワーク担当者に、Dataverse 向け TDS 通信が SSL inspection でブロックされていないか確認する
3. FQDN の許可だけでなく、Power Platform / Dynamics 365 用の IP 許可が必要か確認する

ネットワーク担当者へ依頼される際は、少なくとも次の観点を共有いただくと整理しやすくなります。

- *.crm#.dynamics.com 向けの TCP 1433 / 5558 / 443
- AzureCloud サービスタグの対象リージョン
- 端末やネットワークが IPv6 を優先する場合は IPv6 側の許可

### 確認 6. IPv6 を利用するネットワークか確認する

Power Platform と Dynamics 365 では IPv6 サポートが進んでおり、環境によっては A レコードだけでなく AAAA レコードにも解決されます。
お使いの端末やネットワークが IPv6 を優先する場合、IPv4 側が許可されていても IPv6 側の通信がブロックされると、外部アプリからの TDS 接続に影響することがあります。

そのため、A レコードと AAAA レコードの両方をご確認いただき、必要に応じて IPv4 / IPv6 を分けて疎通をお確かめください。

### 確認 7. Azure SQL へ接続できても Dataverse の確認にはならない

「Azure SQL Database へは接続できたので、Dataverse 側も問題ないはず」とお考えになることもあるかと思いますが、必ずしもそうとは限りません。

Azure SQL Database は *.database.windows.net、Dataverse の TDS endpoint は *.crm#.dynamics.com であり、接続先も前提となる通信経路も異なります。
そのため、Azure SQL への接続成功は Dataverse TDS endpoint の到達性を直接示すものではありません。

<a id='anchor-past-cases'></a>

## 過去の事例で多く見られた原因

弊社サポートにお問い合わせいただいた複数の事例では、共通して **TDS プロトコルの通信経路がネットワーク機器によって阻害されていた** ことが原因でした。
以下に代表的なパターンをご紹介いたします。

### SSL inspection による TDS ポート リダイレクトのブロック

最も多く確認された原因です。

TDS endpoint への接続では、初回の TCP 接続 (1433/5558) が成功した後、サーバー側から 443 へのポート リダイレクトが発生します。
SSL inspection (TLS インターセプト) がこのリダイレクトを「非 SSL ポートから SSL ポートへの不正なリダイレクト」と判断し、ブロックすることがあります。

この場合、`Test-NetConnection` では TCP 1433/5558 に到達できるにもかかわらず、Power BI Desktop 等のアプリからは **handshake before you sign in** エラーとなります。
Dataverse 側のログにも TDS セッションが一切記録されないため、リクエストがサーバーに到達していないことが確認できます。

**対処方法**: Dataverse TDS 通信 (`*.crm#.dynamics.com` のポート 1433/5558/443) を SSL inspection の除外対象に設定してください。

### クラウド プロキシによる TDS プロトコルのブロック

Zscaler、Netskope 等のクラウド プロキシを経由する環境で確認された原因です。

TDS プロトコルは HTTP ではなく TCP レベルのプロトコルであるため、多くの Web プロキシでは正しく処理できません。
Azure Virtual Desktop やシンクライアントからクラウド プロキシ経由でインターネットに接続する構成では、TDS トラフィックがプロキシでインターセプトされ、接続に失敗します。

**対処方法**: PAC ファイルまたはクラウド プロキシの管理画面で、Power Platform 関連の FQDN (`*.dynamics.com`、`*.crm#.dynamics.com` 等) をプロキシ バイパスに設定してください。

### IPv6 経路でのポート ブロック

端末やネットワークが IPv6 を優先する環境で確認された原因です。

DNS 解決で A レコード (IPv4) と AAAA レコード (IPv6) の両方が返される場合、IPv6 が優先されることがあります。
ファイアウォールで IPv4 側のポート 1433 は許可されていても、IPv6 側が許可されていないと、TDS endpoint への接続がタイムアウトとなります。

ある事例では、IPv4 アドレスへの TCP 1433 は成功、IPv6 アドレスへの TCP 1433 は失敗しており、IPv6 の ACL 設定不足が原因でした。

**対処方法**: A レコードと AAAA レコードの両方について、ポート 1433/5558/443 の疎通をご確認ください。IPv6 側がブロックされている場合は、ネットワーク担当者に IPv6 向けの ACL 追加をご依頼ください。

### FQDN ベースの許可だけでは不十分な場合

ファイアウォールで `*.crm#.dynamics.com` を許可していても、TDS のポート リダイレクト後に接続する先が Power Platform インフラの IP アドレスとなる場合があります。
この IP アドレスには逆引き DNS レコード (PTR) が存在しないため、FQDN ベースのフィルタリングでは許可対象と認識されず、ブロックされることがあります。

ある事例では、`*.crm#.dynamics.com` は許可済みにもかかわらず、リダイレクト先の IP (`20.18.7.0/28`、`172.207.69.128/26` 等の PowerPlatformInfra サービスタグに含まれる IP) がファイアウォールでブロックされていました。

**対処方法**: FQDN ベースの許可に加えて、Azure Service Tags (特に `PowerPlatformInfra` および `AzureCloud` の対象リージョン) に含まれる IP アドレス範囲をファイアウォールで許可してください。Azure IP Ranges and Service Tags の一覧は [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=56519) から入手できます。

---

> [!TIP]
> いずれの事例でも、**モバイル テザリングなど社内ネットワークを経由しない経路で接続を試す** ことで、ネットワーク機器が原因かどうかを効率的に切り分けることができました。

<a id='anchor-support-info'></a>

## それでも解決しない場合に整理したい情報

サポートへお問い合わせいただく際は、次の情報をお寄せいただけますと切り分けがスムーズに進みます。

- 表示されるエラー メッセージ全文
- 使用しているアプリケーションとバージョン (Power BI Desktop、SSMS、Excel 等)
- 接続先の Dataverse 環境 URL
- クラウド サービス側 (Power BI Service 等) では接続できるか
- Test-NetConnection の結果
- 別ネットワークで再現するか
- TDS endpoint の設定値
- TDS endpoint のユーザーレベル制御を利用しているか

<a id='anchor-summary'></a>

## まとめ

外部アプリから Dataverse に接続できない場合、特に handshake before you sign in のようなエラーは、認証そのものより前段の TDS endpoint 接続で失敗しているケースがあります。

その場合は、次の順番でご確認いただくと切り分けがしやすくなります。

1. 接続先 URL の書式
2. TDS endpoint の有効化
3. TDS endpoint のユーザーレベル制御と権限
4. TCP 1433 / 5558 の疎通
5. プロキシ、SSL inspection、IPv6 を含むネットワーク経路
6. アプリの資格情報キャッシュ

特に、クラウド サービス側 (Power BI Service 等) では問題なくクライアント アプリのみ失敗する場合は、クライアント端末から Dataverse までのネットワーク経路を重点的にご確認いただければと思います。

<a id='anchor-references'></a>

## 参考情報

- [Dataverse connector - Power Query](https://learn.microsoft.com/power-query/connectors/dataverse)
- [Create a Power BI report using data from Dataverse](https://learn.microsoft.com/power-apps/maker/data-platform/data-platform-powerbi-connector)
- [Use SQL to query data](https://learn.microsoft.com/power-apps/developer/data-platform/dataverse-sql-query)
- [Control access of the TDS endpoint](https://learn.microsoft.com/power-platform/admin/control-tds-settings)
- [Set up the Power BI dashboard - Troubleshooting](https://learn.microsoft.com/power-platform/guidance/coe/setup-powerbi#troubleshooting)
- [Power Platform URLs and IP address ranges](https://learn.microsoft.com/power-platform/admin/online-requirements)