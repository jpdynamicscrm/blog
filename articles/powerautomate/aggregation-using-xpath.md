---
title: XPath を活用した集計
date: 2023-02-03 00:00
tags:
  - Power Automate
  - Cloud flows
---


残念ながら Power Automate には配列を 1 度に集計するようなアクションや関数はご提供がありません。
それでもなるべく少ないアクション数で集計をしたい場合は、XPath の関数の機能を活用いただけます。

例えば、下図のように、数値の列を持つ SharePoint リストがあるとします。
![](troubleshoot-throttling-flow/image01.png)

この amount1、amount2 の列の合計を求めたい場合、素直に考えると Apply to each で数を集計することを考えます。
しかしこの方法では、集計するだけで少なくとも リストの行数 × 集計したい列の数 の回数アクションが実行されます。

※ Apply to each を使った集計の例
![](troubleshoot-throttling-flow/image02.png)

XPath は XML という形式のデータの位置を指定できる言語です。関数も用意されていて、集計 (sum) や個数のカウント (count) ができます。
Power Automate 上のデータは基本的に JSON 形式で扱われます。
ですが、Power Automate には、JSON 形式を XML 形式へ変換する関数と、XPath の処理を行える関数がありますので、それらを使って集計を求められます。

以下に、XML と XPath を使って、先ほどの SharePoint リストを集計するサンプルを記載いたします。
Apply to each を使用しないので、行数が増えても実行されるアクション数が増えません。

※フロー全体図
![](troubleshoot-throttling-flow/image03.png)

(1) 複数の項目の取得  
SharePoint コネクタの「複数の項目の取得」アクションを使用し、リストを取得します。

|パラメータ名|値|
|---|---|
|サイトのアドレス|リストがあるサイトを選択します。|
|リスト名|リストの名前を選択します。|

(2) 選択
データ操作コネクタの「選択」アクションを使用し、集計に必要な列のみの配列を作成します。

|パラメータ名|値|
|---|---|
|開始|「(1) 複数の項目の取得」アクションの出力「value」|
|マップ|集計に必要な列の列名と値を指定します。|

この例では amount1 列と amount2 列を集計したいので、その2つを指定します。
	amount1	「(1) 複数の項目の取得」アクションの出力「amount1」
	amount2	「(1) 複数の項目の取得」アクションの出力「amount2」

(3) 作成1
データ操作コネクタの「作成」アクションを使用し、配列をXML へ変換できる形へ整形します。

|パラメータ名|値|
|---|---|
|入力|`{"root": {"numbers":「(2) 選択」アクションの出力  }}`|

(4) 作成2
データ操作コネクタの「作成」アクションを使用し、先のアクションの出力を XML へ変換します。

|パラメータ名|値|
|---|---|
|入力|以下を式として入力：<br>`xml(outputs('作成1'))`　※1|

※1 「作成1」は「(3) 作成1」アクションのアクション名です。

参考 (xml 関数)：https://learn.microsoft.com/ja-jp/azure/logic-apps/workflow-definition-language-functions-reference#xml

(5) 作成-amount1
データ操作コネクタの「作成」アクションを使用し、XML へ変換した内容と XPath を使用して amount1 列の集計を求めます。
|パラメータ名|値|
|---|---|
|入力|以下を式として入力：<br>`xpath(outputs('作成2'), 'sum(/root/numbers/amount1)')`　※1|

※1 「作成2」は「(4) 作成2」アクションのアクション名です。

参考 (xpath 関数)：https://learn.microsoft.com/ja-jp/azure/logic-apps/workflow-definition-language-functions-reference#xpath

(6) 作成-amount2
データ操作コネクタの「作成」アクションを使用し、amount1 と同じく amount2 の集計を求めます。
|パラメータ名|値|
|---|---|
|入力|以下を式として入力：<br>`xpath(outputs('作成2'), 'sum(/root/numbers/amount2)')`　※1|

※1 「作成2」は「(4) 作成2」アクションのアクション名です。

実行結果：