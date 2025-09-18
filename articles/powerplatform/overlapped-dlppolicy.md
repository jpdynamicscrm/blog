---
title: DLP ポリシーに関する Q&A について
date: 2025-09-12 12:00:00

tags:
  - Connector and Connection
  - Power Platform
  - Power Automate
categories:
  - [Power Automate]
---


こんにちは、Power Platform サポートチームの宮﨑です。  
本記事では、DLP ポリシーに関する様々な Q&A についてご案内致します。


<!-- more -->
# 目次

- [目次](#目次)
- [概要](#概要)
    - [DLP ポリシーとは](#anchor-about-dlp)
    - [個別環境に複数のDLPポリシーが適用された場合](#anchor-multi-dlp-applied)
    - [DLP ポリシーの変更履歴を確認する方法](#anchor-dlp-edit-history)
    - [新しいコネクタが追加されたときの分類](#anchor-default-new-connector-classification)
- [補足](#補足)

<a id='概要'></a>

# 概要
今回はよくあるお問い合わせとして、DLP ポリシーに関する様々な Q&A についてご案内いたします。

<a id='anchor-about-dlp'></a>

# DLP ポリシーとは
DLP ポリシーとは、「Data Loss Preventation (データ損失防止) ポリシー」の略で、企業や組織の重要なデータを守るための仕組みです。  

Power Platform では、様々な「コネクタ」を使用して Microsoft 365 や 外部サービスとデータをやり取りすることが可能です。  
便利な反面、誤って社内機密を外部に送ってしまうというリスクも存在しています。  

DLP ポリシーはこのようなリスクを防ぐために、コネクタの利用範囲を制限することができます。  
具体的には、「社内データは SharePoint や Outlook などのビジネスコネクタだけと連携可能」や、「SNS系コネクタは使用禁止」というルールを設定できます。  
（参考：[データの消失防止 (DLP) ポリシー](https://learn.microsoft.com/ja-jp/power-platform/admin/wp-data-loss-prevention)）

<a id='anchor-multi-dlp-applied'></a>

# 個別環境に複数の DLP ポリシーが適用された場合

ある環境に対して、テナント全体に適用されるDLPポリシーとその環境のみに適用されるDLPポリシーが存在した場合の挙動について、詳細をご説明します。

## 評価の基本ルール
1. すべての DLP ポリシーが同時に評価され、**最も制限の厳しい設定が優先**されます。  
特に「ブロック済み」に分類されているコネクタは、他の DLP ポリシーで許可されている場合でも使用はできません。  
例えば、全体に公開されている DLP ポリシーにおいて HTTP コネクタが「ブロック済み」に分類されている場合、  
個別インスタンスの DLP ポリシーで許可されていたとしても、使用することはできません。


2. 同じコネクタが、ある DLP ポリシーでは「業務」、別の DLP ポリシーでは「非ビジネス」と分類されていた場合：
    そのコネクタは **単体では使用可能ですが、他のコネクタと一緒に使うことができなくなります**。  
    これは、Power Platform ではアプリやフロー内で使用されるすべてのコネクタが、同じ「分類グループ」に属している必要があるためです。

    具体的には：

    - 「業務」に分類されているコネクタ同士は併用可能  
    - 「非ビジネス」に分類されているコネクタ同士は併用可能  
    - 「業務」と「非ビジネス」にまたがるコネクタは **併用不可**

    となっています。

（参考：[Microsoft Learn - DLPポリシーの組み合わせ効果](https://learn.microsoft.com/ja-jp/power-platform/admin/dlp-combined-effect-multiple-policies)）

 
## 実際の例
以降では、簡単な例を用いてより具体的な挙動について解説します。

### 前提 
テナント全体に公開されている DLP ポリシーをグループ A 、個別の環境に指定されている DLP ポリシーをグループ B という前提のもと、進めていきます。

### 実際の挙動
こちらが、グループＡとグループＢでのコネクタ分類と、それに基づく実際の挙動をまとめた表になります。
 
| コネクタ名       | グループＡ (公開 DLP) | グループＢ (個別 DLP) | 個別インスタンスでの実際の挙動                       |
|------------------|------------------------|------------------------|------------------------------------------------------|
| SharePoint       | 業務               | 非ビジネス             | 単体使用は可能。他のコネクタとの併用不可。          |
| Outlook          | 非ビジネス             | 非ビジネス             | 他のコネクタとの併用可能                             |
| Twitter          | ブロック済み           | 非ビジネス             | 使用不可（ブロックが優先）                          |
| Salesforce       | 業務               | ブロック済み           | 使用不可（ブロックが優先）                          |
| Excel Online     | 業務               | 業務               | 他のコネクタとの併用可能                             |
| Dropbox          | 非ビジネス             | ブロック済み           | 使用不可（ブロックが優先）                          |

 
このように、**同じコネクタでもポリシーごとに分類が異なる場合、アプリやフロー内でのコネクタ使用に制限が生じてしまいます**。  
そのため、環境に適用するDLPポリシーの数は最小限に抑えることが推奨されております。  
また、同じコネクタに対して異なる分類（業務／非ビジネス／ブロック済み）を設定しないようご留意ください。

<a id='anchor-dlp-edit-history'></a>

# 設定した DLP ポリシーの変更履歴を確認する方法

DLPポリシーの変更内容については、Microsoft Purviewの「監査ログ」から確認いただけます。

以下の手順で変更履歴を取得することが可能です。

1. アクセス権をもつアカウントで、Microsoft Purview管理センターにアクセスしてください。
2. 「監査＞検索」を選択し、「アクティビティ – 操作名」に「GovernanceApiPolicyOperation」を入力してください。
3. 検索対象となる時間範囲を指定して「検索」を選択いただくことで、指定の時間範囲内のDLPポリシーの変更履歴を確認することが可能です。

![](./overlapped-dlppolicy/purview-log.png)

表示されますデータの例等の詳細につきましては、下記の公開情報をご参照ください。<br>
（参考：[Microsoft Learn - Microsoft Purview のソリューションの概要を使用して Power Platform 管理ログを表示する](https://learn.microsoft.com/ja-jp/power-platform/admin/admin-activity-logging#activity-category-data-policy-events)）

<a id='anchor-default-new-connector-classification'></a>

# 新しいコネクタが追加されたときの分類

新しいコネクタがリリースされたとき、そのコネクタはデフォルトで「非ビジネス | 既定」に分類されます。

![alt text](./overlapped-dlppolicy/newconnector.png)

DLP ポリシーを作成するにあたって、「業務」や「ブロック済み」にコネクタを移動することはできますが、
最初から「業務」と「ブロック済み」に分類されるコネクタはございません。

デフォルトで分類されるグループを変更されたい場合は、以下の公開情報に手順がございますので、ご参照ください。<br>
（参考：[Microsoft Learn - データポリシーを管理する](https://learn.microsoft.com/ja-jp/power-platform/admin/prevent-data-loss?tabs=new#change-the-default-data-group)）

---

<a id='補足'></a>

# 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。  
バージョンアップによって UI の遷移などが若干異なる場合があります。  
その場合は画面の指示に従って進めてください。  

