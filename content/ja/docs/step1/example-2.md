---
title: ② コメントからコードを生成
categories: [技術者向け, GitHub Copilot 基本]
weight: 2
---


GitHub Copilot では、**コメントを使ってコードを生成**するというテクニックも非常に有効です。  
「どのようなメソッド名を使うか」「引数や戻り値はどうなるか」を、コメントで簡潔に指定するだけで、残りの実装を推測して提案してくれます。  

ここでは Java をベースとし、コメントからコードを生成する例を紹介します。

---

## :pen: 例題
1. **途中までコードを書く**  
   - まずはクラスとメソッドの外側を用意します。
   ```java
   public class AverageCalculationExample {
        // 関数名: calculateAverage
        // 関数の引数: numbers (配列)
        // 関数の戻り値の型: number (Javaでは必要に応じてdoubleまたはintを使用)
   }
   ```
   ここで敢えて中身を書かず、**コメントに「関数名・引数・戻り値」** だけを指定して止めておきます。  

2. コメントの末尾で改行をすることで提案が行われます。

3. **Tabキー** で受け入れると、メソッドが完成します。


---

## :robot: Copilot による出力例

```java
public class AverageCalculationExample {
        // 関数名: calculateAverage
        // 関数の引数: numbers (配列)
        // 関数の戻り値の型: number (Javaでは必要に応じてdoubleまたはintを使用)
        public static double calculateAverage(int[] numbers) {
            // 変数の初期化
            int sum = 0;
            // 配列の要素を合計する
            for (int number : numbers) {
                sum += number;
            }
            // 配列の要素の平均を計算する
            double average = (double) sum / numbers.length;
            // 平均を返す
            return average;
        }
    }
```
Copilot がコメントをもとに、しっかり空配列対応も入れたコードを生成してくれるかもしれません。

---

### :bulb: うまく出力されない場合の対処法
改行を行うだけでは、Copilot が意図した通りの提案を行わないことがあります。
例えば、下記のように改行をした場合さらにコメントが追加されコードに関する提案が行われないことがあります。
![Image](https://github.com/user-attachments/assets/b33db224-ecfa-41ba-a759-e0a1a57772ab)

この場合、コード補完と組み合わせて書いてほしいコードの一部を書き始めることで、Copilot に提案を促すことができます。
![Image](https://github.com/user-attachments/assets/139de066-a41d-4ece-93b9-53c14b1f9bd2)



## :memo: 練習

1. **最大値を計算する関数**  
   - 以下のコメントを記述し、Copilot がどんな実装を提案するか試してみましょう。
   ```java
   // 関数名: calculateMax
   // 関数の引数: numbers (array)
   // 関数の戻り値の型: int
   ```

2. **異なる型や要件を付け加える**  
   - 例: “空の場合は-1を返す” と書くとどうなるか  
   - 例: “引数がnullの場合、例外をスローする” と書くとどうなるか

---

## まとめ

- **コメントだけで関数名・引数・戻り値などを指定**し、Copilot に中身を生成させるのは非常に効率的な方法です。  
- **細かい要件**（空配列時の挙動など）をコメントに書くと、Copilot が取り入れて実装してくれることがあります。  