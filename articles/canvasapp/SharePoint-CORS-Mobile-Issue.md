---
title: モバイル端末で SharePoint をソースにしたキャンバスアプリの画像やメディアが表示されない場合の対処方法
date: 2026-01-16 12:00:00
tags:
  - Power Apps
  - SharePoint
  - Mobile
  - Canvas App
categories:
  - [Power Apps]
---

※本記事の執筆には生成 AI を使用しています。
こんにちは、Power Platform サポートの竹内です。  
今回は、モバイル端末で SharePoint をデータソースとしたキャンバスアプリを利用する際に、画像やメディアファイルが表示されない問題について、下記項目の順にご説明いたします。

1. 問題の概要
2. 発生原因
3. 対処方法
4. その他の原因について (参考)
5. 参考情報

<!-- more -->
## 1. 問題の概要
SharePoint リストをデータソースとしたキャンバスアプリにおいて、以下のような事象が発生することがございます。

- **Web ブラウザ (PC)** でアプリを実行すると画像やメディアが正常に表示される
- **Power Apps モバイルアプリ (iOS/Android)** で同じアプリを実行すると画像やメディアが表示されない
- **Teams デスクトップ/モバイルアプリ** 内で公開されたアプリでも同様の問題が発生する

## 2. 発生原因
### 2.1 クロスオリジンでの認証情報の問題

Power Apps（`*.powerapps.com`）と SharePoint（`*.sharepoint.com`）は**異なるオリジン（ドメイン）** です。  
Power Apps から SharePoint の画像を取得しようとすると、クロスオリジンリクエストとなり、**SharePoint オリジンの認証情報が必要**になります。

### 2.2 なぜブラウザでは表示されるのか

Web ブラウザでアプリを実行する場合、ユーザーが以前に SharePoint サイトにログインしていると、ブラウザには `*.sharepoint.com` ドメインの **Cookie（認証情報）** が保存されています。  
Power Apps から SharePoint への画像リクエスト時、ブラウザはこの Cookie を自動的に送信するため、SharePoint が認証済みリクエストとして処理し、画像を返します。

プライベートブラウジングモードや、SharePoint に事前にログインしていない状態では、ブラウザでも画像が表示されない場合がございます。

### 2.3 なぜモバイルアプリでは表示されないのか

Power Apps モバイルアプリはネイティブアプリであり、ブラウザとは独立しています。  
そのため、**SharePoint オリジン（`*.sharepoint.com`）の認証情報（Cookie）を持っていません。**

認証情報なしでリクエストが送信されるため、SharePoint がリクエストを拒否し、画像が表示されません。

### 2.4 Power Apps で URL 書き換えが行われないケース

Power Apps は、SharePoint リストの「画像 (Image)」列タイプのデータを、Power Apps で認証できる形式に URL 書き換えを行います。この書き換えが行われた URL であれば、Power Apps が認証を代行するため、モバイルアプリでも画像が表示されます。

ただし、以下のケースでは URL 書き換えが行われないため、モバイルアプリで画像が表示されません。

| ケース | 説明 |
|-------|------|
| テキスト列に画像 URL を保存している | 「画像 (Image)」列タイプではないため、URL 書き換えが行われません |
| 異なるサイトの画像を参照している | リストがサイト A にあり、画像がサイト B にある場合、URL 書き換えがサポートされていません |
| ドキュメントライブラリ内の画像を参照している | Power Apps は SharePoint リスト単位で認証を行うため、同一サイト内でも別の場所にある画像にはアクセスできない場合があります |

## 3. 対処方法
### 対処方法 1: モバイルブラウザから Power Apps にアクセスする

Power Apps モバイルアプリではなく、モバイルデバイスのブラウザを使用してアプリを実行します。

1. モバイルブラウザで、画像が保存されている **SharePoint サイトに先にアクセス** し、サインインします。  
   これにより、ブラウザに SharePoint の認証 Cookie が保存されます。
2. 同じモバイルブラウザで [Power Apps ポータル](https://make.powerapps.com) にアクセスし、アプリを実行します。

> [!NOTE]
> この方法では、Power Apps モバイルアプリの機能 (カメラ、GPS など) の一部が制限される場合がございます。

### 対処方法 2: 「画像 (Image)」列タイプを使用し、同一サイトに画像を保存する

SharePoint リストで「画像 (Image)」列タイプを使用し、画像を **リストと同一の SharePoint サイト** に保存してください。  
この条件を満たすと、Power Apps が URL 書き換えを行い、モバイルアプリでも画像が表示されます。

- テキスト列に URL を保存するのではなく、「画像 (Image)」列タイプを使用してください
- 画像はリストと同じ SharePoint サイト内に保存してください
- 異なるサイトに保存された画像は Power Apps による URL 書き換えが行われないため表示されません

### 対処方法 3: ファイルを SharePoint リストの添付ファイルとして保存する

画像やメディアファイルをリストアイテムの **添付ファイル (Attachments)** として保存する方法です。  
添付ファイルは Power Apps で認証が行われるため、モバイルアプリでも表示されます。

1. SharePoint リストでアイテムを作成し、画像ファイルを添付ファイルとして追加します。
2. Power Apps で添付ファイルを参照するには、以下のような数式を使用します。

   **Image コントロールの Image プロパティ設定例:**
   ```
   First(Gallery1.Selected.Attachments).Value
   ```

### 対処方法 4: CORS 対応の外部ストレージに画像を保存する

画像を SharePoint ではなく、**Azure Storage** や **Azure CDN** などの CORS 対応ストレージに保存する方法です。  
これらのストレージは公開アクセス設定が可能で、認証なしで画像を取得できるため、モバイルアプリでも表示されます。

> [!NOTE]
> この方法では画像が公開される可能性があるため、セキュリティ要件を確認してください。

## 4. その他の原因について (参考)
本記事で説明した SharePoint の認証問題とは別に、以下の要因で同様の事象が発生する場合がございます。

### 外部サーバー (Azure Storage 等) の CORS 設定
SharePoint ではなく Azure Storage や独自のサーバーに画像を保存している場合、**CORS (Cross-Origin Resource Sharing)** の設定が必要です。

外部サーバーで CORS を構成する場合、以下のドメインからのリクエストを許可してください。
- `*.powerapps.com`
- `web.powerapps.com`
- `apps.powerapps.com`

### プロキシによるヘッダー削除
Zscaler、Blue Coat などのプロキシサーバーが Power Apps のリクエストから認証ヘッダーを削除することがございます。  
これにより、HTTP ステータスコード 0 として処理されることがございます。  
プロキシを経由しない場合に問題が解消するかご確認ください。

> [!NOTE]
> プロキシが認証ヘッダーを削除している場合、Power Apps チームではサポートの対象外となります。

## 5. 参考情報
詳細については、以下の公開情報をご参照ください。

- [キャンバス アプリでマルチメディア ファイルを使用する](https://learn.microsoft.com/ja-jp/power-apps/maker/canvas-apps/add-images-pictures-audio-video)
- [リッチ テキスト エディター コントロール](https://learn.microsoft.com/ja-jp/power-apps/maker/canvas-apps/controls/control-richtexteditor#pasted-images-may-not-appear-consistently)
- [SharePoint コネクタの制限事項](https://learn.microsoft.com/ja-jp/connectors/sharepointonline/#limits)


---
免責事項  
※本情報の内容 (添付文書、リンク先などを含む) は、作成日時点でのものであり、予告なく変更される場合があります。
