---
title: 事前準備
weight: 1
---


このワークショップでは、**「Azure OpenAI Studio」** と **「Bing Chat Enterprise」** を使用します。以下の手順でブラウザから利用できるかを確認してください。


## 1. Azure OpenAI Studioの場合
ワークショップを始める前に、お使いの端末のブラウザから[Azure OpenAI Studio](https://oai.azure.com/)にアクセス出来ることを確認します。

 [Azure OpenAI Studio](https://oai.azure.com/)
  ![image](https://github.com/marumaru1019/github-image/assets/70362624/ebc5a30d-3abb-4225-ae31-7d5db95732e3)
 を開き、[チャット]にアクセスします。


Azure OpenAI Studio の **[チャット]** にプロンプトを入力し、送信すると、生成AIが返答します。

![image](https://github.com/marumaru1019/github-image/assets/70362624/3e6fd489-7d1b-453d-822f-e1fc9fed1077)



チャットを削除したいときは、 **[チャットをクリアする]** ボタンをクリックします。
![image](https://github.com/marumaru1019/github-image/assets/70362624/3c8061c4-8676-43ba-a12d-9997c5bd9f7e)



### モデルのふるまい設定
**設定**では、AI アシスタントがどのように振る舞うべきかを設定できます。複数のテンプレートが用意されており、デフォルト設定では、システムメッセージは「You are an AI assistant that helps people find information.」となっています。

設定を変更したい場合は、[システム メッセージ]フィールドに新しいメッセージを入力します。例えば、「あなたは Azure を使っているお客様の質問に回答するアシスタントです。あなたは、問い合わせに対して事実に基づいた回答のみを提供し、Azure に関係のない回答は提供しません。」といった形で、AIアシスタントの役割を反映させたメッセージを設定してください。これにより、AIアシスタントの振る舞いをより具体的な目的に合わせて調整することができます。

システムメッセージを変更したら、 **[変更を適用する]** ボタンをクリックして設定を保存します。

![image](https://github.com/marumaru1019/github-image/assets/70362624/4e4bcbfa-40bd-4e1a-9335-66c41e4b6e2d)

例を設定することで、より精度の高い回答を得ることができます。このように出力してほしい、などのフォーマットの例を追加したら、 **[+ 追加]** ボタンをクリックして設定を保存します。

![image](https://github.com/marumaru1019/github-image/assets/70362624/283b598b-d020-4889-b486-68032cf2b167)


### パラメータの設定
利用するモデルやパラメータを変更したいときは **[構成]** で設定します。

**[デプロイ]** タブでは、利用するモデルやセッションに含めるメッセージを設定できます。

![image](https://github.com/marumaru1019/github-image/assets/70362624/4f1b294e-700c-4265-8a40-72c9a976e6af)

最大のトークン数やTemperature /Top_Pの値を変更したいときは、 **[パラメーター]** タブで設定します。また、モデルの応答を任意の位置で終了させたいときは、 **[シーケンスの停止]** を設定します。この停止シーケンスは4つまで含めることができます。

長い文章を試したいときは **[最大応答]** の値を大きくしてください。最大値はモデルによって異なります。

詳細については、[Azure OpenAI Service モデル](https://learn.microsoft.com/ja-jp/azure/ai-services/openai/concepts/models)を確認してください。

![image](https://github.com/marumaru1019/github-image/assets/70362624/163017b7-ee4e-42a5-bd5f-93645d13bbc4)

>  [Azure OpenAI Studio](https://oai.azure.com/)にアクセスできない場合は、 [OpenAI社 ChatGPT](https://www.bing.com/)を利用してワークショップを行うことも可能です。ただし、プロンプトに **絶対に機密情報を含めない** よう細心の注意してください。


## 2. BingChat Enterprise の場合
お使いの端末のブラウザから [BingChat Enterprise](https://www.bing.com/search?q=Bing+AI&showconv=1) にアクセス出来ることを確認します。
![](images/bingchatenterprise1.png)

チャットを削除したいときは、 **[新しいトピック]** ボタンをクリックします。
![](images/bingchatenterprise2.png)
