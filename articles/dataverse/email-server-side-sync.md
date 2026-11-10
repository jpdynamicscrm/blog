---
title: Dataverse と Exchange Online のサーバー側同期
date: 2024-12-09 00:00:00
tags:
  - Dataverse
  - Exchange
  - Power Platform
categories:
  - [Power Platform Admin Center]
  - [Dataverse]
---

こんにちは、Power Platform サポート チームの鎌田です。
Dynamics 365 で使用される Dataverse と Outlook のメール サーバーである Exchange サーバーを同期させることで、予定、連絡先、およびタスクを Dynamics 365 と Outlook でシームレスに連携できます。
本記事では、Dataverse と Exchange Online のサーバー側同期についてご案内いたします。

<!-- more -->

<a id='content'></a>

## 目次

- [Power Platform 環境へのメールボックスの同期](#sync-mailbox-to-power-platform-environment)
- [Dynamics 365 App for Outlook](#dynamics-365-app-for-outlook)
- [メールの追跡](#sync-email)
- [予定の追跡](#sync-shedule)
- [個人設定](#personal-settings)
- [Dynamics 365 から電子メールを送信](#send-email)
- [補足](#additional-information)
- [トラブルシューティング](#troubleshooting)
- [参考情報](#references)

<a id='sync-mailbox-to-power-platform-environment'></a>

## Power Platform 環境へのメールボックスの同期

Power Platform 管理センターからメールボックスを同期することで、Outlook のメールを Dynamics 365 アプリで閲覧できたり、Outlook と同じように Dynamics 365 アプリからメールを送信できたりするようになります。

1. [Power Platform 管理センター](https://admin.powerplatform.microsoft.com/environments) より環境を開く
2. [設定] > [サーバー プロファイル] をクリック<br>![](./email-server-side-sync/ppac_setting_server_profile.png)
3. Microsoft Exchange Online (既定) が存在することを確認<br>![](./email-server-side-sync/ppac_server_profile.png)
4. [設定] に戻り [メールボックス] をクリック<br>![](./email-server-side-sync/ppac_setting_mailbox.png)

5. 同期したいメールボックスの [名前] をクリック<br>![](./email-server-side-sync/mailbox2.png)
6. 受信メール、送信メール、(メール以外も同期が必要な場合は) 予定、取引先担当者、タスクを [サーバー側同期] に変更する<br>![](./email-server-side-sync/server_side_sync2.png)

7. 画面上部の ① [電子メールの承認] をクリック、ポップアップが表示されたら [OK] をクリック
<br>![](./email-server-side-sync/approve_test2.png)

8. ステップ 7 の画像の ② [メールボックスのテストと有効化] をクリック、ポップアップが表示されたら受信開始日を設定して [OK] をクリック

9. 受信メール、送信メール、(メール以外も同期が必要な場合は) 予定、取引先担当者、タスクが [成功] になれば完了
<br>![](./email-server-side-sync/success_check3.png)

10. Outlook にもサーバー側同期の完了を通知するメールが届いていることを確認<br>![](./email-server-side-sync/outlook_notification.png)




<a id='dynamics-365-app-for-outlook'></a>

## Dynamics 365 App for Outlook

Dynamics 365 App for Outlook は Outlook から Dynamics 365 のレコードを閲覧、作成、編集、削除などができるアプリ (アドイン) です。Outlook から Dynamics 365 での追跡時の関連を設定したり、テーブルへレコードを作成したりできます。

1. Power Platform 管理センターより [設定] > [Dynamics 365 App for Outlook] をクリック<br>![](./email-server-side-sync/ppac_setting_d365afo.png)
2. Dynamics 365 App for Outlook を追加するユーザーにチェックを入れ、[OUTLOOK へのアプリの追加] をクリック (対象ユーザーに割り当てられているセキュリティ ロールの中に use dynamics 365 app for outlook の特権がついている必要があります)<br>![](./email-server-side-sync/d365afo_sync.png)
3. [状態] が [保留中] から [Outlook に追加済み] になったら完了 (完了までに 10 分から 30 分程度時間がかかります)<br>![](./email-server-side-sync/d365afo_done.png)
4. [Outlook](https://outlook.office365.com/mail/) でメールを開き、[Dynamics 365] のアプリのアイコンが表示されることを確認
(反映されるまで 1 日かかる場合があります)
<br>![](./email-server-side-sync/outlook_d365afo_icon.png)
5. 初めて開くと認証画面が表示されるので、[許可] をクリックして認証<br>![](./email-server-side-sync/outlook_d365afo_auth.png)

<a id='sync-email'></a>

## メールの追跡

以下のいずれかの方法でメールを追跡できます。

- 追跡したいメールを選択した状態で **分類** をクリックし、Tracked To Dynamics 365 の分類を付与することで、対象のメールを追跡できます。Dynamics 365 へ正しく連携できなかった場合、分類のタグが Tracked To Dynamics 365 (Undeliverable) になります。<br>![](./email-server-side-sync/outlook_classification.png)

- Dynamics 365 App for Outlook を利用してメールを追跡することもできます。追跡対象外のメールで [関連を設定せずに追跡] とすると上の方法と同じく Tracked To Dynamics 365 の分類タグがつき、メールを追跡できます。<br>![](./email-server-side-sync/outlook_d365afo_track_mail.png)

Dynamics 365 アプリの [営業ハブ] > [活動] > [すべての電子メール] より、上記の方法で追跡した電子メール レコードが確認できます。同期には 5 分から 15 分程度かかる場合があります。
<br>![](./email-server-side-sync/d365_mail_record.png)

<a id='sync-shedule'></a>

## 予定の追跡

Outlook より [予定表] を開き、追跡対象の予定を右クリックして先ほどのメールの追跡と同様に Tracked To Dynamics 365 の分類を付与することで、対象の予定を追跡できます。<br>![](./email-server-side-sync/outlook_calender_sync.png)
Dynamics 365 アプリの [営業ハブ] > [活動] > [すべての予定] より、上記の方法で追跡した予定レコードが確認できます。同期には 5 分から 15 分程度かかる場合があります。<br>![](./email-server-side-sync/d365_schedule_record.png)

<a id='personal-settings'></a>

## 個人設定

Dynamics 365 アプリの個人設定の [電子メール] タブから設定できる項目についてご紹介します。<br>![](./email-server-side-sync/personalization_settings.png)

<a id='track'></a>

### [Microsoft Dynamics 365 で追跡する電子メール メッセージを選択してください] > [追跡]

同期されたメールボックスのうち、どの電子メール メッセージを追跡するかを設定できます。既定で [Dynamics 365 電子メールに対する返信の電子メール メッセージ] に設定されています。<br>![](./email-server-side-sync/personalization_settings_auto_creation.png)

<a id='configure-folder-tracking-rules'></a>

### [Microsoft Dynamics 365 で追跡する電子メール メッセージを選択してください] > [フォルダー追跡ルールの構成]

Outlook の指定したフォルダーと Dynamics 365 の特定のレコードを紐づけられます。例えば以下のように test というフォルダーのメールを test company という取引先企業と連携したい場合などです。<br>![](./email-server-side-sync/personalization_settings_folder_level_track.png)<br>これにより、以下のように test company という取引先企業のレコードに test というフォルダー内のメールが連携され、タイムラインから閲覧できるようになります。<br>![](./email-server-side-sync/d365_folder_level_track.png)<br>上記の機能を有効にするためには、以下の通り Power Platform 管理センターで [設定] > [電子メールの追跡] > [フォルダー レベルの追跡] をオンにする必要があります。<br>![](./email-server-side-sync/ppac_track_email.png)

<a id='auto-create-records'></a>

### [Microsoft Dynamics 365 でのレコードの自動作成]

[Microsoft Dynamics 365 でのレコードの自動作成] では、追跡するメールの送信者を取引先担当者またはリードとして、レコードを自動作成できます。例えば、取引先担当者を自動作成するように設定した場合、[メールの追跡](#sync-email) の操作により、そのメールの送信者である Microsoft Power Platform が取引先担当者として自動で作成されます。<br>![](./email-server-side-sync/d365_contact_record.png)

<a id='send-email'></a>

## Dynamics 365 から電子メールを送信

これまで紹介してきた操作は、Exchange から Dataverse への同期でしたが、逆に Dataverse から Exchange への同期もできるため、Dynamics 365 アプリから電子メールを送信することもできます。<br>[活動] > [電子メール] とクリックすると下図のように [新しい電子メール] の画面が開きます。宛先や件名、本文を入力し、[送信] ボタンを選択することで同期しているメールボックスからメールを送信できます。正しく送信されているかどうかは Outlook の送信済みアイテムより確認できます。
<br>![](./email-server-side-sync/d365_send_mail.png)

<a id='additional-information'></a>

## 補足

- [Power Platform 環境へのメールボックスの同期](#sync-mailbox-to-power-platform-environment) のステップ 9 のチェックボックスは、これから同期するメールボックスを別の環境で同期したときに、現在の環境の接続を解除して新しい環境に同期しなおすためのものです。メールボックスは 1 つの環境にしか同期できません。

- Dynamics 365 アプリ内の活動テーブルにて電子メール レコードを削除しても、Outlook 内の同期元メールは削除されません。また、Outlook から追跡中の電子メールを削除しても、Dynamics 365 アプリ側の電子メール レコードは削除されません。<br>参考: [Dynamics 365 for Outlook で追跡されたレコードを削除する > 電子メール メッセージ](https://learn.microsoft.com/ja-jp/dynamics365/outlook-addin/user-guide/delete-records-that-have-been-tracked#email-messages)

- 同期済みのメールボックスに対して同期を解除したい場合、[Power Platform 環境へのメールボックスの同期](#sync-mailbox-to-power-platform-environment) のステップ 6 の画面で [同期方法] のうち連携を解除したい項目を [なし] に設定し、[上書き保存] を選択します。

- [Power Platform 環境へのメールボックスの同期](#sync-mailbox-to-power-platform-environment) のステップ 7 の電子メールの承認は、[Power Platform 管理センター] > [設定] > [セキュリティ ロール] より [代理メールボックス承認者] を付与することで、グローバル管理者または Exchange 管理者でなくても、システム管理者セキュリティ ロールを持っていれば代理で実施できるようになります。<br>参考: [Exchange Online への接続 > アクセス許可モデル](https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online#permissions-model)<br>![](./email-server-side-sync/ppac_security_role.png)

<a id='troubleshooting'></a>

## トラブルシューティング

Dynamics 365 App for Outlook によりメールを Dataverse に同期する際に、"[追跡] と [関連を設定] は現在無効になっています..." というメッセージが表示され正常に同期できない場合の対処手順をご紹介します。

1. ブラウザーの InPrivate モードで期待通りの動作となるか確認
2. 期待通りの動作とならない場合、ユーザーに割り当てられているセキュリティ ロールの中に use dynamics 365 app for outlook の特権がついていることを確認<br>![](./email-server-side-sync/ppac_security_role_d365afo.png)
3. 再度 [Power Platform 環境へのメールボックスの同期](#sync-mailbox-to-power-platform-environment) と [Dynamics 365 App for Outlook](#dynamics-365-app-for-outlook) の設定を実施
4. ステップ 1 で期待通りの動作となっている場合、下図のように Outlook のアプリ管理画面から Dynamics 365 App for Outlook の接続先の環境を確認<br>![](./email-server-side-sync/outlook_d365afo_env.png)
5. Power Platform 管理センターから同期を設定した環境と異なる環境が接続対象となっている場合、ブラウザーの設定より、[Cookie とサイトのアクセス許可] > [Cookie とサイト データ] > [すべての Cookie とサイト データを表示する] から Outlook に関連するキャッシュを削除 (Edge の場合)<br>![](./email-server-side-sync/edge_cookie_delete.png)
6. 事象が解決しない場合、サポート窓口にお問い合わせください

<a id='references'></a>

## 参考情報

- [電子メール、予定、取引先担当者、およびタスクのサーバー側同期の設定](https://learn.microsoft.com/ja-jp/power-platform/admin/set-up-server-side-synchronization-of-email-appointments-contacts-and-tasks)
- [Exchange Online への接続](https://learn.microsoft.com/ja-jp/power-platform/admin/connect-exchange-online)
- [Dynamics 365 App for Outlook の展開およびインストール](https://learn.microsoft.com/ja-jp/dynamics365/outlook-app/deploy-dynamics-365-app-for-outlook)
- [Outlook の分類項目を使って、予定と電子メールを追跡する](https://learn.microsoft.com/ja-jp/power-platform/admin/use-outlook-category-track-appointments-emails)
