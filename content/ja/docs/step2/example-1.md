---
title: ①メールを作成する
categories: [業務利活用]
tags: [Mail,Translation]
weight: 1
---

メールの内容を指定することで、メールを作成することができます。

### :pen: 例題
次のプロンプトを入力します。

```text
私はソフトウェアエンジニアとして入社したばかりの新入社員です。  
プログラムの設計会議を調整するためのメールを書いてください。  

#内容  
## 候補日  
11/14 13:00-14:00  
11/28 14:00-15:00  
12/04 10:00-11:00  

##会議のアジェンダ  
* プロジェクトの進捗  
* 要件の再確認  
* 技術的課題とリスクの議論  

## 希望する返信  
候補日はいつがいいか  
アジェンダ以外でディスカッションしたい内容  

##制約条件  
日本語/英語どちらでも読めるように工夫してください。
```

### :robot: 出力例

件名: プログラムの設計会議の調整について  
   
挨拶文  
   
私は、新入社員の〇〇と申します。ソフトウェアエンジニアとして入社したばかりですが、プログラムの設計会議の調整をお願いいたします。  
   
以下に候補日と会議のアジェンダをご確認いただき、返信いただけると幸いです。  
   
【候補日】  
1. 11/14 13:00-14:00  
2. 11/28 14:00-15:00  
3. 12/04 10:00-11:00  
   
【会議のアジェンダ】  
- プロジェクトの進捗  
- 要件の再確認  
- 技術的課題とリスクの議論  
   
また、以下の項目についても返信いただけると助かります。  
   
1. 候補日の中でいつが最適ですか？  
2. アジェンダ以外でディスカッションしたい内容があれば教えてください。  
   
ご多忙の中、お手数をおかけしますが、ご確認のほどよろしくお願いいたします。  
   
よろしくお願いいたします。  
   
〇〇（自分の名前）  
   
-------------------------------------  
   
Subject: Regarding the coordination of program design meeting  
   
Greeting  
   
I am a new employee, recently joined as a software engineer. I would like to request your assistance in coordinating a program design meeting.  
   
Please review the following candidate dates and the agenda for the meeting, and kindly let me know your preferences by replying to this email.  
   
【Candidate Dates】  
1. 11/14 13:00-14:00  
2. 11/28 14:00-15:00  
3. 12/04 10:00-11:00  
   
【Meeting Agenda】  
- Progress of the project  
- Reconfirmation of requirements  
- Discussion on technical challenges and risks  
   
Additionally, it would be helpful if you could provide the following information:  
   
1. Which date among the candidate dates would be the most suitable for you?  
2. If there are any topics you would like to discuss apart from the agenda, please let me know.  
   
I apologize for the inconvenience caused by this request amidst your busy schedule. Thank you very much for your attention and cooperation.  
   
Best regards,  
   
〇〇 (Your Name)

