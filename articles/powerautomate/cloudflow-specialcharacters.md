---
title: Power Automate で改行が反映されない場合の対応方法
date: 2025-08-28 12:00:00
tags:
  - How to
  - Power Automate
categories:
  - [Power Automate, Cloud flow]
---

# はじめに

こんにちは、Power Platform サポートチームの宮﨑です。  
本記事では Power Automate で文字列に含まれる改行が反映されない場合の対応方法についてご案内致します。


<!-- more -->
# 目次
1. [概要](#anchor-intro)
1. [改行を反映させる方法](#anchor-line-break)
   1. [フローの例](#anchor-line-break-flow) 
1. [補足](#補足)

<a id='anchor-intro'></a>

# 概要
今回はよくあるお問い合わせとして、 Power Automate で文字列を扱う際に改行が反映されない場合の対応方法についてご案内いたします。

<a id='anchor-line-break'></a>

## 改行を反映させる方法
Power Automate のクラウドフローを使用して、 Sharepoint や Excel から取得してきた文字列（改行を含む）を 
Teams のメッセージとして投稿する際、改行が反映されないことがございます。

<a id='anchor-line-break-flow'></a>

### フローの例
今回の例では、以下の状況を想定しています。

トリガー：Sharepoint リストにアイテムが追加されたとき  
アクション：アイテムの内容を Teams に投稿する

イメージ図  
![](cloudflow-specialcharacters/line-break/flow-image.png)

フローの全体図は以下の通りです。  
![](cloudflow-specialcharacters/line-break/flow-outline.png)

ここから、フローの詳細を順を追って解説いたします。

#### Sharepoint リストにアイテムが追加されたとき  
※こちらのトリガーはあくまでも一例となります。

下画像のように、ご自身の Sharepoint リストをご指定ください。<br>
![](cloudflow-specialcharacters/line-break/sharepoint.png)


#### アイテムの内容を Teams に投稿する

下画像のように、投稿先の情報についてご指定ください。<br>
![](cloudflow-specialcharacters/line-break/teams.png)

#### メッセージ部分

以下の式をアクション内で設定することで、投稿されたメッセージに改行が反映されるようになります。

```
replace(改行を反映させたい文字列, decodeUriComponent('%0A'), '<br>')
```

![](cloudflow-specialcharacters/line-break/message.png)

---

以上が、Power Automate で文字列を扱う際に改行が反映されない場合の対応方法でございます。
お役に立てましたら幸いです。

<a id='補足'></a>

# 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって UI の遷移などが若干異なる場合があります。その場合は画面の指示に従って進めてください。  

