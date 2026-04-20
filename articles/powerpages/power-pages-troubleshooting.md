---
title: Power Pages 認証キーの更新ガイド
date: 2026-04-20 00:00:00
tags:
    - Power Pages
    - Administration
categories:
    - [Power Pages]
---

# Power Pages トラブルシューティング ガイド: よくある問題と解決方法およびサポートへの問い合わせについて

## はじめに

こんにちは、Power Platformサポートチームの吉永です。
本記事では Power Pages の一般的なトラブルシューティングと、弊社サポートにお問い合わせいただく場合にご提供いただきたい情報についてご紹介させて頂きます。

このため、ご利用いただいている Power Pages サイトや、Power Pages サイトの作成において問題が発生した場合にはこちらで紹介させて頂いている手順についてご参考にしていただき、問題が解消しない場合については、弊社サポートへのお問い合わせをご検討いただけますと幸いです。

なお、本記事では以下の 2 つに分けてご紹介を行わせて頂きます。

- **第 1 部**: お客様ご自身で実施できる一般的なトラブルシューティング手順
- **第 2 部**: それでも解決しない場合にサポートへお問い合わせいただく際に、ご用意いただきたい情報

---

# 第 1 部: 一般的なトラブルシューティング

## 1. まず試すこと — 診断ツールの活用

問題に遭遇したら、まず以下の診断ツールを活用してください。多くの場合、原因の特定と解決策の提示まで自動的に行われます。

### 1.1 サイト チェッカー を実行する

**サイト チェッカー** は、サイトの構成上の問題を自動検出するセルフサービス診断ツールです。結果は **エラー**・**警告**・**合格** の 3 段階で表示され、修正手順も提示されます。

