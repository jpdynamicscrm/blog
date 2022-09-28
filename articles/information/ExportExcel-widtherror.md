---
title: Excelエクスポート時の列幅が規定値で出力される件について
date: 2022-09-22 12:00:00
tags:
  - Power Apps
  - Power Automate
  - Power Platform 
  - Dynamics 365 Customer Engagement
---
こんにちは、Dynamics 365 Customer Engagement サポートの木村です。  
本ブログでは、Excelエクスポート時の列幅が規定値で出力される件についてご説明いたします。

<!-- more -->
## 目次

1. [はじめに](#anchor-intro)
2. [Excelエクスポート時の列幅が規定値で出力される件について](#anchor-excelexport-widhtherror)
2. [今後の修正予定について](#anchor-futurefix)
3. [おわりに](#anchor-finish)

<a id='anchor-intro'></a>

---

## はじめに

6月11日(土)に日本環境に適用したアップデートが原因で、Excelエクスポート時の列幅が規定値で出力される事象が発生しております。
こちらの事象の詳細と今後の修正予定につきまして、当ブログにてご説明いたします。

---

<a id='anchor-excelexport-widhtherror'></a>

## Excelエクスポート時の列幅が規定値で出力される件について

アップデート内容と致しましては、「列の編集」でビューに列を追加した際、その追加した列も出力したExcelに表示されるよう動作変更が行われました。  
上記機能追加に伴い、追加リグレッションが発生していることから、ビューから出力されたファイルの表示幅が規定値(294ピクセル)になるという不具合が発生しております。  
![](./ExportExcel_widtherror/ExportExcel-widtherror-2.png)  

また、高度な検索から出力した場合には、ビューの列幅設定が反映されることを確認しております。
* 高度な検索にてExcelを出力する方法
1. 対象の画面の上部にある「…」を押下し、Create Viewを開く  
![](./ExportExcel_widtherror/ExportExcel-widtherror-6.png) 
2. Resultボタンを押下する  
![](./ExportExcel_widtherror/ExportExcel-widtherror-3.png) 
3. 全ての行を選択し、「Export to Excel」を押下する  
![](./ExportExcel_widtherror/ExportExcel-widtherror-4.png) 
4. Excelが出力される
![](./ExportExcel_widtherror/ExportExcel-widtherror-5.png) 


---
<a id='anchor-futurefix'></a>

## 今後の修正予定について
9月28日現在
修正方法については、検討段階であるため決定事項ではございませんが、修正には少なくとも2週間ほど見積もっており、その他の修正も併せて日本リージョンへの展開まで1か月以上先になる見込みでございます

<a id='anchor-finish'></a>

## おわりに

こちらのブログでは現在弊社にて確認されているExcelエクスポート時の列幅が規定値で出力される件についてご案内いたしました。
今後の修正予定につきましては、更新がございましたら「今後の修正予定について」の内容を更新いたします。
ご不便をいたしますが、修正完了および展開について今しばらくお待ちくださいませ。
