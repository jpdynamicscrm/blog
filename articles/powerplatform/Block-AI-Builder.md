---
title: AI Builder 利用制限について
date: 2024-12-27 11:30:00
tags:
  - Power Platform
  - AI Builder
  - Governance
---

# AI Builder 利用制限について

こんにちは、Power Platform サポートチームの藤田です。  

AI Builder は AI モデルを作成・カスタマイズして Power Platform で利用できるサービスです。<br>
魅力的なサービスではございますが、お客様の社内ポリシー等で利用を制限したいというお問い合わせをいただくことがあります。<br>
本記事では、AI Builder の利用前提条件と利用制限方法について解説します。

## AI Builder の利用前提条件
AI Builder を利用するためには、以下の条件を満たす必要があります：

- **ライセンスの保有：** 
  - Power Automate や Power Apps にアクセスできるライセンスが必要です（試用版やMicrosoft 365付帯ライセンスを含む）。
  - Dynamics 365ライセンスを持つユーザーも利用可能です。
  - 具体的なライセンスや使用権の詳細は [ライセンスガイド](https://go.microsoft.com/fwlink/?LinkId=2085130) をご参照ください。

- **AI Builderクレジット：** 
  - テナントにAI Builder のクレジットがあることが必要です。
  - テナントにクレジットがない場合、ユーザー単位でAI Builder の試用版ライセンスを取得して利用することができます。

- **環境設定：** 
  - プレビュー段階のモデルを利用する場合、環境設定で「プレビュー中のAI Builderモデルの種類の使用を有効にする」をオンにする必要があります。
  - AIプロンプトを利用するためには、「Power PlatformやCopilot StudioでAIプロンプト機能を有効化する」と「リージョン間でデータを移動する」の設定がオンになっている必要があります。

## AI Builder の利用制限方法
AI Builder の利用を制限するためには、以下の設定を行います。

- **AI プロンプトの無効化：** 
  - Power Platform 管理センター > 環境 > 対象環境 > 生成AI機能 – 「リージョン間でデータを移動する」 設定をオフにします。※
  - Power Platform 管理センター > 環境 > 対象環境 > 設定 > 機能より、AI プロンプト – 「Power Platform や Copilot Studio で AI プロンプト機能を有効化する」 設定をオフにします。※<br>
  公開情報：[AIプロンプトを有効または無効にする](https://learn.microsoft.com/ja-jp/ai-builder/administer#enable-or-disable-ai-prompts-in-power-platform-and-copilot-studio)

> [!NOTE]
> ※ 「リージョン間でデータを移動する」 設定、「Power Platform や Copilot Studio で AI プロンプト機能を有効化する」 設定は、
> AI Builder のみならず Copilot Studio 等、Power Platform 上の生成 AI 機能に影響しますのでご注意ください。
> Copilot と生成 AI 機能の無効化について、[こちらのブログ記事](https://jpdynamicscrm.github.io/blog/copilotstudio/copilot-studio-data-handling/#Copilot-%E3%81%A8%E7%94%9F%E6%88%90-AI-%E6%A9%9F%E8%83%BD%E3%81%AE%E7%84%A1%E5%8A%B9%E5%8C%96%E3%81%AB%E3%81%A4%E3%81%84%E3%81%A6)もご参照ください。

- **AI モデルの利用無効化：**
  - テナントにAI Builderのクレジットがない場合：
    - 試用版を無効にするために、[「ユーザーによる未割当のクレジットの使用を許可する」設定](https://learn.microsoft.com/ja-jp/ai-builder/ai-builder-trials#can-i-block-users-in-my-organization-from-signing-up-for-an-ai-builder-trial)をオフにします。
    - [「プレビュー中のAI Builderモデルの種類の使用を有効にする」設定](https://learn.microsoft.com/ja-jp/ai-builder/administer#enable-or-disable-ai-builder-preview-features)をオフにします。

  - テナントにAI Builderのクレジットがある場合：
    - 「ユーザーによる未割当のクレジットの使用を許可する」設定をオフにします。
    - 対象環境にAI Builderのクレジットが割り当てられていないことを確認します。
    - 「プレビュー中のAI Builderモデルの種類の使用を有効にする」設定をオフにします。

これらの設定を行うことで、AI Builderの試用版やクレジットを消費しないプレビュー段階のモデルを含め、AIモデルの作成・使用を制限することができます。

## 注意事項
ユーザーの権限に応じて、既存のモデルやプロンプトの一覧にアクセスすることは可能ですが、上記設定によりモデル・プロンプトの利用はできない状態となります。

なお、外部サイト等にて、セキュリティ ロールを使用した無効化方法を検証した情報があります。

Power Apps や Power Automate を使用するための Environment Maker ロールに AI Builder の利用ができる特権が含まれておりますため、
このロールの剥奪を行うと影響範囲がAI Builderのみではなくなる点、
また既定の環境では Environment Maker の剝奪は出来ない点等から、
セキュリティ ロールを使用した制御は「AI Builder の利用制限」としては推奨される方法ではございませんのでご注意ください。
