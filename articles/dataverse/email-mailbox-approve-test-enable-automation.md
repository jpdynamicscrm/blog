---
title: メールボックスの承認・テストと有効化の自動化について
date: 2023-08-02 00:00:00
tags:
  - Dynamics
  - Environment
  - Dynamics 365
categories:
  - [Dynamics 365]
  - [Dataverse]
---

# メールボックスの承認・テストと有効化の自動化について

こんにちは、Power Platform サポートチームの原です。
本記事では Dynamics 365 における、メールボックスの承認・テストと有効化の自動化についてご紹介します。<br>また、メールボックスの承認・テストと有効化を自動化するための標準機能は、現時点では提供していません。そこで、本記事では代替案をご紹介します。少しでもお客様の業務効率化につながりますと幸いです。

なお、弊社検証環境では下記にご紹介するコマンドにて、電子メールの承認やテストと有効化を実行できることを確認していますが、下記でご案内する Microsoft.Xrm.Data.PowerShell モジュールは、コミュニティにより提供・メンテナンスされるツールのため、本モジュールをご利用いただく中でのエラーやご不明点に関して、弊社からサポートを提供することはできません。<br>
そのため、ご確認いただく中で何か問題があった場合は、基本的には以下の New Issue より、GitHub 経由で問題を報告いただく必要があります。
あらかじめご了承いただきますようお願いいたします。
[Microsoft.Xrm.Data.PowerShell の Issues (GitHub)](https://github.com/seanmcne/Microsoft.Xrm.Data.PowerShell/issues)

<!-- more -->

## はじめに
Dynamics 365 では、Dynamics 365 と Exchange Online を同期できます。これにより、Dynamics 365 のレコードから取引先担当者にメールを送信できるようになったり、営業案件のタイムラインで会話を追跡できるようになったりします。<br>
メールボックスを有効化するためには、承認とテストが既定では必須となっています。
メールボックスの承認・テストと有効化を手動で行う方法については、[Exchange Online への接続](https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online)をご参照ください。<br>


<a id='anchor-powershell'></a>
## PowerShell を使用して実行する
メールボックスの承認やテストと有効化は PowerShell で下記コマンドを入力することで実施できます。<br>
そのため、タスク スケジューラや Power Automate を利用して PowerShell を実行することで、任意のタイミングで自動的にメールボックスの承認やテストと有効化を行えます。<br>また、反復処理などで各ユーザーの ID を置き換える処理を実装することで、複数のメールボックスに対して一括でメールボックスの承認やテストと有効化を行えます。<br>Power Automate を使用して PowerShell を実行する方法に関しては、
[Power Automate を使用して PowerShell を実行する方法](https://jpdynamicscrm.github.io/blog/powerautomate/Execute-PowerShell/#:~:text=Azure%20%E4%B8%8A%E3%81%A7%20PowerShell%20%E3%82%84%20Python%20%E3%81%AA%E3%81%A9%E3%82%92%E5%AE%9F%E8%A1%8C%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%82%8B%E6%A9%9F%E8%83%BD%E3%81%A7%E3%81%99%E3%80%82%20%E3%81%93%E3%81%AE%20Runbook,Forms%20%E3%81%A7%20Microsoft%20365%20%E7%AE%A1%E7%90%86%E3%82%BB%E3%83%B3%E3%82%BF%E3%83%BC%20%E3%81%AE%E3%83%AD%E3%83%BC%E3%83%AB%E7%94%B3%E8%AB%8B%E3%82%92%E8%A1%8C%E3%81%84%E3%80%81%E6%89%BF%E8%AA%8D%E3%81%95%E3%82%8C%E3%81%9F%E3%82%89%E3%80%81%20PowerShell%20%E3%81%A7%E3%83%AD%E3%83%BC%E3%83%AB%E4%BB%98%E4%B8%8E%E3%81%99%E3%82%8B)
をご参照ください。

1. 管理者として PowerShell を起動します。
2. 以下のコマンドを実行し、Microsoft.Xrm.Data.PowerShell のモジュールをインストールします。

   ```powershell
   Install-Module Microsoft.Xrm.Data.PowerShell -Scope CurrentUser
   ```

3. 以下のコマンドを実行します。

   ```powershell
   Import-Module Microsoft.Xrm.Data.PowerShell
   ```

4. 以下のコマンドを実行します。

   ```powershell
   Connect-CrmOnlineDiscovery -InteractiveMode
   ```

5. PowerShell Interactive Login ウィザードが表示されますので、[Display list of available organizations] にチェックを入れ、[Login] を選択し、サインインします。
6. インスタンス一覧が出力されますので、当該インスタンスを選択し、[Login] を選択します。
7. サインイン完了後、以下のコマンドにより指定したユーザーの電子メールの承認ができます。

   ```powershell
   Approve-CrmEmailAddress -UserId <UserId>
   ```

8. また、以下のコマンドによりユーザーのメールボックスに対してメールボックスのテストと有効化を実行できます。

   ```powershell
   Set-CrmUserMailbox -UserId <UserId> -ScheduleTest
   ```

## 補足
**メールボックスの承認を不要にする**<br>
メールボックスを有効化するにあたって、メールボックスの承認は、既定では必須となっています。<br>
ただし、承認の要件は、管理者が設定を変更することで削除できます。<br>
詳しい手順につきましては、[メールボックスを承認するための要件の削除](https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online#remove-the-requirement-to-approve-mailboxes)をご参照ください。
また、メールボックスを承認するための要件を削除せずに、自動化したい場合は、[PowerShell を使用して実行する](#anchor-powershell)をご検討ください。

## 終わりに
本記事では Dynamics 365 における、メールボックスの承認・テストと有効化についてご紹介しました。メールボックスを管理する上でのお役に立てましたら幸いです。ご不明な点や、お困りのことなどがありましたら、弊社サポート一同でお待ちしておりますので、ぜひお気軽にお問い合わせください。

## 参考情報
[Dataverse と Exchange Online のサーバー側同期](https://jpdynamicscrm.github.io/blog/dataverse/email-server-side-sync/)