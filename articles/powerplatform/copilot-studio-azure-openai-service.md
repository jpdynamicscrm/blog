
title: Copilot studioの生成AI機能を利用する際のデータ移行について
date: 2024-06-13 XX:00:00
tags:
  - Copilot studio
  - Power Platform
  - Dynamics 365
  - Power Virtual Agents
disableDisclaimer: false
---

こんにちは。Power Platform サポート チームの トテ です。
今日は Copilot Studio の生成AI機能におけるデータ移行の考え方についてご案内いたします。

<!-- more -->


## はじめに

### お客様のデータ処理に関して
Microsoft によるお客様のデータの処理は、お客様との合意に基づいてのみ行われるとともに、契約で合意した厳格なポリシーと手順に従って行われます。Microsoft の広告主によってサポートされるサービスとの間でお客様のデータを共有することはなく、マーケティングリサーチや広告などの目的でマイニングすることもありません。

### Microsoft のコンプライアンスとデータプライバシー
Microsoft は、最高レベルの信頼、透過性、標準への準拠、および法規制の順守に取り組んでいます。 Microsoft のクラウド製品およびサービスは、顧客の厳格なセキュリティとプライバシーに関する需要に対応するよう、しっかりした基盤の上に構築されています。

個人のデータの収集と使用について規定している国、地域、業界固有の要件に組織が準拠するのを支援するために、Microsoft はクラウドサービスプロバイダーのコンプライアンス認証 (証明書と構成証明を含む) 包括的なセットを提供しています。

### Power Platformのデータの保護に関して
ユーザーデバイスと Microsoft データセンター間で転送中のデータは、セキュリティ保護されます。 顧客と Microsoft データセンターの間で確立される接続は暗号化され、すべてのパブリックエンドポイントは業界標準の TLS を使用して保護されます。 TLS は、セキュリティが強化されたブラウザーからサーバーへの接続を確立し、デスクトップとデータセンターの間でデータの機密性と整合性を確保することができます。 また、顧客エンドポイントからサーバーへの API アクセスも同様に保護されます。

オンプレミスのデータ ゲートウェイ経由で転送されたデータも暗号化されます。 ユーザーがアップロードするデータは通常 Azure Blob Storage に送信され、システム自体のすべてのメタデータおよびアーティファクトはAzure SQL Database および Azure Table storage に保存されます。


<br>

## Copilot Studioの生成AI機能
Copilot Studio の 生成 AI 機能 を使用すると、複雑な会話フローを作成したり、手作業でオーサリングや設定を行うことなく、Copilot を即座に構築できます。これらの機能は、Azure OpenAI API Service と Bing Search を使用しており、米国およびその他のサポートされている地理的場所向けに作成された環境で利用できます。<br>
Power Virtual Agents は、コード不要のガイド付きグラフィカル インターフェイス ソリューションであり、チームのすべてのメンバーが、Teams プラットフォームと簡単に統合できる会話型のチャットボットの作成が可能です。

> [!NOTE]
> Power Virtual Agents 機能は、生成 AI への投資と Microsoft Copilot 全体の統合の強化により、現在 Microsoft Copilot Studio の一部となっています。

<br>

## 生成 AI の地理的な場所を越えたデータ移動
米国外の地域から Copilot Studio 生成 AI 機能にアクセスすると、地域の境界を越えてデータが移動します。 このデータ移動は、Power Platform で有効化または無効化できます。 一度有効にすると、この機能が有効になっている間に行われたデータの移動は、同意を取り消しても元に戻すことはできません。

