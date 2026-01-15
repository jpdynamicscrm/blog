---
title: ## Copilot Studio試用版ライセンスについて
date: 2025-07-01 10:00:00
tags:
  - Microsoft Copilot Studio
  - Power Virtual Agents
  - How to
categories:
  - [Microsoft Copilot Studio]
---

こんにちは、Power Platform サポートチームのヤサルです。
本記事では Copilot Studio (旧称 Power Virtual Agents) の試用版について下記項目の順にご説明いたします。

<!-- more -->

# 目次

- [目次](#目次)
[1. 概要](#1-概要)
[2. 試用版の種類 ](#2-試用版の種類)
[3. ブロックする方法](#3-ブロックする方法)
[4. 付与されているライセンスを確認する方法](#4-付与されているライセンスを確認する方法)


### 1. 概要

Copilot Studio の試用版は、開発者や管理者が Microsoft Copilot Studioの全ての機能を体験することが可能でございます。以下に、試用版の種類、付与方法、ブロック方法、およびライセンス確認方法について詳しく説明します。 

### 2. 試用版の種類
Copilot Studio には主に以下の種類があります。
1. 個人用試用版
2. 組織用試用版

**1. 個人用試用版**
個人の職場または学校アカウントを使用してサインアップし、Copilot Studio の機能を体験できます。ユーザー本人がブラウザからCopilot Stuidoサインアップページよりサインアップができます。
初回は30日間の無料試用が開始されますが試用終了時にさらに30日間の延長が可能です。試用期間終了後、最大90日間はエージェントが引き続き機能します。 
詳細なサインアップ手順については、以下の公開文書を参照してください。
[Copilot Studio試用版へのサインアップ](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/sign-up-individual)
   
**2. 組織用試用版**
組織用試用版は、組織（テナント）単位で Copilot Studioの有償版を試すための試用版です。管理者がMicrosoft 365管理センターから「Microsoft Copilot Studio Trial」を有効にする必要があります。組織（テナント）につき1回のみ有効化可能で、試用期間は最大60日で初回は30日間で1回だけ30日延長はできます。

### 3. ブロックする方法
Copilot Studio の個人用試用版を、組織内のユーザーがセルフサービスで取得できないように以下のコマンドで制御できます。本コマンドはCopilot Studioのみを個別にブロックするものではありません。テナント全体のMicrosotセルフサービスサインアップの試用版を制御する設定となります。本コマンドの実行で組織内ユーザーが新たにセルフサービスで試用版を取得することを防止し、すでにCopilot Studioの試用版を取得しているユーザーのライセンスは自動的には解除されません。

**事前準備**
PowerShellモジュールを以下の公開文書の手順に従ってインストールします。
[PowerShellモジュールのインストール](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell)

**コマンドの実行**
以下のコマンドを実行しセルフサービスサインアップの無効化します。

```PowerShellコマンド
「$params = @{allowedToSignUpEmailBasedSubscriptions = $false}」
「Update-MgPolicyAuthorizationPolicy -BodyParameter $params」
```
コマンドの詳細については下記の公開情報をご参照ください。
[電子メール検証済みユーザーのセルフサービスサインアップ](https://learn.microsoft.com/ja-jp/entra/identity/users/directory-self-service-signup)

### 4. 付与されているライセンスを確認する方法
Microsoft 365管理センターよりユーザーにCopilot Studio試用版ライセンスを付与されているかを確認できます。
![](./copilot-studio-trial-license/④M365AdmiCenter_LicenseAssignment.png)