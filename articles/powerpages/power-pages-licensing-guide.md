---
title: Power Pages ライセンスについて
date: 2025-09-16 10:30:00 +0900
tags:
	- powerpages
	- licensing
	- governance
categories:
	- [Power Pages]
---

# Power Pages ライセンスについて

こんにちは、PowerPlatformサポートチームの三田です。

本記事では Power Pages のライセンスについて以下の Q1〜Q6 の質問に回答する形で整理します。

- [Q1: Power Pages の基本的なライセンスモデル（何をどの単位で購入 / 課金するか）は？](#q1)
- [Q2: 認証ユーザー (Authenticated) と匿名ユーザー (Anonymous) のカウント方式の違いは？](#q2)
- [Q3: 月中で想定より利用が増えた（超過しそう / した）場合はどう判断・対応する？](#q3)
- [Q4: 認証ユーザー数の「ユニーク」判定はどのように行われる？（複数回ログイン時の扱い）](#q4)
- [Q5: 既に Power Apps / Dynamics 365 ライセンスを持つ社内（内部）ユーザーは別途カウント / 課金が必要か？](#q5)
- [Q6: 匿名ユーザー数はどのように一意判定され異なるデバイスやCookie削除時はどう扱われるか？](#q6)

### この記事でわかること
- 現行 Power Pages の容量（Capacity）ベース課金モデルの全体像
- 認証 / 匿名ユーザーの「月次ユニーク」測定基準と非カウント条件
- 既存 Power Apps / Dynamics 365 ライセンス保有ユーザーの扱い（重複課金回避）
- 超過（Overage）したときの対処法
- よくある誤解（旧ポータル Login/Page View モデルとの混同 など）の整理

## 目次
- [Power Pages の基本ライセンスモデル (Q1)](#q1)
- [認証ユーザーと匿名ユーザーのカウント方式 (Q2)](#q2)
- [利用が超過しそうまたは超過した場合の対応 (Q3)](#q3)
- [認証ユーザーのユニーク判定（重複ログインの扱い） (Q4)](#q4)
- [内部ユーザー既存ライセンスとカウント除外 (Q5)](#q5)
- [匿名ユーザーの一意判定とブラウザ差異/Cookie削除の影響 (Q6)](#q6)
- [まとめ](#summary)
- [注意事項（情報の更新可能性）](#notice)

<h2 id="q1">Power Pages の基本ライセンスモデル (Q1)</h2>
【結論】Power Pages は「月次ユニークユーザー数」を軸にした Capacity（容量）ベースのライセンスモデルです。
対象は (1) 認証ユーザー (Authenticated users) と (2) 匿名ユーザー (Anonymous users) の 2 系列です。
サブスクリプション購入（事前割当）と Pay-as-you-go メーター（実使用後精算）が併存します。

【ポイント】
1. ライセンス対象単位: 「サイト単位の月次ユニークユーザー数」
2. 種別: 認証ユーザー (Authenticated users) と 匿名ユーザー (Anonymous users) は人数を「別々に」数える（互いに混ざらない・合算しない）
3. 購入形態: 容量パック (100 認証 / 500 匿名 単位) のサブスクリプション、または Azure 課金の Pay-as-you-go メーター
4. 認証/匿名いずれも「その月に 1 回以上アクセスした一意ユーザー」で 1 カウント（基本的には複数回アクセスしても追加カウントされない）
5. 旧モデル (ログイン数 / ページビュー) とは測定概念が異なる（ログイン回数ではなくユーザー人数）

【サブスクリプション 容量パック 概要（抜粋）】
- 認証: 1 パック = 100 ユーザー / 月
- 匿名: 1 パック = 500 ユーザー / 月

（複数 パック 積み上げでサイト想定上限を設計。内部的には環境ごとに最小割当値: 認証 25 / 匿名 200）

【よくある誤解】
- 誤解: 「ログイン回数が課金対象である」
	- → 正しくは: 月内ユニークユーザー数。1 ユーザーが 30 回ログインしても 1 です。
- 誤解: 「容量パック を買うと未使用ぶんは翌月に繰り越せる」
	- → 正しくは: 繰り越し不可（月ごとリセット）。
- 誤解: 「サイト数に比例して必ず追加 Dataverse 容量が必要」
	- → 正しくは: 今の Power Pages ではサイトを作るときに Dataverse の容量を新しく買い足す必要はありません（最初からある分で始められます）。

＜参考資料＞
- [Power Pages のライセンス体系 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#how-is-power-pages-licensed)
- [Power Pages と旧 Power Apps ポータル ライセンス比較 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-are-the-main-differences-between-power-pages-and-power-apps-portals-licensing)

<h2 id="q2">認証ユーザーと匿名ユーザーのカウント方式 (Q2)</h2>
【結論】認証ユーザーは Dataverse の Contact レコード ID で一意判定し、匿名ユーザーはブラウザ Cookie に保持される匿名ユーザー ID で一意化されます。いずれも「その暦月に初めてカウントされた時点で 1」となり再度ログイン（または閲覧）しても加算されません。

【ポイント】
1. 認証ユーザー: ログインに成功した Contact 1 件 = 1（同月内複数回ログインは再カウント無し）
2. 匿名ユーザー: 匿名ページを閲覧したブラウザ Cookie 単位で初回のみカウント（同月内の複数回訪問は再カウント無し）
3. 匿名ユーザーとして数えない主なケース：「テスト用で触っているだけ」「ログインするための画面だけ見た」「機械（検索ロボット）が来た」「その日あとでログインして本人だと分かった」など。（こうした場合は匿名人数を増やさず、必要なら認証ユーザー側だけ数えます。）
4. 同一人物が異なるブラウザ・端末・Cookie クリアを行うと別匿名ユーザーとして計上され得る
5. 匿名→同日内ログイン移行時は「認証ユーザーのみ」計上（匿名側重複排除）

【よくある誤解】
- 誤解: 「匿名はページビュー合計」
	- → 正しくは: ユニーク匿名ユーザー（Cookie ベース）
- 誤解: 「ログインページ（サインイン）閲覧も匿名ユーザーとしてカウントされる」
	- → 正しくは: ログインページは非カウント対象

＜参考資料＞
- [認証ユーザーの定義と算出方法 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-is-an-authenticated-user-and-how-are-authenticated-users-per-websitemonth-calculated)
- [匿名ユーザーの定義と算出方法 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-is-an-anonymous-user-and-how-are-anonymous-users-per-websitemonth-calculated)
- [匿名ユーザーにカウントされないシナリオ (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-are-the-scenarios-in-which-a-user-isnt-counted-as-an-anonymous-user-even-though-the-user-browses-an-anonymous-page-on-the-website)

<h2 id="q3">利用が超過しそうまたは超過した場合の対応 (Q3)</h2>
【結論】月次集計は月初にリセットされ日次累積で増加します。想定上限に近づいたら (1) 追加 容量パック 割当、(2) 他環境からの再配分、(3) Pay-as-you-go への紐付け検討 の順に評価することを推奨します。また、超過した場合、即座にサイトが停止したり、超過分の料金を請求されることはありません。

【補足】データが反映されるまでに 1 日程度かかることがあります。

【ポイント】
1. 対応オプション: 追加購入・未割当容量の再割当・Pay-as-you-go 化（Azure サブスクリプション）
2. 計画: 事前に、季節変動がある場合はピーク月を基準に容量パックの検討

【よくある誤解】
- 誤解: 「超過したら即座にサイト停止や罰金のような追加請求が起きる」
    - → 正しくは: 即時停止・即ペナルティ的課金はありませんが、継続超過は早期の容量見直しが必要です。
- 誤解: 「Pay-as-you-go にするとサブスクリプション容量は不要になる」
	- → 正しくは: 併用設計可。ベース容量 + 変動部を Pay-as-you-go で吸収するハイブリッドが実務上多い。


＜参考資料＞
- [容量の監視: ライセンス サマリービュー (Docs)](https://learn.microsoft.com/ja-jp/power-pages/admin/capacity-management#licensing-summary-view)
- [容量管理ペイン (Docs)](https://learn.microsoft.com/ja-jp/power-pages/admin/capacity-management#capacity-management)
- [最小割り当て数 / パック (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#for-subscription-based-licenses-what-is-the-minimum-number-of-authenticated-and-anonymous-user-capacity-i-need-to-assign-to-an-environment)


<h2 id="q4">認証ユーザーのユニーク判定（重複ログインの扱い） (Q4)</h2>
【結論】同一 Contact に紐づくユーザーが暦月中に複数回ログインしても 1 カウントのみです。判定キーは Dataverse Contact レコード ID で、月初リセット後に再度初回ログインした時点で新しい月の 1 として加算されます。

【ポイント】
1. 判定キー: Dataverse Contact ID
2. 同月内複数ログイン: 再カウント無し
3. 翌月: 初回アクセス時に再び 1 としてカウント
4. 旧“ログイン” 課金との比較: 旧 Portals Login (日次ユニーク * 日数) と違い総ログイン回数増加は費用へ直結しない


【よくある誤解】
- 誤解: 「同じ人が 1 日に何回もログインすると、その回数分追加料金がかかる」
	- → 正: その月に 1 回でもログインした“その人”は 1 人として数えます。翌月になるまで同じ人が何十回ログインしても増えません。

＜参考資料＞
- [認証ユーザー月次カウント方法 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-is-an-authenticated-user-and-how-are-authenticated-users-per-websitemonth-calculated)
- [旧ログイン容量との違い (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-is-the-difference-between-power-apps-portals-login-capacity-and-power-pages-authenticated-per-usermonth-capacity)

<h2 id="q5">内部ユーザー既存ライセンスとカウント除外 (Q5)</h2>
【結論】社内の人がすでに以下のいずれかのライセンスを持っていて（同じテナントで付与済み）、会社のアカウント（Entra ID）で Power Pages サイトに入るなら、その人は「Power Pages の認証ユーザー数」には数えません。ライセンスが無い社内ユーザーや会社外の人だけが、Power Pages 認証容量を消費します。

対象となる既存ライセンス（どれか 1 つ持っていれば除外）:
- Power Apps per user（1 人で複数アプリ使えるライセンス）
- Power Apps per app（指定された 1 サイト/アプリ分の利用権）
- Dynamics 365 Enterprise ライセンス（該当業務アプリの利用権）

【ポイント】
1. すでに 上記既存ライセンスを保持しているユーザー → Power Pages の認証ユーザー容量を消費しない（カウント除外）
2. 上記いずれのライセンスも持っていない社内ユーザー / 社外ユーザー（取引先・お客様など） → Power Pages の認証ユーザー容量を消費する


【よくある誤解】
- 誤解: 「社内ユーザーは全員 認証ユーザーとしてカウントされる」
  - → 正しくは: 上記の既存ライセンス保持者はカウントされません（容量を消費しません）。


＜参考資料＞
- [Power Apps / Dynamics 365 に含まれる利用権 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-are-the-power-pages-use-rights-included-with-power-apps-and-dynamics-365-enterprise-licenses)
- [社内ユーザー向けライセンス選択 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#if-im-building-a-power-pages-website-for-my-employees-who-log-in-using-their-microsoft-entra-credentials-what-power-pages-licenses-do-i-need)
- [ゲスト ユーザーは内部扱いか (FAQ)](https://learn.microsoft.com/ja-jp/power-pages/faq#are-the-guest-users-treated-as-internal-users-for-licensing)

<h2 id="q6">匿名ユーザーの一意判定とブラウザ差異/Cookie削除の影響 (Q6)</h2>
【結論】匿名ユーザーはブラウザ Cookie に保存される匿名 ID で月内ユニーク判定します。別ブラウザ・別端末・Cookie 削除で ID が変われば別ユーザーとしてカウントされます。

【ポイント】
1. 判定キー: 匿名 Cookie ID（ブラウザ/端末単位）
2. Cookie クリア / ブラウザ変更 / 端末変更: 新規ユーザーとして再カウントされ得る
3. 同日匿名→認証ログイン: 認証側のみ残る（匿名で閲覧した後、認証ログインした場合は、認証ユーザーとしてのみカウントされる）
4. ログインページ / エラーページ等への訪問はカウントされない

【よくある誤解】
- 誤解: 「匿名はページビュー合計なので画像読み込み等も増加させる」
	- → 正しくは: 静的リソースのみではカウントされません。
- 誤解: 「匿名で閲覧した後、ログイン認証した場合は、匿名ユーザーと認証ユーザーで 2 重計上される」
	- → 正しくは: 当日内は認証ユーザーとしてカウントされ、匿名ユーザーではカウントされません。

＜参考資料＞
- [匿名ユーザー月次カウント方法 (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-is-an-anonymous-user-and-how-are-anonymous-users-per-websitemonth-calculated)
- [匿名にカウントされないシナリオ (FAQ)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#what-are-the-scenarios-in-which-a-user-isnt-counted-as-an-anonymous-user-even-though-the-user-browses-an-anonymous-page-on-the-website)

<h2 id="summary">まとめ</h2>
- Power Pages は 認証 / 匿名 の月次ユニークユーザー Capacity モデル。旧 Portals の「Login / Page View」課金とは概念が異なります。
- 認証ユニーク: Contact ID ベース。匿名ユニーク: Cookie ID ベース。どちらも月初リセット。
- 超過が懸念される場合は、追加購入・再割当・Pay-as-you-go を使い分けます。
- 超過してしまっても、即停止・追加請求の心配はありません。
- 内部ユーザーで Power Apps per user / per app / Dynamics 365 Enterprise ライセンス保持者は条件を満たせば認証容量を消費しません。

<h2 id="notice">注意事項（情報の更新可能性）</h2>
本記事は執筆日時点の公開情報に基づいています。ライセンス体系・容量定義・ UI 表示・最小割当値などは将来変更される可能性があります。最新情報は必ず公式ドキュメント（Power Pages Licensing FAQ / 管理センター画面 / 最新 Licensing Guide）でご確認ください。

