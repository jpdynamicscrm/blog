---
title: 非アクティブな Microsoft Dataverse for Teams 環境の自動削除について
date: 2022-09-06 12:00:00
tags:
  - PowerApps
  - PowerPlatform
  - Dataverse
  - DataverseforTeams 
---

こんにちは、Power Platform サポートの山田です。<br>
今回は、Dataverse for Teams 環境の自動削除について、下記項目の順にご説明いたします。  
・Dataverse for Teams環境の自動無効化/削除メカニズムとは  
・自動無効化/削除条件  
・自動無効化/削除メカニズム自体の無効化の可否  
・無効化/削除された環境の復元  
・ご案内メール  
・ご案内メールを受信された際の対応   
・よくあるご質問  
  
下記公開情報を基にご説明いたします。   
[非アクティブな Microsoft Dataverse for Teams 環境の自動削除](https://docs.microsoft.com/ja-jp/power-platform/admin/inactive-teams-environment)   
各項目について公開情報内にも詳細を記載しておりますのでご参照ください。   
  
なお、対象はあくまでDataverse for Teams環境となります。   
環境が紐づいているチーム自体が削除されるわけではございませんので、ご安心ください。   
  
<!-- more -->  
## **<初めに -本メカニズムの停止について->**
本メカニズムのメール通知について多数のお客様からのお問い合わせをいただいており、  
メール通知の内容に分かりづらい点が多く混乱を招く状況となっております。  
<br>
そのため、2022年9月14日時点、すでに展開されているDataverse for Teams環境の無効化に関する本メカニズムは停止されております。  
本メカニズムの公開は一旦延期されることになりました。  
<br>
メール通知を受け取られたお客様には大変ご心配をおかけしておりますが、送付されましたDataverse for Teams環境の削除に  
関する通知につきましては、無視いただいても問題ございませんのでご安心ください。  
<br>
改めて本メカニズムが公開される際にはメッセージセンター等にて通知させていただく予定でございますので、  
その際にはご確認いただけますと幸いでございます。  
<br>

なお、本メカニズムの停止に伴いまして、後述の「環境活動をトリガーする」機能も無効化されております。  
しかしながら、Power Platform 管理センター上の無効化の警告、および「環境活動をトリガーする」機能は表示されたままとなっており、  
「環境活動をトリガーする」を実行しても無効化の警告表示が消えないとのお問い合わせを受けております。  
本メカニズム自体が停止されているため、本来は無効化の警告自体も表示されないべきですが、  
本記事執筆時点では警告が表示されたままとなってしまっております。  
<br>
ご混乱をお招きし大変申し訳ございません。  
無効化の警告が表示されていても無視していただいて問題ございませんので、ご安心ください。  
<br>

再度本メカニズムが有効化された際には、その7日後に対象環境が無効化される動きとなる予定でございますので、  
再有効化後に「環境活動をトリガーする」をご実行いただけますと幸いでございます。  
<br>

下図は本メカニズムの停止に関するメッセージセンターの通知となります。  
![](./Automatic-deletion-of-inactive-Microsoft-Dataverse-for-Teams-environments/message-center-rollout.png)  
<br>


## **<Dataverse for Teams環境の自動無効化/削除メカニズムとは>**
本メカニズムは2022年８月末より徐々に展開されております。  
設定された一定期間（非アクティブ期間）中、ユーザーによる活動のないDataverse for Teams環境が無効化されます。  
無効化状態が30日継続しますと、対象環境が自動削除されます。<br>

無効化された環境では、アプリやフロー、チャットボット等がご利用いただけません。<br>

設定可能な非アクティブ期間は次の通りでございます。  
・15 日、30 日、45 日、60 日、および 90 日間<br>

デフォルトでは90日となっております。<br>

ただし、現時点では期間変更機能は未リリースであり、今後追ってリリースされる予定でございます。  
恐れ入りますが、機能の展開をお待ちくださいますようお願い申し上げます。
<br>

## **<Dataverse for Teams環境の自動無効化/削除条件>**  
対象環境におけるユーザーのアプリ起動などの活動が設定期間中に実施されない場合、自動削除の対象となります。<br>
対象の活動は下記の通りです。<br>
・ユーザーの活動: アプリの起動、フローの実行、Power Virtual Agents ボットとのチャット<br>
・作成者の活動: アプリ、フロー 、Power Virtual Agents ボット、カスタムコネクタの作成、読み取り、更新、削除<br>
・管理者の活動: コピー、削除、バックアップ、復元、リセットなどの環境操作<br>
<br>

## **<Dataverse for Teams環境の自動無効化/削除メカニズム自体の無効化の可否>**<br>
本メカニズムは無効化いただくことが出来ません。<br>
何卒ご理解いただけますと幸いでございます。<br>
<br>

## **<自動無効化/削除された環境の復元>** <br>
環境無効化後、30日以内であれば環境の再有効化が可能でございます。<br>
また、環境削除後も、7日以内であれば環境の復元が可能でございます。<br>
どちらもPower Platform 管理センターよりご実施いただけます。<br>

・無効にした Dataverse for Teams 環境の再有効化<br>
①Power Platform 管理センター にサインイン<br>
②環境　＞　無効化された Dataverse for Teams 環境を選択<br>
③環境ページで 「環境を再度有効にする」 を選択<br>

・削除された Dataverse for Teams 環境の復元<br>
①Power Platform 管理センター にサインイン<br>
②環境　＞　削除された環境を復元する　を選択<br>
③回復する環境　＞　回復 を選択<br>
<br>
 　　  
## **<ご案内メール>** <br>
対象環境の無効化が実施される７日前より、その旨についてのご案内メールが次のユーザーに通知されます。<br>
・対象Dataverse for Teams環境のチームの所有者<br>
・対象Dataverse for Teams環境の作成者<br>
・システム管理者<br><br>
   
環境が無効化される旨のご案内メールは下図のようなものになります。<br>
黒塗りのマスキング部分に対象環境名が記載されます。<br>
![](./Automatic-deletion-of-inactive-Microsoft-Dataverse-for-Teams-environments/announce-mail.jpg)
<br><br>

## **<無効化対象のご案内メールを受信された際の対応>**<br>
下記2通りの方法にて、無効化対象となる非アクティブな期間のカウントを一旦リセットいただけます。<br>
・対象環境において、アプリやフローのご実行など上述のユーザー活動を実施<br>
・Power Platform 管理センターの対象環境ページにて、「環境活動をトリガーする」を実行<br>   
![](./Automatic-deletion-of-inactive-Microsoft-Dataverse-for-Teams-environments/trigger-button.png)<br><br>

## **<よくあるご質問>** <br>
Q：メッセージセンターの番号は?<br>
A：MC415083 となります。<br>
該当メッセージURL:https://admin.microsoft.com/adminportal/home?ref=MessageCenter/:/messages/MC415083<br>
![](./Automatic-deletion-of-inactive-Microsoft-Dataverse-for-Teams-environments/message-center.png) 
<br><br>

Q：Dataverse for Teams環境とは何か。無効化された場合、どのような影響があるか?<br>
A：Microsoft Dataverse for Teams 環境は、チームにDataverse機能を使用したアプリを追加した場合や、<br>
チーム内にチャットボットやPower Appsキャンバスアプリ等を作成した場合に作成される環境です。<br>
無効化されると、環境内に作成されていたアプリやフロー、チャットボット等がご利用いただけなくなります。<br>
具体的なシナリオや詳細について、以下の記事にて解説しておりますので、よろしければご参照ください。<br>
[Dataverse for Teams 環境の管理方法](https://jpdynamicscrm.github.io/blog/powerplatform/Manage-Dataverse-for-Teams/)
<br><br>

Q：Dataverse for Teams環境にて無効化される際に配信される通知メールを配信停止することは可能か?<br>
A：恐れ入りますが、メール配信を停止することのできる機能はございません。<br>
製品動作としてご理解賜りますようお願い申し上げます。<br>
<br>

Q：Power Platform 管理センターの対象環境ページにて、「環境活動をトリガーする」の実効は、通常ユーザにて操作可能なものか?<br>
A：Power Platform管理センターにてDataverse for Teams環境の「環境活動をトリガーする」を実行できるユーザーは以下の通りでございます。<br>
・対象Dataverse for Teams環境のチームの所有者<br>
・対象Dataverse for Teams環境の作成者<br>
・グローバル管理者<br>
・Power Platform管理者<br>
・Dynamics 365管理者<br>
<br>

Q：「環境活動をトリガーする」を実行すると、有効期限は何日まで延びるのか?<br>
A：有効期限は90日間延長され、その90日の期間中に、対象環境におけるユーザーのアプリ起動などの活動が実施されない場合、<br>
再び自動削除の対象となり、7日前（トリガー実行から83日後）にメールにて通知されます。<br>
<br>

Q：無効化される環境内のアプリ当の詳細については、どこから確認できるか?<br>
A：下記より対象環境内のアプリ等をご確認いただけます。<br>
Power Platform 管理センター　＞　環境　＞　対象環境　＞　リソース　＞　Power Apps/フロー<br>
![](./Automatic-deletion-of-inactive-Microsoft-Dataverse-for-Teams-environments/resources.png)<br>
<br>
         
Q：再有効化した場合、無効化前の情報が維持されるか?<br>
A：再有効化後または復元後は、無効前の情報が維持されます。<br>
削除されてしまいますと、無効化前の情報は復元できなくなりますのでご注意ください。<br> 
<br>

免責事項<br>
※本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。<br>

