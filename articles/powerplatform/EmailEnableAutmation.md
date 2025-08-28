---
title: メールボックスの承認・テストと有効化の自動化について
date: 2023-08-02 00:00:00
tags:
  - Dynamics
  - Environment
  - Dynamics 365
categories:
  - [Dynamics 365]
---

# メールボックスの承認・テストと有効化の自動化について

こんにちは、Power Platform サポートチームの原です。
本記事では Dynamics 365における、メールボックスの承認・テストと有効化の自動化についてご紹介いたします。<br>また、メールボックスの承認・テスト有効化を自動化するための標準機能としてご提供している機能は現段階ではございませんが、代替案がございますので、本記事ではそちらの代替案をご紹介させていただきます。少しでもお客様の業務効率化に繋がりますと幸いです。

なお、弊社検証環境では下記にご紹介するコマンドにて、電子メールの承認やテストと有効化が実行可能なことを確認しておりますが、下記でご案内いたします Microsoft.Xrm.Data.PowerShell モジュールにつきましては、コミュニティにより
提供・メンテナンスされるツールでございますため、本モジュールをご利用いただく中での
エラーやご不明点に関しましては弊社からサポートをご提供することがかないません。<br>
そのため、ご確認いただく中で何か問題があった場合は、基本的には以下の New Issue より、Github 経由で問題を報告いただく必要がございます。
予めご了承いただきますようお願いいたします。
URL:https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell/issues

## はじめに
Dynamics 365では、Dynamics 365と Exchange Online の同期を行うことができます。これを行うことで、 Dynamics 365 のレコードから取引先担当者にメールを送信できるようになったり、営業案件のタイムラインで会話を追跡することができるようになります。<br>
メールボックスを有効化するためには、承認とテストが既定では必須となっております。
メールボックスの承認・テストと有効化を手動で行う方法については、<a href="https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online" target="_blank">Exchange Online への接続</a>をご参照ください。<br>


<a id='anchor-powershell'></a>
## PowerShell を使用して実行する
メールボックスの承認やテストと有効化は PowerShell で下記コマンドを入力することで実施いただくことが可能です。<br>
そのため、タスク スケジューラや Power Automate を利用しPowerShell を実行いただくことで、任意のタイミングで自動的にメールボックスの承認やテストと有効化を行うことが可能となります。<br>また、反復処理などで各ユーザーの IDを置き換える処理を実装いただくことで、複数のメールボックスに対して一括でメールボックスの承認やテストと有効化を行うことが可能です。<br>Power Automate を使用して PowerShell を実行する方法に関しては、
<a href="https://jpdynamicscrm.github.io/blog/powerautomate/Execute-PowerShell/#:~:text=Azure%20%E4%B8%8A%E3%81%A7%20PowerShell%20%E3%82%84%20Python%20%E3%81%AA%E3%81%A9%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%82%8B%E6%A9%9F%E8%83%BD%E3%81%A7%E3%81%99%E3%80%82%20%E3%81%93%E3%81%AE%20Runbook,Forms%20%E3%81%A7%20Microsoft%20365%20%E7%AE%A1%E7%90%86%E3%82%BB%E3%83%B3%E3%82%BF%E3%83%BC%20%E3%81%AE%E3%83%AD%E3%83%BC%E3%83%AB%E7%94%B3%E8%AB%8B%E3%82%92%E8%A1%8C%E3%81%84%E3%80%81%E6%89%BF%E8%AA%8D%E3%81%95%E3%82%8C%E3%81%9F%E3%82%89%E3%80%81%20PowerShell%20%E3%81%A7%E3%83%AD%E3%83%BC%E3%83%AB%E4%BB%98%E4%B8%8E%E3%81%99%E3%82%8B" target="_blank" rel="noopener noreferrer"> Power Automateを使用して PowerShell を実行する方法</a>
をご参照ください。

1. 管理者として PowerShell を起動します。
2. 以下のコマンドを実行し、 Microsoft.Xrm.Data.Poweshell のモジュールをダウンロードします。<br>
Install-Module Microsoft.Xrm.Data.PowerShell -Scope CurrentUser
3. 以下のコマンドを実行します。<br>
Import-Module Microsoft.Xrm.Data.Powershell
4.	以下のコマンドを実行します。<br>
Connect-CrmOnlineDiscovery -InteractiveMode
5.	PowerShell Interactive Login ウィザードが表示されますので、[Display list of available organizations] にチェックを入れ、[Login]をクリックし、サインインします。
7.	インスタンス一覧が出力されますので、当該インスタンスを選択し、[Login] をクリックします。
8.	ログイン完了後、以下のコマンドにより指定したユーザーの電子メールの承認が可能です。<br>
Approve-CrmEmailAddress  -UserId \<UserId>
9.	また、以下のコマンドによりユーザーのメールボックスに対してメールボックスのテストと有効化を実行することができます。<br>
Set-CrmUserMailbox -UserId \<UserId>  -ScheduleTest

## 補足
__メールボックスの承認を不要にする__<br>
メールボックスを有効化するにあたって、メールボックスの承認は、既定では必須となっております。<br>
ただし、こちらの承認作業は、管理者にて設定を変更いただくことで、要件を削除することが可能となっております。<br>
詳しい手順につきましては、<a href="https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online#remove-the-requirement-to-approve-mailboxes" target="_blank">メールボックスを承認するための要件の削除</a>をご参照ください。
<br>
また、メールボックスを承認するための要件を削除せずに、自動化をされたい場合は、[  PowerShell を使用して実行する](#anchor-powershell)をご検討ください。

## 終わりに
本記事では Dynamics 365における、メールボックスの承認・テストと有効化についてご紹介いたしました。メールボックスを管理する上でのお役に立てましたら幸いです。ご不明な点や、お困りのことなどがございましたら、弊社サポート一同にてお待ち申し上げておりますので、ぜひお気軽にお問合せください。<br><br>
※本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。

## 参考情報
[Dataverse と Exchange Online のサーバー側同期](https://jpdynamicscrm.github.io/blog/powerplatform/exchange-server-side-sync/)