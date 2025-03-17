---
title: Power Automate for desktop で「フローなし」と表示される場合の対処法
date: 2021-6-7 12:00
tags:
  - Power Automate
  - Desktop flow
  - Power Automate for desktop
categories:
  - [Power Automate, Desktop flow]
---

こんにちは。Power Automate サポートの清水です。

今回は、Power Automate for desktop で「フローなし」のエラーが表示される場合の対処法についてご紹介します。

<!-- more -->

# 現象

Power Automate for desktop をインストールした後、いざデスクトップ フローを作成しよう！とサインインすると、
以下のような エラーが表示される場合があります。

![](./PowerAutomateDesktop-NoFlow/Error.png)

今回は上記のエラー解消に向けての確認ポイントをご案内します。

## 1. ネットワーク環境の確認

Power Automate for desktop を使用するには、いくつかのエンドポイントを許可していただく必要がございます。

プロキシを経由して通信を行っている場合や、ファイアウォールで通信の制御を行っている場合には、
以下の IP アドレスやドメインが許可されているかをご確認ください。
[IP アドレスの構成 - Power Automate | Microsoft Learn](https://learn.microsoft.com/ja-jp/power-automate/ip-address-configuration)

## 2. 既定の環境でのユーザーの状態の確認

Power Automate for desktop に最初にログインした際には、テナントの既定の環境にアクセスいたします。

そのため、既定の環境でユーザーが無効な状態になっている場合にエラーが発生する場合がございますので、
ログインしているユーザーが有効な状態になっているかをご確認ください。

<確認手順>
1. 全体管理者アカウントにて、[Power Platform 管理センター](https://admin.powerplatform.microsoft.com/)にアクセスし、既定の環境をクリックします。
2. [設定] から、[ユーザーとアクセス許可] > [ユーザー] を開きます。
3. ユーザー一覧に該当のユーザーが存在するかを確認し、存在しない場合は [ユーザーの追加] ボタンをクリックします。
4. 対象ユーザーアカウントのメールアドレスを指定し、[追加] をクリックします。
5. ユーザー追加が完了しましたら、ユーザーの設定ページを開き、下図のように状態が “有効” であることを確認します。
![](./PowerAutomateDesktop-NoFlow/UserStatus.png)
6. ユーザーが有効なことが確認できたら、再度 Power Automate for desktop でのフロー作成を試します。

なお、過去に一度も Power Automate にアクセスしたことがない場合は、[Power Automate ポータル](https://japan.flow.microsoft.com)にアクセスすると、
自動で既定の環境に追加されます。
管理者でのユーザーの確認が難しい場合は、一度 Power Automate ポータルにアクセスしてから
少し時間を置いて Powre Automate for desktop にアクセスするなどの操作もお試しください。

## 3. Dataverse (旧名: Common Data Service) のライセンスの確認

Power Automate for desktop で作成されたフローは既定の環境の Dataverse に保存されるため、
Power Automate for desktop を使用するには、有効な Dataverse のライセンスが必要となります。

通常は、Power Automate のライセンスを持っていないユーザーであっても、初めて Power Automate にアクセスした際に
全てのユーザーに無償版の「Power Automate Free」というライセンスが付与されます。
そのため、この無償のライセンスによって Dataverse へのアクセスが可能となり、Power Automate for desktop を使用できます。

しかし、テナント内で無償版のライセンスを禁止としている場合には、Power Automate Free ライセンスが使用できないため、
有効なライセンスがないことから、Power Automate for desktop が使用できないケースがございます。

この場合、一度テナントの管理者にご連絡いただき、Power Automate Free もしくは Power Automate for Office 365 などのライセンスを付与していただくようお願いいたします。

## 4. Power Automate Desktop のバージョンの確認

以下のリンクから、Power Automate for desktopの最新バージョンをインストールし、
事象が解消するかどうかをご確認ください。
https://go.microsoft.com/fwlink/?linkid=2102613

## おわりに

Power Automate for desktop で「フローなし」のエラーが発生する場合の確認点についてご説明いたしました。
Power Automate for desktop を初めて使用するユーザーにとって、少しでもお役に立つ情報がございましたら幸いです。

---
