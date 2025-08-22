---
title: Dataflow お問い合わせの際の情報取得手順
date: 2025-08-20 09:00:00

tags:
  - お問い合わせ全般
  - 情報採取
  - Power Apps
  - Dataflow
categories:
  - [サポートサービスのご利用について]
  - [Power Apps]
  - [Dataflow]
---

# はじめに

こんにちは、Power Platform サポートチームの宮﨑です。  
本記事では、Dataflowのトラブルシューティングにあたって、必要となる情報の取得手順についてご案内いたします。


<!-- more -->
# 目次

1. [概要](#anchor-intro)
1. [情報取得手順詳細](#anchor-how-to-collect)
    1. [事象の発生状況](#anchor-about-situation)
    1. [エラー発生時のネットワークトレース](#anchor-network-trace)
    1. [Dataflowテーブル](#anchor-dataflow-table)
    1. [Dataflowの更新履歴ファイル（csv）](#anchor-dataflow-history)
    1. [ソリューションのエクスポート](#anchor-solution-export)
    1. [ユーザーアカウント](#anchor-user-account)
1.  [補足](#補足)

<a id='anchor-intro'></a>

# 概要
弊社サポートでは、お問い合わせを頂いた際のトラブルシューティングにおいて、お問い合わせの内容をもとに調査方針を立てております。
発生している事象の把握のため、発生している事象の切り分けや情報提供をお願いすることがあります。
Dataflow に関するサポートサービスのお問い合わせの際の、情報取得手順について、以下の通りご案内いたします。

<a id='anchor-how-to-collect'></a>

# 情報取得手順詳細

<a id='anchor-about-situation'></a>

## 事象の発生状況
エラーがどのような状況下で発生するかお知らせください。  
事象の発生条件を特定することで、問題の特定だけではなく弊社環境での再現調査においても有用な情報が得られます。
以下の情報をお知らせいただくことでより明確に事象を把握することができます。  
* 事象が発生しているデータフローのデータソースについてご連携いただけますでしょうか。
* 事象が発生しているデータフローについては、オンプレミスデータゲートウェイはご利用されておりますでしょうか。
* 対象となるデータフローの型については、Standard V2 でお間違い無いでしょうか。

<a id='anchor-network-trace'></a>

## エラー発生時のネットワークトレース
>[!NOTE]
>以下の手順は、データフロー編集時にエラーが起きた場合に実行してください。

エラーが発生する作業を実施したときの、ブラウザのネットワークトレースのご取得をお願いいたします。<br>
データフローの編集を行う直前でログの記録を開始し、エラーが表示されるまでの動作をログに収録いただけますようお願い申し上げます。

ログの取得方法については、お手数ですが以下の公開情報をご参照ください。 
※手順内の1. 及び2.ステップ記録ツール」については実施いただかなくて大丈夫です。 
https://learn.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace#google-chrome-and-microsoft-edge-chromium


<a id='anchor-dataflow-table'></a>

## Dataflowテーブル
対象のデータフローのIDやクエリの詳細を確認させていただくために、Dataverse内のDataflowテーブルのご提供をお願いいたします。 
以下の手順にてDataflowテーブルを表示して、ご提供頂けますようお願いいたします。

1. Power Apps ( https://make.powerapps.com/ ) を開き、[設定] – [開発者リソース] の順にクリックします。
![](./helpful-information-for-dataflow-sr/developer-resource.png) 

2. [Web API エンドポイント] を取得し、末尾に /msdyn_dataflows を追加したリンクを作成します。
例 : https://org **bbca2a.api.crm7.dynamics.com/api/data/v9.2/msdyn_dataflows
![](./helpful-information-for-dataflow-sr/webapi-endpoint.png)

3. 項番 2 にて作成したリンクにアクセスし、保存します。
※ 以下の画面は Edge での例となります。
※ 保存時に、ファイル名に環境 ID などの記載いただけますと幸いです。
![](./helpful-information-for-dataflow-sr/access-link.png)

<a id='anchor-dataflow-history'></a>

## データフローの更新履歴ファイル（csv）
対象のデータフローの更新履歴ファイル（CSV）を開き、該当する問題の実行履歴をダウンロードしていただけますようお願いいたします。<br>
また、過去に成功した実行履歴がある場合は、そちらも併せてダウンロードの上、ご提供いただけますと幸いです。
![](./helpful-information-for-dataflow-sr/dataflow-history.png)
![](./helpful-information-for-dataflow-sr/dataflow-download.png)

<a id='anchor-solution-export'></a>

## ソリューションのエクスポート
対象のデータフローをソリューションに含めていただき、アンマネージメントソリューションにてエクスポートをいただけますようお願いいたします。<br>
実装の内容やご利用のコネクタについて確認をさせていただきます。

<a id='anchor-user-account'></a>

## ユーザーアカウント
対象のデータフロー所有者のユーザーアカウントをご教授ください。

---

<a id='補足'></a>

# 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって UI の遷移などが若干異なる場合があります。その場合は画面の指示に従って進めてください。  

