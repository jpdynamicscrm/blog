---
title: Power Automate for desktopの所有者を一覧で取得する方法
date: 2022-9-29 17:00
tags:
  - Power Automate
  - Desktop flows
  - Power Automate Desktop
---

こんにちは。Power Platform サポートの原野です。
今回は、Power Automate for desktopで作成したデスクトップフローの所有者を、環境単位で一括取得する方法についてご紹介します。

デスクトップフローに関しても、環境内の所有者情報を簡単に一覧表示できます。
お試しいただけましたら幸いです。

<!-- more -->

## はじめに
環境の管理者権限のあるユーザーの場合、デスクトップフローの所有者を確認する方法は大きく分けて2つあります。

### a. Power Automateポータル画面から確認する
環境の管理者は、環境内の全てのデスクトップフローの共同所有者となります。
そのため、 __「マイフロー>デスクトップフロー」__ からフローごとの所有者を確認いただけます。
Power Automateポータル画面からデスクトップフローを確認する方法については以下にも記載されています。  
[デスクトップ フローの共有と管理 - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/manage#list-of-desktop-flows)


### b. プロセステーブルから所有者を一括で取得する
デスクトップフローの所有者等の情報は、Dataverseのプロセス(Process)テーブルに格納されています。
環境の管理者権限のあるユーザーが以下の手順を行うことで、環境ごとのデスクトップフローの所有者を一括で取得いただけます。
取得した情報はExcelファイルにエクスポートすることが可能です。


今回はよくお問い合わせをいただく、 __「b. プロセステーブルから所有者を一括で取得する」__ 方法の具体的な手順についてご案内いたします。


## 1. Power Apps の画面に遷移します
Power Automate 画面より __「データ>テーブル」__ からメニューを選択することでPower Apps の画面に遷移します。
![](./PowerAutomateDesktop-GetOwner/image1.png)
<br>
<br>
## 2. [詳細設定] を選択します
表示されたPower Apps画面の右上のギアアイコンより、__「詳細設定」__ を選択します。
![](./PowerAutomateDesktop-GetOwner/image2.png)
<br>
<br>
## 3. フィルタアイコンを選択します
表示された Dynamics 365 画面右上のフィルタアイコンを選択します。
![](./PowerAutomateDesktop-GetOwner/image3.png)
<br>
<br>
## 4. 「プロセス」を選択した上で、条件をクリアします
表示された「高度な検索」の「検索」画面で、 __「プロセス」__ を選択した上で、赤枠の  __「クリア」__ で条件をクリアします。
![](./PowerAutomateDesktop-GetOwner/image4.png)
<br>
<br>
## 5. 条件を設定し、リボンの [結果] をクリックします
検索条件として、カテゴリから __「デスクトップフロー」__ を選択下さい。
列の編集にて６で表示される列を選択することができます。
![](./PowerAutomateDesktop-GetOwner/image5.png)
<br>
<br>
## 6. デスクトップフローの一覧が表示されます
所有者列を参照することで、所有者を確認することができます。
表示された検索結果は __「プロセスのエクスポート」__ からExcel にエクスポートすることが可能です。
![](./PowerAutomateDesktop-GetOwner/image6.png)
<br>
<br>
## おわりに

デスクトップフローの所有者を、環境単位で一括取得する方法についてご紹介しました。
少しでもお役に立つ情報がございましたら幸いです。

---