**実行方法:**
1. [Power Platform 管理センター](https://aka.ms/ppac) を開く
2. **リソース** > **Power Pages サイト** を選択
3. 対象サイトを選択し、**サイト チェッカー** パネルで **実行** をクリック

**参照**: [サイト チェッカーの実行](https://learn.microsoft.com/power-pages/admin/site-checker)

### 1.2 Power Pages DevTools 拡張機能でエラーを確認する

Edge ブラウザ向けの開発者ツール拡張機能で、サーバーサイドのエラーメッセージや Liquid のカスタムログを確認できます。

**セットアップ手順:**
1. [Microsoft Power Pages extension for Edge](https://go.microsoft.com/fwlink/?linkid=2270261) をインストール
2. Portal Management App で Site Setting `UserTrace/Debug` を `true` に設定（Private サイトはデフォルトで有効）
3. 問題を再現し、DevTools の **Power Pages** タブでエラーメッセージを確認

**参照**: [Power Pages DevTools 拡張機能](https://learn.microsoft.com/power-pages/configure/devtools-addon)

### 1.3 キャッシュをクリアする

設定変更が反映されない場合は、サーバーサイドキャッシュが原因の可能性があります。

- **方法 1**: Design Studio の**プレビュー**ボタンをクリック
- **方法 2**: サイト URL に `/_services/about` を付加してアクセスし、**Clear Cache** をクリック（Website Access Permission が必要）
- **方法 3**: Power Platform 管理センターからサイトを**再起動**

> **注意**: キャッシュの反映 SLA は**最大 15 分**です。設定変更後、すぐに反映されない場合は 15 分待ってから確認してください。Clear Cache は全キャッシュをクリアするため、高負荷サイトでは**非ピーク時**に実行してください。

**参照**: [Power Pages でのサーバー側キャッシュの動作](https://learn.microsoft.com/power-pages/admin/clear-server-side-cache)

### 1.4 既知の問題 (Known Issues) を確認する

お客様が遭遇している問題が既知の問題である可能性があります。以下のページで最新の情報をご確認ください。

**参照**: [Power Pages 既知の問題](https://learn.microsoft.com/power-pages/known-issues)

---

## 2. よくある問題と解決方法

発生している症状ごとに、考えられる原因と対処が記載された公式ドキュメントをまとめました。

### サイトにアクセスできない / サーバーエラーが表示される

まず、Power Platform 管理センターからサイトの**再起動**をお試しください。キャッシュの不整合や一時的な接続の問題であれば、再起動だけで解消する場合があります。また、`/_services/about` ページから **Clear Cache** を実行することも有効です。

再起動やキャッシュクリアで改善しない場合は、以下のドキュメントを順に確認してください。

- [Dataverse 環境 URL が変更されている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#url-of-the-connected-microsoft-dataverse-environment-has-changed)
- [Dataverse 環境が管理モードになっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#microsoft-dataverse-environment-connected-to-your-website-is-in-administration-mode)
- [環境復元により認証接続が切断されている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#authentication-connection-between-dataverse-environment-and-website-is-broken)
- [Dataverse へのリクエストがタイムアウトしている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#request-to-microsoft-dataverse-environment-has-timed-out)
- [Website Binding が見つからない](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#website-binding-not-found)
- [認証キーの有効期限が切れている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-startup-issue#website-is-inaccessible)

### サイトの作成・プロビジョニングに失敗する

サイトの新規作成やプロビジョニング中に問題が発生する場合は、以下を確認してください。

- [サイト作成時の権限エラー (Azure AD アプリ登録権限がない)](https://learn.microsoft.com/ja-jp/power-pages/known-issues#site-creation) — Power Pages Core パッケージを最新に更新することで解消する場合があります
- [プロビジョニングの問題 - Profile フォームが存在しない](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-provisioning-issues) — 「Getting set up」画面のまま進まない場合はこちらを確認
- [サイト作成に関する既知の問題](https://learn.microsoft.com/ja-jp/power-pages/known-issues#site-creation) — 古い環境では Entra アプリ登録権限が必要な場合があります
- [削除したサイトの URL を再利用する場合](https://learn.microsoft.com/ja-jp/power-pages/faq) — リソースのパージに 30 分〜1 時間かかるため、待機が必要です
- [サイト作成全般に関する FAQ](https://learn.microsoft.com/ja-jp/power-pages/faq)

### データを更新したがサイトに反映されない

- [サーバーサイドキャッシュの仕組み](https://learn.microsoft.com/ja-jp/power-pages/admin/clear-server-side-cache) — 反映 SLA は最大 15 分
- [テーブルの Change Tracking が無効になっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-cache-invalidation#tables-not-enabled-for-cache-invalidation)
- [組織の Change Tracking が無効になっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-cache-invalidation#organization-not-enabled-for-change-tracking)

### 「Page Not Found」エラーが表示される

- [Page Not Found サイトマーカーの設定不備](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-cache-invalidation#im-getting-a-page-not-found-error-and-the-page-content-is-different-from-the-default-page-not-found-site-marker-or-web-page)
- [構成の問題 - Page Not Found サイトマーカー](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#page-not-found-site-marker-configuration)

### 特定のページが表示されない / 空白になる

- [Web ページの構成に問題がある](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#webpages)
- [親ページが非アクティブになっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#parent-page-of-an-active-webpage-is-inactive)
- [Home サイトマーカーの設定不備](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#home-site-marker-configuration)
- [Profile サイトマーカーの設定不備](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#profile-site-marker-configuration)

### サイトの読み込みが遅い

- [パフォーマンスの問題（全般）](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-performance)
- [CSS/JS が同期的に読み込まれている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#loading-static-resources-cssjs-asynchronously)
- [ルックアップがドロップダウン表示になっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-performance#basic-form-lookup-configuration)
- [Web ファイル数が多すぎる (500 件超)](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-performance#large-number-of-web-file-records)
- [ヘッダー / フッターの出力キャッシュが無効になっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-performance#header-output-cache-is-disabled)

### ログインしていないユーザーにデータが表示されてしまう

- [Basic Form / List の Table Permission が無効になっている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#anonymous-access-to-basic-forms-lists-and-multistep-form-steps)
- [OData フィードが匿名公開されている](https://learn.microsoft.com/ja-jp/power-pages/admin/site-checker-configuration-issues#anonymous-access-available-to-odata-feed)
- [Dataverse テーブルへの匿名アクセスが許可されている](https://learn.microsoft.com/ja-jp/power-pages/security/site-checker-security)

### セキュリティ設定を強化したい

- [Web Application Firewall (WAF) の有効化](https://learn.microsoft.com/ja-jp/power-pages/security/site-checker-security#web-application-firewall-enabled)
- [HTTP セキュリティヘッダーの設定](https://learn.microsoft.com/ja-jp/power-pages/security/site-checker-security)
- [Web Template Validation の有効化](https://learn.microsoft.com/ja-jp/power-pages/security/site-checker-security#web-template-validation)

> **その他の参考ドキュメント:**
> - [認証の概要](https://learn.microsoft.com/ja-jp/power-pages/security/authentication/)
> - [SAML 2.0 FAQ](https://learn.microsoft.com/ja-jp/power-pages/security/authentication/saml2-faqs)
> - [Power Pages FAQ](https://learn.microsoft.com/ja-jp/power-pages/faq)
> - [Known Issues](https://learn.microsoft.com/ja-jp/power-pages/known-issues)

---

# 第 2 部: サポートへのお問い合わせ時にご用意いただきたい情報

上記のトラブルシューティングで解決しない場合は、Microsoft サポートへのお問い合わせをご検討ください。以下の情報を事前にご準備いただくことで、調査を迅速に進めることができます。

## 3. 全般的に必要な情報

すべてのお問い合わせで以下の情報をご用意ください。

| # | 項目 | 確認方法 |
|---|---|---|
| 1 | **サイト URL** | 例: `https://contoso.powerappsportals.com` |
| 2 | **環境 ID** | [環境の組織 ID と名前を確認する方法](https://learn.microsoft.com/ja-jp/power-platform/admin/determine-org-id-name) |
| 3 | **問題の発生日時** (UTC) | タイムゾーンを明記。可能な限り正確に |
| 4 | **問題の再現手順** | 具体的な操作手順を番号付きで記載。画面録画も有効です — [Snipping Tool を使って画面を録画する方法](https://support.microsoft.com/ja-jp/windows/snipping-tool-%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%82%B9%E3%82%AF%E3%83%AA%E3%83%BC%E3%83%B3-%E3%82%B7%E3%83%A7%E3%83%83%E3%83%88%E3%82%92%E3%82%AD%E3%83%A3%E3%83%97%E3%83%81%E3%83%A3%E3%81%99%E3%82%8B-00246869-1843-655f-f220-97299b865f6b) |
| 5 | **期待される動作と実際の動作** | 何が起きるべきで、何が起きたか |
| 6 | **影響範囲** | 全ユーザーか特定ユーザーか / 全ページか特定ページか |
| 7 | **スクリーンショット / エラーメッセージ** | エラー画面のスクリーンショット、Activity ID があれば記載 |
| 8 | **最近の変更内容** | 問題発生前に行った設定変更・カスタマイズ・ソリューションインポート等 |
| 9 | **データモデルの種類** | **拡張データモデル (Enhanced)** か **標準データモデル (Standard)** か — [Enhanced data model の確認方法](https://learn.microsoft.com/ja-jp/power-pages/admin/enhanced-data-model) |

## 4. 問題カテゴリ別の追加情報

### 4.1 サイトが起動しない / サーバーエラー

| # | 追加情報 | 説明 |
|---|---|---|
| 1 | **エラー画面のスクリーンショット** | エラーページ全体のキャプチャ（Activity ID やエラーメッセージが含まれるようにしてください） |
| 2 | **環境に対する最近の操作** | URL 変更、バックアップ復元、管理モードの有効/無効化 等 |
| 3 | **HAR ファイル** | エラー発生時の HAR ファイル（取得手順はセクション 5 を参照） |

### 4.2 認証・ログインの問題

| # | 追加情報 | 説明 |
|---|---|---|
| 1 | **エラー画面のスクリーンショット** | ブラウザに表示されるエラーページ全体のキャプチャ |
| 2 | **使用している認証プロバイダー** | Microsoft Entra ID / Azure AD B2C / SAML 2.0 / ローカル認証 等 |
| 3 | **HAR ファイル** | ブラウザの開発者ツール > Network タブで問題を再現し、HAR ファイルとして保存 |
| 4 | **影響を受けるユーザーの情報** | 全ユーザーか特定ユーザーか。特定の場合は Contact レコードの GUID |
| 5 | **IDP 側の設定情報** | Reply URL、Issuer URL、Client ID（秘密情報は含めないでください） |

> **HAR ファイルの取得方法**: ブラウザで F12 > **Network** タブを開く > **Preserve log** にチェック > 問題を再現 > **Export HAR** で保存

### 4.3 フォーム・リスト・Liquid の問題

| # | 追加情報 | 説明 |
|---|---|---|
| 1 | **エラー画面のスクリーンショット** | 問題が発生しているページ全体のキャプチャ |
| 2 | **問題が発生するページの URL** | 具体的なページ URL |
| 3 | **Basic Form / List の名前と対象テーブル** | Portal Management App で確認できるフォーム/リストの名前、および対象の Dataverse テーブル名 (論理名) |
| 4 | **HAR ファイル** | 問題のページにアクセスした際の HAR ファイル |
| 5 | **DevTools 拡張機能のエラーログ** | Power Pages タブに表示されるエラーメッセージ |
| 6 | **カスタム Liquid コード** | 問題に関連する Web Template / Content Snippet のコード（該当する場合） |
| 7 | **ブラウザコンソールのログ** | F12 > **Console** タブ > コンソール領域内を右クリック > **Save as...** で `.log` ファイルとして保存 |

### 4.4 パフォーマンスの問題

| # | 追加情報 | 説明 |
|---|---|---|
| 1 | **遅いページの URL** | 特定のページか全体か |
| 2 | **問題が発生する時間帯** | 常時か特定の時間帯か |
| 3 | **HAR ファイル** | 遅いページにアクセスした際の HAR ファイル |
| 4 | **ブラウザの Performance プロファイル** | F12 > **Performance** タブ > 記録ボタン (●) をクリック > 問題を再現 > 停止 > 左上の **↓** (Export) アイコンで `.json` ファイルとして保存 |
| 5 | **CDN / WAF の設定状況** | 有効/無効、外部 CDN (Azure Front Door 等) の利用有無 |
| 6 | **同期プラグインの有無** | 対象テーブルに登録されているプラグインの種類と実行タイミング |

### 4.5 Design Studio / 管理画面の問題

| # | 追加情報 | 説明 |
|---|---|---|
| 1 | **エラー画面のスクリーンショット** | Design Studio で表示されるエラー画面全体のキャプチャ |
| 2 | **ブラウザの種類とバージョン** | Edge / Chrome 等、バージョン番号 |
| 3 | **操作しているユーザーの権限** | システム管理者 / Maker 等 |
| 4 | **HAR ファイル** | Design Studio で問題を再現した際の HAR ファイル |
| 5 | **ブラウザコンソールのログ** | F12 > **Console** タブ > コンソール領域内を右クリック > **Save as...** で `.log` ファイルとして保存 |

---

## 5. HAR ファイルの取得手順

多くのお問い合わせで **HAR ファイル** の提供をお願いしています。以下の手順で取得してください。

### Microsoft Edge / Google Chrome の場合

1. 問題が発生するページを開く
2. **F12** キーを押して開発者ツールを開く
3. **Network** タブを選択
4. **Preserve log** (ログを保持) にチェックを入れる
5. ページを更新して**問題を再現**
6. Network タブ内を右クリック > **Save all as HAR with content** を選択
7. ファイルを保存

> **注意**: HAR ファイルにはセッション情報や Cookie が含まれる場合があります。サポートチケットの添付ファイルとして安全にご送付ください。

---

## まとめ

### トラブルシューティングのフローチャート

```
問題発生
│
├─ 1. Site Checker を実行
│     → 問題が検出されたら提示された手順で修正
│
├─ 2. DevTools 拡張機能でサーバーエラーを確認
│     → エラーメッセージから原因を特定
│
├─ 3. キャッシュクリア（15分待機 or /_services/about）
│     → 設定変更が反映されない場合に有効
│
├─ 4. Known Issues ページを確認
│     → 既知の問題と回避策がないか確認
│
└─ 5. 上記で解決しない場合 → サポートへお問い合わせ
       → 第 2 部の情報をご準備ください
```

## 参考リンク

| リソース | URL |
|---|---|
| Power Pages 公式ドキュメント | https://learn.microsoft.com/power-pages/ |
| Known Issues | https://learn.microsoft.com/power-pages/known-issues |
| FAQ | https://learn.microsoft.com/power-pages/faq |
| Site Checker | https://learn.microsoft.com/power-pages/admin/site-checker |
| DevTools 拡張機能 | https://learn.microsoft.com/power-pages/configure/devtools-addon |
| サーバーサイドキャッシュ | https://learn.microsoft.com/power-pages/admin/clear-server-side-cache |
| 認証の概要 | https://learn.microsoft.com/power-pages/security/authentication/ |
| Power Pages ブログ | https://powerpages.microsoft.com/en-us/blog/ |
| コミュニティ | https://powerusers.microsoft.com/t5/Microsoft-Power-Pages-Community/ct-p/MPPCommunity |

<h2 id="notice">注意事項（情報の更新可能性）</h2>
本記事の内容は執筆日時点の公開情報に基づいております。仕様や UI、制限事項は将来変更される可能性がございます。最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
