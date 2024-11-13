---
title: Power Automate / Power Platform 要求数制限
date: 2024-11-24 09:00:00
tags:
  - Power Platform
  - Power Automate
---

# Power Automate / Power Platform 要求数制限

こんにちは、Power Platform サポートチームの網野です。 <br>
今回は Power Automate における Power Platform 要求数の制限についてご案内いたします。

> [!IMPORTANT]
> [公開情報](https://learn.microsoft.com/ja-jp/power-platform/admin/online-requirements) が正となりますので、ライセンスをご用意する際は必ず最新の公開情報を確認してください。


<!-- more -->

## Power Platform 要求数とは
Power Automate ではコネクタのアクションを組み合わせることで、複数の製品をまたいだ処理を自動化することができます。  
コネクタの各アクションにて API リクエストを通じてコネクタ接続先とデータのやり取りを行っており、Power Automate ではその API リクエストを Power Platform 要求数と呼びます。

もう少し詳しくイメージをつかむために、単純なフローを用いて説明します。  
指定のメールフォルダにメールが届いたときにメールの内容をエクセルに転記するフローを作成するとします。  
このフローを実行すると、「新しいメールが届いたとき(V3)」トリガーと「表に行を追加」アクションでそれぞれ 1 回ずつ、合計 2 回分 Power Platform 要求数を消費します。
![](power-automate-ppr/api-request.png)



## Power Platform 要求数の制限とは
フローは上記のように、内部的に Power Platform 要求数を消費して実行されます。
Power Platform 要求数の実行回数はライセンスごとに上限値が決まっており、上限値を超えて利用するとフローが低速化する動作となります。  
一例をあげますと、通常 1 分程度で完了するフローが完了まで 2 時間程度かかるというような動作になります。  
この制限のことを Power Platform 要求数の制限と呼びます。

参考
* [フローが制限を超過している場合はどうすればよいですか?](https://learn.microsoft.com/ja-jp/power-platform/admin/power-automate-licensing/faqs#what-can-i-do-if-my-flow-is-above-limits)
* [低速フローのトラブル シューティング](https://jpdynamicscrm.github.io/blog/powerautomate/troubleshoot-throttling-flow/)
<br>

フローは 24 時間あたりの Power Platform 要求数を監視しており、要求数がライセンスの制限値を上回りますと内部的にアクションが待機状態になります。  
待機状態になったアクションは、24 時間あたりの Power Platform 要求数がライセンスの制限値を下回りますと再び動き出します。

## ライセンスごとの API リクエスト数

ライセンスごとの Power Platform 要求数は以下のとおりです。  
2024 年 11 月現在は、「24時間あたりのリクエスト数（移行期間）」列に記載された制限が適用されています。  
移行期間の終了日は決まっていませんが、移行期間後の制限に合わせてライセンスをご用意いただきますようお願いをしています。

| ライセンス | 24時間あたりのリクエスト数（移行期間<b><font color=red>後</font></b>） | 24時間あたりのリクエスト数（移行期間）
| :- | :- | :- |
| Power Automate Premium| ユーザーあたり40,000 | クラウドフローあたり 200,000
| Power Automate Process| ライセンスあたり250,000 | ライセンスあたり 500,000
| Power Automate per user (レガシー)| ユーザーあたり40,000 | クラウドフローあたり 200,000
| Power Automate per flow (レガシー)| ライセンスあたり250,000 | ライセンスあたり 500,000
| Power Automate free| ユーザーあたり 6,000 | クラウドフローあたり 10,000
| Microsoft/Office 365 | ユーザーあたり 6,000 | クラウドフローあたり 10,000

なお、2024 年 11 月現在は Power Automate にのみ制限がかかっていますが、移行期間後は Power Apps / Copilot Studio / Dataverse にも Power Platform 要求数の制限が適用される予定です。  
決まっている内容につきましては、[要求の制限と割り当て](https://learn.microsoft.com/ja-jp/power-platform/admin/api-request-limits-allocations#what-is-a-microsoft-power-platform-request) に記載していますのでご参照ください。

> [!IMPORTANT]
> 移行期間中は上記に加えて、1日あたりユーザーごとに100万アクションの制限があります。

## テナント内の Power Platform 要求数を確認する方法
### 管理者
[ライセンスユーザーレポート](https://learn.microsoft.com/ja-jp/power-platform/admin/api-request-limits-allocations#licensed-user-report) および [フローごとのレポート](https://learn.microsoft.com/ja-jp/power-platform/admin/api-request-limits-allocations#per-flow-report) から確認できます。

参考：
* [管理者が環境の使用状況を分析するためのどのようなツールがありますか?](https://learn.microsoft.com/ja-jp/power-platform/admin/power-automate-licensing/faqs#as-an-admin-what-tools-do-i-have-to-analyze-my-environments-usage)


> [!NOTE]
> Power Platform 要求数のレポート機能は 2024 年 11 月現在、プレビュー機能として提供しています。

### フロー作成者、共同所有者
恐れ入りますが、フロー作成者や共同所有者が Power Platform 要求数の消費状況を確認する方法は現時点ではご用意がございません。  
フロー単位で消費された Power Platform 要求数は [フローのアクション要求数を数えてみる](https://jpdynamicscrm.github.io/blog/powerautomate/troubleshoot-throttling-flow/#%E3%83%95%E3%83%AD%E3%83%BC%E3%81%AE%E3%82%A2%E3%82%AF%E3%82%B7%E3%83%A7%E3%83%B3%E8%A6%81%E6%B1%82%E6%95%B0%E3%82%92%E6%95%B0%E3%81%88%E3%81%A6%E3%81%BF%E3%82%8B) に記載の方法で確認できます。


参考：
* [作成者が使用状況を分析するためのどんなツールがありますか?](https://learn.microsoft.com/ja-jp/power-platform/admin/power-automate-licensing/faqs#as-a-maker-what-tools-do-i-have-to-analyze-my-usage)

## Power Platform 要求数のカウント方法

### Power Platform 要求数を消費するケース
* フロー内のすべてのトリガー / アクション  
   「変数の初期化」や「スコープ」アクションも Power Platform 要求数を 1 消費します。
* アクション実行時に行われたリトライ  
   コネクタから API を実行してリトライ可能なエラーコードが返ってきた場合、コネクタは各アクションの設定に応じてリトライ処理を行います。リトライの回数分 Power Platform 要求数を消費します。
* 失敗したアクション  
   アクションがエラーとなった場合も Power Platform 要求数を消費します。
* ループ内のアクション  
   ループアクションがある場合、ループ内のアクションは実行された回数分の Power Platform 要求数を消費します。例えば、「Apply to each」の中に アクションが 3 つあり、3 回ループした場合、「3（アクション） × 3（ループ回数）+ 1 (Apply to each) = 10」と計算されます。

### Power Platform 要求数を消費しないケース
* スキップされたアクション  
   スキップされて実行されなかったアクションは Power Platform 要求数を消費しません。  
   更新を定期的に監視するポーリングトリガーのフローでは、データの更新があったときのみトリガーの Power Platform 要求数が消費されます。

参考
* [何が Power Platform 要求と見なされますか?](https://learn.microsoft.com/ja-jp/power-platform/admin/power-automate-licensing/faqs#what-counts-as-power-platform-request)


## Power Platform 要求数の消費先

### フローに付与するライセンス

Power Automate Process / Power Automate per flow (レガシー) などフローに付与するタイプのライセンスを利用している場合は、フローに付与されたライセンスの Power Platform 要求数を消費します。

### ユーザーに付与するライセンス
フローにライセンスを付与していない場合は、ユーザーの持つライセンスの Power Platform 要求数を消費します。  

#### フロー実行者の Power Platform 要求数を消費するケース
ボタンや Power Appsなどのインスタントクラウドフロー 

例）
SharePoint の「項目が選択されたとき」トリガーはインスタントクラウドフローとなるため、SharePointリストからフローを実行したユーザーの Power Platform 要求数を消費します。

#### フロー作成者の Power Platform 要求数を消費するケース
* 自動フロー 
* スケジュールフロー

例）
SharePoint の「項目が作成されたとき」トリガーは自動フローとなるため、SharePoint に項目を作成したユーザーが誰かに関わらず、フロー作成者の Power Platform 要求数を消費します。


> [!NOTE]
> * フロー所有者がサービスプリンシパルの場合、[ライセンスのないユーザーの制限](https://learn.microsoft.com/ja-jp/power-platform/admin/api-request-limits-allocations#non-licensed-user-request-limits) を利用します。


### 子フロー
親フローが子フローを呼び出す場合、子フローは親フローの制限を引き継ぎます。  
つまり、親フローがインスタントフローであれば親フロー実行者の制限を利用し、自動フローであれば親フロー作成者の制限を利用します。  
<br>
子フローにフローに付与するライセンスがある場合は、フローのライセンスが利用されます。
<br>
子フローに Power Automate Process がある場合は、子フローの呼び出し元/先のフローに子フローのプロセスライセンスの制限が利用されます。呼び出し元／先のフローにもプロセスライセンスが付与されている場合は、付与されているライセンスが利用されます。

参考
* [クラウド フロー で使用されるリクエスト制限は誰のものですか?](https://learn.microsoft.com/ja-jp/power-platform/admin/power-automate-licensing/faqs#whose-power-platform-request-limits-are-used-by-the-cloud-flow)


## よくある質問
### アクションの接続を別のユーザーに変更していますが、Power Platform 要求数の消費先も変わりますか
いいえ、変わりません。  
Power Platform 要求数の消費先は、アクションの接続先に依存しません。

### 制限値を超えたら即時フローが低速化しますか
いいえ、多少の超過は許可しています。


### フローの低速化が解消されるのは何時間後かわかりますか
恐れ入りますが、明示的に解消される時間帯をご案内することができません。  
フローのアクション数が制限値を下回ったときに解消されます。


### 移行期間はいつまでですか
移行期間終了日につきましては現時点で未定です。  
Power Platform 要求数のレポート機能が一般公開されてから、最低でも 6 カ月の猶予をもって移行期間を終了する予定です。  
移行期間の終了日が決まりしたら、メッセージセンターやブログ等にて通知を行う予定です。

### Power Platform 要求数のレポートの一般公開はいつですか。
現時点で未定です。  
実際の利用状況や利用ユーザーからのお問い合わせの状況等を踏まえて一般公開の日にちを決めていく予定です。

### 試用版アカウントの要求数はいくつですか？
有償版と同じです。

----
