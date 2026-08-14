---
【Standard Harness】Copilot Studio エージェント単位の Copilot クレジット消費量を予測・管理する方法
date: 2026-07-15
categories: Microsoft Copilot Studio
tags:
  - Microsoft Copilot Studio
  - License
  - Power Platform
  - How to
---

こんにちは、Power Platform サポートチームの中村です。

本記事では、Microsoft Copilot Studio において**エージェント単位**で Copilot クレジットの消費量を予測し、実績管理するために利用できる機能をまとめてご紹介します。

> [!IMPORTANT]
> 本記事は **Copilot Studio の Standard Harness** を使用した場合を前提としています。GitHub Copilot Harness では、テスト実行時の課金動作やエージェントの分析画面が異なる場合があります。
>
> 本記事で紹介するツールや機能は、クレジット消費量の**目安を把握するための参考情報**です。特定のシナリオにおけるクレジット消費量を保証するものではなく、本記事の内容をもって「このエージェント構成ではこれだけ消費される」と確定させることはできません。
> 
> また、「特定のシナリオでのクレジット消費がライセンス上正しいか」「自社の利用形態に必要なライセンス数」等、**使用権・ライセンス要件に関するご質問は製品サポートでのご支援の対象外**となっております。このようなご確認が必要な場合は、貴社ご担当の **Microsoft 営業担当者**または**販売パートナー**へお問い合わせください。

<!-- more -->

### この記事でわかること

- Copilot クレジットの概要
- 開発段階でエージェントのクレジット消費を予測するために利用できる機能
- 本番リリース後にエージェント単位でクレジット消費を監視・制御するために利用できる機能
- 各機能の使い分けと予実管理の全体像

## 目次

