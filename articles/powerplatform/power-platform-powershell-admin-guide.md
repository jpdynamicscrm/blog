---
title: Power Platform 管理者向け PowerShell でできること代表例まとめ
date: 2026-02-10
tags:
  - Power Platform
  - PowerShell
  - 環境管理
  - ユーザー管理
  - セキュリティロール
categories:
  - Power Platform
---

# Power Platform 管理者向け PowerShell でできること代表例まとめ

こんにちは、PowerPlatformサポートチームの粟野です。

本記事では Power Platform 管理者向け PowerShell を使ってできる代表的な操作について、以下の Q1〜Q6 の質問に回答する形でご説明させていただきます。

- [Q1: Power Platform の PowerShell モジュールはどうやって準備すればよいですか？](#q1)
- [Q2: PowerShell で環境を作成するにはどうすればよいですか？](#q2)
- [Q3: 環境の一覧取得や詳細確認はできますか？](#q3)
- [Q4: Dataverse にユーザーを追加するにはどうすればよいですか？](#q4)
- [Q5: ユーザーに環境ロールを付与するにはどうすればよいですか？](#q5)
- [Q6: 環境のバックアップや復元は PowerShell でできますか？](#q6)

### この記事でわかること
- Power Platform 管理用 PowerShell モジュールの準備方法とサインインの手順
- PowerShell を使った環境の新規作成やデータベース付きの環境作成の方法
- 環境の一覧取得や特定の環境の情報を確認する方法
- Dataverse 環境にユーザーを追加する手順
- 環境管理者や環境作成者のロールをユーザーに付与する方法
- 環境のバックアップ・復元を PowerShell で実行する方法

## 目次
- [Power Platform の PowerShell モジュールの準備 (Q1)](#q1)
- [PowerShell で環境を作成する (Q2)](#q2)
- [環境の一覧取得・詳細確認 (Q3)](#q3)
- [Dataverse にユーザーを追加する (Q4)](#q4)
- [ユーザーに環境ロールを付与する (Q5)](#q5)
- [環境のバックアップ・復元 (Q6)](#q6)
- [まとめ](#summary)
- [注意事項（情報の更新可能性）](#notice)

<h2 id="q1">Power Platform の PowerShell モジュールの準備 (Q1)</h2>

【結論】Power Platform を PowerShell で管理するには、専用の PowerShell モジュールを事前に PC へ入れておく必要があります。
モジュールのインストールとサインインの 2 ステップで準備が整います。

### モジュールのインストール

管理者として PowerShell を開き、以下のコマンドを実行してください。

```powershell
Install-Module -Name Microsoft.PowerApps.Administration.PowerShell
Install-Module -Name Microsoft.PowerApps.PowerShell -AllowClobber
```

PC の管理者権限がない場合は、末尾に `-Scope CurrentUser` を追加してください。

```powershell
Install-Module -Name Microsoft.PowerApps.Administration.PowerShell -Scope CurrentUser
Install-Module -Name Microsoft.PowerApps.PowerShell -AllowClobber -Scope CurrentUser
```

### サインイン

モジュールを入れたら、以下のコマンドでサインインします。
ブラウザーのサインイン画面が表示されるので、管理者アカウントの資格情報を入力してください。

```powershell
Add-PowerAppsAccount -Endpoint prod
```

【補足】コマンドレットで管理操作を行うには、Microsoft Entra ID のテナント管理者、Power Platform 管理者、Dynamics 365 管理者のいずれかの権限が必要です。

＜参考資料＞
- [Power Apps と Power Automate の PowerShell サポート (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell)
- [Power Platform 管理者用 PowerShell のインストール (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powershell-installation)
- [Power Platform 管理者用 PowerShell の概要 (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powershell-getting-started)

<h2 id="q2">PowerShell で環境を作成する (Q2)</h2>

【結論】`New-AdminPowerAppEnvironment` コマンドを使うと、PowerShell から環境を新しく作成できます。
環境の種類（本番、サンドボックス、試用版、開発者用など）や、場所（リージョン）を指定して作成します。

### 基本的な環境の作成

以下は、日本リージョンに「テスト環境」という名前の試用版環境を作成する例です。

```powershell
New-AdminPowerAppEnvironment -DisplayName 'テスト環境' -LocationName japan -EnvironmentSku Trial
```

### Dataverse データベース付きの環境を作成する

Dataverse（データを保存するためのデータベース）を同時につくりたい場合は、`-ProvisionDatabase` を付けて、言語と通貨も指定します。

```powershell
New-AdminPowerAppEnvironment -DisplayName 'Japan Dev' -LocationName japan -EnvironmentSku Sandbox -ProvisionDatabase -LanguageName 1041 -CurrencyName JPY
```

【ポイント】
1. `-EnvironmentSku` で指定できる種類は `Production`（本番）、`Sandbox`（サンドボックス）、`Trial`（試用）、`Developer`（開発者用）などがあります
2. `-LocationName` はリージョンです。日本の場合は `japan` を指定します
3. `-ProvisionDatabase` を付けると Dataverse データベースが自動で作られます
4. データベースを作る場合は `-LanguageName`（言語コード）と `-CurrencyName`（通貨コード）を必ず指定してください
5. `-SecurityGroupId` を指定すると、環境にアクセスできるユーザーをセキュリティグループで絞り込めます

＜参考資料＞
- [New-AdminPowerAppEnvironment (Docs)](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/new-adminpowerappenvironment?view=pa-ps-latest)
- [Power Apps と Power Automate の PowerShell サポート (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell)

<h2 id="q3">環境の一覧取得・詳細確認 (Q3)</h2>

【結論】`Get-AdminPowerAppEnvironment` コマンドで、テナント内のすべての環境の一覧や、特定の環境の詳細を確認できます。

### すべての環境を一覧表示する

```powershell
Get-AdminPowerAppEnvironment
```

このコマンドを実行すると、テナント内にあるすべての環境の名前（ID）、表示名、場所、作成者などが一覧で表示されます。

### 既定の環境だけを確認する

```powershell
Get-AdminPowerAppEnvironment -Default
```

### 特定の環境の詳細を確認する

```powershell
Get-AdminPowerAppEnvironment -EnvironmentName '環境のID（GUID）'
```

【補足】`-EnvironmentName` に指定するのは画面上の「表示名」ではなく、GUID と呼ばれる環境の内部的な識別子です。
一覧表示の結果や PowerPlatform 管理センターから確認できます。

＜参考資料＞
- [Power Apps と Power Automate の PowerShell サポート - 環境コマンド (Docs)](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell#環境コマンド)

<h2 id="q4">Dataverse にユーザーを追加する (Q4)</h2>

【結論】`Add-AdminPowerAppsSyncUser` コマンドを使うと、指定した Dataverse 環境にユーザーを追加できます。

以下は、環境にユーザーを追加する例です。

```powershell
Add-AdminPowerAppsSyncUser -EnvironmentName '環境のID' -PrincipalObjectId 'ユーザーのオブジェクトID'
```
【補足】`-PrincipalObjectId` に指定する値は、Microsoft Entra ID（旧 Azure AD）上でのユーザーの「オブジェクト ID」です。
Microsoft Entra 管理センターや Azure ポータルの「ユーザー」画面から確認できます。

＜参考資料＞
- [Add-AdminPowerAppsSyncUser (Docs)](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/add-adminpowerappssyncuser?view=pa-ps-latest)

<h2 id="q5">ユーザーにロールを付与する (Q5)</h2>

【結論】ロールの付与方法は、環境に Dataverse データベースがあるかどうかで異なります。
Dataverse がない環境では PowerShell の `Set-AdminPowerAppEnvironmentRoleAssignment` を、Dataverse がある環境では Power Platform CLI の `pac admin assign-user` を使います。

### Dataverse がない環境の場合：PowerShell

`Set-AdminPowerAppEnvironmentRoleAssignment` で「環境管理者」や「環境作成者」のロールを付与できます。

```powershell
Set-AdminPowerAppEnvironmentRoleAssignment `
  -EnvironmentName 'bbbbbbbb-1111-2222-3333-cccccccccccc' `
  -RoleName EnvironmentAdmin `
  -PrincipalType User `
  -PrincipalObjectId 'aaaaaaaa-0000-1111-2222-bbbbbbbbbbbb'
```

テナント全体のユーザーに環境作成者ロールを付与する場合は以下のとおりです。

```powershell
Set-AdminPowerAppEnvironmentRoleAssignment `
  -EnvironmentName 'bbbbbbbb-1111-2222-3333-cccccccccccc' `
  -RoleName EnvironmentMaker `
  -PrincipalType Tenant
```

【ポイント】
1. `-RoleName` には `EnvironmentAdmin`（環境管理者）または `EnvironmentMaker`（環境作成者）を指定します
2. `-PrincipalType` には `User`（ユーザー個人）、`Group`（セキュリティグループ）、`Tenant`（テナント全体）のいずれかを指定します
3. `User` や `Group` を指定した場合は `-PrincipalObjectId` で対象のオブジェクト ID も指定してください

### Dataverse がある環境の場合：Power Platform CLI

Dataverse がある環境では上記の PowerShell コマンドは 403 エラーになります。
代わりに Power Platform CLI に含まれる `pac admin assign-user` コマンドを使います。

```powershell
pac admin assign-user `
  --environment 00000000-0000-0000-0000-000000000000 `
  --user "user@contoso.com" `
  --role "Basic User"
```

【補足】`pac admin assign-user` を使うには、あらかじめ Power Platform CLI をインストールし、`pac auth create` で認証を済ませておく必要があります。

＜参考資料＞
- [Set-AdminPowerAppEnvironmentRoleAssignment (Docs)](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/set-adminpowerappenvironmentroleassignment?view=pa-ps-latest)
- [pac admin assign-user (Docs / 英語のみ)](https://learn.microsoft.com/en-us/power-platform/developer/cli/reference/admin#pac-admin-assign-user)

<h2 id="q6">環境のバックアップ・復元 (Q6)</h2>

【結論】PowerShell を使えば、環境のバックアップ作成と復元をコマンドで実行できます。
手動バックアップを定期的に取得したい場合や、復元作業を自動化したい場合に便利です。

### バックアップを作成する

```powershell
$backupRequest = [pscustomobject]@{
    Label = "手動バックアップ 2026年2月"
    Notes = "定期バックアップ"
}

Backup-PowerAppEnvironment -EnvironmentName '環境のID' -BackupRequestDefinition $backupRequest
```

### バックアップ一覧を確認する

```powershell
Get-PowerAppEnvironmentBackups -EnvironmentName '環境のID'
```

### バックアップから復元する

```powershell
$restoreRequest = [pscustomobject]@{
    SourceEnvironmentId = "復元元の環境ID"
    TargetEnvironmentName = "復元先の環境表示名"
    RestorePointDateTime = "2026-02-10 09:00:00"
    SkipAuditData = $true
}

Restore-PowerAppEnvironment -EnvironmentName '復元先の環境ID' -RestoreToRequestDefinition $restoreRequest
```

【補足】復元操作を行うと、復元先の環境のデータが上書きされます。
上書きされたデータは元に戻せませんので、実行前に十分ご確認ください。

＜参考資料＞
- [Backup-PowerAppEnvironment (Docs)](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/backup-powerappenvironment?view=pa-ps-latest)
- [Restore-PowerAppEnvironment (Docs)](https://learn.microsoft.com/ja-jp/powershell/module/microsoft.powerapps.administration.powershell/restore-powerappenvironment?view=pa-ps-latest)

<h2 id="summary">まとめ</h2>

- Power Platform 管理用 PowerShell モジュールを使えば、環境の作成・一覧取得・バックアップ・復元などの環境管理をコマンドで自動化できます
- `Add-AdminPowerAppsSyncUser` で Dataverse へのユーザー追加、`Set-AdminPowerAppEnvironmentRoleAssignment` で環境ロール付与が PowerShell から実行できます
- 定型的な管理作業をスクリプト化することで、作業の効率化やミスの防止が期待できます
- まずはテスト用の環境で各コマンドの動きを確認してから、本番環境に適用することをおすすめいたします

<h2 id="notice">注意事項（情報の更新可能性）</h2>

本記事の内容は執筆日時点の公開情報に基づいております。
仕様や UI、制限事項は将来変更される可能性がございます。
最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
