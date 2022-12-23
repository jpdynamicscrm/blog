---
title: Power Automate for desktop お問い合わせの際の情報取得手順
date: 2022-12-28 12:00
tags:
  - Power Automate
  - Desktop flows
  - Power Automate Desktop
---

こんにちは。Power Platform サポートの原野です。  
本記事では Power Automate for desktop 関連のお問い合わせの際の、情報取得手順についてご案内いたします。

<!-- more -->
# 目次

1. [概要](#anchor-intro)
1. [情報取得手順詳細](#anchor-how-to-collect)
      1. [Power Automate for desktopのバージョン](#anchor-pad-version)
      1. [OSのバージョン](#anchor-os-version)
      1. [コンピューター ログ](#anchor-computer-log)
      1. [Fiddler ログ](#anchor-fiddler-log)
      1. [マシン登録の情報](#anchor-machine-information)
      1. [リモートデスクトップ設定](#anchor-remote-desktop-setting)
      1. [Power Automate for desktopのフロー](#anchor-desktopflow)
      1. [デスクトップ フローの実行履歴URL](#anchor-desktopflow-url)
      1. [クラウド フローの実行履歴CSV](#anchor-cloudflow-run-history-csv)
      1. [クラウド フローのアクションの未加工入力と未加工出力](#anchor-cloudflow-run-history-csv)

<a id='anchor-intro'></a>
# 概要

Power Automate for desktop に関するサポートサービスのお問い合わせの際の、情報取得手順についてご案内致します。


<a id='anchor-how-to-collect'></a>

# 情報取得手順詳細

<a id='anchor-pad-version'></a>

## 1. Power Automate for desktopのバージョン
Power Automate for desktopのバージョンが確認できる画面キャプチャをご提供ください。
1. Power Automate for desktop > ヘルプ > バージョン情報 を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/pad-version.png)     
2. 以下の画面キャプチャを取得してご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/pad-version2.png) 

なお、Power Automate for desktop のバージョンは以下からもご確認いただけます。 
* Power Automate for desktopのexeファイル > プロパティ > 詳細  
![](./power-automate-desktop-helpful-information-for-sr/pad-version3.png) 
  
<a id='anchor-os-version'></a>

## 2. OSのバージョン
OS のバージョンが確認できる以下の画面キャプチャをご提供ください。
* システム > バージョン情報 を開き以下の画面キャプチャを取得します。
![](./power-automate-desktop-helpful-information-for-sr/os-version.png)  


<a id='anchor-computer-log'></a>

## 3. コンピューター ログ
コンピューター ログの取得方法は大きく分けて３つございます。
Power Automate コンピューター ランタイムをご利用いただいている場合は a) の方法から、ご利用いただけない場合は b) の方法からログを取得してご提供ください。  

また、RunDefinition.json ファイルと Actions.log ファイルを保持するレジストリを登録いただいている場合は、c) の方法で取得できるログも併せてご提供ください。

### a) Power Automate コンピュータ ランタイムからログを収集する
フロー実行後のログの出力に関して、過去３日間のログが Power Automate コンピューター ランタイムから出力いただけます。
Power Automate コンピュータ ランタイムからエクスポートしたzipファイルをご提供ください。
* Power Automate コンピュータ ランタイム > トラブルシューティング > ログのエクスポート を選択します。
![](./power-automate-desktop-helpful-information-for-sr/computer-log.png)

公開情報にも手順の記載がございますので、ご参照いただけますと幸いです。  
[デスクトップ フローのトラブルシューティング - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/troubleshoot#collect-machine-logs)


### b) 管理者権限のあるアカウントでフォルダからログを収集する
管理者権限のあるアカウントで以下のフォルダ内にあるファイルをzipファイルにまとめてご提供ください。  
* フォルダ：%ProgramData%\Microsoft\Power Automate\Logs

### c) レジストリを設定いただいている場合に収集できるログ
以下のレジストリを登録いただきマシンを再起動した上で、以下でご案内するフォルダに格納されているファイルを取得してご提供ください。

1. 必要となるレジストリを確認する  
以下の公開情報を参考に、必要となるレジストリが登録されていることをご確認ください。  
公開情報にも手順の記載がございますので、ご参照いただけますと幸いです。  
[Power Automate でのガバナンス - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/governance#configure-power-automate-for-desktop-to-keep-the-flow-run-details)
![](./power-automate-desktop-helpful-information-for-sr/computer-log2.png)
	
2．ファイルを取得する  
以下のフォルダに格納されているファイルを取得し、ご提供ください。  
* フォルダ：%localappdata%\Microsoft\Power Automate Desktop\Scripts\{scriptid}\Runs\{runid}  
a) Actions.log：デスクトップ フローに設定された各アクションの実行結果が記載されたログです。  
b) RunDefinition.json：デスクトップ フローの実行開始日、実行終了日、フローの実行結果(成功、失敗)が記載されたログです。  

{ScriptId} にはデスクトップ フローの ID を、{RunId} にはフローの実行 ID を挿入します。  
デスクトップフローのIDとフローの実行IDの確認方法は有償版か無償版かにより異なりますので、順にご案内いたします。 

<有償版の場合>  
Power Automateポータルのデスクトップ フローの実行履歴からデスクトップフローの ID とフローの実行 ID を確認いただけます。
![](./power-automate-desktop-helpful-information-for-sr/computer-log3.png)

<無償版の場合>  
デスクトップ フローの ID は以下の手順で取得します。
![](./power-automate-desktop-helpful-information-for-sr/computer-log4.png)
フローの実行 ID に関しましては、Windows のエクスプローラーの更新日時からどの実行かをご判断いただけますと幸いでございます。


<a id='anchor-fiddler-log'></a>

## 4. Fiddler ログ
以下の採取手順をご確認の上でログファイルをご提供ください。  
[Fiddler ログの採取手順 (microsoft.com)](https://social.technet.microsoft.com/Forums/ja-JP/fe5f977a-2992-44c3-b643-38ad570a3d18/fiddler-12525124641239825505214622516338918?forum=DCRMSupport)

<a id='anchor-machine-information'></a>

## 5. マシン登録の情報
該当のマシンの登録情報が確認できる画面キャプチャをご提供ください。

1. Power Automate ポータルの画面 > 監視 > マシン
![](./power-automate-desktop-helpful-information-for-sr/machine-information.png)
1. マシンまたはコンピューター グループから該当のコンピューターの詳細が分かる画面キャプチャをご提供ください。
![](./power-automate-desktop-helpful-information-for-sr/machine-information2.png)
  
<a id='anchor-remote-desktop-setting'></a>

## 6. リモートデスクトップ設定 
実行される端末のリモートデスクトップの設定の画面キャプチャをご提供ください。   
* 実行される端末 > システムのプロパティ > リモートデスクトップ   
![](./power-automate-desktop-helpful-information-for-sr/remote-desktop-setting.png)  

<a id='anchor-desktopflow'></a>

## 7. Power Automate for desktop のフロー
デスクトップ フローがマイ フローのフローか、ソリューションに含まれるフローかにより、フローの共有方法が異なります。
### 1. デスクトップフロー (マイフロー)
デスクトップ フローのデスクトップ フローの編集画面から、フロー内のアクション (Ctrl + C) をコピーし、テキスト エディター (Ctrl + V) に貼り付けてご提供ください。 
![](./power-automate-desktop-helpful-information-for-sr/desktopflow.png)  
一度にコピーできるフローは 1 つだけであるため、フロー内に複数のサブフローがある場合は、サブフローごとに上記の手順を繰り返し、アクションを個別のテキスト ファイルに保存した上でご提供ください。  

### 2. デスクトップフロー (ソリューション)
ソリューション内に作成したデスクトップフローの場合は、エクスポートしたソリューション ファイル (zip) をご提供ください。
* ソリューション > (ソリューションを選択) > エクスポート  
![](./power-automate-desktop-helpful-information-for-sr/desktopflow2.png)

ソリューション全体の提供が難しい場合は、必要なコンポーネントのみが含まれる新しいソリューションを作成し、ご提供ください。  
※弊社環境にインポートできるよう、依存関係のあるコンポーネントを含めてご提供をお願い致します  

ソリューションのエクスポートの詳細に関して、以下に記載がございますのでご参照いただけますと幸いです。    
[Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-flowpackage-in-solution)


<a id='anchor-desktopflow-url'></a>
 
## 8. デスクトップ フローの実行履歴URL  
1. マイ フロー>デスクトップ フローから該当のフローを選択します。実行履歴から該当の日時を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/desktopflow-url.png)  
1. URLをコピーしてご提供ください。
エラー時にフローが実行されず実行履歴に残っていない場合は、１の画面のURLをコピーしてご提供いただけますと幸いです。

<a id='anchor-cloudflow-run-history-csv'></a>

## 9. クラウドフローの実行履歴CSV
1. デスクトップ フローを呼び出したクラウドフローの詳細画面を開き、「28 日間の実行履歴」から「すべての実行」を選択します。
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-run-history-csv.png)
2. 「.csv ファイルを取得」を選択し、取得した CSV ファイルをご提供ください。
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-run-history-csv2.png)

クラウド フローの実行履歴 CSV の取得方法の詳細に関して、以下に記載がございますのでご参照いただけますと幸いです。  
[Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-flowrunhistory-csv)

<a id='anchor-cloudflow-raw-input-output'></a>

## 10. クラウドフローのアクションの未加工入力と未加工出力

実行履歴から「デスクトップ用 Power Automate で構築したフローを実行する」アクション等のデスクトップ フローを呼び出すアクションを展開します。  
未加工入力および未加工出力として表示されるテキストをコピーし、ご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-raw-input-output.png)  

クラウド フローのアクションの未加工入力と未加工出力の取得方法の詳細に関して、以下にも記載がございますのでご参照いただけますと幸いです。  
[Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-raw-input-output)


---

## 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって若干の UI の遷移など異なる場合があります。その場合は画面の指示に従って進めてください。



---
