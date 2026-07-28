---
title: "Power Pages で外部 ID プロバイダー連携時にクレームが取引先担当者に反映されない場合の考慮事項"
date: 2026-07-16 00:00:00
tags:
  - Power Pages
  - Power Platform
  - Dataverse
categories:
  - [Power Pages]
---

## はじめに

こんにちは、Power Platform サポートチームの 早川 です。
本記事では、Power Pages と外部 ID プロバイダー (Microsoft Entra External ID) を OpenID Connect (OIDC) で連携する際に、サインアップ時に入力した氏名が取引先担当者 (Contact) に反映されないという事象を題材に、外部 ID プロバイダーが既定で発行するクレームの考え方と、必要なクレームを追加するための構成についてご案内いたします。

「サインアップ時に氏名を入力したのに Contact に氏名が引き継がれない」「診断ログに `Claim given_name is null in token` と出力される」といったお問い合わせをいただくことがございます。本記事では、その原因と対処方法を整理してお伝えいたします。

<!-- more -->

> [!NOTE]
> 本記事は執筆時点の情報に基づいています。製品の仕様は予告なく変更される場合がありますので、最新の情報は公式ドキュメントをご確認ください。

## 背景: Power Pages のクレームマッピングの仕組み

Power Pages で OpenID Connect による外部 ID プロバイダー連携を構成する場合、外部 ID プロバイダーが発行するトークン内のクレームを、取引先担当者 (Contact) の列にマッピングできます。

このマッピングは、以下のサイト設定で構成します。

- `Authentication/OpenIdConnect/{ProviderName}/RegistrationClaimsMapping` : 新規登録 (サインアップ) 時に使用されるマッピング
- `Authentication/OpenIdConnect/{ProviderName}/LoginClaimsMapping` : サインイン時に使用されるマッピング

たとえば、氏名を Contact の名 (firstname) / 姓 (lastname) にマッピングする場合、次のように設定します。

```text
firstname=given_name,lastname=family_name,fullname=name
```

この設定では、`given_name` クレームの値を firstname に、`family_name` クレームの値を lastname にマッピングしています。<br>
ここで重要な点は、**マッピング元となるクレーム (`given_name` / `family_name` など) が、実際に発行されるトークンに含まれている必要がある** ということです。トークンにクレームが含まれていない場合、マッピング先の列には値が設定されません。

## 事象: `Claim given_name is null in token`

外部 ID プロバイダー連携を構成した Power Pages サイトで、サインアップ時に氏名を入力しても Contact に氏名が反映されない場合、Power Pages の診断ログに次のようなメッセージが記録されることがあります。

```text
Claim given_name is null in token
Claim family_name is null in token
```

このメッセージは、Power Pages がクレームマッピングを処理する際に、トークン内に `given_name` および `family_name` クレームが存在しなかったことを示しています。

## 原因: 既定のトークンに含まれるクレームは限定的

Microsoft Entra External ID では、ユーザー フローで収集した属性 (氏名など) はディレクトリに保存されますが、**既定で ID トークンに含まれるクレームのセットは意図的に限定されています**。

そのため、ユーザー フローで氏名を収集していても、`given_name` や `family_name` といったクレームは、明示的に追加しない限り既定では ID トークンに含まれません。アプリケーションがこれらのクレームを必要とする場合は、アプリ登録側でトークンに含めるよう構成する必要があります。

今回の事象は、この既定の動作によるものであり、製品の仕様に基づく想定内の挙動です。

## 対処: オプションのクレームを追加する

`given_name`、`family_name`、`email`、`name` など、Microsoft Entra ID で定義済みの標準クレームを ID トークンに追加する場合は、アプリ登録の [トークン構成] にある [オプションのクレーム] を使用します。

以下の手順で構成します。

1. Microsoft Entra 管理センターで、Power Pages の ID プロバイダーとして使用しているアプリ登録を開きます
2. **[トークン構成]** (Token configuration) を選択します
3. **[オプションの要求を追加]** (Add optional claim) をクリックします
4. トークンの種類として **[ID]** を選択します
5. Power Pages のマッピングで使用しているクレーム (例: `given_name`、`family_name`) を選択して追加します

構成後、あらためてサインアップを実施すると、氏名が ID トークンに含まれるようになり、Contact に氏名が反映されることを確認できます。

> [!NOTE]
> クレームを追加または変更した後は、Power Pages 側の `RegistrationClaimsMapping` および `LoginClaimsMapping` で参照しているクレーム名と一致しているかをご確認ください。異なるクレーム名で発行した場合は、Power Pages 側のマッピングも対応するクレーム名に合わせて変更する必要があります。

## セキュリティ上の考慮事項

トークンには、アプリケーションが実際に使用するクレームのみを含めることをお勧めします。

不要な個人情報 (PII) をトークンに追加しないことで、万一トークンが漏えいした場合に露出する情報を必要最小限に抑えられます。追加するクレームは、Power Pages のマッピングで実際に参照しているものに限定することが望ましい構成です。

外部 ID プロバイダーとの連携を設計する際は、「既定でどのクレームが発行されるか」を把握したうえで、**アプリケーションが必要とするクレームを、必要になった時点で明示的に追加する** という方針で構成することをお勧めします。

## まとめ

本記事では、Power Pages と Microsoft Entra External ID を OpenID Connect で連携する際に、氏名が取引先担当者に反映されない事象を題材に、以下の点をご案内しました。

- Power Pages のクレームマッピングは、トークンに含まれるクレームを Contact 列にマッピングする仕組みであり、マッピング元のクレームがトークンに含まれている必要がある
- Microsoft Entra External ID では、既定で ID トークンに含まれるクレームは限定的であり、`given_name` / `family_name` などは明示的に追加しない限り含まれない
- 標準クレームを追加する場合は、アプリ登録の [トークン構成] > [オプションのクレーム] を使用する方法がシンプルで運用しやすい
- トークンには、アプリケーションが実際に使用するクレームのみを含める

外部 ID プロバイダー連携を構成する際は、既定で発行されるクレームを確認し、必要なクレームを適切に追加することで、想定どおりの動作を実現できます。

## 参考情報

- [OpenID Connect プロバイダーの設定 - 追加のクレームの設定 (Microsoft Learn)](https://learn.microsoft.com/power-pages/security/authentication/openid-settings#set-up-additional-claims)
- [アプリケーションでオプションのクレームを構成する (Microsoft Learn)](https://learn.microsoft.com/entra/identity-platform/optional-claims#configure-optional-claims-in-your-application)
- [Amazon Cognito から Microsoft Entra External ID への移行 (Microsoft Learn)](https://learn.microsoft.com/entra/external-id/customers/migrate-from-cognito-to-external-id#step-2-prepare)

---

※ 本情報の内容（添付文書、リンク先などを含む）は、作成日時点でのものであり、予告なく変更される場合があります。
