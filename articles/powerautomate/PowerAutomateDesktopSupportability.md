---
title: Power Automate for desktop のサポート範囲について
date: 2022-11-11 12:00
tags:
  - Power Automate
  - Power Automate for desktop
  - お問い合わせ全般
categories:
  - [Power Platform, Power Automate, Power Automate for desktop]
---

こんにちは。Power Platform サポート チームの 王 です。  
今日は Power Automate for desktop のサポート範囲についてご案内いたします。  

<!-- more -->

## 初めに

Power Automate for desktop は Windows をご利用のお客様であればどなたでも追加費用なしでご利用いただける RPA ツールです。2021 年 3 月 3日 (日本時間) に公開されました。  
[Power Automate for desktop の概要](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/introduction)  

今回は Power Automate for desktop のサポート範囲についてご案内します。  

## Power Automate for desktop のサポートについて

Windows のライセンスに含まれる Power Automate for desktop は、PC にインストールして使用する無料のアプリケーションです。  
このアプリケーションには、SLA や Microsoft サポートが含まれないため、ビジネス クリティカルな用途で使用すべきではありません。  
Power Automate for desktop をビジネス クリティカルな用途で使用する場合、サポートが含まれる適切な Microsoft ライセンスの購入をご検討ください。  
対象となるプランは以下です ※2023/8 時点  
- Power Automate Premium (旧名: Power Automate per user plan with attended RPA)
- Power Automate Process
- 従量課金制プラン
- Power Automate 試用版ライセンス
- Power Automate unattended RPA add-on（前提としてPower Automate per user with attended RPA または Power Automate per flow が必要) (※ 2023/8/1~ レガシーに変更)  

ライセンスのご購入状況は、Microsoft 365 管理センターの [課金情報] - [サービスを購入する] からご確認いただけます。  

Power Automate for desktop のサポートに関しましては、[プレミアム機能](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/premium-features#premium-feature-list)の一部として含まれておりますため、  
上記いずれのライセンスもお持ちではない場合には、サポートにお問い合わせをいただいた場合もご支援することができません。  

ライセンスをお持ちでない場合は、以下のコミュニティをご活用ください。  
[Microsoft Power Automate Community](https://powerusers.microsoft.com/t5/Microsoft-Power-Automate/ct-p/MPACommunity)

また、お問い合わせをいただく際には、弊社にてライセンスのチェックを行わせていただく場合がございますので、以下の情報を添えてお問い合わせいただけますと幸いでございます。

### お問い合わせ時の情報採取手順
1. Power Automate for desktop にサインインしているユーザーで [Power Automate ポータル](https://make.powerautomate.com/) にアクセスします。
2. 画面右上から、デスクトップ フローを作成中の環境を選択します。
3. キーボードの `Ctrl` + `Alt` + `A` を押下し、別タブにて表示される情報を全てコピーし、テキスト形式でお寄せください。

手順 3 にて取得した情報に以下のいずれかが含まれる場合、サポートが含まれるプランをお持ちのため、ご支援が可能でございます。
- "environment" 内の "isPayAsYouGoEnabled" が true の場合
- "userServicePlans" 内で "isCurrent" が true のプランにて、"rpaAttendedAllowed" または "rpaUnattendedAllowed" のいずれかが true の場合

サポート範囲については以下ライセンスガイドからもご確認いただくことが可能です。  
英語版　：https://go.microsoft.com/fwlink/?LinkId=2085130&clcid=0x409  
日本語版：https://go.microsoft.com/fwlink/?LinkId=2085130&clcid=0x411  

## 終わりに

Power Automate for desktop で弊社サポートをご利用いただけるライセンスにについてご案内いたしました。  
製品やライセンスに関する情報は随時更新されるため、最新の情報については公開情報もしくは上記ライセンスガイドをご参照ください。  

--
