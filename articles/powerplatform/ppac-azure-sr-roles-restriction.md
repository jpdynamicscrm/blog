---
title: サポート リクエスト起票をPowerPlatform管理センターからのみに制限する方法
date: 2025-09-01 00:00
tags:
  - サポート リクエスト
  - SR
  - 役割
  - RBAC
  - Power Platform 管理センター
  - Azure ポータル
categories:
  - [Power Platform, サポート]
---


# サポート リクエスト起票をPowerPlatform管理センターからのみに制限する方法

こんにちは、Power Platform サポート チームの三田です。

本記事では、Power Platform 管理センター（PPAC）とAzure portal のサポート リクエスト起票が可能なロールと、サポート リクエストの起票をPPACからのみにする方法についてご紹介いたします。サポート リクエスト内容と起票する窓口が異なる場合、適切な担当者が見つかるまでお時間をいただく可能性がございますので、Power Platformに関するサポート リクエストはPPACから起票していただけますと幸いです。

以下の関連記事も併せてご参照ください。

[Power Platform に問い合わせを行うための権限について](https://jpdynamicscrm.github.io/blog/powerapps/SR_authority/)


## 目次
- [この記事からわかること](#この記事からわかること)
- [PPACからサポート リクエストの起票が可能なロール](#ppacからsrの起票が可能なロール)
- [Azure portalからサポート リクエストの起票が可能なロール](#azure-portalからsrの起票が可能なロール)
- [サポート リクエストの起票をPPACからのみに制限する方法](#srの起票をppacからのみに制限する方法)
- [まとめ](#まとめ)

## この記事からわかること
- PPAC における サポート リクエスト 起票が可能なロールの整理
- Azure portal における サポート リクエスト 起票が可能なロールの整理（サブスクリプションあり／なし）
- サポート リクエスト 起票を PPAC に限定するためのロールについて

## PPACからサポート リクエストの起票が可能なロール
Power Platform 管理センター（PPAC）では、サポート リクエストを作成するために、以下のサポート作成が有効化されたセキュリティ ロールのいずれかを保持している必要がございます。

> - Microsoft Entra ロール管理者
> - 環境管理者 (または Dataverse のシステム管理者ロール)
> - 会社管理者
> - 課金管理者
> - サービス管理者
> - CRM サービス管理者
> - CRM 組織の管理者
> - Power Platform 管理者
> - セキュリティ管理者
> - パートナー代理管理者
> - SharePoint 管理者
> - チーム管理者
> - Exchange 管理者
> - Power BI 管理者
> - Power Apps 環境管理者
> - Power Apps Full 管理者
> - コンプライアンス管理者
> - ヘルプデスク管理者
> - LCS ユーザー

また、技術的サポートを受けるためには、ご契約のサポート プラン（Subscription／ProDirect／Unified など）が要件を満たしている必要がございますのでご留意ください。

＜参考文献＞
- [サポートを受ける](https://learn.microsoft.com/ja-jp/power-platform/admin/get-help-support)  

- [サポートの概要](https://learn.microsoft.com/ja-jp/power-platform/admin/support-overview#using-support)


## Azure portalからSRの起票が可能なロール

1) サブスクリプションありの場合

    Azure portal から サポート リクエスト を作成する場合、サブスクリプションが関与するシナリオでは、対象サブスクリプションのスコープで [所有者]、[共同作成者]、または [サポートリクエスト共同作成者] のいずれか、もしくは [Microsoft.Support/*] アクションを含むカスタム ロールを保持している必要がございます。

2) サブスクリプションなしの場合

    一方、サブスクリプションを伴わないシナリオ（例: Microsoft Entra の課題）では、管理者（Admin）ロールが必要条件として定義されております。


    *＜注意＞
[管理者]はテナント側の管理者でありサービスサポート管理者以上を指します。*


なお、権限が不足している場合には Azure portal 上で「You don’t have permission to create a support request」といった表示が行われる可能性がございます。
この場合は、Microsoft.Support/supportTickets/write を含むロール（例: Support Request Contributor）が適切なスコープで割り当てられているかをご確認ください。

＜参考文献＞
- [Azure サポート リクエストを作成する](https://learn.microsoft.com/ja-jp/azure/azure-portal/supportability/how-to-create-azure-support-request#getting-started) 

- [サポート リクエスト共同作成者](https://learn.microsoft.com/ja-jp/azure/role-based-access-control/built-in-roles/management-and-governance#support-request-contributor) 

- [Azure RBAC のトラブルシューティング](https://learn.microsoft.com/ja-jp/azure/role-based-access-control/troubleshooting#access-denied-or-permission-errors) 


## サポート リクエストの起票をPPACからのみに制限する方法

サブスクリプションが関与するシナリオの場合は、[PPACからサポート リクエストの起票が可能なロール](#PPACからSRの起票が可能なロール)を満たし、かつ[サブスクリプションありの場合](#サブスクリプションありの場合)を満たさないロールを割り当てることでサポート リクエストの起票をPPACからのみに制限することが可能でございます。

一方で、サブスクリプションを伴わないシナリオの場合は[システム管理者]ロールを割り当てることでサポート リクエストの起票をPPACからのみに制限することが可能でございます。



## まとめ
本記事では、Power Platform 管理センター（PPAC）とAzure portalのサポート リクエスト起票が可能な権限についてご紹介しました。原則、権限をお持ちであれば、どの窓口からでもサポート リクエストの起票は可能ではございますが、PPACからのみにサポート リクエスト起票を制限することをご希望される方は、本記事をご参考にしていただけますと幸いです。




## 注意事項（情報の更新可能性）
本記事の内容は執筆日時点の情報に基づいております。製品の仕様や UI、ロールの名称・権限、制限事項、およびリンク先の内容は将来的に変更される可能性がございます。最新情報は公式ドキュメントをご確認くださいますようお願い申し上げます。
