---
title: Dataverse for Teams 環境の自動作成と削除の仕組みまとめ
date: 2026-03-24
tags:
  - Dataverse for Teams
  - PowerPlatform
  - Teams
  - Governance
categories:
  - [PowerPlatform]
---

# Dataverse for Teams 環境の自動作成と削除の仕組みまとめ

こんにちは、Power Platformサポートチームの中下です。

本記事では Dataverse for Teams 環境の自動作成・削除の仕組みについて、以下の Q1〜Q6 の質問に回答する形でご説明させていただきます。

- [Q1: Dataverse for Teams 環境はどのような操作で自動的に作成されますか？](#q1)
- [Q2: 環境の自動作成を止める方法はありますか？](#q2)
- [Q3: 環境ごと・テナントごとの容量の上限はどうなっていますか？](#q3)
- [Q4: Dataverse for Teams 環境を手動で削除する方法を教えてください](#q4)
- [Q5: 使われていない環境は自動的に削除されますか？](#q5)
- [Q6: 削除された環境を元に戻すことはできますか？](#q6)

### この記事でわかること
- Dataverse for Teams 環境が自動で作られる 3 つのきっかけ
- 環境の自動作成を管理者が制御する方法
- 環境ごと 2 GB・テナント全体の容量の上限とその仕組み
- 手動で環境を削除する手順
- 使われていない環境が自動的に無効化・削除されるまでの流れ
- 削除された環境を 7 日以内に復元する方法

## 目次
- [Dataverse for Teams 環境が自動作成されるタイミング (Q1)](#q1)
- [環境の自動作成を止める方法 (Q2)](#q2)
- [環境ごと・テナントごとの容量の上限 (Q3)](#q3)
- [手動で環境を削除する方法 (Q4)](#q4)
- [使われていない環境の自動削除 (Q5)](#q5)
- [削除された環境の復元 (Q6)](#q6)
- [まとめ](#summary)
- [注意事項（情報の更新可能性）](#notice)

<h2 id="q1">Dataverse for Teams 環境が自動作成されるタイミング (Q1)</h2>

【結論】Teams 上で「アプリを作る」「エージェントを作る」「アプリをインストールする」のいずれかの操作を初めて行うと、そのチームに対して Dataverse for Teams 環境が自動的に作られます。

具体的には、以下の 3 つのきっかけで環境が作られます。

1. **Power Apps を使ってアプリを作成する**：Teams 内で Power Apps を使ってはじめてアプリを作ると、そのチーム専用の環境が自動で作られます。
2. **Copilot Studio（旧 Power Virtual Agents）でエージェントを作成する**：同様に、Teams 内でエージェント（チャットボット）をはじめて作成したタイミングでも環境が作られます。
3. **アプリ カタログからアプリをインストールする**：Power Apps で作成されたアプリを Teams のアプリ カタログからチームにはじめてインストールした場合にも作られます。これには「Employee Ideas」「Inspection」「Issue Reporting」などのサンプル アプリも含まれます。

1 つのチームに対して作られる環境は 1 つだけです。
すでに環境があるチームで 2 つ目のアプリを作っても、新しい環境は作られません。

また、環境の作成は Power Platform 管理センターからは行えず、Teams の中からのみ可能です。
作成された環境は Power Platform 管理センターの環境一覧で、種類が「Microsoft Teams」と表示されます。

【ポイント】
1. Power Apps でアプリを初めて作ると環境が作られる
2. Copilot Studio でエージェントを初めて作っても環境が作られる
3. アプリ カタログからアプリを初めてインストールしても環境が作られる
4. 1 つのチームに対して環境は 1 つだけ
5. Power Platform 管理センターからは環境を作れない

＜参考資料＞
- [Teams 環境の Microsoft Dataverse について (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/about-teams-environment)
- [Power Platform ライセンス FAQ - Dataverse (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#dataverse)

<h2 id="q2">環境の自動作成を止める方法 (Q2)</h2>

【結論】Teams 管理センターのアプリのアクセス許可ポリシーを使い、環境作成のきっかけとなるアプリをブロックすることで制御できます。

環境が自動で作られるのを完全に止めるには、環境作成のきっかけとなるすべてのアプリを制限する必要があります。
具体的には、以下のアプリを Teams 管理センターでブロックします。

- **Power Apps**：アプリ作成を制限します
- **Copilot Studio（旧 Power Virtual Agents）**：エージェント作成を制限します
- **Employee Ideas / Inspection / Issue Reporting**：サンプル アプリからの環境作成を制限します

【よくある誤解】
- 誤解: Power Apps と Copilot Studio をブロックすれば、環境は作られなくなる。
  - → 正しくは: サンプル アプリ（Employee Ideas、Inspection、Issue Reporting）をチームに追加しても環境は作られます。これらも合わせてブロックする必要があります。

設定の手順は以下の通りです。

1. **Microsoft Teams 管理センター** にサインインします。
2. 左側のメニューから [Teams のアプリ] > [アクセス許可ポリシー] を選びます。
3. 対象のポリシーを選択または新規作成します。
4. 「Microsoft アプリ」のセクションで「Power Apps」「Copilot Studio」「Employee Ideas」「Inspection」「Issue Reporting」をブロック対象に追加します。
5. ポリシーを対象のユーザーまたはグループに割り当てます。

なお、Power Apps をブロックすると、ユーザーは Power Apps で作成されたアプリにもアクセスできなくなります。
既存のアプリを引き続き使わせたい場合は、「同僚による構築（Built by your colleagues）」カタログからアプリを Teams チャネルにピン留めすることで対応できます。

＜参考資料＞
- [Teams 環境の Microsoft Dataverse について - Dataverse for Teams を管理する機能 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/about-teams-environment#ability-to-govern-dataverse-for-teams)
- [Microsoft Teams でアプリのアクセス許可ポリシーを管理する (Docs)](https://learn.microsoft.com/ja-jp/microsoftteams/teams-app-permission-policies)

<h2 id="q3">環境ごと・テナントごとの容量の上限 (Q3)</h2>

【結論】環境ごとにデータベースとファイルを合わせて 2 GB、テナント全体では「5 つ + Microsoft 365 ライセンス 20 個につき 1 つ」が環境数の上限です。

Dataverse for Teams 環境の容量は、通常の Dataverse とは別のしくみで管理されています。
テナント全体の Dataverse の容量とは別に、Dataverse for Teams 専用の容量が用意されています。
この 2 つの容量を移し替えることはできません。

**環境ごとの上限**

| 項目 | 上限 |
|---|---|
| データベース + ファイルの合計容量 | 2 GB |
| 保存できるレコード数の目安 | 約 1,000,000 件 |

容量の 80% に達すると、Teams 内のアプリ作成画面で警告が表示されます。
100% に達すると、既存のアプリは引き続き動きますが、新しいアプリやフローの作成・インストールはできなくなります。

**テナント全体の上限**

| 項目 | 上限 |
|---|---|
| 環境の数 | 5 つ + 対象の Microsoft 365 ライセンス 20 個ごとに 1 つ追加 |
| テナント全体の合計容量 | 環境数 × 2 GB（最大 19.5 TB） |

テナント全体の環境数が上限の 100% に達すると、新しい Dataverse for Teams 環境の作成がブロックされます。

容量を確認するには、Power Platform 管理センターの [ライセンス] > [キャパシティ アドオン] > [Microsoft Teams] タブから確認できます。

【ポイント】
1. テナントの Dataverse 容量とは別に管理されている
2. 環境ごとの上限は 2 GB で、増やすことはできない
3. テナントの環境数は Microsoft 365 ライセンスの数に応じて増える
4. 上限を増やしたい場合は、不要な環境を削除するか Dataverse にアップグレードする

＜参考資料＞
- [Teams 環境の Microsoft Dataverse について - キャパシティ制限 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/about-teams-environment#capacity-limits)
- [Power Platform ライセンス FAQ - Dataverse (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#dataverse)

<h2 id="q4">手動で環境を削除する方法 (Q4)</h2>

【結論】Power Platform 管理センターから削除する方法と、紐づいている Teams のチームを削除する方法の 2 通りがあります。

**方法 1: Power Platform 管理センターから削除する**

1. **Power Platform 管理センター** にサインインします。
2. 左側のメニューから [環境] を選びます。
3. 種類が「Microsoft Teams」と表示されている環境を選択します。
4. 上部のコマンドバーで [削除] を選びます。
5. 環境名を入力して [確認] を選びます。

**方法 2: Teams のチームを削除する**

Dataverse for Teams 環境は、紐づいている Teams のチームと連動しています。
チームを削除すると、そのチームの Dataverse for Teams 環境も自動的に削除されます。

【よくある誤解】
- 誤解: チームを削除しても環境だけは残る。
  - → 正しくは: チームを削除すると環境も自動的に削除されます。ただし、環境を Dataverse にアップグレード済みの場合は、チームを削除しても環境は残ります。

【補足】環境を削除しても、Teams のチーム自体やチャネル、SharePoint サイトなどの他の Teams の機能には影響しません。
削除されるのは Dataverse for Teams のデータベース（アプリ、フロー、エージェント、データ）のみです。

＜参考資料＞
- [Teams 環境の Microsoft Dataverse について - 環境のライフサイクル (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/about-teams-environment#environment-lifecycle)

<h2 id="q5">使われていない環境の自動削除 (Q5)</h2>

【結論】90 日間使われていない Dataverse for Teams 環境は自動的に無効になり、そのまま 30 日間放置すると自動的に削除されます。

Power Platform には、使われていない Dataverse for Teams 環境を自動でクリーンアップする仕組みがあります。
管理者が特に何もしなくても、不要な環境は自動的に片づけられます。

具体的なスケジュールは以下の通りです。

| 経過日数（操作がない期間） | 何が起きるか |
|---|---|
| 83 日 | 管理者に「まもなく無効になります」という警告メールが届く |
| 87 日 | 2 回目の警告メールが届く |
| 90 日 | **環境が自動的に無効になる**（アプリが動かなくなる） |
| 113 日 | 管理者に「まもなく削除されます」という警告メールが届く |
| 117 日 | 2 回目の削除警告メールが届く |
| 120 日 | **環境が自動的に削除される** |

ここでいう「操作」とは、以下のようなものを指します。

- **利用者の操作**：アプリを開く、フローが実行される（自動実行も含む）、エージェントとチャットする
- **作成者の操作**：アプリやフロー、エージェントを作る・変える・消す
- **管理者の操作**：バックアップや復元などの環境操作

なお、環境が無効になると、その環境のアプリは開けなくなり、フローは止まり、エージェントも動かなくなります。
ただし、Teams のチームやチャネル、SharePoint サイトなどには影響しません。

警告メールは環境のシステム管理者（＝チームの所有者）と環境の作成者に届きます。

＜参考資料＞
- [非アクティブな Dataverse for Teams 環境の自動削除 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/inactive-teams-environment)

<h2 id="q6">削除された環境の復元 (Q6)</h2>

【結論】自動削除・手動削除いずれの場合も、削除されてから 7 日以内であれば Power Platform 管理センターから復元できます。

復元の手順は以下の通りです。

1. **Power Platform 管理センター** にサインインします。
2. 左側のメニューから [環境] を選びます。
3. 環境の一覧ページで、コマンドバーにある [削除された環境の復元] を選びます。
4. 復元したい環境を選択します。
5. [続行] を選んで復元を確定します。

また、自動削除の場合は、削除される前の「無効」状態（90 日〜120 日の間）であれば、管理者が環境を再び有効にすることで削除を防げます。

**無効になった環境を再度有効にする手順**

1. Power Platform 管理センターにサインインします。
2. [環境] の一覧ページで、無効になった環境を選びます。
3. 「詳細」ペインの [編集] を選びます。
4. 「管理モード」の設定を「有効」に切り替えます。
5. [保存] を選んで変更を反映します。

【ポイント】
1. 削除後 7 日以内であれば復元が可能
2. 自動削除の場合、無効の状態（90 日〜120 日）なら再有効化で復活できる
3. 7 日を過ぎると完全に削除され、復元はできない

＜参考資料＞
- [非アクティブな Dataverse for Teams 環境の自動削除 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/inactive-teams-environment)
- [環境の復元 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/recover-environment)

<h2 id="summary">まとめ</h2>

- Dataverse for Teams 環境は、Teams 内でアプリ・エージェントの作成やアプリのインストールを行うと自動的に作られます。管理センターからは作成できません。
- 環境作成を止めたい場合は、Teams 管理センターで Power Apps、Copilot Studio、サンプル アプリをブロックします。
- 環境ごとに 2 GB の容量上限があり、テナント全体の環境数にも上限があります。
- 90 日間使われていない環境は自動で無効化され、さらに 30 日放置すると自動で削除されます。
- 削除された環境は 7 日以内であれば復元できます。

<h2 id="notice">注意事項（情報の更新可能性）</h2>

本記事の内容は執筆日時点の公開情報に基づいております。
仕様や UI、制限事項は将来変更される可能性がございます。
最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
