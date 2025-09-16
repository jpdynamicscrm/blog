---
title: Power Pages ライセンスについて
date: 2025-09-16
tags:
  - PowerPages
  - ライセンス
  - ユーザカウント
categories:
  - [powerplatform]
---

# Power Pages ライセンスについて

こんにちは、PowerPlatformサポートチームの三田でございます。

本記事ではPower Pages ライセンス体系について、以下の Q1〜Q6 の質問に回答する形でご説明させていただきます。

- [Q1: Power Pages のライセンスは Power Apps / Power Automate と異なりますか？](#power-pages-ライセンスの位置付けと基本モデル-q1-と-q2-統合)
- [Q2: Power Pages のライセンスの仕組みは？](#power-pages-ライセンスの位置付けと基本モデル-q1-と-q2-統合)
- [Q3: 購入したライセンスより多くのユーザがアクセスした場合はどうなりますか？](#上限をこえるアクセスが起きた場合の扱い-q3)
- [Q4: 認証ユーザのカウントの仕方は？](#認証ユーザ月次の数え方-q4)
- [Q5: 既に内部ユーザが Power Pages 利用権を含むライセンスを持っている場合カウントされますか？](#内部ライセンス保持ユーザのカウント除外条件-q5)
- [Q6: 匿名ユーザのカウントの仕方は？](#匿名ユーザ月次の数え方-q6)

### この記事でわかること
- Power Pages が「認証」「匿名」の 2 種の月次ユニーク人数モデルである点。
- 環境ごとの最小割り当て数と割り当て配分の基本。
- ユニーク人数の判定キー（Dataverse 連絡先 ID / ブラウザ Cookie）。
- 内部ライセンス保持者が二重カウントされない条件。
- 上限をこえる可能性がある際の見張りと調整の考え方。
- 認証 / 匿名ユーザの具体的なカウント挙動と注意点。


## 目次
- [Power Pages ライセンスの位置付けと基本モデル (Q1 と Q2)](#power-pages-ライセンスの位置付けと基本モデル-q1-と-q2-)
- [上限をこえるアクセスが起きた場合の扱い (Q3)](#上限をこえるアクセスが起きた場合の扱い-q3)
- [認証ユーザ（月次）の数え方 (Q4)](#認証ユーザ月次の数え方-q4)
- [内部ライセンス保持ユーザのカウント除外条件 (Q5)](#内部ライセンス保持ユーザのカウント除外条件-q5)
- [匿名ユーザ（月次）の数え方 (Q6)](#匿名ユーザ月次の数え方-q6)

## Power Pages ライセンスの位置付けと基本モデル (Q1 と Q2)
【結論】Power Pages は「認証ユーザ」と「匿名ユーザ」の 2 区分の月次ユニーク人数（MAU（月内に1回以上アクセスしたユニーク人数））に基づく利用できる上限モデルでございます。また、従来の Portals ログイン/ページビュー方式とは集計単位が異なりシンプルです。

【補足】従来の “ログイン数” や “ページビュー数” ではなく「その月に 1 回以上アクセスしたユニーク人物」を数えるため繰り返しログインが多いユーザで人数が急増しにくいです。
認証ユーザは 100 人単位パック、匿名ユーザは 500 人単位パック（いずれもサブスクリプション例）で追加し環境ごとに割り当て配分できます。
環境への最小割り当ては認証 25、匿名 200 です。

【ポイント】
1. ユニーク基準で重複ログイン分は 1 人扱い。
2. 匿名と認証は別々に上限を管理。
3. 割り当ては後から再配分が可能。
4. 内部の適格ライセンス保持者は重複カウント除外。


【よくある誤解】
- 誤解: ページビュー数がそのまま課金対象である。
  - → 正しくは: 月次ユニーク人数が基準です。

- 誤解: 1 人が複数回ログインすると複数人分として数えられる。
  - → 正しくは: 月内であれば、何度ログインしても 1 人分です。

＜参考資料＞
- [Power Platform ライセンス FAQ (Power Pages セクション)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Manage and monitor capacity (Power Pages)](https://learn.microsoft.com/ja-jp/power-pages/admin/capacity-management)
- [Power Platform ライセンス ガイド (PDF / 英語)](https://go.microsoft.com/fwlink/?linkid=2085130)

## 上限をこえるアクセスが起きた場合の扱い (Q3)
【結論】月次利用が想定より増える兆しがあれば管理画面の利用できる上限ビューで早期に把握し追加購入かアクセス平準化を検討します。また、過去 3 か月の推移を見て必要な配分に調整できます。

【補足】管理センターでは日次と月次の推移グラフを確認でき増加カーブを把握できます。
環境間で未使用分があれば再割り当てで余裕をつくれます。
想定を上回る傾向が続く場合は早めにパック追加か Pay-as-you-go（従量）方式を検討していただけると幸いです。

【ポイント】
1. 日次推移で早期兆候を把握。
2. 過去 3 か月データで増加傾向を判断。
3. 余剰の再配分で無駄な追加購入を回避。
4. 定常増ならサブスクリプション追加。
5. 一時的増ならアクセス整理やキャッシュ活用。

【よくある誤解】

- 誤解: 上限をこえた瞬間に自動で強制停止される。
  - → 正しくは: 上限を超えた場合でも、強制停止されることはございません。また、Pay-as-you-go（従量）方式でなければ超過分に対する追加支払いも必要ございません。

＜参考資料＞
- [Manage and monitor capacity (Power Pages)](https://learn.microsoft.com/ja-jp/power-pages/admin/capacity-management)
- [Website capacity consumption reports](https://learn.microsoft.com/ja-jp/power-pages/admin/website-consumption-reports)
- [Power Platform ライセンス FAQ (Power Pages セクション)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#power-pages)

## 認証ユーザ（月次）の数え方 (Q4)
【結論】認証ユーザはその月にサイトへログインしたユニーク人物数で数え、同一人物の複数ログインは 1 人扱いです。また、判定キーは紐付く Dataverse 連絡先（Contact）レコード ID です。

【補足】ユーザは認証プロバイダ経由でサインインし Dataverse の連絡先と関連付けられます。
同一ユーザが別日や別時間帯に何度ログインしても人数は増えません。
異なるサイト（別環境）にアクセスした場合はサイトごとにカウントされます。

【ポイント】
1. 月内 1 回以上ログインで 1 人。
2. 重複ログインは増加しない。
3. Contact ID がユニーク判定キー。
4. 環境が違えば別サイトとして独立集計。
5. 内部適格ライセンス保持者は別条件で除外検討。

【よくある誤解】

- 誤解: ログイン回数がそのまま人数としてカウントされる。
  - → 正しくは: ユニーク人物数のみです。

＜参考資料＞
- [Power Platform ライセンス FAQ (Authenticated user 定義)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Website capacity consumption reports](https://learn.microsoft.com/ja-jp/power-pages/admin/website-consumption-reports)

## 内部ライセンス保持ユーザのカウント除外条件 (Q5)
【結論】Power Apps Per User / Per App または Dynamics 365 Enterprise など対象ライセンスを同一テナントで割り当て済みの内部認証ユーザは Power Pages 認証上限へ二重計上されません。また、条件を満たさない外部コラボゲスト等は通常どおり計上されます。

【補足】認識にはユーザが同一テナントの Entra ID でサインインし標準プロバイダを利用する必要があります。
対象外テナントのライセンスや異なる認証経路だと除外されません。

【ポイント】
1. 対象ライセンス: Power Apps Per App / Per User / Dynamics 365 Enterprise。
2. 同一テナント割り当てが必須。
3. 標準認証プロバイダ利用が前提。
4. 条件不一致なら認証ユーザとして計上。
5. 運用前にテストアカウントで挙動確認。

【よくある誤解】

- 誤解: いかなる内部社員も常にMAU（月内に1回以上アクセスしたユニーク人数）の除外対象である。
  - → 正しくは: 適格ライセンス + 条件充足時のみです。

＜参考資料＞
- [Power Platform ライセンス FAQ (内部利用権)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Power Platform ライセンス ガイド (PDF / 英語)](https://go.microsoft.com/fwlink/?linkid=2085130)

## 匿名ユーザ（月次）の数え方 (Q6)
【結論】匿名ユーザはログインせず公開ページに月内 1 回以上到達したユニーク人物数で数え Cookie の匿名 ID で判定します。また、ブラウザや端末が変わるか Cookie を消すと別人物として数えられます。

【補足】
匿名 ID はブラウザ Cookie に保存されるためシークレットウィンドウや別端末は新規カウントになりやすいです。
過度な Cookie クリア手順の案内は不要な人数増加を招くため避けます。

【ポイント】
1. 月内公開ページ訪問で 1 人。
2. Cookie 削除や別ブラウザで別人扱い。
3. 認証後は認証ユーザ集計に移行。
4. ログインページだけ匿名公開なら人数不要。
5. 計測増要因: マルチデバイス / Cookie クリア。

【よくある誤解】

- 誤解: 月内であれば、同一人物の端末切替でも必ず 1 人とカウントされる。
  - → 正しくは: Cookie 単位なので別カウントになる可能性があります。

＜参考資料＞
- [Power Platform ライセンス FAQ (Anonymous user 定義)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-flow-licensing-faq#power-pages)
- [Website capacity consumption reports](https://learn.microsoft.com/ja-jp/power-pages/admin/website-consumption-reports)
- [Manage and monitor capacity (Power Pages)](https://learn.microsoft.com/ja-jp/power-pages/admin/capacity-management)

## まとめ
- 認証 / 匿名の 2 種ユニーク人数モデルで月次上限管理。
- 判定キー: 認証は Contact ID、匿名は Cookie の匿名 ID。
- 環境最小割り当て: 認証 25 / 匿名 200。
- 内部の適格 Power Apps / Dynamics 365 ライセンス保持者は重複計上除外条件あり。
- 管理センターの利用できる上限ビューで早期兆候を把握し再配分や追加手当を判断。

## 注意事項（情報の更新可能性）
本記事の内容は執筆日時点の公開情報に基づいております。仕様や UI、制限事項は将来変更される可能性がございます。最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
