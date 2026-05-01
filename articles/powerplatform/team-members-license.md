---
title: Dynamics 365 Team Members ライセンスのカスタマイズ制限と活用方法
date: 2026-05-01 12:00:00
tags:
  - Power Platform
  - Dynamics 365
  - Team Members
  - ライセンス
categories:
  - [Dynamics 365]
---

# Dynamics 365 Team Members ライセンスのカスタマイズ制限と活用方法

こんにちは、Power Platform サポートチームの三田です。

本記事では Dynamics 365 Team Members ライセンスのカスタマイズ制限と活用方法について、以下の Q1〜Q5 の質問に回答する形でご説明させていただきます。

- [Q1: Team Member アプリにテーブルを追加したら 15 個制限のエラーが出ました。参照だけでも制限されますか？](#q1)
- [Q2: 15 個制限の対象になるテーブルはどれですか？もともと含まれているテーブルもカウントされますか？](#q2)
- [Q3: 15 個の枠を使い切った場合、他のテーブルのデータを見る方法はありますか？](#q3)
- [Q4: 登録・更新・削除ができるテーブル数に上限はありますか？](#q4)
- [Q5: Team Members ライセンスのユーザーで SharePoint へファイルをアップロードできますか？](#q5)

<!-- more -->

### この記事でわかること
- Team Member アプリに追加できるテーブル数の上限（15 個）とその意味
- 15 個制限にカウントされるテーブルとされないテーブルの違い
- 15 個を超えたテーブルのデータを見るための代替手段
- 登録・更新・削除が許可される操作の範囲（数の上限ではなくシナリオで決まる）
- SharePoint ファイルアップロードの可否と必要な権限
- システムダッシュボードと個人ダッシュボードでの動作の違い

## 目次
- [Team Member アプリの 15 テーブル制限について (Q1)](#q1)
- [15 個制限にカウントされるテーブルの範囲 (Q2)](#q2)
- [15 個を超えたテーブルのデータを参照する方法 (Q3)](#q3)
- [登録・更新・削除が可能なテーブル数の上限について (Q4)](#q4)
- [SharePoint へのファイルアップロードについて (Q5)](#q5)
- [まとめ](#summary)
- [注意事項（情報の更新可能性）](#notice)

<h2 id="q1">Team Member アプリの 15 テーブル制限について (Q1)</h2>

【結論】Team Member アプリに追加できるテーブルは最大 15 個までです。
この制限は「参照のみ」の場合でも同様に適用されます。

Team Member アプリ（Sales Team Member や Customer Service Team Member）は、Dynamics 365 Team Members ライセンスのユーザー向けに設計された軽量なアプリです。
アプリの発行元である Dynamics 365 によって「アプリに含めることができるテーブル数は最大 15 個まで」という上限が設定されています。
この上限を超えてテーブルを追加しようとすると、以下のようなエラーが表示されます。

> This operation failed since it exceeded the maximum entity limit of 15 total entities for the app msdyn_TeamMember_Sales set by its owning publisher Dynamics 365.

この制限はシステム側で強制されるため、参照目的であっても回避できません。
ただし、アプリに直接追加しなくてもデータを参照する方法がありますので、[Q3](#q3) で詳しくご説明します。

＜参考資料＞
- [Dynamics 365 Team Members license - Customization restrictions (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/get-started/team-members-license#customization-restrictions)
- [Dynamics 365 Team Members license - Frequently asked questions (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/get-started/team-members-license#frequently-asked-questions)

<h2 id="q2">15 個制限にカウントされるテーブルの範囲 (Q2)</h2>

【結論】もともとアプリに含まれているテーブル（既定のテーブル）は 15 個にカウントされません。
15 個制限の対象は「元のアプリに含まれていないテーブルを追加した場合」のみです。

公式ドキュメントの FAQ には次のように記載されています。

> Can I customize these new Team member apps?
> Yes, ... However—as mentioned in the licensing guide—you can only have up to 15 entities in the app. These entities can include core Dataverse entities that aren't part of the original app module, other entities published by Microsoft, or custom entities.

つまり、Sales Team Member アプリに最初から含まれているテーブル（取引先企業、連絡先、潜在顧客、営業案件、活動など）は 15 個のカウントに含まれません。
15 個制限に含まれるのは以下のテーブルです。

1. 元のアプリに含まれていない Dataverse の標準テーブル
2. Microsoft が発行したその他のテーブル
3. カスタムテーブル（自分で作成したテーブル）

【ポイント】
1. 最初からアプリに入っているテーブルは制限の対象外
2. あとから追加したテーブルだけが 15 個にカウントされる
3. 標準テーブルでもカスタムテーブルでも追加したら同じようにカウントされる

＜参考資料＞
- [Dynamics 365 Team Members license - Frequently asked questions (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/get-started/team-members-license#frequently-asked-questions)

<h2 id="q3">15 個を超えたテーブルのデータを参照する方法 (Q3)</h2>

【結論】サブグリッド、ダッシュボード、ルックアップの 3 つの方法を使えば、15 個の枠を消費せずに追加テーブルのデータを見ることができます。

公式ドキュメントには次のように記載されています。

> The Team Member app can only include up to 15 entities, but you can access additional entities through subgrids, dashboards, or lookups.

それぞれの方法について詳しく説明します。

### サブグリッド

すでにアプリに含まれているテーブル（たとえば取引先企業や連絡先）のフォーム上に、別のテーブルのデータを一覧として埋め込む方法です。
アプリにテーブルを直接追加する必要がないため、15 個の枠を使いません。

### ルックアップ

ルックアップ（参照）フィールドを通じて関連するテーブルのデータを参照する方法です。
こちらも 15 個の枠を使いません。

### ダッシュボード

ダッシュボードについては、種類によって動作が異なります。

| 種類 | 15 個の枠を使うか | 説明 |
|---|---|---|
| **個人用ダッシュボード** | 使わない | ユーザーが自分で作成するダッシュボード。アプリに登録されていないテーブルのデータも表示できます |
| **システムダッシュボード** | 使う | 管理者がアプリデザイナーから追加するダッシュボード。追加時に使用するテーブルが自動的にアプリに追加され、15 個の枠を消費します |

【補足】公式 FAQ で「ダッシュボード経由でアクセスできる」と書かれているのは、主に**個人用ダッシュボード**のことを指しています。
システムダッシュボードをアプリに追加すると、そのダッシュボードが使っているテーブルが自動的にアプリに追加されるため、15 個の枠を消費してしまいます。

### アプリにテーブルを追加する必要がある場合

以下の操作を行いたい場合は、テーブルをアプリに追加する必要があります（15 個の枠を消費します）。

| やりたいこと | アプリへの追加 | 15 個の枠 |
|---|---|---|
| サイトマップにテーブルの一覧ページを表示したい | 必要 | 消費する |
| アプリ内で表示するフォームを選びたい | 必要 | 消費する |
| サブグリッドでデータを表示したい | 不要 | 消費しない |
| 個人用ダッシュボードでデータを表示したい | 不要 | 消費しない |
| ルックアップで関連データを見たい | 不要 | 消費しない |

【よくある誤解】
- 誤解: システムダッシュボードを使えば 15 個制限を回避できる。
  - → 正しくは: システムダッシュボードは追加時に使用テーブルが自動追加されるため、15 個の枠を消費します。回避したい場合は個人用ダッシュボードをご利用ください。

＜参考資料＞
- [Dynamics 365 Team Members license - Frequently asked questions (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/get-started/team-members-license#frequently-asked-questions)
- [モデル駆動型アプリのコンポーネントの追加または編集 (Docs)](https://learn.microsoft.com/ja-jp/power-apps/maker/model-driven-apps/add-edit-app-components#add-a-component)
- [ダッシュボード、ダッシュボード コンポーネント、および FormXML について (Docs)](https://learn.microsoft.com/ja-jp/power-apps/developer/model-driven-apps/understand-dashboards-dashboard-components-formxml)

<h2 id="q4">登録・更新・削除が可能なテーブル数の上限について (Q4)</h2>

【結論】登録・更新・削除ができるテーブルの「数の上限」は定められていません。
ただし、登録・更新・削除が許可される操作は、ライセンスガイドでシナリオごとに決められています。

「15 個制限」はアプリに追加できるテーブル数の上限であり、登録・更新・削除の操作だけに適用される制限ではありません。
Team Members ライセンスでどの操作ができるかは、「〇個まで」という数ではなく、「どの機能・どのテーブルに対して何ができるか」がシナリオとして列挙されています。

### Sales Team Member の場合の主な操作範囲

| テーブル | できる操作 |
|---|---|
| 連絡先 (Contact) | 作成・更新・削除 |
| メモ (Note) | 作成・更新・削除 |
| 活動 (タスク、メール、電話、予定) | 作成・更新・削除 |
| 取引先企業 (Account) | **参照のみ** |
| 潜在顧客 (Lead)・営業案件 (Opportunity) | 参照のみ |

公式ドキュメントでは以下のように記載されています。

> Customer management: work with contacts or **see** accounts.

「work with contacts」は作成・更新・削除を含む操作が可能であることを意味し、「see accounts」は参照のみであることを意味しています。

### システムによる強制について

公式 FAQ には次のように記載されています。

> The license enforcement is focused on providing the Team Member experience and aligning usage to the Team Member apps. Specific privileges, such as updating an account or editing a case within the app module, aren't part of license enforcement.

つまり、登録・更新・削除の制限は**システムレベルでは強制されません**。
技術的にはできてしまう操作でも、ライセンスガイドの使用権を超える操作はライセンス違反となります。
準拠状況は Power Platform 管理センター (PPAC) の「Team Member conformance report」で確認できます。

【ポイント】
1. 「何個まで」という数の上限はない
2. 「何ができるか」がシナリオとして決められている
3. 制限はシステムで自動的にブロックされるものではなく、ライセンス契約上のルール
4. 準拠状況は管理センターのレポートで確認できる

＜参考資料＞
- [Sales Team Member app - Features available (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/sales/sales-team-member#features-available-in-the-sales-team-member-app)
- [Dynamics 365 Team Members license - Frequently asked questions (Docs)](https://learn.microsoft.com/ja-jp/dynamics365/get-started/team-members-license#frequently-asked-questions)
- [Dynamics 365 ライセンス ガイド (PDF / 英語)](https://go.microsoft.com/fwlink/?LinkId=866544&clcid=0x409)

<h2 id="q5">SharePoint へのファイルアップロードについて (Q5)</h2>

【結論】Team Members ライセンスのユーザーでも、SharePoint へのファイルアップロード（ドキュメント管理）は技術的に利用可能です。
また、対象テーブル（例: 取引先企業）に対する登録・更新・削除の権限は不要です。

### なぜ取引先企業への登録・更新・削除権限が不要か

Dynamics 365 の SharePoint ドキュメント管理は以下のような仕組みで動作します。

1. ファイルの実体は **SharePoint** 上に保存されます（Dataverse には保存されません）
2. Dynamics 365 側では「このレコードに紐づくファイルはここにある」という場所の情報（SharePointDocumentLocation）だけを管理します
3. ファイルをアップロードしても取引先企業レコード自体は変更されません

つまり、取引先企業の画面からファイルをアップロードする場合でも、必要なのは以下の権限です。

| 必要な権限 | 内容 |
|---|---|
| 取引先企業の**参照**権限 | 画面を開いてドキュメントタブを表示するため |
| SharePointDocumentLocation の作成・書き込み権限 | ファイルの場所情報を記録するため（Dataverse 側） |
| SharePoint サイトの適切な権限 | 実際のファイルアップロードのため（SharePoint 側） |

取引先企業に対する登録・更新・削除の権限がなくても（参照のみでも）、SharePoint へのファイルアップロードは可能です。

### 注意点

- SharePoint 側の権限は Dynamics 365 のライセンスとは別に管理されます（通常は Microsoft 365 ライセンスに含まれます）
- ドキュメント管理機能を使うには、管理者が事前に SharePoint 連携の設定と対象テーブルのドキュメント管理を有効化している必要があります

＜参考資料＞
- [ドキュメント管理タスクに必要なアクセス許可 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/permissions-required-document-management-tasks)
- [SharePoint Online を使用するように Dynamics 365 を設定する (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/set-up-dynamics-365-online-to-use-sharepoint-online)
- [SharePoint を使用した共同作業 (Docs)](https://learn.microsoft.com/ja-jp/power-apps/user/collaborate-using-sharepoint)

<h2 id="summary">まとめ</h2>

- Team Member アプリに追加できるテーブルは最大 15 個まで。この制限は参照目的でも適用されるが、もともとアプリに含まれているテーブルはカウントされない
- 15 個を超えるテーブルのデータを見たい場合は、サブグリッド・個人用ダッシュボード・ルックアップを活用する
- 登録・更新・削除は「テーブル数の上限」ではなく「ライセンスガイドに記載されたシナリオの範囲内」で許可される
- SharePoint へのファイルアップロードは Team Members ライセンスでも技術的に利用可能で、対象テーブルへの登録・更新・削除権限は不要
- ライセンスの準拠状況は Power Platform 管理センターの conformance report で確認できる

<h2 id="notice">注意事項（情報の更新可能性）</h2>

本記事の内容は執筆日時点の公開情報に基づいております。
ライセンスの使用権や制限事項は、ライセンスガイドの更新に伴い変更される可能性がございます。
最新の使用権については、[Dynamics 365 ライセンス ガイド (PDF / 英語)](https://go.microsoft.com/fwlink/?LinkId=866544&clcid=0x409) をご確認くださいますようお願い申し上げます。
