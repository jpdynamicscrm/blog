---
title: Copilot Studio お問い合わせの際の情報取得手順
date: 2024-10-10 15:00:00
tags:
  - Copilot Stuio
  - 情報採取
  - Power Virtual Agents
---

こんにちは、Power Platform サポートチームの竹内です。  
本記事では Copilot Studio (旧称 Power Virtual Agents) 関連のお問い合わせの際に、調査のために必要となる情報について、その取得手順をご案内致します。


<!-- more -->
# 目次

1. [概要](#anchor-intro)
1. [情報取得手順詳細](#anchor-how-to-collect)
      1. [事象の発生状況](#anchor-about-situation)
      2. [事象発生時のエラーメッセージや画面キャプチャ・動画](#anchor-about-screencapture)
      3. [環境 ID](#anchor-flowrunhistory-csv)
      4. [Bot ID](#anchor-botid)
      5. [Conversation ID](#anchor-conversationid)
      6. [事象再現時のスナップショット zip ファイル](#anchor-snapshot-zip)
      7. [ナレッジの有効化状態](#anchor-knowledge-enable-state)
      8. [会話入出力](#anchor-raw-input-output)
      9.  [会話トランスクリプションテーブルデータ](#anchor-conversation-transcript)
      10. [Web ブラウザのネットワーク トレース・コンソール ログ](#anchor-about-networkha)

<a id='anchor-intro'></a>

# 概要

弊社サポートではお問合せを頂いた際のトラブルシューティングにおいてお問い合わせの内容をもとに調査方針を立てております。  
発生している事象の把握のため、直面されている事象の切り分けや情報提供をお願いすることがあります。  
Copilot Studio (旧称 Power Virtual Agents) に関するサポートサービスへお問合せの際の情報取得手順について以下のとおりご案内いたします。


<a id='anchor-how-to-collect'></a>

# 情報取得手順詳細

<a id='anchor-about-situation'></a>

## 1. 事象の発生状況
  エラーや意図しない状況がどのような状況下で発生するかお知らせください。  
  事象の発生条件を特定することで、問題の特定だけではなく弊社環境での再現調査においても有用な情報が得られます。  
  以下の情報をお知らせいただくことでより明確に事象を把握することができます。  

<a id='anchor-about-screencapture'></a>

## 2. 事象発生時のエラーメッセージや画面キャプチャ・動画
  エラーの内容を具体的に表すメッセージや画面キャプチャなどの情報をお寄せください。  
  事象再現時の動画がありますと事象の発生状況をより正確に把握することができます。  

### エラーメッセージや画面キャプチャ
  エラーの内容が分かるよう画面キャプチャをご取得ください。  
  エラーメッセージ内にタイムスタンプやエラーコードが記載されている場合はそれらの情報を **テキスト形式** でご取得ください。  

### 動画
  以下のいずれかの方法で事象発生時の動画をご取得ください。  

  > [!IMPORTANT]
  > 事象発生の事前に取得開始し、事象発生後に採取を行う必要がございます。
  - Windows ゲーム バーでの画面収録  
    [ゲーム バーを使用して PC にゲーム クリップを記録する](https://support.microsoft.com/ja-jp/windows/%E3%82%B2%E3%83%BC%E3%83%A0-%E3%83%90%E3%83%BC%E3%82%92%E4%BD%BF%E7%94%A8%E3%81%97%E3%81%A6-pc-%E3%81%AB%E3%82%B2%E3%83%BC%E3%83%A0-%E3%82%AF%E3%83%AA%E3%83%83%E3%83%97%E3%82%92%E8%A8%98%E9%8C%B2%E3%81%99%E3%82%8B-2f477001-54d4-1276-9144-b0416a307f3c)
  - Power Point での画面収録  

<a id='anchor-environmentid'></a>

## 3. 環境ID
  以下の手順で環境 ID を取得し、ご提供ください。
  必要に応じて URL をそのままご提供ください。

  - Copilot Studio ポータルを開きます。開いている環境が対象の環境であることを確認してください  
  - URL 内の environments/ の直後にある ID が環境 ID です。
  - URL をテキストファイル、あるいは文字列としてメール等に張り付けてご提供ください。  
  ![](./helpful-information-for-copilotstudio-sr/image_EnvironmentID.png)  

<a id='anchor-botid'></a>

## 4. Bot ID
  以下の手順で環境 ID を取得し、ご提供ください。
  必要に応じて URL をそのままご提供ください。

  - Copilot Studio ポータル>（対象の環境選択）＞カスタムコパイロット＞対象のコパイロットを開きます。
  - URL 内の bots/ の直後にある ID が Bot ID です。
  - URL をテキストファイル、あるいは文字列としてメール等に張り付けてご提供ください。
  ![](./helpful-information-for-copilotstudio-sr/image_BotID.png)  

<a id='anchor-conversationid'></a>

## 5. Conversation ID
  以下の手順で Conversation ID を取得し、ご提供ください。

  - Copilot Studio ポータル>（対象の環境選択）＞カスタムコパイロット＞対象のコパイロットを開きます。  
  - 対象のコパイロットとのチャットウィンドウに '/debug conversationid' とご入力ください。  
  - 得られた応答をテキストファイルとしてご提供ください。また、下画像と同様のスクリーンショットをご提供ください。  
  ![](./helpful-information-for-copilotstudio-sr/image_ConversationID.png)  

<a id='anchor-snapshot-zip'></a>

## 6. 事象再現時のスナップショット zip ファイル
  以下の手順でスナップショット zip ファイルを取得し、ご提供ください。

  - Copilot Studio ポータル>（対象の環境選択）＞カスタムコパイロット＞対象のコパイロットを開きます。  
  - 対象のコパイロットとのチャットウィンドウ右上の三点リーダーより、「スナップショットの保存」を選択します。  
  - 得られた zip ファイルをご提供ください。  
  ![](./helpful-information-for-copilotstudio-sr/image_snapshot.png)  

<a id='anchor-knowledge-enable-state'></a>

## 7. ナレッジの有効化状態
  以下の手順でスクリーンショット画像ファイルを取得し、ご提供ください。

  - Copilot Studio ポータル>（対象の環境選択）＞カスタムコパイロット＞対象のコパイロットを開きます。  
  - 対象のコパイロットの「ナレッジ」セクションで「有効」のトグルボタンがオンであるかを確認します。  
  - 確認した際の、下画像と同様のスクリーンショットをご提供ください。  
  ![](./helpful-information-for-copilotstudio-sr/image_knowledge_enabling.png)  

<a id='anchor-raw-input-output'></a>

## 8. 会話入出力 
  以下の手順でスクリーンショット画像ファイルを取得し、ご提供ください。

  - Copilot Studio ポータル>（対象の環境選択）＞カスタムコパイロット＞対象のコパイロットを開きます。  
  - 対象のコパイロットとのチャットウィンドウでコパイロットと会話を行ってください。  
  - その会話画面について、下画像と同様のスクリーンショットをご提供ください。  
  ![](./helpful-information-for-copilotstudio-sr/image_conversation_inout.png) 

<a id='anchor-conversation-transcript'></a>

## 9. 会話トランスクリプションテーブルデータ
  下記公開情報にてご案内している方法で、会話トランスクリプションテーブルのデータを取得してください。  
  取得した zip ファイルをご提供ください。  
  https://learn.microsoft.com/ja-jp/microsoft-copilot-studio/analytics-sessions-transcripts#export-conversation-transcripts

<a id='anchor-about-networkhar'></a>

## 10. Web ブラウザのネットワーク トレース・コンソール ログ
  カスタム Copilot 編集中、あるいは実行中に Copilot Studio サービスへ送信する HTTP リクエストや Copilot Studio サービスから受信する HTTP レスポンスの内容を確認することで通信上の問題を特定します。  

 > [!IMPORTANT]
 > 事象発生の事前に取得開始し、事象発生後に採取を行う必要がございます。 

 > [!NOTE]
 > 事象の内容により、netsh trace コマンドやサードパーティ製のツール「Fiddler」によるネットワーク キャプチャの取得をお願いする場合があります。  

### ブラウザネットワークトレース
  ご取得方法は以下公開情報をご参照ください。  
  [ブラウザーでネットワーク トレースを収集する (ブラウザーベースのアプリのみ)](https://learn.microsoft.com/ja-jp/azure/azure-web-pubsub/howto-troubleshoot-network-trace#collect-a-network-trace-in-the-browser-browser-based-apps-only)

  > [!IMPORTANT]
  > ご取得の際は「ログの保持」「キャッシュを無効にする」にチェックを有効にしご取得ください。  
  > ![](./helpful-information-for-copilotstudio-sr/image_browser_trace.png) 　

###  コンソールログ
  Console タブをクリックし、ログ領域を右クリックし **「名前を付けて保存」** にて保存いたします。  
  ![](./helpful-information-for-copilotstudio-sr/image_console_log.png)　　


---

## 補足

本手順は執筆時点でのユーザー インターフェイスを基に紹介しています。バージョンアップによって若干の UI の遷移など異なる場合があります。その場合は画面の指示に従って進めてください。  

