---
title: モデル駆動型アプリの削除ガイド（権限・依存関係・削除失敗対処）
date: 2025-09-16
tags:
  - powerapps
  - dataverse
  - ガバナンス
categories:
  - [powerapps]
---

# モデル駆動型アプリの削除ガイド（権限・依存関係・削除失敗対処）

こんにちは、PowerPlatformサポートチームの三田でございます。

本記事ではモデル駆動型アプリの削除に関する主要ポイントについて、以下の Q1〜Q3 の質問に回答する形でご説明させていただきます。

- [Q1: モデル駆動型アプリの削除に必要な権限は？](#モデル駆動型アプリの削除に必要な権限-q1)
- [Q2: モデル駆動型アプリを削除する際に注意しなければならない「依存関係」とは？](#削除前に確認すべき依存関係-q2)
- [Q3: 削除を阻止する「依存関係」がないのに削除できないときの対処法は？](#依存関係が表示されないのに削除できない場合の対処-q3)

### この記事でわかること
- 削除に必要な既定ロールとカスタムロール設定時の権限再現要点。
- 削除ブロックとなる依存関係の見分け方と代表例。
- Managed / Unmanaged の違いによる削除可否の差。
- 削除画面で依存関係が表示されないのに失敗する内部要因例とエラー発生時の再試行手順。

## 目次
- [モデル駆動型アプリの削除に必要な権限 (Q1)](#モデル駆動型アプリの削除に必要な権限-q1)
- [削除前に確認すべき依存関係 (Q2)](#削除前に確認すべき依存関係-q2)
- [依存関係が表示されないのに削除できない場合の対処 (Q3)](#依存関係が表示されないのに削除できない場合の対処-q3)
- [まとめ](#まとめ)
- [注意事項（情報の更新可能性）](#注意事項情報の更新可能性)

## モデル駆動型アプリの削除に必要な権限 (Q1)
【結論】削除可能な既定ロールはシステム管理者 (System Administrator) とシステムカスタマイザー (System Customizer) でございます。

【補足】カスタムロールで代替する場合は App Module の Delete と Solution への Read / Write / Delete / Append / Append To と Publish Customizations を同等に付与する必要がございます。
Managed ソリューション配下のアプリは個別削除できずソリューションのアンインストールが前提でございます。

【ポイント】
1. 権限不足防止のため既定ロール 2 種の利用が推奨。
2. Delete 単独では不十分の可能性が高い。
3. Solution 系 5 権限 + Publish が必須。
4. Managed は個別削除不可。


【よくある誤解】
- 誤解: 作成者は必ず削除できる。
  - → 正しくは: 権限が揃わなければ削除できません。
- 誤解: Managed も Maker 画面で直接削除できる。
  - → 正しくは: ソリューションアンインストールが必要です。
- 誤解: Publish Customizations は不要。
  - → 正しくは: 削除後の反映で必要になり得ます。
- 誤解: App Module Delete があれば十分。
  - → 正しくは: Solution 権限群と Publish が無いと失敗します。

＜参考資料＞
- [セキュリティ ロールおよび特権 (Dataverse)](https://learn.microsoft.com/ja-jp/power-platform/admin/security-roles-privileges)
- [Dataverse のカスタマイズに必要な特権](https://learn.microsoft.com/ja-jp/power-apps/maker/model-driven-apps/privileges-required-customization)
- [Managed and unmanaged solutions](https://learn.microsoft.com/power-platform/alm/managed-unmanaged-solutions) (英語のみ)
- [Delete a model-driven app](https://learn.microsoft.com/power-apps/maker/model-driven-apps/delete-model-driven-app) (英語のみ)

## 削除前に確認すべき依存関係 (Q2)
【結論】削除ブロック対象は依存関係ビューの Delete blocked by タブに列挙される要素でございます。

【補足】Uses / Used by の表示は参照関係の把握用で Delete blocked by に現れないものは削除阻止要素ではございません。

代表例: SiteMap / テーブル / フォーム / ビュー / ビュー内チャート / ダッシュボード / Business Process Flow / Web リソース / URL Suffix / ロール依存。

Power Apps ポータルで依存関係を確認する手順 (簡易):
1. 対象環境で ソリューション を開き、アプリを含むソリューションを選択
	![ソリューション内アプリ一覧](./model-driven-app-delete-guide/solution_app.png)
2. アプリ行を選択後、該当行のコマンドバー (… など) から「詳細 > 依存関係を表示」をクリック
	![アプリ詳細から依存関係を表示](./model-driven-app-delete-guide/app_detail.png)
3. 削除ブロック / 使用者 (Used by) / 用途 (Uses) の各タブで確認可能
	![依存関係ビュー](./model-driven-app-delete-guide/view.png)


【ポイント】
1. 判定基準は Delete blocked by。
2. Uses / Used by は副作用確認。
3. 代表要素群を優先確認。
4. Managed もビューで確認可能。
5. 不要削除は他アプリへ影響。
手順 (簡易):
1. ソリューションを開く。
2. 対象アプリ行の詳細 > 依存関係を表示。
3. タブ (Delete blocked by / Used by / Uses) を確認。

【よくある誤解】
- 誤解: Used by 全項目が削除ブロック。
  - → 正しくは: Delete blocked by に無いものはブロックしません。
- 誤解: アプリ削除で参照ビュー等も同時削除。
  - → 正しくは: 個別に残るため不要なら別途クリーンアップが必要です。
- 誤解: Managed では依存関係が見えない。
  - → 正しくは: Managed でも同じ画面で確認可能です。

＜参考資料＞
- [Power Apps でコンポーネントの依存関係を表示する](https://learn.microsoft.com/ja-jp/power-apps/maker/data-platform/view-component-dependencies)
- [テーブルとモデル駆動型アプリ間の依存関係を削除する](https://learn.microsoft.com/ja-jp/power-platform/alm/remove-table-app)
- [依存関係の削除の概要](https://learn.microsoft.com/ja-jp/power-platform/alm/removing-dependencies)
- [Managed and unmanaged solutions](https://learn.microsoft.com/power-platform/alm/managed-unmanaged-solutions) (英語のみ)
- [Delete a model-driven app](https://learn.microsoft.com/power-apps/maker/model-driven-apps/delete-model-driven-app) (英語のみ)

## 依存関係が表示されないのに削除できない場合の対処 (Q3)
【結論】内部設定テーブル (AppSettingBase など) の外部キー参照が残ると Delete blocked by に出ずに削除が失敗する場合がございます。

エラー例:
```
Sql Number: 547 (REFERENCE constraint appmodule_appsetting_parentappmoduleid)
```

【補足】Sql Number 547 は参照整合性違反を示し AppSettingBase の ParentAppModuleId が残留している可能性を示唆します。

対処として保存して公開 → 再生 (起動) → 再度削除 の手順で内部参照が再評価され解消される事例がございます。

手順:
1. Maker ポータルで対象アプリを開き軽微編集後に保存して公開。
2. 直後に再生で一度起動。
3. 再度削除を試行。

上記の手順でもエラーが解消しない場合は、サポートへ障害時刻と環境名を添えてサポートリクエストを起票していただけますと幸いです。

【よくある誤解】
- 誤解: 依存関係ビューに表示されなければ必ず削除成功する。
  - → 正しくは: 内部参照残存や未公開状態で失敗する可能性があります。
- 誤解: 547 エラーは再試行しても無意味。
  - → 正しくは: 再公開で内部参照が更新され成功する例があります。

＜参考資料＞
- [Power Apps でコンポーネントの依存関係を表示する](https://learn.microsoft.com/ja-jp/power-apps/maker/data-platform/view-component-dependencies)
- [依存関係の削除の概要](https://learn.microsoft.com/ja-jp/power-platform/alm/removing-dependencies)
- [Managed and unmanaged solutions](https://learn.microsoft.com/power-platform/alm/managed-unmanaged-solutions) (英語のみ)
- [Troubleshoot common solution issues](https://learn.microsoft.com/power-platform/alm/troubleshoot-common-solution-issues) (英語のみ)

## まとめ
- 削除権限は既定 2 ロールか同等権限複製ロールで確保。
- 削除ブロック判定は Delete blocked by タブを最優先。
- Managed 配下はソリューションアンインストールが必要。
- 内部参照残存時は保存して公開 → 再生 → 削除で再評価。
- 547 エラー継続時はサポートチケット検討。

## 注意事項（情報の更新可能性）
本記事の内容は執筆日時点の公開情報に基づいております。仕様や UI、権限名称、依存関係の表示挙動は将来変更される可能性がございます。最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
