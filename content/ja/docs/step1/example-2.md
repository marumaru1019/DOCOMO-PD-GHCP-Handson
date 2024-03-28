---
title: ②出力形式を指定する
categories: [業務利活用,技術者向け]
tags: [json, csv, Format]
weight: 2
---


どのようなフォーマットで回答やデータを生成してほしいかを明示しましょう。たとえば、結果を箇条書きにしてほしいのか、JSONやCSVなどのデータ形式にしてほしいのかを具体的に指示します。


### :pen: 例題
次のプロンプトを入力します。

```text
ユーザー情報の送信を行います。
次のキーでJSON形式で提供してください。
username, password, email, phone, address

**説明や装飾は不要です。出力はJSONのみ回答してください**
```

### :robot: 出力例
以下は、指定されたキーでJSON形式で提供されたデータです。

```json
{
  "username": "example_user",
  "password": "example_password",
  "email": "example@example.com",
  "phone": "1234567890",
  "address": "123 Example Street, City, Country"
}
```

### :memo: 練習

* データを任意の値に変えて実行してみて下さい。
* テストデータを作成したり、データをCSV形式や表形式などに出力してみてください。
* 「説明は不要です。～」の1行を消して再実行してみて下さい。
* インターネットで検索したニュースの文章を箇条書きにできるか試してみましょう。


