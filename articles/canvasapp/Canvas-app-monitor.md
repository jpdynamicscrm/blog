---
title: キャンバス アプリのモニターログ取得手順
date: 2021-06-17 00:00:00
tags:
  - Power Apps
  - Canvas app
  - モニター
  - 情報採取
  - お問い合わせ全般
categories:
  - [Power Platform, Power Apps, キャンバスアプリ]
---

こんにちは、Power Platform サポートの清水です。
今回は、Power Apps のキャンバス アプリの調査の際に必要なモニター ログの取得方法についてご説明します。
<!-- more -->

モニター ログは、アプリの編集時のものと再生時のものがどちらも取得可能ですので、状況に応じて必要なログを取得してください。

## 編集画面でのモニター ログ取得方法

**[手順]**

1. アプリを編集画面から開き、[高度なツール] ＞ [モニターを開く] をクリックします。
![](./Canvas-app-monitor/img01.png)
2. 別タブにてモニターが開きますので、事象発生部分を編集モードで実行します。
3. ログの記録が確認できたら、モニター画面にて [ダウンロード] をクリックします。
![](./Canvas-app-monitor/img02.png)
4. Json ファイルがダウンロードされますので、弊社までお寄せください。

## 再生画面でのモニター ログ取得方法

**事前準備**: アプリ再生時の情報を取得するために、アプリを編集画面で開き、  
[設定] > [全般] にて「公開されたアプリのデバッグ」をオンに設定します。
![](./Canvas-app-monitor/img03.png)

**[手順]**

1. エラーが発生するユーザーにて Power Apps にサインインし、[アプリ] をクリックします。
2. エラーの発生するアプリの […] > [監視] をクリックします。
![](./Canvas-app-monitor/img04.png)
3. 別タブにてモニターが開きますので、[公開したアプリの再生] をクリックします。
![](./Canvas-app-monitor/img05.png)
4. アプリが再生されるので、事象発生部分を実行します。
5. ログの記録が確認できたら、モニター画面にて [ダウンロード] をクリックします。
![](./Canvas-app-monitor/img02.png)
6. Json ファイルがダウンロードされますので、弊社までお寄せください。

### 参考情報

[モニターの概要 | Microsoft Docs](https://learn.microsoft.com/ja-jp/power-apps/maker/monitor-overview)
[モニターでキャンバス アプリをデバッグする | Microsoft Docs](https://learn.microsoft.com/ja-jp/power-apps/maker/monitor-canvasapps)

---
