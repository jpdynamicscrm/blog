---
title: Copilot studio の生成 AI 機能を利用する際のデータのお取り扱いについて
date: 2024-06-14 13:30:00
tags:
  - Power Platform
  - Microsoft Copilot Studio
  - AI Builder
categories:
  - [Microsoft Copilot Studio]
---

# Copilot studio の生成 AI 機能を利用する際のデータのお取り扱いについて
こんにちは。Power Platform サポートチームの トテ です。
今回はよく寄せられるお問い合わせのひとつ、 Copilot Studio の生成 AI 機能におけるデータのお取り扱いについてご案内いたします。

<!-- more -->

## はじめに

### お客様のデータ処理に関して
Microsoft によるお客様のデータの処理は、お客様との合意に基づいてのみ行われるとともに、契約で合意した厳格なポリシーと手順に従って行われます。
Microsoft の広告主によってサポートされるサービスとの間でお客様のデータを共有することはなく、マーケティングリサーチや広告などの目的でマイニングすることもありません。

<br>

### Microsoft のコンプライアンスとデータプライバシー
Microsoft は、信頼、透過性、標準への準拠、および法規制の順守に取り組んでいます。 Microsoft のクラウド製品およびサービスは、顧客の厳格なセキュリティとプライバシーに関する需要に対応するよう、しっかりした基盤の上に構築されています。

個人のデータの収集と使用について規定している国、地域、業界固有の要件に組織が準拠するのを支援するために、Microsoft はクラウド サービス プロバイダーのコンプライアンス認証 (証明書と構成証明を含む) 包括的なセットを提供しています。

<br>

### Power Platform のデータの保護に関して
ユーザーデバイスと Microsoft データセンター間で転送中のデータは、セキュリティ保護されます。 顧客と Microsoft データセンターの間で確立される接続は暗号化され、すべてのパブリックエンドポイントは業界標準の TLS を使用して保護されます。 TLS は、セキュリティが強化されたブラウザーからサーバーへの接続を確立し、デスクトップとデータセンターの間でデータの機密性と整合性を確保することができます。 また、顧客エンドポイントからサーバーへの API アクセスも同様に保護されます。

### オンプレミスデータゲートウェイのデータ保護
オンプレミスのデータゲートウェイ経由で転送されたデータも暗号化されます。 ユーザーがアップロードするデータは通常 Azure Blob Storage に送信され、システム自体のすべてのメタデータおよびアーティファクトは Azure SQL Database および Azure Table storage に保存されます。

Dataverse データベースの環境は、SQL Server Transparent Data Encryption (TDE) を使用して、ディスクへの書き込み時にリアルタイムの暗号化を実行しています。

<br>

