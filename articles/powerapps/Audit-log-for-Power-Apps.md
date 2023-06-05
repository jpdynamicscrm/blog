---
title: 監査ログでのアプリ実行履歴等の確認について
date: 2022-09-14 12:00:00
tags:
  - Power Apps
  - Audit Log
---

こんにちは、Power Platform サポートの山田です。<br>
今回は、監査ログでの Power Apps の利用状況確認について、下記項目の順にご説明いたします。  

1. 監査ログ  
2. ログの出力  
3. 監査ログ利用方法    
4. 監査ログの注意点
  
<!-- more -->  
## 1. 監査ログ
Microsoft 365 コンプライアンスの「監査ログ」機能でアプリの実行状況をご確認いただけます。  
監査ログにて、「アクティビティ」の「Power Apps アプリのアクティビティ」より、  
「アプリの起動」にて、アプリの各起動 (実行) ログ等をご確認いただけます。   
![](./Audit-log-for-Power-Apps/activity-menu.png)
<br>

ユーザー欄にて対象ユーザーをフィルターいただくことが可能でございます。  
![](./Audit-log-for-Power-Apps/user-selection.png)
<br>

## 2. ログの出力  
ログは下図のような形となります。  
各ログの「アイテム」欄が各アプリの詳細画面にてご確認いただけるアプリ ID を示しております。   
誰がいつ、どのアプリを実行したかご確認いただけます。  
各ログを選択いただきますと、より詳細な情報をご確認いただきます。  
アクティビティによって、詳細の中には IP や対象環境情報などが含まれておりますので、ご活用ください。  
![](./Audit-log-for-Power-Apps/item-id.png)  
![](./Audit-log-for-Power-Apps/app-id.png)
<br>

## 3. 監査ログ利用方法  
[Microsoft 365 コンプライアンス](https://compliance.microsoft.com/homepage)の "監査" にて、  
ログの時刻範囲を決定、アクティビティにて「アプリの○○」を選択し、検索します。   
![](./Audit-log-for-Power-Apps/classic-top.png)
<br>

※本記事執筆の 2022 年 9 月時点では「新しい検索 (プレビュー)」と「クラシック検索」の 2 種類がございますが、  
「新しい検索 (プレビュー)」はプレビュー機能のため、「クラシック検索」についてご案内いたします。
<br>

## 4. 監査ログの注意点 
監査ログの注意点として、下記公開情報に記載のように、   
イベントの発生からログ出力までの時差やログ保存期間の制限等ございますのでご留意ください。  
監査レコードは通常は 90 日間保持されます。  
Office 365 E5 または Microsoft E5 ライセンスがが割り当てられたユーザーのアクティビティの場合、  
ポリシーを設定することにより、1 年間まで保持されます。  
詳細は下記公開情報の「よく寄せられる質問」等をご参照ください。   
[Microsoft 365 コンプライアンス センターの監査ログ検索ツール - よく寄せられる質問](https://learn.microsoft.com/ja-jp/microsoft-365/compliance/audit-log-search?view=o365-worldwide#frequently-asked-questions)  
[既定の監査ログの保持ポリシー](https://learn.microsoft.com/ja-jp/microsoft-365/compliance/audit-log-retention-policies?view=o365-worldwide#default-audit-log-retention-policy)  
<br>

なお、初回時には下図例の「ユーザーと管理者のアクティビティの記録を開始する」ボタンが表示されます。   
ボタンを押して頂き、案内に従ってください。   
なお、こちらのボタンは一度では有効化されない場合がございます。   
下図のようなメッセージが表示される場合には、何度か押下して頂けますと幸いでございます。   
![](./Audit-log-for-Power-Apps/start-recording.png)  
![](./Audit-log-for-Power-Apps/confirm-refresh.png)  
![](./Audit-log-for-Power-Apps/try-again.png)
<br>

その他監査ログの詳細につきましては、下記公開情報をご参照ください。  
[コンプライアンス ポータルで監査ログを検索する](https://learn.microsoft.com/ja-jp/microsoft-365/compliance/audit-log-search?view=o365-worldwide#search-the-audit-log)  
[Power Apps のアクティビティ ログ](https://learn.microsoft.com/ja-jp/power-platform/admin/logging-powerapps)
<br>

免責事項  
※本情報の内容 (添付文書、リンク先などを含む) は、作成日時点でのものであり、予告なく変更される場合があります。<br>

