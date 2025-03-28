---
title: アプリごとの Power Apps プラン (per app plan) ライセンスについて 
date: 2022-11-18 12:00:00
tags:
  - Power Apps
  - License
categories:
  - [Power Apps]
---

こんにちは、Power Platform サポートの山田です。<br/>
今回は、アプリごとの Power Apps プラン (per app plan) ライセンスについて、下記項目の順にご説明いたします。  

1. 前提  
2. アプリごとの Power Apps プランライセンスとは  
3. ライセンスに含まれる Power Automate の使用権  
4. ライセンス割り当て方法  
5. ライセンス割り当て可能なユーザー  
6. ライセンスを割り当て解除するには  
7. レポート機能について  
8. 従量課金プランについて  
<br/><br/>

<!-- more --> 
## 1. 前提  
キャンバス アプリについては、アプリを実行する際に、実行するユーザーに有効なライセンスが必要となります。
<br/><br/>
そのため、有償版のライセンスがないユーザーでも、有償機能を含むアプリの作成は可能でございます。  
有償機能を含むアプリを実行する場合、実行する際に、対象ユーザーに有償版のライセンスの付与が必要となります。
<br/><br/>
必要なライセンスは、アプリのライセンスの種類で確認が可能でございます。
<br/><br/>
※下記いずれかの場合、アプリのライセンス種類が「プレミアム」になります。  

- アプリ内で有償機能を利用している  
- アプリからフローを呼び出している場合、フロー内で有償機能を利用している  

