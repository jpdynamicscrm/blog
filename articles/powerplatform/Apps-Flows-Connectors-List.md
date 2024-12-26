---
title: キャンバス アプリ・クラウド フローで使用しているコネクタの一覧取得
date: 2024-12-27 11:30:00
tags:
  - Power Platform
  - Power Automate
  - Power Apps
---

# キャンバス アプリ・クラウド フローで使用しているコネクタの一覧取得

こんにちは、Power Platform サポートチームの藤田です。  

よくお問合せをいただく、Power Apps キャンバス アプリ・Power Automate クラウド フローで使用されているコネクタの一覧取得方法についてご案内いたします。

特定のコネクタが使用されているかどうか、どのフローやアプリでプレミアム コネクタが使用されているか、といったことを確認する際に是非お役立てください。
<!-- more -->

---
# PowerShell による出力

アプリ・フローで使用されているコネクタにつきましては、後述のPowerShell スクリプトをご実行いただくことで取得することが出来ます。

前提として、Power Appsの管理モジュールのインストールが必要となります。
既にご利用でない場合は、以下をご参照いただき導入をお願いいたします。

公開情報：[Power Apps と Power Automate の PowerShell サポート](https://learn.microsoft.com/ja-jp/power-platform/admin/powerapps-powershell#cmdlets)

以下はスクリプトの例です。<br>
出力されたcsvファイルの　ConnectionName　の列にコネクタ名が、Tierの列に　Premium・Standard　の種別が表示されます。

```
$outputFile=".\powerplatinventory.csv"

$environments = Get-AdminPowerAppEnvironment
$powerPlatObjects=@()
foreach ($e in $environments) {
    write-host "Environment: " $e.displayname
    $powerapps = Get-AdminPowerApp -EnvironmentName $e.EnvironmentName
    foreach ($pa in $powerapps) {
        write-host "  App Name: " $pa.DisplayName " - " $pa.AppName
        foreach ($conRef in $pa.Internal.properties.connectionReferences) {
            foreach ($con in $conRef) {
                foreach ($conId in ($con | Get-Member -MemberType NoteProperty).Name) {
                    $conDetails = $($con.$conId)
                    $apiTier = $conDetails.apiTier
                    if ($conDetails.isCustomApiConnection) {$apiTier = "Premium (CustomAPI)"}
                    if ($conDetails.isOnPremiseConnection ) {$apiTier = "Premium (OnPrem)"}
                    Write-Host "    " $conDetails.displayName " (" $apiTier ")"
                    $paObj=@{
                        type="Power App"
                        ConnectionName=$conDetails.displayName
                        ConnectionId=$conDetails.id
                        Tier=$apiTier
                        Environment=$e.displayname
                        AppFlowName=$pa.DisplayName
                        createdDate=$pa.CreatedTime
                        createdBy=$pa.Owner
                    }
                    $powerPlatObjects+=$(new-object psobject -Property $paObj)
                } #foreach $conId
            } #foreach $con
        } #foreach $conRef
    } #foreach power app
    $flows=Get-AdminFlow -EnvironmentName $e.EnvironmentName
    foreach ($f in $flows) {
        Write-Host "  Flow Name: " $f.DisplayName " - " $f.FlowName
        $fl=get-adminflow -FlowName $f.FlowName
        foreach ($conRef in $fl.Internal.properties.connectionReferences) {
            foreach ($con in $conRef) {
                foreach ($conId in ($con | Get-Member -MemberType NoteProperty).Name) {
                    $conDetails = $($con.$conId)
                    $apiTier=$conDetails.apiDefinition.properties.tier
                    if ($conDetails.apiDefinition.properties.isCustomApi) {$apiTier = "Premium (CustomAPI)"}
                    Write-Host "    " $conDetails.displayName " (" $apiTier ")"
                    $paObj=@{
                        type="Power Automate"
                        ConnectionName=$conDetails.displayName
                        ConnectionId=$conDetails.id
                        Tier=$apiTier
                        Environment=$e.DisplayName
                        AppFlowName=$f.DisplayName
                        createdDate=$f.CreatedTime
                        createdBy=$f.CreatedBy
                    }
                    $powerPlatObjects+=$(new-object psobject -Property $paObj)
                } #foreach $conId
            } #foreach $con
        } #foreach $conRef
    } #foreach flow
} #foreach environment
$powerPlatObjects | Export-Csv $outputFile -NoTypeInformation -Encoding UTF8
```
※ [GitHub](https://gist.github.com/jameswh3/b1ddca1705a1e747ce3c8453e1f6dc7e) より一部引用し日本語に対応するよう追記しています。

尚、上記の方法では、接続に基づいて出力を行うため、接続が作られないプレミアム コネクタであるHTTPコネクタは出力出来ない結果となります。

# Microsoft Power Platform セルフサービス分析

プレビュー機能とはなっておりますが、テナントの Power Appsキャンバス アプリ・Power Automateクラウド フローの在庫情報をData Lakeへエクスポートすることが出来る機能もございます。

公開情報：[Microsoft Power Platform セルフサービス分析を設定して、インベントリと使用状況のデータをエクスポートする (プレビュー)](https://learn.microsoft.com/ja-jp/power-platform/admin/self-service-analytics)

上記機能でData Lakeへ出力された情報のうち、<br>
Power Apps キャンバス アプリについてはpowerplatform  > powerapps > Connections および ConnectionReference<br>
Power Automate クラウド フローについては powerplatform > powerautomate > FlowConnectionReference <br>
に、各アプリ・フローで使用している接続・接続参照の情報が格納されます。

公開情報：[Microsoft Power Platform セルフサービス型の分析スキーマの定義 (プレビュー)](https://learn.microsoft.com/ja-jp/power-platform/admin/self-service-analytics-schema-definition)

フロー毎・アプリごとのファイルとなっており、そのままでは確認しやすい一覧の形となっておらず恐縮ではございますが、ご要件に合わせて格納された情報を用いていただくことが可能です。

