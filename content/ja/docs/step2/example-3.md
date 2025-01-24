---
title: ③ 段階的に指示を与える
categories: [GitHub Copilot, Prompt Engineering]
weight: 3
---

**Chain-of-Thought Prompting** では、「このようなステップで考えてほしい」と**事前にステップを明示**し、Copilot に**段階的な推論**を促す方法を取ります。やや複雑なロジックを**1ステップずつ**解説したうえでコード化を依頼すると、Copilot が**工程ごとの意図**を把握しやすくなるのが特徴です。ここでは、**Java** の例を通じて、外れ値を除去して平均を算出する一連の流れを実装してもらう方法を示します。

---

### :pen: 例題

以下は、「標準偏差を計算→外れ値を除去→残りの要素で平均を返す」という多段階処理を行う Java の関数を生成させるプロンプト例です。

#### プロンプト例

```text
次のステップを踏むJavaの関数を作ってください (Java 17):

Step:
1) 入力は int[] (整数配列)
2) まず配列の平均値 (mean) を計算
3) 次に標準偏差 (stdev) を計算
4) その後、要素のうち "mean + 2*stdev" を超えるものを外れ値として除去
5) 最後に、外れ値除去後の要素だけで平均を計算して返す

戻り値: double (最終的な平均値)
関数名: computeFilteredAverage
```

Copilot はこのように思考過程を丁寧に書くと、順を追った処理を模倣して実装してくれる可能性が高まります。

---

### :robot: 出力例

```java
public class StatisticsUtils {

    public static double computeFilteredAverage(int[] values) {
        // 1) 平均を計算
        double mean = 0;
        for (int v : values) {
            mean += v;
        }
        mean /= values.length;

        // 2) 標準偏差を計算
        double sumSq = 0;
        for (int v : values) {
            double diff = v - mean;
            sumSq += diff * diff;
        }
        double variance = sumSq / values.length;
        double stdev = Math.sqrt(variance);

        // 3) 外れ値を除去 (mean + 2*stdev より大きい要素)
        java.util.List<Integer> filtered = new java.util.ArrayList<>();
        double threshold = mean + 2 * stdev;
        for (int v : values) {
            if (v <= threshold) {
                filtered.add(v);
            }
        }

        // 4) 残り要素の平均を計算
        if (filtered.isEmpty()) {
            return 0.0; // 全部外れ値なら0でも返す
        }
        double sum = 0;
        for (int f : filtered) {
            sum += f;
        }
        return sum / filtered.size();
    }
}
```

---

### :memo: 練習

1. **ステップを増やす・修正する**  
   - たとえば「標準偏差計算後、さらに最小値/最大値を求めるステップを追加」してみる  
   - Copilot が追随してくれるか試す  
2. **ゼロショットとの比較**  
   - 同じロジックを「ステップを書かず」に頼んだときの提案と比べ、どの程度違いがあるか観察  
3. **他の言語・シナリオ**  
   - Java以外でも「ステップを明示→実装」を試す  
   - 例えば「ステップ1: データ取得→2: フィルタ→3: 結果をJSON化」など

---

## まとめ

- **Chain-of-Thought** では、**段階的な手順**を先に書き、「どのステップで何をするか」をCopilotに明確に伝える  
- **複雑なロジック**でも、Copilot が**推論しやすい形**になるため、**誤り**を減らせる  
- ステップを追加・修正していくと、Copilot は再度**一連の流れ**を踏まえたコードを生成する  

このように、**思考プロセスを段階的に明示**することで、Copilot に複雑な処理をスムーズに落とし込んでもらうことが可能です。