その場合には、アプリを実行するユーザー全員に Power Apps の有償ライセンスが必要となります。
<br/><br/>
アプリのライセンス種類の確認方法については、下記公開情報をご参照ください。  
[アプリのライセンス指定の確認方法 ](https://learn.microsoft.com/ja-jp/power-apps/maker/canvas-apps/license-designation#check-app-license-designation-from-app-settings)    
<br/><br/>


## 2. アプリごとの Power Apps プランライセンスとは  
Power Apps の全ての機能を利用可能でございます。  
1 ライセンスで 1 ユーザーが 1 アプリを利用可能でございます。
<br/><br/>
実行したいアプリが複数ある場合には、実行したいアプリ数分のライセンスが必要となります。  
1 つのアプリを複数ユーザーが実行したい場合、実行するユーザー数分のライセンスが必要となります。
<br/><br/>
詳細については下記公開情報､ライセンスガイドをご参照ください。  
[アプリごとの Power Apps プランについて](https://learn.microsoft.com/ja-jp/power-platform/admin/about-powerapps-perapp)    
[Microsoft Power Apps、Microsoft Power Automate、Microsoft Power Virtual Agents ライセンスガイド](https://go.microsoft.com/fwlink/?LinkId=2085130&clcid=0x411)  
※PDF ファイルがダウンロードされます。
<br/><br/>
  
　  
## 3. アプリごとの Power Apps プランライセンスに含まれる Power Automate の使用権  
アプリのコンテキスト内であればプレミアムコネクタを含むフローの利用が可能でございます。
<br/><br/>
アプリのコンテキスト内とは、下記トリガーのフローを指します。  

1. Power Apps トリガーのフロー  
2. Power Apps (V2) トリガーのフロー  
3. スケジュールトリガーでアプリ内のデータソースを操作するためのフロー  
4. 自動トリガーでアプリ内のデータソースを操作するためのフロー  
5. 手動トリガーでアプリ内のデータソースを操作するためのフロー  

ただし、アプリごとの Power Apps プランライセンスのみ付与の場合、現状の動作制限として、上記 3, 4, 5 のフローかつプレミアム コネクタを含むフローを作成することはできません。  
フローが作成可能な Power Apps の試用版ライセンスを取得する必要がございます。  
<br/><br/>

## 4.アプリごとの Power Apps プランライセンス割り当て方法  　  
環境にライセンス割り当て　→　アプリに割り当て　→　ユーザーにアプリ共有　というイメージとなります。

1. Power Platform管理センターから、該当の環境に対してパスを割り当てます。    リソース > 容量 > アドオン > パス割り当て対象の環境のペンマーク > アプリのパス  ![](./power-apps-per-app-license/assign-to-envs.png)
2. Power Apps ポータルサイトにて対象アプリの「アプリ パスごとの自動割り当て」(または「アプリごとのライセンス」) を「オン」に変更します。  
　1 で対象環境にパスの割り当てがされていない場合、その環境下でアプリに対して「アプリ パスごとの自動割り当て」が設定出来ません。  
　パスの設定が per app プランでアプリを共有するための前提の設定となります。 

　アプリ > 対象アプリを選択 > 設定 > 「アプリパスごとの自動割り当て」(または「アプリごとのライセンス」)   
　![](./power-apps-per-app-license/assign-to-apps.png)  
　![](./power-apps-per-app-license/assign-to-apps2.png)  

　※環境にアプリごとの Power Apps プランライセンスのパスを割り当てている場合、2020 年 10 月 1 日以降に作成したアプリについては、自動にオンとなります。   
　※アプリの設定は、対象アプリにおける下記のユーザーの場合に、設定可能でございます。  
　・作成者   
　・共同所有者   

3. 「アプリ パスごとの自動割り当て」(または「アプリごとのライセンス」) を有効化後、  
対象のユーザーに対してアプリを共有することで、 共有されたユーザーに対して Power Apps per app plan が自動的に有効となります。  
アプリ共有方法は下記公開情報をご参照ください。   
[キャンバス アプリを組織と共有する](https://learn.microsoft.com/ja-jp/power-apps/maker/canvas-apps/share-app)    
<br/><br/>
　　  
## 5. アプリごとの Power Apps プランライセンス割り当て可能なユーザー  
下記より設定いただけます。  
Power Platform 管理センター > 設定 > アドオン容量の割り当て   
![](./power-apps-per-app-license/who-can-assign.png)
<br/><br/>
「任意の環境管理者」の場合、対象環境において「システム管理者 (System Administrator)」のセキュリティロールが
割り当てられているユーザーが割り当てが可能でございます。
<br/><br/>
「特定の管理者のみ」の場合、下記 3 種の管理者ユーザーが割り当て可能でございます。  

- グローバル管理者  
- Dynamics 365 管理者  
- Power Platform 管理者  

詳細については下記公開情報をご参照ください。  
[アドオン容量を割り当てることができるコントロール](https://learn.microsoft.com/ja-jp/power-platform/admin/capacity-add-on?WT.mc_id=ppac_inproduct_settings#control-who-can-allocate-add-on-capacity)    
<br/><br/>
  

## 6. アプリごとの Power Apps プランライセンス割り当て解除するには
下記いずれかをご実行ください。  

- アプリの共有を停止   
- 環境に割り当てるパスを解除 (0 にする)   

<br/><br/>

## 7. レポート機能について
現状、各種管理センターなどから、ユーザーに対してアプリごとの Power Apps プランライセンスが割り当てられたことを確認できる機能のご用意はない状況でございます。  
アプリごとの Power Apps プランライセンスの使用レポート機能は現在開発中となっております。  
下記公開情報にて最新情報を記載しておりますので、ご参照ください。  
[アプリ ライセンスごとの Power Apps を使用しているユーザー リストを見ることができますか?](https://learn.microsoft.com/ja-jp/power-platform/admin/about-powerapps-perapp#when-will-i-be-able-to-see-the-list-of-users-who-are-using-the-power-apps-per-app-license)   
<br/>
レポート機能が未実装のたため、現状ではライセンスを消費する制御も未実装となっております。

1. 環境にライセンスを割り当てる  
2. 該当のアプリの設定で「アプリごとのライセンス」をオンにする  
3. アプリをユーザーに共有する  

上記の 1, 2, 3 を満たす場合、環境に割り当てられたパスの数に関わらず、  
アプリが共有されたユーザーは Power Apps per app plan の範囲内でアプリの利用が可能となります。  
※環境に割り当てられたパスの数が3つだとしても、4 人以上のユーザーへの共有、アプリの実行が可能でございます。  
<br/>
実際に環境内でアプリの利用が可能な状態のユーザーの数と、  
割り当てられたアプリのパスの数に差異が生じてしまう場面もあるかと存じますが、  
環境に割り当てられたパスの数と、実際に Power Apps per app plan の利用権でアプリを利用しているユーザーの数を  
お守りいただくようにお客様にはご案内を差し上げている状況でございます。
<br/>

また、今後、数の整合性を取るためのチェック機能が実装される可能性もございますため、  
環境に割り当てられたパスの数と実際に利用するユーザーの数が乖離しないようご運用をいただけますと幸いでございます。  
<br/>
想定される数のパスをご購入いただき、運用をいただけておりましたら、  
弊社としてはライセンス要件上問題ないと考えております。
<br/>
製品としての制御もございませんので、問題なくご利用いただけますためご安心いただけますと幸いでございます。
<br/><br/>

## 8. 従量課金プランについて
従量課金のプランがございます。
詳細については下記公開情報をご参照ください。  
[従量課金制プランの概要](https://learn.microsoft.com/ja-jp/power-platform/admin/pay-as-you-go-overview)   
