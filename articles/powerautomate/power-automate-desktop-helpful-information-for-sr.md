---
title: Power Automate for desktop お問い合わせの際の情報取得手順
date: 2025-10-29 12:00
tags:
  - Power Automate
  - Power Automate for desktop
  - 情報採取
  - お問い合わせ全般
categories:
  - [Power Automate, Desktop flow]
---

# はじめに

こんにちは。Power Platform サポートの大友、林、竹内です。  
本記事では Power Automate for desktop 関連のお問い合わせの際の、情報取得手順についてご案内いたします。

<!-- more -->
# 目次

1. [概要](#anchor-intro)
1. [情報取得手順詳細](#anchor-how-to-collect)
   1. [Power Automate for desktop のバージョン](#anchor-pad-version)
   1. [OS のバージョン](#anchor-os-version)
   1. [コンピューター ログ](#anchor-computer-log)
   1. [コンピューター ログ (詳細)](#anchor-verbose-log)
   1. [トラブルシューティングの診断実行結果](#anchor-analysis-result)
   1. [Windows イベントログ](#anchor-event-log)
   1. [ネットワークトレース](#anchor-networktrace)
      1. [HTTP ビューアー / 開発者ツール](#anchor-httpviewer)
      1. [Fiddler ログ](#anchor-fiddler-log)
   1. [デスクトップフロー実行詳細ログおよびアクション詳細ログ](#anchor-action-log)
   1. [マシン登録の情報](#anchor-machine-information)
   1. [リモートデスクトップ設定](#anchor-remote-desktop-setting)
   1. [Power Automate for desktop のレジストリ設定](#anchor-registry-setting)
   1. [Power Automate for desktop のフロー](#anchor-desktopflow)
   1. [デスクトップ フローの実行履歴 URL](#anchor-desktopflow-url)
   1. [デスクトップ フローの実行履歴の画面キャプチャ](#anchor-desktopflow-runhistory-screenshot)
   1. [デスクトップ フローの実行履歴のエラー・入力・出力の詳細](#anchor-desktopflow-runhistory-error-input-output)
   1. [クラウド フローの実行履歴 CSV (クラウドフローから呼び出している場合)](#anchor-cloudflow-run-history-csv)
   1. [クラウド フローのアクションの未加工入力・出力 (クラウドフローから呼び出している場合)](#anchor-cloudflow-raw-input-output)
   1. [インストーラー ログ](#anchor-installer-log)
   1. [プロキシ設定](#anchor-proxy-settings)

<a id='anchor-intro'></a>

# 概要
Power Automate for desktop に関するサポートサービスのお問い合わせの際の、情報取得手順についてご案内致します。

<a id='anchor-how-to-collect'></a>

# 情報取得手順詳細

<a id='anchor-pad-version'></a>

## 1. Power Automate for desktop のバージョン
Power Automate for desktop のバージョンが確認できる画面キャプチャをご提供ください。
1. Power Automate for desktop > ヘルプ > バージョン情報 を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/pad-version.png)
1. コピーの詳細を選択し、テキストエディターに貼り付け`Ctrl+V`したファイルをご提供ください。   
![](./power-automate-desktop-helpful-information-for-sr/pad-version2.png)
  

<a id='anchor-os-version'></a>

## 2. OS のバージョン
OS のバージョンが確認できる以下の画面キャプチャを取得しご提供ください。
* システム > バージョン情報  
![](./power-automate-desktop-helpful-information-for-sr/os-version1.png)  


<a id='anchor-computer-log'></a>

## 3. コンピューター ログ
フロー実行後のログの出力に関して、過去 3 日間のログが コンピューターログとして出力されます。  

1. Power Automate コンピュータ ランタイム がインストールされている端末  
Power Automate コンピュータ ランタイムからエクスポートした zip ファイルをご提供ください。  
Power Automate コンピュータ ランタイム > トラブルシューティング > ログのエクスポート を選択します。    
![](./power-automate-desktop-helpful-information-for-sr/computer-log.png)  
> [!NOTE]
> 公開情報にも手順の記載がございますので、ご参照いただけますと幸いです。  
> [デスクトップ フローのトラブルシューティング - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/troubleshoot#collect-machine-logs)  

2. Power Automate コンピュータ ランタイム がインストールされていない端末  
Power Automate コンピューター ランタイムをご利用いただいていない場合は管理者権限のあるアカウントで以下のフォルダ内にあるファイルを zip ファイルにまとめてご提供ください。  
インストーラー版 : `%ProgramData%\Microsoft\Power Automate\Logs`  
ストア版 : `%LocalAppData%\Microsoft\Power Automate Desktop\DataCollection`  


<a id='anchor-verbose-log'></a>

## 4. コンピューター ログ (詳細)
1. Power Automate for desktop > ヘルプ > トラブルシューティング を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/analysis-result1.png)
1. "詳細なログ" をオンにします。  
![](./power-automate-desktop-helpful-information-for-sr/verbose-log1.png)
1. 発生している事象を再現させてください。  
1. "詳細なログ" をオフにし、"ログのエクスポート" からエクスポートされた zip ファイルをご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/verbose-log2.png)


<a id='anchor-analysis-result'></a>

## 5. トラブルシューティングの診断実行結果
1. Power Automate for desktop > ヘルプ > トラブルシューティング を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/analysis-result1.png)
1. 事象に関連する診断の"実行"を選択してください。   
![](./power-automate-desktop-helpful-information-for-sr/analysis-result2.png)
1. 表示された画面のスクリーンショットおよび画面右下部から生成されるレポートCSVファイルをご提供ください。   
![](./power-automate-desktop-helpful-information-for-sr/analysis-result3.png)


<a id='anchor-event-log'></a>

## 6. Windows イベントログ
1. Windows にインストールされている "イベントビューアー" を実行してください。  
![](./power-automate-desktop-helpful-information-for-sr/event-log1.png)

1. アプリケーションログの採取  
   Windows ログ > Application を選択します。  
   画面右部 "現在のログをフィルター…" から事象発生日時が含まれるようフィルター処理を設定してください。  
   画面右部 "フィルターされたログ ファイルの名前を付けて保存…" から保存された evtx ファイルをご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/event-log2.png)

1. システムログの採取  
   Windows ログ > システム を選択します。  
   画面右部 "現在のログをフィルター…" から事象発生日時が含まれるようフィルター処理を設定してください。  
   画面右部 "フィルターされたログ ファイルの名前を付けて保存…" から保存された evtx ファイルをご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/event-log3.png)


なお、イベントログについては、下記の公開情報にも記載がございます。  
[ログのイベント ビューアー](https://learn.microsoft.com/ja-jp/troubleshoot/power-platform/power-automate/desktop-flows/how-to-get-power-automate-desktop-installer-logs#event-viewer-logs)  


<a id='anchor-networktrace'></a>

## 7. ネットワークトレース  

<a id='anchor-httpviewer'></a>

### 7.1 HTTP ビューアー / 開発者ツール  
> [!NOTE]
> HTTP ビューアー / 開発者ツールは Power Automate for desktop のバージョン 2.70 以降で利用できます。

1. 環境変数への追加  
   実行される端末 > システムのプロパティ > 詳細設定 > 環境変数 を選択し、ユーザー環境変数に以下を追加します。  
   - 変数名 : PAD_DEV_TOOLS_ENABLED  / 変数値 : true  
   - 変数名 : PAD_HTTP_VIEWER_ENABLED  / 変数値 : true  
   ![](./power-automate-desktop-helpful-information-for-sr/set-env.png)  
2. Power Automate for desktop を終了させます。（タスクトレイ に残っている場合は、タスクトレイ からも終了させます。）  

3. Power Automate for desktop を起動させます。  

4. ヘルプのメニューに追加される HTTP ビューアー または 開発者 ツール を起動します。  
   ※追加されるメニューの表示名は端末やインストールバージョンで変わりますが、利用方法に大きな違いはございません。  
   (以下、HTTP ビューアーの例)    
    ![](./power-automate-desktop-helpful-information-for-sr/httpviewer-menu.png)   
> [!NOTE]
> Power Automate for desktop が起動/サインインできない場合は "C:\Program Files (x86)\Power Automate Desktop\dotnet\PAD.HttpViewer.exe" を直接実行してください。 (インストーラー版のみ実行可能)  

5. 事象再現直前まで操作し クリア を押下します。  
    ![](./power-automate-desktop-helpful-information-for-sr/httpviewer-clear.png)  

6. 事象を再現させます。  

7. ログを出力します。  
   - HTTP ビューアー の場合は ファイル > ログのエクスポート  
       ![](./power-automate-desktop-helpful-information-for-sr/httpviewer-store.png)  
   - 開発者ツールの場合は フロッピー ボタン  
       ![](./power-automate-desktop-helpful-information-for-sr/devtool.png)  

8. 1で設定した環境変数を削除します。    

<a id='anchor-fiddler-log'></a>

### 7.2 Fiddler ログ
以下の採取手順をご確認の上でログファイルをご提供ください。  
[Fiddler ログの採取手順 (microsoft.com)](https://learn.microsoft.com/ja-jp/power-query/web-connection-fiddler)

なお、情報取得の際はキャプチャ対象を **Non-Browser** に変更いただくようお願いいたします。  
![](./power-automate-desktop-helpful-information-for-sr/fiddler-nonbrowser.png)


<a id='anchor-action-log'></a>

## 8. デスクトップフロー実行詳細ログおよびアクション詳細ログ

以下のレジストリを登録いただきマシンを再起動した上で、以下でご案内するフォルダに格納されているファイルを取得してご提供ください。
   1. 以下の公開情報を参考に、必要となるレジストリを登録いただきマシンを再起動します。  
![](./power-automate-desktop-helpful-information-for-sr/computer-log2.png)
> [!NOTE]
> 公開情報にも手順の記載がございますので、ご参照いただけますと幸いです。  
> [Power Automate でのガバナンス - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/desktop-flows/governance#configure-power-automate-for-desktop-to-keep-the-flow-run-details)

> [!WARNING]
> こちらのレジストリを有効にした状態のままご運用頂くと端末のストレージを逼迫させる場合があります。
> 情報ご取得後はレジストリの無効化をお勧め致します。

   2. 事象を再現致します。
   3. ファイルをご取得ください。
以下のフォルダに格納されているファイルを取得し、ご提供ください。  
`%localappdata%\Microsoft\Power Automate Desktop\Scripts\{scriptid}\Runs\{runid}`  
`{ScriptId}` にはデスクトップ フローの ID を、`{RunId}` にはフローの実行 ID を挿入します。
      * RunDefinition.json：デスクトップ フローの実行詳細が記録されたログです。
      * Actions.log：デスクトップ フローに設定された各アクションの実行詳細が記録されたログです。

   > [!NOTE]
   > デスクトップフローの ID とフローの実行 ID はPower Automateポータルのデスクトップ フローの実行履歴のURLより確認いただけます。  
   > ![](./power-automate-desktop-helpful-information-for-sr/computer-log3.png)


<a id='anchor-machine-information'></a>

## 9. マシン登録の情報
Power Automate ポータルの画面 > 監視 > マシン  
該当のコンピューターの詳細画面の画面キャプチャをご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/machine-information.png)
  
<a id='anchor-remote-desktop-setting'></a>

## 10. リモート デスクトップ設定
実行される端末の以下リモート デスクトップの設定の画面キャプチャをご提供ください。
* 実行される端末 > システムのプロパティ > リモートデスクトップ  
![](./power-automate-desktop-helpful-information-for-sr/remote-desktop-setting2.png)
* 実行される端末 > システムのプロパティ > リモートデスクトップ > ユーザーの選択  
![](./power-automate-desktop-helpful-information-for-sr/remote-desktop-setting3.png)

<a id='anchor-registry-setting'></a>

## 11. Power Automate for desktop のレジストリ設定
* レジストリ内容
以下のコマンドを実行し、出力結果をご提供ください。
   ```
   reg query "HKLM\Software\Microsoft\Power Automate Desktop" /s
   reg query "HKLM\Software\Wow6432Node\Microsoft\Power Automate Desktop" /s
   ```

<a id='anchor-desktopflow'></a>

## 12. Power Automate for desktop のフロー  
デスクトップフロー本体をソリューションに追加し、エクスポート頂いたファイルをご提供下さい。
弊社環境にインポート出来るよう、依存関係のあるコンポーネント含めご提供をお願い致します。

> [!NOTE]
> ソリューションのエクスポート手順は、以下記事に手順がございますのでご参照いただけますと幸いです。      
> [Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-flowpackage-in-solution)  

ソリューションでのご提供が難しい場合、テキストファイルでのご提供をお願いいたします。

デスクトップ フローのデスクトップフローの編集画面から、エディタ領域をクリックしたのちアクションを全選択`Ctrl+A`し、コピー`Ctrl+C`し、テキストエディターに貼り付け`Ctrl+V`したファイルをご提供ください。 
![](./power-automate-desktop-helpful-information-for-sr/desktopflow.png)  
一度にコピーできるフローは 1 つだけであるため、フロー内に複数のサブフローがある場合は、サブフローごとに上記の手順を繰り返し、アクションを個別のテキスト ファイルに保存した上でご提供ください。  

> [!WARNING]
> テキストでのご提供手順では入出力変数の定義やフロー内のアクションで使用していない UI 要素など一部の情報が含まれません。  
> エラーが発生しているアクションで入出力変数を使用している場合や、デスクトップ フロー全体の容量等を確認する必要がある場合は、弊社のサポートエンジニアからソリューションでの提供のご検討をお願いする場合がございます。

<a id='anchor-desktopflow-url'></a>
 
## 13. デスクトップ フローの実行履歴 URL  
1. マイ フロー>デスクトップ フローから該当のフローを選択します。実行履歴から該当の日時を選択します。    
![](./power-automate-desktop-helpful-information-for-sr/desktopflow-url.png)  
1. URLをコピーしてご提供ください。
エラー時にフローが実行されず実行履歴に残っていない場合は、1 の画面のURLをコピーしてご提供下さい。

<a id='anchor-desktopflow-runhistory-screenshot'></a>

## 14. デスクトップ フローの実行履歴の画面キャプチャ
1. 「13. デスクトップ フローの実行履歴 URL」と同じ手順で実行履歴の画面を表示します。
    1. マイ フロー > デスクトップ フローから該当のフローを選択します。
    1. 実行履歴から該当の日時を選択します。
1. ページ全体の画面キャプチャをご提供ください。     
![](./power-automate-desktop-helpful-information-for-sr/desktopflow-runhistory-screenshot.png)

<a id='anchor-desktopflow-runhistory-error-input-output'></a>

## 15. デスクトップ フローの実行履歴のエラー・入力・出力の詳細
1. 「13. デスクトップ フローの実行履歴 URL」と同じ手順で実行履歴の画面を表示します。
    1. マイ フロー > デスクトップ フローから該当のフローを選択します。
    1. 実行履歴から該当の日時を選択します。
1. 「アクションの詳細」パネルで、該当のアクションの「開始」列のリンクを押下します。
![](./power-automate-desktop-helpful-information-for-sr/desktopflow-runhistory-error-input-output.png)

【エラーの詳細】
1. 「エラーの詳細」のテキストをコピーし、テキスト ファイルへ保存します。
1. 「エラーのコンテキスト」の画像を右クリック > 名前を付けて画像を保存を押下します。
1. 保存したテキスト ファイル、画像ファイルをご提供ください。

【入力の詳細】
1. 「入力の詳細を確認します」のリンクを押下します。
1. 表示された入力の内容をコピーし、テキスト ファイルへ保存します。
1. 保存したテキスト ファイルをご提供ください。

【出力の詳細】
1. 「出力の詳細を確認します」のリンクを押下します。
1. 表示された出力の内容をコピーし、テキスト ファイルへ保存します。
1. 保存したテキスト ファイルをご提供ください。


<a id='anchor-cloudflow-run-history-csv'></a>

## 16. クラウド フローの実行履歴 CSV (クラウドフローから呼び出している場合)
デスクトップフローをクラウドフローから呼び出している場合は、クラウドフローの実行履歴もご提供ください。
1. デスクトップ フローを呼び出したクラウドフローの詳細画面を開き、「28 日間の実行履歴」から「すべての実行」を選択します。  
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-run-history-csv.png)
1. 「.csv ファイルを取得」を選択し、取得した CSV ファイルをご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-run-history-csv2.png)

> [!NOTE]
> クラウド フローの実行履歴 CSV の取得方法は、以下記事にも手順が掲載されております。  
> [Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-flowrunhistory-csv)

<a id='anchor-cloudflow-raw-input-output'></a>

## 17. クラウド フローのアクションの未加工入力・出力 (クラウドフローから呼び出している場合)
デスクトップフローをクラウドフローから呼び出している場合は、クラウドフローのアクションの未加工入力・未加工出力もご提供ください。
実行履歴から「デスクトップ用 Power Automate で構築したフローを実行する」アクション等のデスクトップ フローを呼び出すアクションを展開します。  
未加工入力および未加工出力として表示されるテキストをコピーし、ご提供ください。  
![](./power-automate-desktop-helpful-information-for-sr/cloudflow-raw-input-output.png)  

> [!NOTE]
> クラウド フローのアクションの未加工入力・出力の取得方法は、以下記事にも手順が掲載されております。  
> [Power Automate お問い合わせの際の情報取得手順 | Japan Dynamics CRM & Power Platform Support Blog (jpdynamicscrm.github.io)](https://jpdynamicscrm.github.io/blog/powerautomate/helpful-information-for-powerautomate-sr/#anchor-raw-input-output)

<a id='anchor-installer-log'></a>

## 18. インストーラー ログ
以下の採取手順をご確認の上でログファイルをご提供ください。  
[インストーラー ログ](https://learn.microsoft.com/ja-jp/troubleshoot/power-platform/power-automate/desktop-flows/how-to-get-power-automate-desktop-installer-logs#installer-logs)

<a id='anchor-proxy-settings'></a>

## 19. プロキシ設定
プロキシ設定の確認のため、以下の情報をご提供ください。
1. Windows のプロキシ設定
   以下のコマンドを実行し、出力結果をご提供ください。
   ```
   reg query "HKCU\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   ```
1. Power Automate for desktop のプロキシ設定 (レジストリ)  
  [Power Automate for desktop のレジストリ設定](#anchor-registry-setting) を参照し、出力結果をご提供ください。

1. Power Automate for desktop のプロキシ設定 (設定ファイル)  
   `C:\Program Files (x86)\Power Automate Desktop` 配下の以下のファイルをご提供ください。
   1. PAD.Proxy.config
   1. UIFlowService.Proxy.config
   1. Microsoft.Flow.RPA.LogShipper.Proxy.config
   1. Microsoft.Flow.RPA.UpdateService.Proxy.config

---

## 補足
本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって若干の UI の遷移など異なる場合があります。その場合は画面の指示に従って進めてください。

---
