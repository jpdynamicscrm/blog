---
title: Power Automate クラウド フローの実行履歴を Dataverse の FlowRun テーブルから取得する
date: 2026-08-05 11:28:00
tags:
  - Power Automate
  - Cloud flow
  - Dataverse
categories:
  - [Power Automate, Cloud flow]
---

# はじめに

こんにちは、Power Platform サポートチームの早坂です。  
本記事では、Power Automate クラウド フローの実行結果を Dataverse の `FlowRun` テーブルから取得する方法と、同テーブルに保存される情報の範囲をご紹介します。

`FlowRun` テーブルでは、フローの開始時刻、終了時刻、状態、エラー概要などの実行単位の情報を取得できます。一方、各アクションの名前、入力、出力、追跡 ID などの詳細情報は保存されません。用途に応じて Power Automate の実行履歴や Application Insights と使い分ける必要があります。

<!-- more -->

### この記事でわかること

- Dataverse の `FlowRun` テーブルに保存される情報と、保存されない情報
- Dataverse Web API と PowerShell を使用して実行履歴を取得する方法
- `FlowRun` の保持期間と、データの完全性に関する注意点
- Power Automate ポータル、`FlowRun`、Application Insights の使い分け

# 目次

1. [対象範囲と前提条件](#anchor-prerequisites)
1. [FlowRun テーブルに保存される情報](#anchor-flowrun-data)
1. [Dataverse Web API から実行履歴を取得する](#anchor-web-api)
1. [アクション単位の詳細が必要な場合](#anchor-action-details)
1. [保持期間とデータの完全性](#anchor-retention)
1. [検証結果](#anchor-validation)
1. [まとめ](#anchor-summary)
1. [参考情報](#anchor-references)

---

<a id='anchor-prerequisites'></a>

# 対象範囲と前提条件

`FlowRun` テーブルへ実行履歴が保存される対象は、定義が Dataverse に保存されている**ソリューション クラウド フロー**です。

> [!IMPORTANT]
> ソリューションに含まれていないクラウド フローは、`FlowRun` テーブルへの実行履歴保存の対象ではありません。

本記事では、Dataverse Web API を使用してデータを読み取る例を紹介します。API の実行には、対象環境の Dataverse テーブルを読み取る権限と、Dataverse Web API 用のアクセストークンが必要です。

<a id='anchor-flowrun-data'></a>

# FlowRun テーブルに保存される情報

`FlowRun` は Dataverse の弾力テーブルです。主に次の実行単位の情報が保存されます。

| 論理名 | 内容 |
|---|---|
| `name` | フロー実行の識別子 |
| `starttime` | 実行開始時刻 |
| `endtime` | 実行終了時刻 |
| `duration` | 実行時間（ミリ秒） |
| `status` | 実行結果 |
| `triggertype` | トリガーの種類 |
| `errorcode` | エラー コード |
| `errormessage` | エラー メッセージの概要 |
| `clienttrackingid` | クライアント追跡 ID |
| `workflowid` | クラウド フローの識別子 |
| `parentrunid` | 親フローの実行識別子 |
| `partitionid` | 論理パーティションの識別子 |
| `ttlinseconds` | レコードの保持期間を表す秒数 |

> [!NOTE]
> `duration` のスキーマ名は `DurationInMs` であり、実機検証でもミリ秒として取得されました。クラウド フローの実行履歴に関する概要ページには秒単位との記載もありますが、Dataverse Web API の値を扱う場合はミリ秒として計算してください。

一方、次のようなアクション単位の情報は `FlowRun` テーブルへ保存されません。

- アクションの表示名
- アクションごとの状態や実行時間
- アクションの入力と出力
- アクション追跡 ID
- `serviceRequestId`

失敗した実行の `errorcode` と `errormessage` には、たとえば `ActionFailed` と、その概要メッセージが保存されます。ただし、どのアクションが失敗したかを特定できる詳細情報が常に含まれるわけではありません。

<a id='anchor-web-api'></a>

# Dataverse Web API から実行履歴を取得する

`FlowRun` テーブルのエンティティ セット名は `flowruns` です。次のような GET 要求で実行履歴を取得できます。

```http
GET https://<組織 URL>/api/data/v9.2/flowruns
  ?$select=name,starttime,endtime,duration,status,triggertype,errorcode,errormessage,clienttrackingid,workflowid,partitionid,ttlinseconds
  &$filter=workflowid eq '<クラウド フロー ID>'
  &$orderby=starttime desc
  &$top=100
Accept: application/json
OData-Version: 4.0
Authorization: Bearer <アクセストークン>
```

PowerShell から取得する場合は、Dataverse Web API 用のアクセストークンを取得したうえで、次のように要求できます。

```powershell
$webApiUrl = 'https://<組織 URL>/api/data/v9.2'
$accessToken = '<Dataverse Web API 用のアクセストークン>'
$flowId = '<クラウド フロー ID>'

$select = 'name,starttime,endtime,duration,status,triggertype,errorcode,errormessage,clienttrackingid,workflowid,partitionid,ttlinseconds'
$filter = [uri]::EscapeDataString("workflowid eq '$flowId'")
$requestUri = "$webApiUrl/flowruns?`$select=$select&`$filter=$filter&`$orderby=starttime desc&`$top=100"

$headers = @{
    Authorization      = "Bearer $accessToken"
    Accept             = 'application/json'
    'OData-Version'    = '4.0'
    'OData-MaxVersion' = '4.0'
}

$response = Invoke-RestMethod -Method Get -Uri $requestUri -Headers $headers
$response.value
```

> [!NOTE]
> `FlowRun` は弾力テーブルであり、ユーザーごとの論理パーティションに分割されます。大量データを継続的に取得する場合は、最初に取得したレコードの `partitionid` を確認し、対象パーティションを限定する設計を検討してください。

<a id='anchor-action-details'></a>

# アクション単位の詳細が必要な場合

アクション名、アクションごとの成否、入力、出力、追跡 ID を確認する場合は、Power Automate ポータルの実行履歴を使用します。

長期的な監視や分析でアクション単位のテレメトリが必要な場合は、Application Insights へのエクスポートを検討します。Application Insights では、クラウド フローの実行は `requests`、トリガーとアクションは `dependencies` に保存されます。

次の KQL は、指定した環境とフローの失敗したアクションを検索する例です。

```kql
let environmentId = '<環境 ID>';
let flowId = '<クラウド フロー ID>';
dependencies
| where timestamp > ago(1d)
| where customDimensions['resourceProvider'] == 'Cloud Flow'
| where customDimensions['signalCategory'] == 'Cloud flow actions'
| where customDimensions['environmentId'] == environmentId
| where customDimensions['resourceId'] == flowId
| where success == false
| project timestamp, name, resultCode, duration, customDimensions
| order by timestamp desc
```

> [!IMPORTANT]
> クラウド フローの Application Insights 連携は、マネージド環境でサポートされます。また、Application Insights のテレメトリもトランザクション データではないため、すべてのログが欠落なく保存されることは保証されません。

<a id='anchor-retention'></a>

# 保持期間とデータの完全性

`FlowRun` レコードの既定の保持期間は 28 日、秒数では `2,419,200` 秒です。Power Platform 管理センターでは、環境の設定から 7 日、14 日、28 日、または無効を選択できます。

組織テーブルの `FlowRunTimeToLiveInSeconds` を変更すると、新しく作成される `FlowRun` レコードの保持期間が変更されます。値を `0` にすると、新しいレコードの取り込みが停止します。

> [!WARNING]
> `FlowRun` への書き込みに使用されるデータ ストリームはトランザクション データではなく、100% の完全性は保証されません。監査や完全な実行記録が必要な設計では、`FlowRun` だけに依存しないでください。

`FlowRun` データが不完全となる可能性がある場合は、`FlowEvent` テーブルの `FlowRunIngestion` イベントを確認できます。保持期間の無効化、ストレージ容量、パーティション上限、取り込みレート、権限不足などに関するシグナルが記録されます。

<a id='anchor-validation'></a>

# 検証結果

2026 年 8 月 5 日に、検証用 Dataverse 環境とソリューション クラウド フローを使用して確認しました。

| 確認項目 | 結果 |
|---|---|
| Dataverse Web API から `flowruns` を取得 | 成功 |
| 成功した実行の状態、実行時間、トリガー種別を取得 | 成功 |
| `ttlinseconds` | `2419200` を確認 |
| 失敗した実行の `errorcode` | `ActionFailed` を確認 |
| 失敗した実行の `errormessage` | 実行単位の概要メッセージを確認 |
| Power Automate ポータルのアクション名、アクション追跡 ID | ポータルでは確認可能 |
| アクション名、アクション追跡 ID を `FlowRun` から取得 | 対応する列が存在しないため取得不可 |

また、実行直後のレコードが Dataverse Web API の応答へすぐに現れない場合がありました。取得処理では、即時反映を前提とせず、再試行や待機時間を考慮してください。

<a id='anchor-summary'></a>

# まとめ

Dataverse の `FlowRun` テーブルは、ソリューション クラウド フローの実行結果を横断的に取得し、状態や実行時間を集計する用途に適しています。

一方、アクション名、アクションごとの入力と出力、アクション追跡 ID などは保存されません。実行単位の集計には `FlowRun`、個別実行の調査には Power Automate ポータル、アクション単位の長期分析には Application Insights を使用してください。

<a id='anchor-references'></a>

# 参考情報

- [Dataverse でのクラウド フローの実行履歴](https://learn.microsoft.com/ja-jp/power-automate/dataverse/cloud-flow-run-metadata)
- [FlowRun テーブル/エンティティ参照](https://learn.microsoft.com/ja-jp/power-apps/developer/data-platform/reference/entities/flowrun)
- [Web API を使用してデータのクエリを実行する](https://learn.microsoft.com/ja-jp/power-apps/developer/data-platform/webapi/query-data-web-api)
- [弾力テーブルを作成して編集する](https://learn.microsoft.com/ja-jp/power-apps/maker/data-platform/create-edit-elastic-tables)
- [Application Insights を使用してクラウド フローを監視する](https://learn.microsoft.com/ja-jp/power-platform/admin/app-insights-cloud-flow)
- [オートメーション センターの概要](https://learn.microsoft.com/ja-jp/power-automate/automation-center-overview)

※本記事の執筆には生成 AI を使用しています。[参考](https://learn.microsoft.com/ja-jp/principles-for-ai-generated-content)

---
免責事項
※本情報の内容 (添付文書、リンク先などを含む) は、作成日時点でのものであり、予告なく変更される場合があります。
