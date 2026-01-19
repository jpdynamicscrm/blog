---
title: Dataverse Search のストレージ内訳と削減の考え方（Q&Aガイド）
date: 2026-01-19T00:00:00+09:00
tags:
	- Dataverse
	- 検索
	- ストレージ
	- Copilot
categories:
	- [powerplatform]
---

# Dataverse Search のストレージ内訳と削減の考え方（Q&Aガイド）

こんにちは、PowerPlatformサポートチームの鈴木です。

本記事ではDataverse Search のストレージ内訳について、以下の Q1〜Q5 の質問に回答する形でご説明させていただきます。

- [Q1: PPAC の「Dataverse Search」ストレージは何の合算値ですか？](#q1)
- [Q2: 検索インデックス（Search）の中身と削減のコツは？](#q2)
- [Q3: 構造化インデックス（Structured）の中身と削減のコツは？](#q3)
- [Q4: テキストデータインデックス（TextData）の中身と削減のコツは？](#q4)
- [Q5: 画面で見えない内訳をどう管理すればよいですか？](#q5)

### この記事でわかること
- PPAC に表示される Dataverse Search ストレージの意味。
- 3 種類のインデックス（Search / Structured / TextData）の役割。
- それぞれの容量が増える主因と削減アプローチ。
- Copilot / エージェントとインデックスの関係。
- PPACで直接は見えない内訳の考え方と運用ポイント。

## 目次
- [Q1: PPAC の「Dataverse Search」ストレージは何の合算値ですか？](#q1)
- [Q2: 検索インデックス（Search）の中身と削減のコツは？](#q2)
- [Q3: 構造化インデックス（Structured）の中身と削減のコツは？](#q3)
- [Q4: テキストデータインデックス（TextData）の中身と削減のコツは？](#q4)
- [Q5: 画面で見えない内訳をどう管理すればよいですか？](#q5)

<h2 id="q1">PPAC の「Dataverse Search」ストレージは何の合算値ですか？ (Q1)</h2>
【結論】PPAC に表示される Dataverse Search ストレージは、「検索インデックス（Relevance / Search）」「構造化インデックス（Structured）」「非構造化（テキスト）インデックス（Unstructured / TextData）」の合算です。

【補足】インデックスは、グローバル検索や Copilot のためにデータを探しやすくする仕組みです。
Power Platform 管理センター のテーブル容量画面では合計のみが表示され、各内訳の使用量は直接は見えません。

＜参考資料＞
- [Dataverse 検索とは（インデックスの種類）](https://learn.microsoft.com/ja-jp/power-apps/user/relevance-search-benefits)
- [Dataverse 検索の構成（費用・容量に関する記載）](https://learn.microsoft.com/ja-jp/power-platform/admin/configure-relevance-search-organization#how-much-will-dataverse-search-cost)
- [Dataverse 検索による AI 搭載エクスペリエンスの強化 (Release Plan)](https://learn.microsoft.com/ja-jp/power-platform/release-plan/2025wave1/data-platform/enhance-ai-powered-experiences-dataverse-search)

<h2 id="q2">検索インデックス（Search）の中身と削減のコツは？ (Q2)</h2>
【結論】検索インデックスは、モデル駆動アプリのグローバル検索用に、対象テーブルの「検索対象列」とレコードを素早く探すための情報を保持します。対象テーブル数・検索対象列数・レコード数が多いほど容量が増えます。

【補足】不要なテーブルや列を検索対象から外すことで容量を抑えられます。
必要に応じて、環境設定の Dataverse 検索 を「既定（Default）」または「オフ（Off）」にすることで、容量を抑えられます。(モデル駆動型アプリのグローバル検索用の検索ボックスは表示されなくなります。)

【容量削減のためのポイント】
1. 検索対象テーブルの見直し（業務で検索しない履歴テーブル等は対象外）。
2. 検索対象列の削減（フリーテキストに不要な列は対象外）。
3. 必要な場合のみ環境で検索機能を「既定」や「オフ」に設定。

＜参考資料＞
- [テーブルと行を Dataverse 検索で検索する（対象列種の説明）](https://learn.microsoft.com/ja-jp/power-apps/user/relevance-search#understand-search-results)
- [Dataverse 検索の構成（対象テーブル・列の設定手順）](https://learn.microsoft.com/ja-jp/power-platform/admin/configure-relevance-search-organization#select-tables-for-dataverse-searchs-global-search)
- [Dataverse 検索の効率的な管理（テーブル/列の外し方）](https://learn.microsoft.com/ja-jp/power-apps/user/relevance-search-benefits#what-actions-can-makers-or-admins-take-to-manage-dataverse-search-efficiently)

<h2 id="q3">構造化インデックス（Structured）の中身と削減のコツは？ (Q3)</h2>
【結論】構造化インデックスは、Copilot や AI エージェントが Dataverse のテーブルデータを理解し、関係性をたどって推論するための索引です。
App Copilot や Copilot Studio のエージェント、Dynamics 365 の一部 Copilot が利用します。

【補足】不要なアプリでの App Copilot 無効化、未使用エージェントや知識ソースの削除で容量を抑えられます。
一部のファーストパーティ Copilot は専用の管理画面で無効化が必要です。

【容量削減のためのポイント】
1. アプリ単位で App Copilot を無効化。
2. 環境設定で Copilot をオフ。
3. Copilot Studio の未使用エージェントや Dataverse 知識ソースの削除。

【補足】Dynamics 365 Sales Close Agent（SCA）で製品（Product）向けのセマンティック検索が有効だと、構造化インデックスが増えることがあります。
無効化により当該インデックスが削除され、容量が削減される見込みです。

【運用ベストプラクティス】
1. 対象環境のモデル駆動型アプリへ管理者権限でアクセスします。
2. F12 で開発者ツールを開き、コンソールタブを表示します。
3. 次の JavaScript を実行して該当の検索スキルレコードを無効化します。

```javascript
Xrm.WebApi.retrieveMultipleRecords(
	"dvtablesearch",
	"?$filter=name eq 'Product_UkrWydE7w4YL3rT7pU6s3'"
).then(function (result) {
	if (result.entities.length > 0) {
		var dvtablesearchId = result.entities[0].dvtablesearchid;

		Xrm.WebApi.updateRecord("dvtablesearch", dvtablesearchId, {
			"statecode": 1
		}).then(
			function success(res) {
				console.log("dvtablesearch has been updated successfully:", res);
			},
			function (error) {
				console.error("Update failed, please retry:", error.message);
			}
		);
	} else {
		console.log("No record found with name = Product_UkrWydE7w4YL3rT7pU6s3.");
	}
});
```

【補足】実行後、数時間以内に該当インデックスが削除され容量が減る見込みです。
管理センター上の容量反映は翌日以降となる場合があります。

【補足】無効化状態の確認は次の API で行えます。

```
<環境URL>/api/data/v9.0/dvtablesearchs?$filter=(name%20eq%20%27Product_UkrWydE7w4YL3rT7pU6s3%27)
```

【補足】statecode が 1 の場合は当該機能がオフと判断いただけます。

【リスク軽減】本操作は環境に影響しますので、検証環境での事前確認や変更管理の手続きを行ったうえで実施してください。


＜参考資料＞
- [モデル駆動型アプリの Copilot チャット](https://learn.microsoft.com/ja-jp/power-apps/maker/model-driven-apps/add-ai-copilot#disable-copilot-chat-for-a-model-driven-app)
- [環境内のモデル駆動型アプリの Copilot ](https://learn.microsoft.com/ja-jp/power-apps/maker/model-driven-apps/add-ai-copilot#enable-copilot-for-model-driven-apps-in-your-environment)
- [MCS（Microsoft Copilot Studio）で Knowledge Source の追加](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/knowledge-add-dataverse)
- [営業ハブ における Copilot の機能](https://learn.microsoft.com/ja-jp/dynamics365/sales/enable-setup-copilot#turn-copilot-features-on-or-off-in-sales-hub)
- [Customer Service における Copilot の機能](https://learn.microsoft.com/en-us/dynamics365/customer-service/administer/configure-copilot-features#opt-out-of-using-copilot-features)


<h2 id="q4">テキストデータインデックス（TextData）の中身と削減のコツは？ (Q4)</h2>
【結論】テキストデータインデックスは、Copilot / エージェントが参照する非構造化データ（PDF や Word、OneDrive / SharePoint の文書など）を検索・要約するための索引です。
アップロードや連携したファイルが多いほど容量が増えます。

【補足】未使用ファイルの削除、不要な OneDrive / SharePoint の連携解除で容量を抑えられます。

【容量削減のためのポイント】
1. エージェント内の未使用ファイルを削除。
2. 参照しない OneDrive / SharePoint の接続を解除。
3. 必要なファイルやフォルダのみを選ぶ（深いフォルダを丸ごと選ばない）。

＜参考資料＞
- [Copilot Studio: ファイルを知識ソースに追加](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/knowledge-add-file-upload)
- [Copilot Studio: SharePoint を知識ソースに追加](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/knowledge-add-sharepoint)
- [Copilot Studio: 非構造化データの追加 ](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/knowledge-unstructured-data)

<h2 id="q5">画面で見えない内訳をどう管理すればよいですか？ (Q5)</h2>
【結論】Power Platform 管理センター では合計のみの表示ですので、アプリ・エージェント単位で「どの機能がどのインデックスを使っているか」を整理し、不要な対象を段階的に外すのが有効です。 また、環境の Dataverse Search を「既定」にするとグローバル検索のインデックス生成を抑えられます。

【補足】Copilot などの生成 AI は構造化/非構造化インデックスを使います。 方針に合わせて機能を無効化・対象縮小すると容量が削減されます。

【よくある誤解】
- 誤解: Dataverse Search を「既定」にするとすべてのインデックスが止まる。
  - → 正しくは: グローバル検索用は抑制されますが、Copilot など他機能向けの索引は個別設定により維持される場合があります。
- 誤解: アプリで Copilot をオフにしても環境全体の索引が消える。
  - → 正しくは: アプリ単位の影響に限定されます。環境や他アプリ・エージェントの設定は別途管理が必要です。


<h2 id="summary">まとめ</h2>
- Power Platform 管理センター の Dataverse Search ストレージは検索・構造化・テキストの 3 インデックスの合算です。
- 検索インデックスは、グローバル検索対象のテーブル/列/レコード数の見直しで削減できます。
- 構造化インデックスは Copilot/エージェントの無効化・対象縮小で抑えられます。
- テキストデータインデックスは未使用ファイル削除・連携解除で減らせます。

<h2 id="notice">注意事項（情報の更新可能性）</h2>
本記事の内容は執筆日時点の公開情報に基づいております。
仕様や UI、制限事項は将来変更される可能性がございます。
最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
