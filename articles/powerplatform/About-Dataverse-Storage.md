---
title: Dataverse のストレージ容量について
date: 2023-03-13 12:00:00
tags:
  - Power Platform
  - Dataverse
  - Dataverse ストレージ
---

こんにちは、Power Platform サポートの原です。

今回は、Dataverse のストレージ容量についてご説明いたします。

<!-- more -->
## 目次

1. [Dataverse ストレージの概要](#anchor-about-storage)
2. [よくあるご質問](#anchor-QA)
3. [おわりに](#anchor-finish)

<a id='anchor-about-storage'></a>
## 1. Dataverse ストレージの概要

### 保存されるデータと容量の種類について
Power Platform では、各 Dataverse 環境に格納された Dataverse のデータを「Dataverse ストレージ容量」としてテナントごとに計上します。
Dataverse のデータはその特性に応じて以下の 3 種類の領域に格納されます。

* データベース容量 : 通常の業務データなど、リレーショナル データベースでの保存に適したデータが格納されます。
* ファイル容量 : BLOB 形式での保存に適したデータが格納されます。
* ログ容量 : 監査ログなど、長期保存される履歴データが格納されます。

### テナントごとの容量割り当てについて
弊社では Power Platform のクラウド リソースを各お客様に適切に利用いただくため、ご購入いただいたライセンスに応じてテナントごとに容量上限を割り当てております。
容量上限を超過した場合にも、Dataverse へのデータ登録を含めて通常の製品機能を引き続きご利用いただけますが、一部の環境管理操作の実行が制限されます。
容量上限を超過した場合の具体的な挙動や、容量削減の方法につきましては、後述の[よくあるご質問](#anchor-QA)セクションをご参照ください。

ストレージの詳細につきましては、以下の公開情報もぜひご参照ください。
* [新しい Microsoft Dataverse ストレージ容量](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#example-storage-capacity-scenarios-overage-enforcement)

***
<a id='anchor-QA'></a>
## 2. よくあるご質問
### 容量が不足した場合の対処法はありますか？ 
<a id='anchor-reduce-storage'></a>
1. 不要なデータや、システムにより蓄積された内部的な履歴データなどを削除することで、運用に影響を及ぼさない形で容量を確保いただけます。
手順の詳細につきましては、下記の公開情報をご参照ください。
   * [ストレージの空きを増やす](https://learn.microsoft.com/ja-jp/power-platform/admin/free-storage-space)
   * [AsyncOperationBase テーブルと WorkflowLogBase テーブルからレコードをクリーンアップする](https://learn.microsoft.com/ja-jp/power-platform/admin/cleanup-asyncoperationbase-table)

2. 使用していない環境を削除することで容量を確保いただけます。
検証などにより一時的に使用していたものの、現在では使用していない環境などがございましたら、ぜひ削除をご検討ください。
手順の詳細につきましては、下記の公開情報をご参照ください。
   * [環境を削除する](https://learn.microsoft.com/ja-jp/power-platform/admin/delete-environment)

3. ライセンス購入により、容量を追加いただけます。
1,2 をお試しいただいた上でも容量が不足する場合に備え、容量割り当て用のアドオン ライセンスをご利用いただけます。
ライセンスの詳細につきましては、下記のライセンス ガイドをご参照ください。<br>
※ 下記のリンクにアクセスしていただきますと、PDF ファイルのダウンロードが開始される可能性がございますのでご注意ください。
   * [Dynamics 365 ライセンス ガイド](https://go.microsoft.com/fwlink/?LinkId=866544&clcid=0x411) > キャパシティ ライセンス
   * [Power Apps および Power Automate ライセンス ガイド](https://go.microsoft.com/fwlink/?LinkId=2085130&clcid=0x411) > キャパシティ アドオン

   また、テナントに割り当てられている容量の詳細は、 Power Platform 管理センター > [リソース] > [容量] > [概要] タブ > [ソースごとのストレージ容量] からご確認いただけます。

### 容量を超過するとどうなりますか？
テナントの容量上限を超過した場合にも、Dataverse へのデータ登録を含む通常の製品機能を引き続きご利用いただけます。
ただし、以下の環境管理操作については実施いただけなくなります。

* 新しい環境の作成 (1 GB 以上の容量が必要です)
* 環境のコピー
* 環境の復元
* 試用環境を有料環境に変換 (1GB 以上の容量が必要です)
* 環境の回復 (少なくとも 1GB の空き容量が必要)
* Dataverse データベースを環境に追加

※ご参考 : [記憶域容量の権利を超えるための変更](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#changes-for-exceeding-storage-capacity-entitlements)

上記の機能は、一般ユーザーではなく管理者様や開発者様にて、開発作業や障害発生時・メンテナンス時などにご利用いただくことを想定したものでございます。
スムーズに管理作業を行い、安全に Power Platform を運用いただくためにも、Dataverse 容量の上限を遵守いただきますようお願い申し上げます。

### 特定のストレージ領域のみ容量が超過した場合も、テナント全体の容量上限を超過しますか？
Dataverse では、各データの特性にあわせて保存コストの異なる領域にデータを格納しております。
そのため、より上位のストレージ領域に十分な空き容量が存在する場合には、上位ストレージの空き容量で下位ストレージの不足分を補完いただけます。
具体的には、各領域に以下の補完関係がございます。
1. データベース容量：補完いただけません。
2. ログ容量：データベース領域で補完いただけます。
3. ファイル容量：データベースおよびログ領域で補完いただけます。

以下の公開情報でも、補完関係のパターン例をご紹介しておりますので、ご参考になりましたら幸いです。
* [ストレージ容量シナリオの例、超過分の強制 / 超過分なし](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#example-storage-capacity-scenarios-overage-enforcement)

### 環境**作成時**には、どのようにストレージが消費されますか？
環境を作成いただくにあたっては、環境の種類や構成に関わらず、テナントにて 1 GB の空き容量が必要でございます。
空き容量が 1 GB 未満の場合には、環境を作成いただくことができません。

なお、環境作成時の実際の消費容量は、環境の構成によって異なります。
Dataverse なしの環境では、環境作成時の内部処理にて、データベース容量が必ず 1 GB 消費されます。
Dataverse ありの環境では、環境作成時に選択いただいた言語やアプリなどの構成要素によって消費容量が変化いたします。
そのため Dataverse ありの環境では、実際の容量が 1 GB 未満となることや 1 GB を超えることがございますが、目安として 1 GB の空き容量制限を設けております。

### 環境**作成後**には、どのようにストレージが消費されますか？

* Dataverse なしの場合<br>
環境作成時に消費されたデータベース容量 1 GB 以外は消費されることは記事執筆時点ではありません。

* Dataverse ありの場合<br>
ご自身の環境に追加いただいた業務データ等が容量として消費されます。

また、以下の例のように、業務データ以外の要因でも容量が増加する場合がございます。

1. Dataverse では、お客様により快適にサービスをご利用いただくため、週次アップデートなどの製品改善を行っております。このアップデートに伴い、データ構造や製品内部のプログラム ファイルに変更が発生し、消費容量が増加する場合がございます。
2. Dataverse のデータベース領域では、検索性能向上のために随時インデックスの最適化を行っております。この過程で消費容量が増える場合がございます。<br>
ご参考 : [インデックスはデータベース ストレージの使用に影響しますか。](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#do-indexes-affect-database-storage-usage)
3. Excel インポートなどの非同期処理を実行いただいた場合には、Dataverse にジョブの実行履歴データが保管されます。長期間環境を運用いただくと、この履歴データが容量を圧迫する場合がございます。<br>
ご参考 : [AsyncOperationBase テーブルと WorkflowLogBase テーブルからレコードをクリーンアップする](https://learn.microsoft.com/ja-jp/power-platform/admin/cleanup-asyncoperationbase-table)

Dataverse では、上記のように業務と直接関連のないデータが容量を消費する場合もございます。
これらの容量には、上記 1 や 2 のように削除いただけないものもございますが、一方で 3 などのデータは削除いただけます。
ご不要なデータを削減いただくことで、容量消費状況が大きく改善することもございますので、もし容量がひっ迫した場合には [「容量が不足した場合の対処法はありますか？」](#anchor-reduce-storage) のセクションをご参照ください。

### 既定環境や Teams 環境など、実稼働・サンドボックス以外の環境を利用しています。テナントの容量について考慮すべき点はありますか？
以下の環境は、製品をお試しいただく目的などでの小規模利用を想定していることから、テナントの割り当て容量を消費しません。
* Dataverse for Teams
* 試用版
* プレビュー
* サポート
* 開発者

※ ご参考 : [Dataverse タブ](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#dataverse-tab) > 注意 セクション

また、既定環境には公開情報に定められた所定の容量が個別に割り当てられ、割り当て分を超過した容量のみがテナント側の容量を消費いたします。

上記の種別の環境につきましては、個別に容量の制約が存在するものもございます。
ご利用にあたってはぜひ以下の公開情報もご参照ください。
* [既定環境](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#dataverse-tab)
* [Dataverse for Teams 環境](https://learn.microsoft.com/ja-jp/power-platform/admin/about-teams-environment#capacity-limits)
* [開発者環境](https://learn.microsoft.com/ja-jp/power-apps/maker/developer-plan#%E9%96%8B%E7%99%BA%E8%80%85%E7%92%B0%E5%A2%83%E3%81%AE%E5%AE%B9%E9%87%8F%E5%88%B6%E9%99%90%E3%81%AF%E3%81%A9%E3%82%8C%E3%81%8F%E3%82%89%E3%81%84%E3%81%A7%E3%81%99%E3%81%8B)

### テナントの Dataverse 容量について通知する機能はありますか？
テナント Dataverse 容量がひっ迫した際には、以下のタイミングで通知メールが送信されます。
* Dataverse 空き容量が全体の 15 % 未満となった場合
* Dataverse 空き容量が全体の 5 % 未満となった場合
* Dataverse 容量が上限を超過した場合

通知の詳細や送信対象者などにつきましては、以下の公開情報をご参照ください。
* ご参考 : [記憶域容量の権利を超えるための変更](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-storage#changes-for-exceeding-storage-capacity-entitlements)

なお記事執筆時点では、通知の送信対象や容量の閾値を変更する機能のご用意がございません。
管理者役割ではないユーザーに通知を送信されたい場合には、通知メールを適宜転送いただく運用をご検討いただけますと幸いです。

### どのアプリやフローを作成した際に、容量を使用しますか？
モデル駆動型アプリや Dynamics 365 のアプリケーションを作成することで、ストレージ容量（データベース容量・ファイル容量・ログ容量）を消費いたします。
また、キャンバス アプリやフローに関しましては、基本的にはストレージ容量を消費いたしません。   
ただし、例外的に以下の場合はストレージ容量を消費いたします。
* アプリやフローがソリューション内に入っている場合
* デスクトップフロー

### 

## 3.おわりに
<a id='anchor-finish'></a>

本記事が、Power Platform の Dataverse ストレージ領域を管理いただく上でのお役に立てましたら幸いです。
ご不明な点や、もし具体的に容量削減についてお困りのことなどがございましたら、弊社サポート一同にてお待ち申し上げておりますので、ぜひお気軽にお問合せください。
