---
title: Power Pages ライセンス体系とユーザカウント・超過時対応・利用状況確認ガイド
date: 2025-09-08 10:00:00 +0900
tags:
	- powerpages
	- licensing
    
categories:
	- [powerplatform]
---

# Power Pages ライセンス体系について

こんにちは、PowerPlatformサポートチームの三田でございます。

本記事ではPower Pages ライセンス体系について、以下の Q1〜Q6 の質問に回答する形でご説明させていただきます。

- [Q1: Power Pages のライセンスは Power Apps / Power Automate と異なりますか？](#power-pages-ライセンスの位置付けとモデル-q1-と-q2-統合)
- [Q2: Power Pages のライセンスの仕組みは？](#power-pages-ライセンスの位置付けとモデル-q1-と-q2-統合)
- [Q3: 購入したライセンスより多くのユーザがアクセスした場合はどうなりますか？](#ライセンスキャパシティ超過時の挙動と対応-q3)
- [Q4: 認証ユーザのカウントの仕方は？](#認証ユーザカウントと内部ライセンス保持者の扱い-q4-と-q5-統合)
- [Q5: 既に内部ユーザが Power Pages 利用権を含むライセンスを持っている場合カウントされますか？](#認証ユーザカウントと内部ライセンス保持者の扱い-q4-と-q5-統合)
- [Q6: 匿名ユーザのカウントの仕方は？](#匿名ユーザのカウント方式-q6)
<!-- Q7 (利用状況の確認方法) は監視運用詳細除外方針により本稿では扱わない -->

### この記事でわかること
- Power Pages ライセンスモデル（認証／匿名）と他 Power Platform 製品との位置付け
- 認証ユーザ（Authenticated）と匿名ユーザ（Anonymous）のユニークカウント方法
- 既存 Power Apps / Dynamics 365 ライセンス保持内部ユーザのカウント免除条件
- 月間キャパシティ超過時の挙動・リスクと対処オプション
- 環境割当（最小単位）とキャパシティ管理ベストプラクティス
- 誤解しやすいポイントと運用上の注意事項

<!-- 概要簡略ブロックは仕様方針により削除 -->

## 目次
- [ライセンス早見表](#ライセンス早見表)
- [Power Pages ライセンスの位置付けとモデル (Q1 と Q2 統合)](#power-pages-ライセンスの位置付けとモデル-q1-と-q2-統合)
- [認証ユーザカウントと内部ライセンス保持者の扱い (Q4 と Q5 統合)](#認証ユーザカウントと内部ライセンス保持者の扱い-q4-と-q5-統合)
- [匿名ユーザのカウント方式 (Q6)](#匿名ユーザのカウント方式-q6)
- [ライセンス（キャパシティ）超過時の挙動と対応 (Q3)](#ライセンスキャパシティ超過時の挙動と対応-q3)
- [まとめ](#まとめ)
- [注意事項（情報の更新可能性）](#注意事項情報の更新可能性)

## ライセンス早見表
| 項目 | 認証 (Authenticated) | 匿名 (Anonymous) | 備考 |
|------|----------------------|------------------|------|
| 計測単位 | ユニーク Contact ID / 月 | ユニーク Cookie ID / 月 | 同月複数回は 1 |
| 1 パック容量 | 100 ユーザ | 500 ユーザ | 全ティア共通 |
| 環境最小割当 | 25 | 200 | 割当後は 1 ずつ増減可 (匿名は初回 200) |
| 免除条件 | 内部ユーザが適格 Power Apps / Dynamics 365 | ― | 免除時は認証カウント不消費 |
| PAYG 対応 | あり (ユニーク実績) | あり | 予測困難時に有効 |
| カウントキー | Contact レコード ID | Cookie の匿名ユーザ ID | Cookie クリアで再カウント可能性 |
| 旧 Portals との差 | ログイン数課金→MAU | ページビュー課金→MAU | 移行期間後新 SKU |

注:
- MAU: “Monthly Active Users” の略で、当該月に 1 回以上アクセスしたユニーク利用者（認証=Contact、匿名=Cookie ID）を 1 と数える方式。
- PAYG: “Pay-As-You-Go” の略で、事前購入パックに依存せず当月実績ユニーク利用数を Azure 課金で従量精算する方式（安定利用はパックの方が割安となる場合がございます）。

<!-- 用語詳細セクションは簡略方針により削除。 MAU / PAYG のみ早見表直下で定義。 -->

主な判断早見:
- “予測しやすい & 安定利用” → サブスクリプション割当
- “初期トライアル / 季節変動大” → PAYG 併用で過剰購入回避
- “内部利用主体” → 先に社内ライセンス保有状況を棚卸して免除最大化

＜参考資料＞
- [Power Platform licensing FAQs (Power Pages)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)

## Power Pages ライセンスの位置付けとモデル (Q1 と Q2 統合)
【結論】Power Pages は認証ユーザと匿名ユーザの二系統に分かれるユニーク月間アクティブユーザ (MAU: 当該月に少なくとも 1 回アクセスしたユニーク利用者) ベースの課金モデルでございます。旧来のログイン数およびページビュー課金モデルは新ライセンス体系へ移行しており、適格な Power Apps もしくは Dynamics 365 ライセンスを保有する内部ユーザは認証キャパシティを消費せずに利用できるため、総コスト最適化が可能でございます。

【ポイント】
1. 2 メーター: Authenticated / Anonymous
2. パック: 100 / 500 ユーザ（共通値）
3. カウント: 月内ユニーク（月内再訪は基本カウントしない）
4. 内部既存ライセンス = 免除余地
5. 追加 Dataverse 容量不要（旧との変更点）

【補足】本モデルでは各環境に対し認証 25 ユニットおよび匿名 200 ユニットの最小割当が適用されております。PAYG は “不足した分を自動で上乗せ購入する” 仕組みではなく、明示的に有効化された環境に対して当月確定したユニーク利用実績を Azure 課金に計上する従量方式でございます。初期は PAYG で実績傾向を把握し、利用が安定した段階で必要最小限のパックへ移行する二段階アプローチが経済的でございます。旧 SKU からの移行時には割当や付随ストレージの条件に差異が生じる場合がございますが、追加 1GB ストレージ要件は現行モデルでは撤廃されております。

【よくある誤解】
- 誤解: アクセス回数（ページビュー）に比例して課金される。
	- → 正しくは: 月内に 1 回以上利用したユニーク利用者（認証は Contact、匿名は Cookie ID）が 1 カウントされる MAU ベースでございます。
- 誤解: すべての内部ユーザは常に認証キャパシティを消費しない。
	- → 正しくは: 適格な Power Apps もしくは Dynamics 365 ライセンスを保持し要件を満たす内部ユーザのみ免除対象となり、条件外の内部ユーザは消費いたします。
- 誤解: ログインページだけを公開している場合でも匿名キャパシティが消費される。
	- → 正しくは: 公開範囲がログインページのみであれば匿名キャパシティはカウントされません。

＜参考資料＞
- [Power Platform licensing FAQs (Power Pages)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform licensing FAQs (Capacity packs 明細)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform licensing FAQs (差異比較表)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)

## 認証ユーザカウントと内部ライセンス保持者の扱い (Q4 と Q5 統合)
【結論】認証ユーザ数は当月にログインした Dataverse の Contact レコード ID を用いてユニーク集計されます。内部ユーザが適格な Power Apps（Per User / Per App）または Dynamics 365 Enterprise ライセンスを保持し要件を満たす場合、そのユーザは認証キャパシティの消費対象外となりコスト抑制につながります。

【ポイント】
1. ユニークキー: Contact ID
2. パック単位: 100 認証 MAU
3. 最小割当: 環境あたり 25
4. 免除条件: 適格 Power Apps / Dynamics 365 内部ユーザ
5. PAYG: 実績ベース従量課金の併用可

【補足】PAYG は当月の実績に基づく従量課金であり、サイト単位の需要変動に柔軟に追随できる仕組みでございます。認証ユーザは複数ブラウザや複数デバイスで利用しても同一 Contact レコードであれば追加でカウントされることはございません。

【運用ベストプラクティス】
- 内部ユーザのライセンス保持状況を定期照合し、不要な認証キャパシティ割当前提で見積もらない。
- Contact 重複（重複マージ未対応）により過剰カウントされないよう重複検出ルールを設定。

＜参考資料＞
- [Power Platform licensing FAQs (Authenticated 定義)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform licensing FAQs (内部ユーザ利用権一覧)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform licensing FAQs (最小割当例)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)

## 匿名ユーザのカウント方式 (Q6)
【結論】匿名ユーザ数はブラウザに保存される匿名 Cookie ID をキーとして当月内ユニークで集計され、同一ブラウザでの再訪は追加カウントされません。公開範囲がログインページのみの場合は匿名キャパシティの消費対象外でございます。

【ポイント】
1. ユニークキー: Cookie ID
2. パック単位: 500 匿名 MAU
3. 最小割当: 環境あたり 200
4. 免除条件: 公開がログインページのみの場合は消費なし
5. 再カウント要因: Cookie クリア / 別ブラウザ利用

【留意点】
1. 広報キャンペーン等で初回訪問が急増すると月初に短期間で大半の匿名キャパシティが消費され得る。
2. Cookie バナー / 同意管理実装で Cookie 抑止された場合、計測上追加ユニーク扱いとなる可能性をモニタリング。

【リスク軽減】
- キャパシティ残余トレンドを週次で確認し閾値（例: 割当前 80% 到達）時に追加購入 / PAYG 検討。

＜参考資料＞
- [Power Platform licensing FAQs (Anonymous 定義)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform licensing FAQs (匿名最小割当)](https://learn.microsoft.com/en-us/power-platform/admin/powerapps-flow-licensing-faq#power-pages)

## ライセンス（キャパシティ）超過時の挙動と対応 (Q3)
【結論】キャパシティは月初にリセットされ日次累積で推移いたします。超過が見込まれる際は (1) 環境間再配分、(2) 追加購入または PAYG の活用、(3) 利用抑制と最適化策の適用という段階的アプローチが推奨でございます。消費状況は Capacity Management ビューおよび通知カードで最大 24 時間の遅延を伴って可視化されます。

【ポイント】
1. リセット: 月初に使用量リセット
2. 計上: Billed Quantity は月内初回ユニークのみ
3. 再配分: 環境間で割当調整可
4. 対応順: 再配分→追加購入/PAYG→需要平準化
5. 可視化遅延: 反映最大約 24 時間

【推奨アプローチ（概要）】超過予兆がある場合は (1) 未使用割当の再配分、(2) 追加パック購入または PAYG 活用、(3) 中長期的な需要平準化（認証導線最適化や不要ページ整理）の順で段階的に対応することが有効でございます。

【よくある誤解】
- 誤解: 超過した場合、サイト利用が停止される。
	- → 正しくは: 超過した場合でも、サイト利用が停止されることはございません。
- 誤解: 超過した分は即座に自動で従量課金（追加請求）される。
  - → 正しくは: サブスクリプション割当モデルでは超過分が即自動課金されるわけではなく、管理者による追加パック購入 / 環境間再配分 / PAYG 有効化などの能動的対応が必要でございます。PAYG を有効化している場合に限り、その月の実績に基づき従量課金が発生いたします。

＜参考資料＞
- [Manage and monitor capacity](https://learn.microsoft.com/en-us/power-pages/admin/capacity-management)

<!-- Q7 セクション削除: 監視/運用手順は本稿範囲外 -->

## まとめ
- Power Pages は認証 / 匿名 2 系列の “ユニーク MAU（当該月 1 回以上アクセスした利用者）” キャパシティモデルで他 Power Platform 製品と課金軸が異なる。
- 認証 MAU は Contact ID、匿名 MAU は Cookie ベース ID で月内ユニーク計測。
- 内部ユーザが Power Apps / Dynamics 365 適格ライセンスを保持する場合、認証キャパシティを消費しない条件がある。
- 超過リスクは割当再配分・追加購入・PAYG の段階的活用で吸収する。
 

本記事が Power Pages のライセンス新規ご検討中の皆様ならびに既にご利用中で追加購入や最適な割当見直しを検討されている皆様の判断と社内説明の一助となりましたら幸いです。


## 注意事項（情報の更新可能性）
本記事の内容は執筆日時点の公開情報に基づいております。仕様や価格、最小割当、移行ポリシー等は将来変更される可能性がございます。必ず最新の公式ドキュメントをご確認くださいますようお願い申し上げます。
