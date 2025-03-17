---
title: キャンバス アプリ所有者の退職・異動への備え
date: 2022-02-17 00:00:00
tags:
  - Canvas app
  - Power Apps
  - PowerShell
categories:
  - [Power Apps, Canvas app]
---

こんにちは、Power Platform サポートの谷です。<br/>
今回は、キャンバス アプリ所有者の退職・異動に際し、Power Apps のキャンバス アプリ所有者を変更する方法についてご説明します。

<!-- more -->


## アプリの所有者・共同所有者・ユーザーとは？

キャンバス アプリを作成・実行するためには以下の 3 種類の権限があります。<br/>
それぞれ、役割に応じた権限を割り当て、安全にアプリを運用していきます。

**所有者**

アプリ作成者は自動的に所有者となります。
アプリの実行、編集、削除、共有設定を行うことが可能です。
 
**共同所有者**

アプリの実行、編集、共有設定を行うことが可能です。
アプリの削除は行うことができません。

**ユーザー**

アプリの実行を行うことが可能です。


## アプリ作成者 (所有者) が退職あるいは離任する場合はどうしたらいいか？

退職や異動により、アプリ所有者のアカウントが削除された場合、アプリ自体は削除されません。<br/>
アプリを実行することは可能な状態ですが、所有者、及び共同所有者が一人も存在しないアプリは編集や削除ができません。

以下のケースごとに対応方法をご説明します。<br/>
- ケース 1：アプリ作成者 (所有者) が退職してしまったがアプリを継続して利用 (編集) したい
- ケース 2：アプリ作成者 (所有者) が退職してしまったのでアプリを削除したい

### ケース 1 アプリ作成者 (所有者) が退職してしまったがアプリを継続して利用したい
アプリ作成者 (所有者) のアカウントを削除後も、アプリを実行/編集するには、下記の対応が必要となります。
 
**<共同所有者がいない場合>**
- 対応方法 1 管理者向け Power Apps コマンドレットで新たな所有者を割り当てる
- 対応方法 2 Power Platform 管理センターでアプリの共同所有者、あるいはユーザーを割り当てる
 
**<共同所有者がいる場合>**
- 必要な対応はございません。引き続き、アプリを実行いただけます。


### ケース 2 アプリ作成者 (所有者) が退職してしまったのでアプリを削除したい

新たな所有者、あるいは Power Platform 管理者などの管理者がアプリを削除することができます。
 
- 対応方法 1 管理者用 Power Apps コマンドレットで新たな所有者を割り当て、所有者としてアプリを削除する
- 対応方法 2 Power Platform 管理センターで、管理者としてアプリを削除する


以降では、それぞれの対応方法の詳細についてご説明いたします。

## 管理者用 Power Apps コマンドレットでアプリの所有者を変更する方法

グローバル管理者、Azure Active Directory グローバル管理者、Dynamics 365 管理者などの管理者はアプリの所有者を変更することができます。<br/>
当コマンドレットは所有者のアカウントが存在する場合でも所有者を変更することが可能です。

なお、管理者用 Power Apps コマンドレットをご利用前に PowerShell モジュールをインストールいただく必要があります。<br/>
[インストール手順](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell#installation)


**実行例**
1. (事前準備) 管理者向け Power Apps コマンドレットをインストールします
2. 管理者として PowerShell を実行します
3. 以下のコマンドを実行します

```
Set-AdminPowerAppOwner –AppName '<アプリの GUID>' -AppOwner '<新たな所有者の GUID>' –EnvironmentName '<環境の GUID>'
```

**実行コマンドで指定する各種引数について**
- <アプリの GUID>、<環境の GUID>
    - 管理者用 Power Apps コマンドレット「[Get-AdminPowerApp](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/get-adminpowerapp?view=pa-ps-latest)」コマンドでご取得ください
    - ![](./canvas-app-change-app-owner/image01.png)
- <新たな所有者の GUID>
    - 「[Get-AzureADUser](https://learn.microsoft.com/ja-jp/microsoft-365/enterprise/view-user-accounts-with-microsoft-365-powershell?view=o365-worldwide#view-additional-property-values-for-a-specific-account)」コマンド、あるいは Azure Active Directory 管理センターご確認ください
    - Get-AzureADUser 実行例
        - 例えば、以下のとおり実行いただきますとテナント内の全ユーザーについて ObjectId (AppOwner に指定する値) が取得できます。
        - ```
          Get-AzureADUser | Select DisplayName, UserPrincipalName, ObjectId
          ```
        -![](./canvas-app-change-app-owner/image02.png)
    - Azure Active Directory 管理センターにおけるユーザーの GUID の確認例
        - [Azure Active Directory 管理センター](https://aad.portal.azure.com) > ユーザー > 対象のユーザー
        - ![](./canvas-app-change-app-owner/image03.png)

## Power Platform 管理センターでアプリの共同所有者を追加する、あるいは削除する方法

**実行例**
1. 管理者の役割を持つユーザーで [Power Platform 管理センター](https://admin.powerplatform.com) にアクセスします
2. 対象の環境の「…」メニューから「Power Apps」を選択します
    ![](./canvas-app-change-app-owner/image04.png)
3. 対象のアプリの「…」メニューから、共同所有者を追加する場合「共有」を、アプリを削除する場合「削除」を選択します
    ![](./canvas-app-change-app-owner/image05.png)

## 参考情報
- [キャンバス アプリのオーナーとしてログイン ユーザーを設定する](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell#set-logged-in-user-as-the-owner-of-a-canvas-app)
- [Power Appsの管理](https://learn.microsoft.com/ja-jp/power-platform/admin/admin-manage-apps)
- [キャンパス アプリを削除する](https://learn.microsoft.com/ja-jp/power-apps/maker/canvas-apps/delete-app)

---
