---
title: Dynamics 365 トラブルシューティングのための情報収集ガイド
date: 2026-01-06 09:00:00
tags:
  - Dynamics 365
  - トラブルシューティング
  - 情報採取
  - お問い合わせ全般
categories:
  - [Dynamics 365]
---

# Dynamics 365 トラブルシューティングのための情報収集ガイド

こんにちは、Power Platform サポートチームの鎌田です。

本記事では、Dynamics 365 に関するお問い合わせの際に必要となる情報の取得手順をご説明します。
適切な情報をご提供いただくことで、問題の早期解決につながります。

<!-- more -->

## 目次

1. [識別情報の取得](#identification-info)
   1. [環境 URL](#environment-url)
   2. [UPN（ユーザー プリンシパル名）](#upn)
   3. [セッション ID・バージョン情報](#session-id)
2. [セキュリティロールとチームの確認](#security-role-and-team)
3. [ソリューション レイヤーの確認](#solution-layers)
4. [インポート履歴の確認](#import-history)
5. [Web API レスポンスの取得](#web-api-response)
6. [診断情報の取得](#diagnostic-info)
   1. [ネットワーク トレース（HAR ファイル）](#network-trace)
   2. [動画の録画](#video-recording)

---

<a id='identification-info'></a>

## 1. 識別情報の取得

サーバー側のログ調査に必要な識別情報です。

<a id='environment-url'></a>

### 1-1. 環境 URL

1. Dynamics 365 にサインインします
2. ブラウザのアドレスバーから URL をコピーします
例: `https://contoso.crm7.dynamics.com`

<a id='upn'></a>

### 1-2. UPN（ユーザー プリンシパル名）

事象が発生しているユーザーを特定するために必要です。
例: `user@contoso.onmicrosoft.com`

Power Platform 管理センターで環境を開き、[設定] > [ユーザーとアクセス許可] > [ユーザー] のユーザー名よりご確認いただけます。
![](./dynamics365-troubleshooting-info-collection/upn.png)

<a id='session-id'></a>

### 1-3. セッション ID・バージョン情報

1. Dynamics 365 アプリの画面右上の歯車アイコンをクリックします
2. 「情報」をクリックするとダイアログに情報が表示されます
![](./dynamics365-troubleshooting-info-collection/sessionid.png)

---

<a id='security-role-and-team'></a>

## 2. セキュリティロールとチームの確認

アクセス権限に関連する問題の調査に必要です。

1. Power Platform 管理センターで環境を開き、[設定] > [ユーザーとアクセス許可] > [ユーザー] を開きます
2. 対象ユーザーをクリックします
3. サイドバーの「直接割り当てられたロール」から、割り当てられたセキュリティ ロールの一覧を確認できます（チーム経由で割り当てられたロールはここには表示されません）
4. サイドバーの「チーム」から、所属しているチームの一覧を確認できます
![](./dynamics365-troubleshooting-info-collection/user-info.png)

---

<a id='solution-layers'></a>

## 3. ソリューション レイヤーの確認

特定のコンポーネント (アプリ、テーブル、フォームなど) がどのソリューション経由でインストール・カスタマイズされたかを確認する方法です。
コンポーネントの出自や変更履歴の調査に必要となります。

1. [Power Apps ポータル](https://make.powerapps.com/) にサインインし、対象の環境を選択します
2. 左ナビゲーションから [ソリューション] を選択します
3. [既定のソリューション] を開きます
   ![](./dynamics365-troubleshooting-info-collection/solution-list.png)
4. 左ペインのオブジェクト一覧から対象のコンポーネントの種類を選択します（例: モデル駆動型アプリの場合は [アプリ]）
5. 対象のコンポーネントの [︙] メニューを開き、[詳細] > [ソリューション レイヤーの表示] をクリックします
   ![](./dynamics365-troubleshooting-info-collection/solution-layers-menu.png)
6. 表示されたソリューション レイヤーの画面のスクリーンショットを取得します
   ![](./dynamics365-troubleshooting-info-collection/solution-layers-result.png)

ソリューション レイヤーには、そのコンポーネントに影響を与えたソリューションが上から順（最新が上）に表示されます。最下層の [システム] レイヤーはプラットフォーム既定の定義です。

**参考資料：** [ソリューション レイヤー](https://learn.microsoft.com/power-apps/maker/data-platform/solution-layers)

---

<a id='import-history'></a>

## 4. インポート履歴の確認

データ インポートに関する問題の調査では、対象のインポート レコードを特定するための情報が必要です。

1. Dynamics 365 アプリの左上のアプリ名をクリックし、アプリ一覧から「Power Platform 環境設定」を選択します
   ![](./dynamics365-troubleshooting-info-collection/import-app-selection.png)
2. 左ナビゲーションの [データ管理] をクリックし、[インポート] を選択します
   ![](./dynamics365-troubleshooting-info-collection/import-data-management.png)
3. 問題が発生しているインポートを特定し、この画面のスクリーンショットを取得します。複数のインポートが表示されている場合は、該当のインポートがどれかわかるように示してください
   ![](./dynamics365-troubleshooting-info-collection/import-history.png)

スクリーンショットでは、以下の情報が確認できるようにしてください：
- **インポート名**: 問題が発生しているインポートがどれか特定するために必要です
- **ステータス**: 完了、エラーなどの状態
- **成功・部分的な失敗・エラーの件数**: 問題の規模を把握するために必要です
- **作成日**: インポートが実行された日時

> [!NOTE]
> 一覧に表示される日時が日本時間ではない場合は、その旨をサポート担当者にお知らせください。

---

<a id='web-api-response'></a>

## 5. Web API レスポンスの取得

サーバー側のデータや設定を確認するため、Web API のレスポンスを JSON ファイルとして提供いただく場合があります。

**取得手順（Microsoft Edge）：**
1. ブラウザで調査対象の環境 (`https://<環境 URL>.dynamics.com`) にアクセスし、Dynamics 365 にサインインできることを確認します
2. 新しいタブを開き、サポート担当者から指定された URL をアドレスバーに入力してアクセスします
   例: `https://<環境 URL>.dynamics.com/api/data/v9.2/accounts`
3. レスポンスが表示されたら、画面左上の「整形出力」にチェックを入れます
4. ページ上で右クリックし、「名前を付けて保存」を選択します
   ![](./dynamics365-troubleshooting-info-collection/web-api-save.png)
5. ファイルの種類が「JSON ファイル」になっていることを確認し、保存します
6. 保存した JSON ファイルを、サポート担当者から提供されたファイル アップロード用のリンクからアップロードしてください

---

<a id='diagnostic-info'></a>

## 6. 診断情報の取得

問題の詳細な調査に必要な診断情報です。

<a id='network-trace'></a>

### 6-1. ネットワーク トレース（HAR ファイル）

ブラウザと Dynamics 365 サーバー間の通信を記録します。
ネットワーク トレースとは、ブラウザーがページを表示するために送受信した通信を時系列で記録したものです。
問題が発生した操作を記録し HAR ファイルとして共有いただくことで、調査がスムーズになります。

> [!IMPORTANT]
> **事象再現の直前**に取得を開始し、**事象を再現させた直後**に保存してください。不要な通信が増え肝心な要求が埋もれるため、不要なページ遷移はせず最小の再現だけに留めてください。

**取得手順（Microsoft Edge）：**
1. ブラウザで `F12` キーを押し、開発者ツールを開き、「ネットワーク」タブを選択します
2. 以下のオプションを有効にします：
   - 「ログを保持」にチェック
   - 「キャッシュを無効にする」にチェック
3. 「クリア」ボタンで古いログを消去します
   ![](./dynamics365-troubleshooting-info-collection/trace-start.jpg)
4. 事象を**最小の手順で 1 回だけ**再現させます
5. 再現後、「Export HAR」ボタン（↓アイコン）をクリックして保存します（ファイル名は `YYYYMMDD_issue.har` のように日付と概要を含めてください）
   ![](./dynamics365-troubleshooting-info-collection/trace-end.jpg)

**参考資料：** [ブラウザー トレースの取得（Azure Portal）](https://learn.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)

<a id='video-recording'></a>

### 6-2. 動画の録画

事象再現時の動画があると、発生状況をより正確に把握できます。

> [!IMPORTANT]
> 事象発生の**事前**に録画を開始し、事象発生を確認した**後**に停止してください。

#### Snipping Tool での画面録画

1. `Windows + Shift + S` キーを押して Snipping Tool を起動、または検索から「Snipping Tool」を起動します
2. 動画ボタン（ビデオカメラアイコン）をクリックします
   ![](./dynamics365-troubleshooting-info-collection/snipping-tool.png)
3. 録画する画面の範囲をドラッグで選択し、「スタート」をクリックして録画を開始します
   ![](./dynamics365-troubleshooting-info-collection/start-recording.png)
4. 事象を再現させます
5. 録画停止ボタンを押して終了し、ファイルを保存します

#### その他の録画方法

- [PowerPoint で画面を記録する](https://support.microsoft.com/ja-jp/office/powerpoint-%E3%81%A7%E7%94%BB%E9%9D%A2%E3%82%92%E8%A8%98%E9%8C%B2%E3%81%99%E3%82%8B-0b4c3f65-534c-4cf1-9c59-402b6e9d79d0)
- [問題ステップ レコーダーの使用方法](https://learn.microsoft.com/ja-jp/office/troubleshoot/settings/how-to-use-problem-steps-recorder)
