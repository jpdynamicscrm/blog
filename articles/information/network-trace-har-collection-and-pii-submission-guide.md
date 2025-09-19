---
title: ネットワークトレースの取り方と機密情報を含む場合の対処
date: 2025-09-19 09:00:00 +0900
tags:
  - HAR
  - サポート
  - ネットワークトレース
  - 機密情報
categories:
  - [information]
---

# ネットワークトレースの取り方と機密情報を含む場合の対処

こんにちは、PowerPlatformサポートチームの三田です。

本記事ではネットワークトレース取得と機密情報を含む場合の共有配慮について、以下の Q1〜Q2 の質問に回答する形でご説明します。

- [Q1: ネットワークトレースって？](#q1)
- [Q2: 機密情報が含まれてしまうけどサポートエンジニアに共有して大丈夫？](#q2)

### この記事でわかること
- ネットワークトレースの概要と役割
- 主要ブラウザーでの具体的な取得手順
- 記録前の準備と再現時のポイント
- 安全に最小化し共有するための手順
- サニタイズ（sanitized）と手動マスキングの使い分け
- サポート エンジニアの機密情報取り扱いについて

## 目次
- [ネットワークトレースの説明と基本的な取得手順 (Q1)](#q1)
- [機密情報を含む可能性と安全な共有手順 (Q2)](#q2)

<h2 id="q1">ネットワークトレースの説明と基本的な取得手順 (Q1)</h2>

【結論】ネットワークトレースとはブラウザーがページを表示するために送受信した通信を起きた順番で並べた記録です。
問題が出た操作を 1 回だけ記録し HAR ファイルとして共有すると原因調査が速く正確になります。
（HAR はその通信記録を 1 つにまとめたファイル形式です。）
古いログを消し発生直前に開始し発生直後に停止することで関係ない通信や機密情報を減らせます。

【補足】再現が複数画面遷移を含む場合は Preserve log（ログ保持）を有効にすると途中で消えにくくなります。


【手順】ネットワーク トレース (HAR) の採取手順 (Edge 例)

1. ブラウザーで F12 キーを押し開発者ツールを開きます。
2. 以下の画像を参考に、① Network(ネットワーク) タブ → ② 必要なら Preserve log をオン → ③ Clear(クリア) で古いログを消します。

![Edge 開発者ツールで Network タブを選び Preserve log をチェックし Clear を押す例](./network-trace-har-collection-and-pii-submission-guide/Networktracing_1.jpg)

3. 同じタブ内で問題の操作を最小の手順で 1 回だけ再現します。
4. 以下の画像を参考に、① 記録停止 (Stop recording network log) を押し ② Export HAR (sanitized があればそちらを優先) をクリックして保存します。
  アイコンが隠れている場合は開発者ツール領域の幅を広げてください。

![記録停止後に Export HAR アイコンを押して HAR を保存する例](./network-trace-har-collection-and-pii-submission-guide/Networktracing_2.jpg)

5. 必要なら Console タブでログを保存し HAR と一緒に ZIP 圧縮します。
6. ファイル名は `YYYYMMDD_issue.har` のように日付と短い説明を入れると区別しやすくなります。

【補足】操作手順（GUI）は Microsoft Edge の例ですが Chrome でもほぼ同じです。
細部が異なる場合は下記の参考資料を参照してください。



【ポイント】
1. 問題直前に開始し終わったらすぐ停止する。
2. 余計なタブや拡張機能操作を避け最小の再現だけ行う。
3. 記録中に不要なページ遷移やリロードをしない。
4. 停止後 Export HAR (sanitized) があればそれを使う。
5. Console ログが必要なら一緒に保存する。
6. HAR を ZIP 圧縮し分かる名前 (日付+概要) を付けて添付する。


【よくある誤解】
- 誤解: 画面を更新し続けるほど詳しい情報が増えて解析が良くなる。
  → 正しくは: 不要な通信が増え肝心な要求が埋もれるため最小の再現だけに留めるべきです。
- 誤解: HAR を取得する前に問題を何度も再現しておいた方が良い。
  → 正しくは: 記録開始後の 1 回の再現のみで十分です。

＜参考資料＞
- [ブラウザー トレースの取得 (Azure Portal) ](https://learn.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)
- [Power BI サービスの追加診断情報取得 (Network ログ出力) ](https://learn.microsoft.com/ja-jp/power-bi/support/service-admin-capturing-additional-diagnostic-information-for-power-bi)
- [Web PubSub ネットワーク トレース収集 ](https://learn.microsoft.com/ja-jp/azure/azure-web-pubsub/howto-troubleshoot-network-trace#collect-a-network-trace)

<h2 id="q2">機密情報を含む可能性と安全な共有手順 (Q2)</h2>

【結論】サポート エンジニアは NDA（秘密保持契約）の下でお客様の情報を守る体制になっています。
HAR には認証トークンや氏名やメールアドレスなどの機密情報が含まれることがあるため可能な範囲で sanitized 出力（ブラウザーが自動で一部の機密情報を除外した HAR 出力。完全ではない簡易版）や短時間記録で余計な情報を減らし負担がなければ簡易マスキングを行ってください。
マスキング作業が負担で難しい場合は無加工のままでも NDA に基づき適切に扱われます。

【補足】Export HAR (sanitized) は一部の機密情報を自動除外しますが完全ではありません。
送る前に自分で中身を開き最小化されているか確認してください。
マスキングを省略した場合でもサポート側で厳重に扱います。
マスキングが再現解析に影響しそうな場合は「トークン末尾 3 文字のみ伏字」「ユーザー名を仮名へ置換」など概要を添えてください。

【ポイント】
1. 再現直前に開始し再現直後に停止して不要通信を避ける。
2. sanitized 出力があればまずそれを選ぶ (なければ通常の HAR で問題ありません)。
3. 負担にならなければ HAR をテキストエディターで開き以下の語を検索する: "Authorization" "Bearer " "token" "Set-Cookie" "api-key" "pwd" メールアドレス形式 `@` GUID らしき 32/36 文字。
4. マスキングする場合は値の一部を残し末尾を `***` などで伏字 (例: `abc123XYZtoken` → `abc123***`)。
5. 値を区別したい場合は先頭 6〜8 文字だけ残すなどルールを 1 つに統一。
6. 添付時にマスキングした/しなかった理由を一言添える (例: 「時間の都合で未マスキング」「トークン末尾のみ伏字」)。

【よくある誤解】
- 誤解: HAR は自動で安全化されるので中身を開かず送ってよい。
  → 正しくは: 自動除去は限定的なので送付前セルフチェックが必要です。
- 誤解: 機密情報やトークンを全部削除しないと共有してはならない。
  → 正しくは: NDA 下で扱われるため最小化が難しい場合は無加工でも構いません。

＜参考資料＞
- [ブラウザー トレースの取得 (Azure Portal) ](https://learn.microsoft.com/ja-jp/azure/azure-portal/capture-browser-trace)
- [Azure DevOps ブラウザートレース取得 (敏感情報注意) ](https://learn.microsoft.com/ja-jp/azure/devops/user-guide/capture-browser-trace?view=azure-devops)
- [Support environments and consent to access customer data (Microsoft Learn, 英語)](https://learn.microsoft.com/en-us/power-platform/admin/support-environment)

<h2 id="summary">まとめ</h2>

- ネットワークトレースはブラウザー通信の記録で問題解析を助けます。
- 取得手順は「開発者ツール → Network → 記録 → 再現 → 停止 → HAR 保存」です。
- 不要操作を避け最小操作でノイズを減らします。
- sanitized 出力と短時間記録と手動マスキング (可能なら) で機密情報を減らします。
- マスキングが難しくても NDA の下で安全に扱われ必要に応じて同意ベースアクセス統制も機能します。

<h2 id="notice">注意事項（情報の更新可能性）</h2>
本記事の内容は執筆日時点の公開情報に基づいています。
仕様や UI、制限事項は将来変わる可能性があります。
最新情報は公式ドキュメントをご確認ください。

