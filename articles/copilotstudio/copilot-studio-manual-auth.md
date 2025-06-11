---
title: Microsoft Copilot Studioの手動認証
date: 2025-06-06
tags:
  - Microsoft Copilot Studio
categories:
  - [Microsoft Copilot Studio, 認証]
---

# はじめに

こんにちは、Power Platform サポートチームの山田です。  
本記事では Copilot Studioの手動認証についてご説明いたします。  
<br>
<!-- more -->
# 目次

1. [エージェントの手動認証](#anchor-manual-auth)
1. [ナレッジ使う際のアクセス権について](#anchor-knowledge-access)
1. [「管理者の承認」について](#anchor-admin-consent)
---  



<a id='anchor-manual-auth'></a>

# エージェントの手動認証  
Copilot Studioの各エージェントでは、複数の認証オプションが用意されています。  
手動認証では、サポートされて認証プロバイダを利用しサインインし、エージェントを利用します。  
認証方式の種類については下記公開情報をご参照ください。  
[Copilot Studio でユーザー認証の構成](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/configuration-end-user-authentication#authenticate-manually)
<br> 

エージェントの手動認証、ユーザーの認証を管理するためには、Azureポータルでのアプリ登録が事前に必要となります。  
アプリにアクセス権を付与し、そのアプリに与えたアクセス権を行使して、エージェントを利用しております。  
手動認証時には、アプリに対して下記のAPIアクセス許可が必要となります。

* openid 
* profile

詳細は下記公開情報をご参照ください。  
[Microsoft Entra ID を使用してユーザー認証を構成する](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/configuration-authentication-azure-ad?tabs=fic-auth)  
また、アプリの登録自体の詳細は下記公開情報をご参照ください。  
[チュートリアル: Microsoft Entra ID に登録する](https://learn.microsoft.com/ja-jp/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory)
<br>

アプリのメインの目的は上記の通りとなりますが、他にも使用できる＝認証できるユーザー範囲を限定する目的でもアプリをご使用いただけます。  
詳細については、下記ブログ記事をご参照ください。  
[「管理者の承認が必要」のメッセージが表示された場合の対処法](https://jpazureid.github.io/blog/azure-active-directory/azure-ad-consent-framework/#4-%E7%AE%A1%E7%90%86%E8%80%85%E3%81%AE%E5%90%8C%E6%84%8F%E3%81%8C%E5%BF%85%E8%A6%81%E3%81%A8%E8%A1%A8%E7%A4%BA%E3%81%95%E3%82%8C%E3%81%9F%E9%9A%9B%E3%81%AE%E5%AF%BE%E5%87%A6%E6%B3%95)
<br><br>

<a id='anchor-knowledge-access'></a>

# ナレッジを使う際のアクセス権について  
手動認証を利用しているエージェントにおいてナレッジを使う際、Azureポータルで登録するアプリに対して、対象ナレッジソースへのアクセス権の付与も必要となります。  
例えばSharePointをナレッジソースとして使う際には下記アクセス許可が必要となります。

* Sites.Read.All  
* Files.Read.All  

詳細は下記公開情報をご参照ください。  
[生成型の回答に SharePoint コンテンツを使用する](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/nlu-generative-answers-sharepoint-onedrive)  

ナレッジとして接続先サービスごとに必要なアクセス権の更なる詳細などについては、下記公開情報をご参照ください。  
[Copilot Studio の認証の構成](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/configuration-authentication-azure-ad#configure-authentication-in-copilot-studio)  
[SharePoint ソースを指す生成型の回答が結果を返しません](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/generative-answers-sharepoint-no-response)
<br><br>

<a id='anchor-admin-consent'></a>

# 「管理者の承認」について  
管理者の同意が必要なAPI/アクセス許可をご利用の場合や、APIのアクセス許可に対して、管理者同意を与えずにアプリをAzureポータルで登録し、管理者権限のないユーザーでエージェントにアクセスした場合、「管理者の承認」が必要な旨のメッセージが表示されます。  
この場合、メッセージの通り、Entra観点によるテナントの管理者による同意が必要となります。  
![](./copilotstudio-manual-auth/ad-consent.png)  

詳細は下記ブログ記事をご参照ください。  
[「管理者の承認が必要」のメッセージが表示された場合の対処法](https://jpazureid.github.io/blog/azure-active-directory/azure-ad-consent-framework/)
<br><br>

## 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって若干の UI の遷移など異なる場合があります。その場合は画面の指示に従って進めてください。