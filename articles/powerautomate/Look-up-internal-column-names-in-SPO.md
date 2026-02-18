---
title: SharePointの内部列名を確認する方法
date: 2021-06-01 11:30:00
tags:
  - Power Automate
  - SharePoint Online
  - Cloud flow
  - How to
categories:
  - [Power Automate, Cloud flow]
---

# SharePointの内部列名を確認する方法

こんにちは、Power Platform サポートチームの網野です。

今回は SharePoint リストを利用したフローを作成するときに、必ず知っておくべき内部列名とその確認方法をご案内します。
内部列名を使いこなせれば、トリガー条件やフィルタークエリを使って、SharePoint を更に便利に使いこなせます！
便利な使い方は別の機会にたくさん紹介しますので、今回は内部列名の確認方法についてしっかり覚えていきましょう。

この記事の対象者

- Power Automate をはじめたばかりの方
- SharePoint リストと Power Automate の連携に興味のある方

Power Automate を使って、一緒にビジネスを加速させましょう！
<!-- more -->

---

# 目次

1. [はじめに](#anchor-intro)
1. [内部列名って何？](#anchor-what-is-internal-column)
1. [内部列名の確認方法](#anchor-how-to-look-up-internal-column)

<a id='anchor-Intro'></a>

## はじめに

### こんなことしたいなって思ったことありませんか？

- SharePoint リストの期限列と現在日時を比較して、期限間近のアイテムを Teams やメールでリマインドしたい
- ステータス列が「完了」以外のときだけフローを実行したい

Power Automate を使えば、「期限列」や「ステータス列」の値を利用して、～この列が〇〇のとき～のような条件式を簡単に作成することができます!

詳しい方法はまた別の機会に blog として更新しますので、今回は列を指定するときに必要な内部列名について学んでいきます。

それでは早速 SharePoint リストの内部列名とは何かから、学んでいきましょう！


基礎知識は飛ばして確認方法を知りたいという方は[内部列名の確認方法](#anchor-how-to-look-up-internal-column)へ進んでください。

<a id='anchor-what-is-internal-column'></a>

## 内部列名って何?

内部列名はシステム側で列を特定するためのキー項目です。<br>

### 内部列名がないと・・・

例えば、以下のようなデータがあったとします。

| 列名 | 値 |
| :- | :- |
| Title | A 社様見積もり |
| 承認者 | 〇〇部長 |
| 期限 | 2021/06/01 |

ここに、二次承認者を加えることになりました。<br>
そこで「承認者」と「期限」の列名を変更することにしました。<br>
<br>

| 表示上の列名 | 値 |
| :- | :- |
| Title | A 社様見積もり |
| <font color=red>一次</font>承認者 | ？ |
| <font color=red>一次承認</font>期限 | ？ |
| 二次承認者 | - |
| 二次承認期限 | - |

<br>
「承認者」と「期限」という列名がなくなってしまったので、システム側が元々の値を見つけられなくなってしまいます。<br>
では、二次承認を加える前のデータに内部列名を追加してみましょう。
<br>

| 表示上の列名 | 内部列名 | 値 |
| :- | :- | :- |
| Title | <font color=red>Title</font> | A 社様見積もり |
| 承認者 | <font color=red>Approver</font>| 〇〇部長 |
| 期限 | <font color=red>deadline</font> | 2021/06/01 |

<br>
次に、「承認者」と「期限」の列名を変更して、二次承認者の情報を増やします。
<br>

| 表示上の列名 | 内部列名 | 値 |
| :- | :- | :- |
| Title | Title | A 社様見積もり |
| <font color=red>一次</font>承認者 | <font color=red>Approver</font> | 〇〇部長 |
| <font color=red>一次承認</font>期限 | <font color=red>deadline</font> | 2021/06/01 |
| 二次承認者 | Secondary Approver | - |
| 二次承認期限 | Secondary Approve deadline | - |

<br>
表示上の列名が変更されても内部列名が変わらないので、システム側も値を見つけることができますね。<br>
<br>
Power Automate から SharePoint の列を扱うときも、同様の理由で表示名ではなく、内部列名を利用します。<br>
この仕組みがあるから誰かが表示上の列名を変更された場合でも、フローを編集する必要はありませんね。
<br>
<br>
今回は説明の便宜上わかりやすい内部列名を利用しましたが、SharePoint 登録時に内部列名を利用者が設定することはできません。<br>
例えば、「承認期限」という名前で列を作ると、「_x627f__x8a8d__x671f__x9650_」という内部列名が自動で作成されます。
<br>
<br>

一見取っつきにくい表記に思えますが、どの列かを識別するためのものなので、列を表す記号と捉えて次に進みましょう。
<br>
<a id='anchor-to-use-internal-column'></a>

## 内部列名を Power Automate で利用するために

Power Automate 利用時に注意が必要になるのが、内部列名はそのままで利用できる場合と、加工が必要な場合があります。<br>
決まりなので覚えていきましょう。<br>

<br>
以下の列を持つリストを例にして、確認していきます。

![](./Look-up-internal-column-names-in-SPO/SPO-list.png)

<br>
各列の内部列名を確認していくと、以下のようになります。<br>

- 文字の種類がアルファベットからはじまる場合は、<strong><font color=red>OData_</font></strong> という接頭辞が<font color=blue>不要</font>
- 文字の種類がアルファベット以外からはじまる場合<font color=red>以外</font>は、<strong><font color=red>OData_</font></strong> という接頭辞が<font color=blue>必要</font>

<br>

| 文字の種類 | 列名 | 内部列名  |Power Automate 利用時|
| :-: | :-: |:-:|:-:|
| アルファベット | Details | Details | Details|
| アルファベット×日本語 | Details_承認内容 | Details__x627f__x8a8d__x5185__x5 | Details__x627f__x8a8d__x5185__x5 |
| 日本語 | 承認期限 | \_x627f__x8a8d__x671f__x9650_ | <font color=red>OData_</font>\_x627f__x8a8d__x671f__x9650_ |
| 記号 ( 2byte ) | ・ | \_x30fb_  | <font color=red>OData_</font>\_x30fb_ |
| 記号 ( 1byte ) | /// | \_x002f__x002f__x002f_ | <font color=red>OData_</font>\_x002f__x002f__x002f_ |
| 数値 | 123456 | \_x0031_23456  | <font color=red>OData_</font>\_x0031_23456 |

<br>
<br>

<a id='anchor-how-to-look-up-internal-column'></a>

## 内部列名の確認方法

では実際に、内部列名を確認していきましょう。

1. SharePoint にて、内部列名を確認したいリストを表示します。
2. 歯車アイコンからリストの設定を開きます。
    ![](./Look-up-internal-column-names-in-SPO/open-list-setting.png)
3. 列の一覧から、確認したい列を選択します。
    ![](./Look-up-internal-column-names-in-SPO/open-column-setting.png)
4. URL の <strong>Field= </strong>以降の値が内部列名となります。
    ![](./Look-up-internal-column-names-in-SPO/column-internal-name.png)
5. 最後に以下の表の規則に従い、アルファベットからはじまる場合以外は OData_ を付与します。

   | 文字の種類 | 列名 | 内部列名 | SharePoint 列の設定での表示 |Power Automate 利用時|
   | :-: | :-: |:-:|:-:|:-:|
   | アルファベット | Details | Details | ![](./Look-up-internal-column-names-in-SPO/column-alfabet.png) | Details|
   | アルファベット×日本語 | Details_承認内容 | Details__x627f__x8a8d__x5185__x5 |![](./Look-up-internal-column-names-in-SPO/column-alfabet_japanese.png) | Details__x627f__x8a8d__x5185__x5 |
   | 日本語 | 承認期限 | \_x627f__x8a8d__x671f__x9650_ | ![](./Look-up-internal-column-names-in-SPO/column-deadline.png) | <font color=red>OData_</font>\_x627f__x8a8d__x671f__x9650_ |
   | 記号 ( 2byte ) | ・ | \_x30fb_ | ![](./Look-up-internal-column-names-in-SPO/column-mark-2byte.png) | <font color=red>OData_</font>\_x30fb_ |
   | 記号 ( 1byte ) | /// | \_x002f__x002f__x002f_ | ![](./Look-up-internal-column-names-in-SPO/column-slash.png) | <font color=red>OData_</font>\_x002f__x002f__x002f_ |
   | 数値 | 123456 | \_x0031_23456 | ![](./Look-up-internal-column-names-in-SPO/column-num.png) | <font color=red>OData_</font>\_x0031_23456 |

これで内部列を取得することができました。
<br>
内部列名についてのご案内は以上となります。<br>

<br>
いかがでしたでしょうか。
<br>
内部列名を使えると SharePoint リストを使ったフローの幅を広げることができますので、どんどん使っていってくださいね。
<br>

---
Hope to accelerate your business with Power Automate!