- [Copilot クレジットとは](#copilot-クレジットとは)
- [開発段階: クレジット消費の予測に利用できる機能](#開発段階-クレジット消費の予測に利用できる機能)
  - [エージェント使用量見積もりツール (Agent Usage Estimator)](#エージェント使用量見積もりツール-agent-usage-estimator)
  - [Copilot Studio の 監視 タブ (Analytics - Billing)](#copilot-studio-の-監視-タブ-analytics---billing)
  - [テスト実行時のクレジット消費について](#テスト実行時のクレジット消費について)
- [本番リリース後: クレジット消費の実績管理に利用できる機能](#本番リリース後-クレジット消費の実績管理に利用できる機能)
  - [Power Platform 管理センター - Copilot Studio ライセンス画面](#power-platform-管理センター---copilot-studio-ライセンス画面)
  - [Azure Cost Management (Pay-as-you-go 利用時)](#azure-cost-management-pay-as-you-go-利用時)
  - [Copilot Studio の 監視 タブ (エージェント個別)](#copilot-studio-の-監視-タブ-エージェント個別)
- [まとめ: 予実管理の全体像](#まとめ-予実管理の全体像)
- [使用権に関するご質問について](#使用権に関するご質問について)
- [注意事項（情報の更新可能性）](#注意事項情報の更新可能性)

## Copilot クレジットとは

2025 年 9 月 1 日より、Copilot Studio のエージェント消費の共通通貨は「メッセージ」から **Copilot クレジット** に変更されました。Copilot クレジットは、エージェントが情報を取得し、プロンプトに応答し、アクションやスキルを実行するために必要な時間と労力を測定する単位です。消費されるクレジット数は、エージェントの設計、ユーザーとのやり取りの頻度、使用する機能によって異なります。

＜参考資料＞

- [Copilot Studio licensing - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)
- [Billing rates and management - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/requirements-messages-management)

## 開発段階: クレジット消費の予測に利用できる機能

### エージェント使用量見積もりツール (Agent Usage Estimator)

[Microsoft エージェント使用量見積もりツール](https://microsoft.github.io/copilot-studio-estimator/) は、組織が月ごとの Copilot クレジット消費量を事前に予測するための公開ツールです。

主な機能は以下のとおりです。

- エージェントの種類（Copilot Studio カスタム、Dynamics 365 Sales / Service / Finance / Supply Chain Management等）を選択して見積もりを作成できます
- トラフィック量、オーケストレーション方法、ナレッジソース、ツール / アクションの構成に基づいてクレジット消費量を算出します
- 低・中・高のボリュームシナリオをテストできます
- 複数エージェントの合算コストを算出できます
- PDF レポートを生成し、調達承認やステークホルダーレビューに利用できます

利用例としては、以下のような場面が挙げられます。

- 予算計画のための月間クレジット消費量の予測
- 異なるエージェント構成間のコスト比較
- 機能の利用がクレジット消費に与える影響の把握

> [!NOTE]
> このツールは **見積もり目的のみ** であり、最終コストを保証するものではありません。実際の消費量は実運用パターンに依存するため、見積もり値に対して 10〜20% のバッファーを加えることがお勧めされています。

また、本ツールが**適さない**ユースケースは以下のとおりです。

- 確定的な価格見積もりや契約上のコミットメントの作成
- 特定シナリオにおけるクレジット消費量の保証
- 過去の実績使用量のトラッキング（本ツールは将来予測のみ提供）

＜参考資料＞

- [Agent usage estimator - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/agent-usage-estimator)

### Copilot Studio の [監視] タブ (Analytics - Billing)

Copilot Studio のエージェント個別画面にある [監視] タブでは、エージェントごとの Copilot クレジット消費状況を確認できます。

本番リリース前であっても、パイロット運用（テスト チャット以外のチャネルで実際にエージェントを公開し、限定的にユーザーに利用させる運用）を行っている場合は、このタブで実消費データを確認できます。Agent Usage Estimator の予測値と比較することで、本番リリース前に予測精度を高めることが可能です。

確認できる情報は以下のとおりです。

- 選択した期間における合計課金クレジット数
- **請求の傾向 (Billing trend)**: 期間中のクレジット消費量の推移をチャートで表示します
- **コスト配分 (Cost distribution)**: どの種類のアクティビティ（ナレッジ、アクション、フローなど）がクレジット消費に寄与しているかを表示します

> [!NOTE]
> 分析データの更新には数時間かかるため、直近のアクティビティがすぐに反映されない場合があります。

＜参考資料＞

- [View agent's billing consumption - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/analytics-consumption)

### テスト実行時のクレジット消費について

開発・テスト段階での注意点として、**組み込みテスト チャットでのメッセージは Copilot クレジットの課金対象にカウントされません**。フロー デザイナーやエージェントのテスト チャットからのテスト実行も同様に課金対象外です。そのため、開発段階ではテストを気にせず実施し、Agent Usage Estimator で予測した値と本番リリース後の実績値を比較する運用がお勧めです。

＜参考資料＞

- [FAQ for Copilot Studio billing and licensing - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/faq-billing-licensing)
- [Agent flows overview - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/flows-overview#manage-agent-flow-capacity-usage)
- [Billing rates and management - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/requirements-messages-management#overage-enforcement)

## 本番リリース後: クレジット消費の「実績管理」に利用できる機能

### Power Platform 管理センター - Copilot Studio ライセンス画面

Power Platform 管理センター ([https://admin.powerplatform.microsoft.com](https://admin.powerplatform.microsoft.com)) の [ライセンス] > [Copilot Studio] から、以下などの情報を確認・管理できます。

#### [概要 (Summary)] タブ

| カード | 内容 |
|---|---|
| 従量課金制 Copilot クレジット | アクティブな請求プランの数と月初来のCopilotクレジット合計 |
| プリペイドキャパシティ | 購入済み容量の割り当て・消費状況 |
| 製品ごとの容量消費量 | プリペイド / 従量課金制ごとの消費内訳 |
| 容量の消費を合計した傾向 | 月ごとの容量消費トレンド（前払い / 従量課金制 / 日ごと使用状況） |
| 上位 5 個 (人) のエージェントとユーザー | クレジット使用量別の上位エージェントおよびユーザー |
| 環境別の Copilot クレジットの使用状況 | 上位 5 件の環境のクレジット使用量 |

#### [環境 (Environments)] タブ

環境ごとの詳細な消費状況を確認できます。

- プリペイド容量から差し引かれたクレジット数
- Pay-as-you-go プランで課金されたユニット数
- 環境のステータス（容量内 / 超過 / 容量割り当て済み / Pay-as-you-go 使用中）
- **エージェント別の消費明細** (メッセージ消費の詳細 グリッド): エージェントごとの消費明細を確認できます

＜参考資料＞

- [Manage Copilot Studio credits and capacity - Power Platform | Microsoft Learn](https://learn.microsoft.com/power-platform/admin/manage-copilot-studio-messages-capacity)

### Azure Cost Management (Pay-as-you-go 利用時)

Pay-as-you-go プランを利用している場合、Azure ポータルの Azure Cost Management からも課金情報を確認できます。

- メーターごと、Azure リソースごとに課金額を表示します
- 各課金プランは Power Platform アカウントリソースに対応しています
- 使用状況は通常 24 時間以内に反映されます

＜参考資料＞

- [View usage and billing information - Power Platform | Microsoft Learn](https://learn.microsoft.com/power-platform/admin/pay-as-you-go-usage-costs)

### Copilot Studio の [監視] タブ (エージェント個別)

前述のとおり、本番リリース後も Copilot Studio 内の各エージェントの [監視] タブから、そのエージェント固有の課金クレジット情報、トレンド、コスト配分を確認できます。[請求を表示する] ボタンをクリックすると、請求の傾向やコスト配分の詳細パネルが開きます。

＜参考資料＞

- [View agent's billing consumption - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/microsoft-copilot-studio/analytics-consumption)

## まとめ: 予実管理の全体像

| フェーズ | 機能 | 主な用途 |
|---|---|---|
| 開発 (予測) | Agent Usage Estimator | 月間消費量の見積もり、予算計画、複数エージェントの合算コスト予測 |
| 開発 (予測) | Copilot Studio [監視] タブ | パイロット運用中の実消費データによる予測精度の向上 |
| 本番 (実績) | PPAC [Licensing] > [Copilot Studio] | テナント / 環境レベルの消費監視、容量割り当て管理 |
| 本番 (実績) | Azure Cost Management | Pay-as-you-go 利用時の詳細課金分析 |
| 本番 (実績) | Copilot Studio [監視] タブ | エージェント個別の課金トレンド、コスト分布 |

## 使用権に関するご質問について

> [!NOTE]
> 本記事は Copilot クレジットの消費量を予測・管理する技術的な方法をご紹介しています。
> 「特定のシナリオでのクレジット消費がライセンス上正しいか」等、**使用権に関するご確認は製品サポートの対応範囲外**となります。

- クレジットおよびライセンスの公式情報: [Copilot Studio のライセンスと課金](https://learn.microsoft.com/microsoft-copilot-studio/billing-licensing)
- 使用権・契約に関する個別のご確認: 貴社ご担当の **Microsoft 営業担当者**、または**販売パートナー**へお問い合わせください。
- サポートへのお問い合わせ時の留意事項: [Power Apps/Power Automate/Copilot Studio お問い合わせ時の留意事項 - ライセンスに関するお問い合わせについて](https://jpdynamicscrm.github.io/blog/powerplatform/about-power-platform-support/#%E3%83%A9%E3%82%A4%E3%82%BB%E3%83%B3%E3%82%B9%E3%81%AB%E9%96%A2%E3%81%99%E3%82%8B%E3%81%8A%E5%95%8F%E3%81%84%E5%90%88%E3%82%8F%E3%81%9B%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)