参考 URL
- [データ保護とプライバシー](https://www.microsoft.com/ja-jp/trust-center/privacy)
- [コンプライアンスとデータプライバシー](https://learn.microsoft.com/ja-jp/power-platform/admin/wp-compliance-data-privacy)

<br>


## Copilot Studio の生成 AI 機能
Copilot Studio の生成 AI 機能 を使用すると、複雑な会話フローを作成したり、手作業で設定を行うことなく、Copilot を構築できます。これらの機能は、Azure OpenAI API Service と Bing Search を使用しており、米国およびその他のサポートされている地理的場所向けに作成された環境で利用できます。<br>
また、Copilot Studio は、コード不要のガイド付きグラフィカルインターフェイスソリューションであり、チームのすべてのメンバーが、Teams プラットフォームと簡単に統合できる会話型のチャットボットの作成が可能です。

> [!NOTE]
> Copilot Studio 機能は、生成 AI への投資と Microsoft Copilot 全体の統合の強化により、現在 Microsoft Copilot Studio の一部となっています。

<br>

参考 URL
- [Microsoft Copilot Studio](https://www.microsoft.com/ja-jp/microsoft-copilot/microsoft-copilot-studio)
- [責任ある AI プラクティスの強化](https://www.microsoft.com/ja-jp/ai/responsible-ai)

<br>

## 生成 AI の地理的な場所を越えたデータ移動
米国外の地域から Copilot Studio 生成 AI 機能にアクセスすると、地域の境界を越えてデータが移動します。 このデータ移動は、Power Platform で有効化または無効化できます。 一度有効にすると、この機能が有効になっている間に行われたデータの移動は、同意を取り消しても元に戻すことはできません。

Bing を利用した機能は、[Microsoft サービス契約](https://go.microsoft.com/fwlink/?linkid=2178408) および [Microsoft プライバシー ステートメント](https://go.microsoft.com/fwlink/?LinkId=521839) によって個別に管理されます。

米国以外の環境で生成 AI 機能を有効または無効にできるのは、グローバル管理者と Power Platform 管理者のみです。

<br><br>

## Copilot と生成 AI 機能に関わるリージョン
Copilot と生成 AI 機能を使用すると、入力 (プロンプト) と出力 (結果) がリージョン外、生成 AI 機能がホストされている場所に移動する場合があります。**お客様のデータは、Azure OpenAI サービス基盤モデルのトレーニング、再トレーニング、または改善には使用されません。**

> [!IMPORTANT]
> データは、あくまでデータ処理のため、一時的に別リージョンにデータが移りますが、それらが保存されることはありません。

<br>
次の表は、コパイロットと生成 AI 機能に関係する領域を示しています。<br>

| Power Platform または Dynamics 365 環境がホストされている地域 | Azure OpenAI Service がホストされている地域 | Bing 検索のデータが保存および処理される地域 |
| ---- | ---- | ---- |
| オーストラリア、インド、イギリス、アメリカ合衆国 | お客様の Power Platform または Dynamics 365 環境の<br>地理的地域内	 | アメリカ合衆国 |
| ヨーロッパ* | スウェーデンまたはスイス | アメリカ合衆国 |
| フランス、ドイツ、ノルウェイ、スイス | スウェーデンまたはスイス | アメリカ合衆国 |
| アジア、ブラジル、カナダ、**日本**、韓国、シンガポール、南アフリカ、アラブ首長国連邦 | アメリカ合衆国 | アメリカ合衆国 |
| 政府機関クラウド (GCC、GCC High) | 米国 (商用クラウド) | アメリカ合衆国 |


*Power Platform と Dynamics 365 の環境が EU データ境界でホストされている場合、同じ境界で Azure OpenAI エンドポイントを使用します。

<br>

参考 URL
- [コパイロットと生成 AI 機能をオンにする](https://learn.microsoft.com/ja-jp/power-platform/admin/geographical-availability-copilot)

<br>

## Copilot と生成 AI 機能の無効化について
Power Platform の管理センターで新たに環境を作る際は、環境を作成した段階で、デフォルト機能で生成 AI 機能がオンになっています。お客さま自身が有効化を明示的に実行しない場合においてもデフォルト機能として有効化されていることをご認識ください。この機能をオフにするには、下記の手順を実行します。
1. [Power Platform 管理センター ](https://admin.powerplatform.microsoft.com/)にサインインします。
2. 左側のパネルで 環境 を選択します。
3. 生成 AI 機能 カードで、編集 を選択します。（下記画像参照）

![alt text](./copilot-studio-data-handling/image-4.png)
<br>
すると、生成 AI 機能に関する要件が表示されます。（下記画像参照）

![alt text](./copilot-studio-data-handling/image-5.png)

4. 各要件内容のチェックボックスを外し、保存をクリックしていただくことにより、生成 AI 機能を無効化することができます。
    > リージョン間でデータを移動する チェックボックスがすでに選択されている場合にのみ、Bing 検索チェックボックスを選択できます。

<br><br>

## Dynamics 365 と Power Platform の Copilot データ セキュリティとプライバシーに関する FAQ
Copilot for Dynamics 365 と Power Platform の機能は、中核となる一連のセキュリティとプライバシーの取り組みおよび Microsoft [責任ある AI の原則](https://www.microsoft.com/ai/principles-and-approach) に従います。 Dynamics 365 と Power Platform のデータは、業界をリードする包括的なコンプライアンス、セキュリティ、プライバシーの制御によって保護されます。

Copilot は  [Microsoft Azure OpenAI Service](https://learn.microsoft.com/ja-jp/azure/cognitive-services/openai/overview) を基盤として構築されており、完全に Azure Cloud 内で実行されます。 Azure OpenAI は、地域可用性と、責任ある AI のコンテンツフィルターを提供します。<br> Copilot は [Microsoft Azure のセキュリティ機能](https://learn.microsoft.com/ja-jp/azure/security/fundamentals/overview) をすべて備えた OpenAI モデルを使用します。 OpenAI は独立した組織です。**顧客のデータは OpenAI と共有されません。**
> [!IMPORTANT]
>ひとつ前の項では、Copilot と生成 AI 機能の無効化の方法をご紹介させていただきました。**ご説明させていただいた通り、デフォルト機能として生成 AI 機能は有効化されているため、お客さま自身が有効化を明示的に実行しない場合においてもその状態が保たれています。しかし、無効化に変更せずに、そのまま使用していただいた場合においても、顧客のデータは OpenAI と共有されることはございません。**

<br><br>
以降では、頻繁に寄せられる質問のうち、公式サイトで公開している FAQ を抜粋してご紹介させていただきます。
[Dynamics 365 と Power Platform の Copilot データ セキュリティとプライバシーに関する FAQ 全文](https://learn.microsoft.com/ja-jp/power-platform/faqs-copilot-data-security-privacy)



<br>

>Copilot を使用するとデータはどうなりますか？

お客様のデータは、お客様がコントロールします。 許可を付与しない限り、Microsoft は顧客のデータをサードパーティと共有しません。 それに加えて、当社は顧客が同意しない場合は、Copilot やその AI 機能のトレーニングに顧客データを使用しません。 Copilot は既存のデータ アクセス許可とポリシーに準拠しており、ユーザーに対する応答は、そのユーザーが個人的にアクセスできるデータのみに基づいています。 データの制御方法と処理方法の詳細については、[Dynamics 365 アプリと Power Platform の Copilot](https://learn.microsoft.com/ja-jp/power-platform/faqs-copilot-data-security-privacy#copilot-in-dynamics-365-apps-and-power-platform) の下にリストされた記事を参照してください。

Copilot は瞬時の処理により、サービスの不正使用や危険な使用を監視します。 当社は不正行為を監視する目的で、Copilot の入出力の保存や人間によるレビューを行いません。

<br><br>

>Copilot は、どのようにユーザーのデータを使用しますか？

それぞれのサービスや機能は、ユーザーが提供したデータや Copilot で処理するように設定したデータに基づき、Copilot を使用します。

プロンプト (入力) と Copilot の応答 (出力や結果):
- 他のお客様はご利用いただけません。
- サードパーティの製品やサービス (OpenAI モデルなど) のトレーニングや改善には使用されません。
- テナント管理者が当社とのデータ共有をオプトインしない限り、Microsoft AI モデルのトレーニングや改善には使用されません。*

<br><br>

>Copilot は顧客データをどのように保護しますか？

Copilot は Azure OpenAI Service を基盤としており、顧客に対する当社の既存のプライバシー、セキュリティ、規制の取り組みに準拠しています。
<br>
- セキュリティ、プライバシー、コンプライアンスに対する Microsoft の包括的なアプローチに基づいて構築されています。 Copilot は Dynamics 365 や Power Platform などの Microsoft サービスに統合されており、多要素認証やコンプライアンス境界など、セキュリティ、プライバシー、コンプライアンスのポリシーとプロセスを継承します。

- 組織データは複数の保護形式により保護されます。サービス側のテクノロジにより、保存や転送に際して組織コンテンツを暗号化し、堅牢なセキュリティを実現します。 接続はトランスポート層セキュリティ ( TLS ) で保護され、Dynamics 365、Power Platform、Azure OpenAI 間のデータ転送は Microsoft バックボーンネットワーク経由で行われるため、信頼性と安全性が両方とも保証されます。

- テナントレベルと環境レベルの両方でデータを保護するように設計されています。テナント管理者が当社とのデータ共有をオプトインしていない限り、Microsoft AI モデルはテナントデータやプロンプトに基づいてトレーニングされず、そこから学習もしません。 環境内では、設定したアクセス許可に基づいてアクセスを制御できます。認証と承認のメカニズムにより、テナント間の共有モデルへの要求が分離されます。顧客データを保護するために当社が長年使用してきたのと同様のテクノロジにより、Copilot は顧客がアクセスできるデータのみを利用します。
<br><br>

ここまでの質疑の回答内にありました「許可を付与しない限り」、「オプトインしない限り」の詳細につきましては以降の説明をご覧ください。

> [!IMPORTANT]
**Power Platform の Copilot Studio で生成 AI 機能を利用してもデータをトレーニングに利用されることはありません。ただし、米国テナントに限り、お客さま自らが設定を有効化することで共有をすることも可能です。下記を参照の通り、この設定は現在、日本エリアの場合には選定できません。よって、米国エリア以外は、お客さまのご意思をもってしてもデータを共有することはできず、これに照らして、データがトレーニングに利用されることはございません。**
![alt text](./copilot-studio-data-handling/image-3.png)

<br>

米国テナントの場合にかぎるオプトインについての情報は次の通りです。米国テナントの環境で動作している且つ、お客さま自らがデータを共有したいと望む場合には下記を参照ください。

> [!NOTE]
**Copilot AI 機能のデータ共有とは何ですか？**<br><br>
マイクロソフトは、Dynamics 365 Copilot および Power Platform Copilot の新しい AI を活用した Copilot 機能の品質を向上させ、より正確な結果を生成するよう常に取り組んでいます。 Copilot 機能を改善する重要な方法の 1 つは、Copilot 機能へのユーザーの入力、関連する出力、およびテレメトリをより深く理解することです。<br>
***Power Platform 管理センターの Dynamics 365 Copilot および Power Platform Copilot AI 機能のデータ共有設定を有効にすると、*** マイクロソフトは、Dynamics 365 および Power Platform Copilot AI 機能、サービス、機械学習モデル、および関連システムを構築、改善、検証するために、ユーザーの自然言語入力、出力、および関連テレメトリを含め (ただし必ずしもこれらに限定されない)、顧客データを取得し、手動でレビューできるようになります。<br>
当社は、Azure OpenAI Service 基盤モデルの学習に顧客データを使用しません。

<br>


>[!NOTE]
**顧客データへのアクセスが可能な範囲はどうなっていますか？**<br><br>
Azure OpenAI Service 上に保存された不正使用を監視するためのデータは不正利用の監視を目的として、必要な場合に限り Microsoft 従業員しか確認でません。

<br>
上記、Note 参考URL

[Dynamics 365 および Power Platform における Copilot AI 機能のオプションのデータ共有に関する FAQ](https://learn.microsoft.com/ja-jp/power-platform/faqs-copilot-data-sharing)


<br><br>

>Copilot の回答は常に事実に基づいていますか？

他の生成 AI と同様に、Copilot の応答が 100% 事実である保証はありません。 事実に基づいて問い合わせに回答するように当社は改善を継続していますが、他のユーザーに送信する前に必ず自分の判断で出力を確認する必要があります。 Copilot は下書きと概要を提供して作業を効率化しますが、これは完全に自動です。 AI で生成したコンテンツは常に確認する必要があります。

当社のチームは[責任ある AI の原則](https://learn.microsoft.com/ja-jp/dynamics365/faqs-copilot-data-security-privacy)に沿って、誤報や偽情報、コンテンツのブロック、データの安全性、有害なコンテンツや差別的なコンテンツを助長するなどの問題に積極的に取り組んでいます。

<br><br>

>Copilot は有害なコンテンツをどのようにブロックしますか？

Azure OpenAI Service には、中核モデルと並列に動作するコンテンツフィルターシステムが含まれます。 このシステムは、有害なコンテンツの出力を特定し、ブロックするように設計された分類モデルにより、入力プロンプトと応答の両方を実行することで機能します。[Azure OpenAI コンテンツ フィルターの詳細情報](https://learn.microsoft.com/ja-jp/azure/ai-services/openai/concepts/content-filter?tabs=warning%2Cpython#harm-categories)



<br><br>

参考 URL
- [コパイロットに関するよくあるご質問](https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/faqs-copilot)
- [Copilot データ共有に関するよくあるご質問](https://learn.microsoft.com/ja-jp/power-platform/faqs-copilot-data-sharing)
- [Copilot のセキュリティとプライバシーに関するよくあるご質問](https://learn.microsoft.com/ja-jp/dynamics365/faqs-copilot-data-security-privacy)

<br><br><br>


<br>
FAQ では実際にご質問をいただくことが多い内容を中心に取り上げさせていただきました。<br>ここからは少し毛色を変えて、同様に最近問い合わせの増えている AI Builder のデータの取り扱いについてご紹介させていただきます。
<br>

## Power Platform における AI Builder の位置づけ
**AI Builder は、Microsoft Power Platform の一部です。** AI Builder を使えば、Microsoft Power Platform 機能を使用した際に、ビジネスプロセスを最適化する AI モデルを作成することができます。例えば、ニーズに合わせて調整されたカスタムモデルを作成したり、業務シナリオで使用できる事前構築済みモデルの選択が可能です。 <br>AI Builder を使用すると、インテリジェンスを使用してプロセスを自動化し、Power Apps と Power Automate でデータからインサイトを収集することができます。<br><br>

## AI Builder のテキスト生成モデル
### クラウドフローにおける AI Builder のテキスト生成モデル

クラウドフローの Copilot は、Power Automate フローについて説明し、その過程で役立つガイダンスを提供するだけで、作成および編集ができるように設計されています。

AI Builder Power Automate Power Apps のテキスト生成モデルを使用すると、テキストの要約、回答の下書き、テキストの分類など、さまざまなシナリオで、フローや組み込みアプリで GPT モデルを直接使用できます。

<br>
AI Builder のテキスト生成モデルも、米国の環境が前提条件で、Azure OpenAI Service に搭載されています。そのため、格納場所は米国であり、使用用途や閲覧可能ユーザー、データ保護などもここまでの記載と同様の扱いになっています。

<br>

### AI builder のカスタムプロンプト
AI Builder プロンプトは、Azure OpenAI Service を利用した GPT-3.5 Turbo モデルで実行されています。

>>Available in the United States, Australia, UK, and Europe
>>GPT Prompts and Prompt Builder will be generally available this week across the United States, Australia, the United Kingdom, and Europe. GPT Prompts currently leverage the GPT-3.5-Turbo model from Azure’s OpenAI Service, meaning your prompts and outputs: <br>

> [!IMPORTANT]
are NOT available to other customers.
are NOT available to OpenAI.
are NOT used to improve OpenAI models.
are NOT used to improve any Microsoft or 3rd party products or services.
are NOT used for automatically improving Azure OpenAI models for your use in your resource.

上記からも判断できる通り、テキストを作成の際のプロンプトの内容が、教師データとして用いられることはありません。

<br>

参考 URL
- [AI Builder プロンプトについて](https://learn.microsoft.com/ja-jp/ai-builder/create-a-custom-prompt)
- [AI Builder GPT Prompts are generally available](https://powerautomate.microsoft.com/en-us/blog/ai-builder-gpt-prompts-are-generally-available/)
<br><br><br>


免責事項<br>
※本情報の内容 (添付文書、リンク先などを含む) は、作成日時点でのものであり、予告なく変更される場合があります。<br>