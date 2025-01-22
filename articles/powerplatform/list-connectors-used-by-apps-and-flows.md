---
title: キャンバス アプリ・クラウド フローで使用しているコネクタの一覧取得
date: 2024-12-27 11:30:00
tags:
  - Power Platform
  - Power Automate
  - Power Apps
---

# キャンバス アプリ・クラウド フローで使用しているコネクタの一覧取得

こんにちは、Power Platform サポートチームの藤田、網野です。  

よくお問合せをいただく、Power Apps キャンバス アプリ・Power Automate クラウド フローで使用されているコネクタの一覧取得方法についてご案内いたします。

特定のコネクタが使用されているかどうか、どのフローやアプリでプレミアム コネクタが使用されているか、といったことを確認する際に是非お役立てください。
<!-- more -->

---
## PowerShell による出力
### 事前準備
[公開情報 - Power Apps と Power Automate の PowerShell サポート](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell#cmdlets) に従い、PowerApps の管理モジュールを PowerShell を実行する端末にインストールしてください。

### コマンド
アプリ・フローで使用されている接続を使用するコネクタにつきましては、Gist に登録されている PowerShell スクリプトをご実行いただくことで取得することが出来ます。

具体的なコマンドは [Gist - Get-PowerAppsFlowsConnections.ps1](https://gist.github.com/jameswh3/b1ddca1705a1e747ce3c8453e1f6dc7e) をご参照ください。

日本語が含まれるアプリやフローの情報を取得する場合は、PowerShell コマンドの最終行に "-Encofing UTF8" を追加し、文字コードの指定してください。
```
$powerPlatObjects | Export-Csv $outputFile -NoTypeInformation -Encoding UTF8
```

### 出力結果
コマンド内で指定したフォルダに以下のような形式で CSV ファイルとして出力されます。
![](./list-connectors-used-by-apps-and-flows/csv.png)

### 活用例
#### ◆ フローやアプリで使用しているコネクタを確認する
"ConnectorName" 列または"ConnectionId" 列から確認できます。
![](./list-connectors-used-by-apps-and-flows/csv-connector.png)

"ConnectoinId" 列から "/providers/Microsoft.PowerApps/scopes/admin/apis/shared_" を削除すると、コネクタIDが取得できます。  
取得したコネクタ ID を利用して、コネクタの公開情報へアクセスすることができます。  
"https://learn.microsoft.com/ja-jp/connectors/{コネクタID}/"  

例)
* Sharepoint : https://learn.microsoft.com/ja-jp/connectors/sharepointonline/  
* SQL Server : https://learn.microsoft.com/ja-jp/connectors/sql/  

#### ◆ フローやアプリでプレミアムコネクタが使われているか確認する
"Tier" 列が "Premium" となっているコネクタがプレミアムコネクタとなります。
![](./list-connectors-used-by-apps-and-flows/csv-premium.png)


### 留意点
本方法は接続に基づいて出力を行うため、HTTP コネクタなど接続が作られないコネクタは出力することができません。