Bing を利用した機能は、[Microsoft サービス契約](https://go.microsoft.com/fwlink/?linkid=2178408)および[Microsoft プライバシー ステートメント](https://go.microsoft.com/fwlink/?LinkId=521839)によって個別に管理されます。

米国以外の環境で生成 AI 機能を有効または無効にできるのは、グローバル管理者と Power Platform 管理者のみです。

<br><br>

## Copilot と生成 AI 機能に関わるリージョン
Copilot と生成 AI 機能を使用すると、入力 (プロンプト) と出力 (結果) がリージョン外、生成 AI 機能がホストされている場所に移動する場合があります。**お客様のデータは、Azure OpenAI サービス基盤モデルのトレーニング、再トレーニング、または改善には使用されません。**

> [!IMPORTANT]
> データは、Power Platform のリージョンが日本の場合にはアメリカの Azure OpenAI Service 上に保存されることはありません。あくまでデータ処理のため、一時的に別リージョンにデータが移りますが、それらが保存されることはありません。

次の表は、コパイロットと生成 AI 機能に関係する領域を示しています。<br>

| Power Platform または Dynamics 365 環境が<br>ホストされている地域 | Azure OpenAI Service がホストされている地域 | Bing 検索のデータが保存および処理される地域 |
| ---- | ---- | ---- |
| オーストラリア、インド、イギリス、アメリカ合衆国 | お客様の Power Platform または Dynamics 365 環境の<br>地理的地域内	 | アメリカ合衆国 |
| ヨーロッパ* | スウェーデンまたはスイス | アメリカ合衆国 |
| フランス、ドイツ、ノルウェイ、スイス | スウェーデンまたはスイス | アメリカ合衆国 |
| アジア、ブラジル、カナダ、**日本**、韓国、シンガポール、<br>南アフリカ、アラブ首長国連邦 | アメリカ合衆国 | アメリカ合衆国 |
| 政府機関クラウド (GCC、GCC High) | 米国 (商用クラウド) | アメリカ合衆国 |


*Power Platform と Dynamics 365 の環境が EU データ境界でホストされている場合、同じ境界で Azure OpenAI エンドポイントを使用します。


<br><br>

## Copilot と生成 AI 機能を有効化する
Copilot および生成 AI 機能を使用するには、Power Platform 管理センターで利用規約に同意する必要があります。 同意するには、Microsoft 365 グローバル管理者、Power Platform 管理者、または Dynamics 365 管理者である必要があります。

1. [Power Platform 管理センター ](https://admin.powerplatform.microsoft.com/)にサインインします。
2. 左側のパネルで 環境 を選択します。
3. 生成 AI 機能 カードで、編集 を選択します。
4. 利用規約を確認し、地域間でデータを移動する チェックボックスを選択します。
    > 前述の通り、Copilotと生成 AI 機能を使用すると、入力 (プロンプト) と出力 (結果) がリージョン外、生成 AI 機能がホストされている場所に移動する場合があります。
5. サービス利用条件を確認して、Bing 検索 チェックボックスを選択します。
    > リージョン間でデータを移動する チェックボックスがすでに選択されている場合にのみ、Bing 検索チェックボックスを選択できます。
6. 保存を選びます。

<br><br>

## Dynamics 365 と Power Platform の Copilot データ セキュリティとプライバシーに関する FAQ
Copilot for Dynamics 365 と Power Platform の機能は、中核となる一連のセキュリティとプライバシーの取り組みおよび Microsoft [責任ある AI の原則](https://www.microsoft.com/ai/principles-and-approach)に従います。 Dynamics 365 と Power Platform のデータは、業界をリードする包括的なコンプライアンス、セキュリティ、プライバシーの制御によって保護されます。

Copilot は [Microsoft Azure OpenAI Service](https://learn.microsoft.com/ja-jp/azure/cognitive-services/openai/overview) を基盤として構築されており、完全に Azure Cloud 内で実行されます。 Azure OpenAI は、地域可用性と、責任ある AI のコンテンツ フィルターを提供します。 Copilot は [Microsoft Azure のセキュリティ機能](https://learn.microsoft.com/ja-jp/azure/security/fundamentals/overview)をすべて備えた OpenAI モデルを使用します。 OpenAI は独立した組織です。顧客のデータは OpenAI と共有されません。


<br>

>Copilot を使用するとデータはどうなりますか？

お客様のデータは、お客様がコントロールします。許可を付与しない限り、Microsoft は顧客のデータをサードパーティと共有しません。加えて、当社は顧客が同意しない場合は、Copilot やその AI 機能のトレーニングに顧客データを使用しません。 Copilot は既存のデータアクセス許可とポリシーに準拠しており、ユーザーに対する応答は、そのユーザーが個人的にアクセスできるデータのみに基づいています。 
Copilot は、サービスの不正使用や危険な使用を監視します。当社は不正行為を監視する目的で、Copilot の入出力の保存や人間によるレビューは行いません。
<br><br>

>Copilot は、どのようにユーザーのデータを使用しますか？

それぞれのサービスや機能は、ユーザーが提供したデータや Copilot で処理するように設定したデータに基づき、Copilot を使用します。

プロンプト (入力) と Copilot の応答 (出力や結果):
- 他のお客様はご利用いただけません。
- サードパーティの製品やサービス (OpenAI モデルなど) のトレーニングや改善には使用されません。
- テナント管理者が当社とのデータ共有をオプトインしない限り、Microsoft AI モデルのトレーニングや改善には使用されません。
<br><br>

>Copilot は顧客データをどのように保護しますか？

マイクロソフトは、エンタープライズ対応の AI を提供できる独自の立場にあります。Copilot は Azure OpenAI Service を基盤としており、顧客に対する当社の既存のプライバシー、セキュリティ、規制の取り組みに準拠しています。

- セキュリティ、プライバシー、コンプライアンスに対する Microsoft の包括的なアプローチに基づいて構築されています。 Copilot は Dynamics 365 や Power Platform などの Microsoft サービスに統合されており、多要素認証やコンプライアンス境界など、セキュリティ、プライバシー、コンプライアンスのポリシーとプロセスを継承します。
- 組織データは複数の保護形式により保護されます。サービス側のテクノロジにより、保存や転送に際して組織コンテンツを暗号化し、堅牢なセキュリティを実現します。 接続はトランスポート層セキュリティ (TLS) で保護され、Dynamics 365、Power Platform、Azure OpenAI 間のデータ転送は Microsoft バックボーンネットワーク経由で行われるため、信頼性と安全性が両方とも保証されます。
- テナントレベルと環境レベルの両方でデータを保護するように設計されています。テナント管理者が当社とのデータ共有をオプトインしていない限り、Microsoft AI モデルはテナントデータやプロンプトに基づいてトレーニングされず、そこから学習もしません。 環境内では、設定したアクセス許可に基づいてアクセスを制御できます。認証と承認のメカニズムにより、テナント間の共有モデルへの要求が分離されます。顧客データを保護するために当社が長年使用してきたのと同様のテクノロジにより、Copilot は顧客がアクセスできるデータのみを利用します。
<br><br>

>顧客データへのアクセスが可能な範囲はどうなっていますか？

Azure OpenAI Service 上に保存された不正使用を監視するためのデータは不正利用の監視を目的として、必要な場合に限り Microsoft 従業員しか確認でません。
<br>

## AI Builder のテキスト生成モデル
### クラウドフローにおける Copilot と、AI Builder のテキスト生成モデル

クラウドフローの Copilot は、Power Automate フローについて説明し、その過程で役立つガイダンスを提供するだけで、作成および編集ができるように設計されています。AI Builder Power Automate Power Apps のテキスト生成モデルを使用すると、テキストの要約、回答の下書き、テキストの分類など、さまざまなシナリオで、フローや組み込みアプリでGPTモデルを直接使用できます。

AI Builder のテキスト生成モデルも、米国の環境が前提条件で、Azure OpenAI Service に搭載されています。そのため、格納場所は米国であり、使用用途や閲覧可能ユーザー、データ保護などもここまでの記載と同様になっています。


### AI builder のカスタムプロンプト
AI Builder プロンプトは、Azure OpenAI Service を利用した GPT-3.5 Turbo モデルで実行されています。

>>Available in the United States, Australia, UK, and Europe  
>>GPT Prompts and Prompt Builder will be generally available this week across the United States, Australia, the United Kingdom, and Europe. GPT Prompts currently leverage the GPT-3.5-Turbo model from Azure’s OpenAI Service, meaning your prompts and outputs: <br>

> [!IMPORTANT]
    >>are NOT available to other customers. 
    >>are NOT available to OpenAI. 
    >>are NOT used to improve OpenAI models. 
    >>are NOT used to improve any Microsoft or 3rd party products or services. 
    >>are NOT used for automatically improving Azure OpenAI models for your use in your resource

上記からも判断できる通り、テキストを作成の際のプロンプトの内容が、教師データとして用いられることはありません。


<!-- 文字修飾 -->

**太字**

~~訂正せん~~

*斜め文字*

<!-- 引用 -->

> 引用
>> 引用

<!-- 見出し -->

## 見出し (大)
### 見出し (中)
#### 見出し (小)

<!-- リスト -->

- リスト
   - リスト
   - リスト
- リスト

<!-- 数字リスト -->

1. 数字リスト
   1. 数字リスト
   2. 数字リスト
2. 数字リスト
3. 数字リスト

<!-- 区切り線 -->

---

<!-- リンク -->

[Japan Azure IaaS Core Support Blog](https://jpaztech.github.io/blog/)

<!-- 画像 (リポジトリ内ファイルを参照 -->

![](./example/example01.png)

<!-- 画像 (外部 URL を参照) -->

![](https://jpaztech.github.io/blog/vm/re-install-windows-azure-guest-agent/service.png)

<!-- コード ブロック -->

```CMD
net stop rdagent
net stop WindowsAzureGuestAgent
net stop WindowsAzureTelemetryService
```

<!-- 表 -->

| 見出し | 見出し |
| ---- | ---- |
| セル | セル |
| セル | セル |

<!-- ノート -->

> [!NOTE]
> THIS IS NOTE
> `[!NOTE]` と書くと `Note` になります。
> これだけ MS 公式ドキュメントと異なります。 (公式ドキュメントだと `NOTE` も `注意` に変換される…)

<!-- ヒント -->

> [!TIP]
> THIS IS TIP
> `[!TIP]` とかくと `ヒント` になります。

<!-- 重要 -->

> [!IMPORTANT]
> THIS IS IMPORTANT
> `[!IMPORTANT]` とかくと `重要` になります。

<!-- 注意事項 -->

> [!CAUTION]
> THIS IS CAUTION
> `[!CAUTION]` と書くと `注意事項` になります。

<!-- 注意 -->

> [!WARNING]
> THIS IS WARNING
> `[!WARNING]` と書くと `警告` になります。
