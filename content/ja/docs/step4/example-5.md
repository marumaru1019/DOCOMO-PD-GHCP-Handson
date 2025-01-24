---
title: ⑤ トラブルシュート手順書の作成
categories: [運用, GitHub Copilot]
weight: 5
---

運用現場では、**ライブラリ不足**などのコード起因だけでなく、**環境設定**や**アクセス権**のような外部要因でエラーが発生するケースが多々あります。ここでは、**Azure環境の設定ミス**が原因でアプリが起動に失敗した例をもとに、**GitHub Copilot** を使って「トラブルシュート手順書」を自動生成する流れを示します。

---

### :pen: 例題

#### シナリオ

- Javaアプリを**Azure Web App**で動かしているが、**起動時にDB接続エラー**  
- ログ内に「**DB_HOST not set or empty**」「Unknown host 'null'」などが記録  
- ローカルでは動くが、本番環境(Azure)でのみ落ちる → **環境変数(DB_HOST)未設定**が原因

##### プロンプト例

```text
Copilot, 下記ログと状況を参考に、環境変数が未設定でアプリが起動失敗する問題に関するトラブルシュート手順書をMarkdownで作ってください。

ログ抜粋:
"DB_HOST not set or empty"
"Unknown host 'null'"

状況:
- Azure Web App上で動かすJavaアプリ
- DB_HOSTが設定されていない

手順書には以下を含めて:
1) 原因の概要
2) 対処手順(Azure PortalやCLIでの設定例)
3) 再発防止策
```

---

### :robot: 出力例

```markdown
# トラブルシュート手順書 - 環境変数(DB_HOST)未設定による起動失敗

## 1. 原因の概要
- アプリの起動時に `DB_HOST` 環境変数が未設定
- ログに "DB_HOST not set or empty" とあり、DB接続先が null で失敗

## 2. 対処手順

### 2-1. Azure Portal から設定
1. Azure Portal にログイン → 対象の Web App を選択
2. 「Configuration」→「Application Settings」タブに移動
3. 「New application setting」で Name: `DB_HOST`、Value: `your-db-hostname`
4. 保存し、Web App を再起動

### 2-2. Azure CLI で設定
```bash
az login
az webapp config appsettings set \
  --resource-group MyResourceGroup \
  --name MyAppService \
  --settings DB_HOST=your-db-hostname

az webapp restart \
  --resource-group MyResourceGroup \
  --name MyAppService

- これで `DB_HOST` が設定される → 再起動後に環境変数を読めるようになる

### 2-3. Javaコード側の確認
- `System.getenv("DB_HOST")` がちゃんと値を取得できるか確認
- 必須の環境変数が無い場合、起動時にエラーを出すようにしておくと気づきやすい

## 3. 再発防止策
1. **環境変数チェック**:
   - CI/CDパイプラインや起動スクリプトで必須変数をチェック
2. **ドキュメント化**:
   - この設定内容を運用手順書やReadmeに明記しておく
3. **ローカルと本番の差分管理**:
   - Local settings と本番Azure環境の設定を比較しやすいようスクリプトやIaCを整備
```

Copilot が**原因の概要**と**対処手順**(Azure Portal/CLI)、**再発防止策**をMarkdown形式で提案し、**トラブルシュート手順書**を素早く仕上げられます。

---

### :memo: 練習

1. **別の環境要因**  
   - 例: Firewall設定ミス、アクセス権不足 → 似た形で Copilot に手順書を作らせる  
2. **Docker / Kubernetes**  
   - 「KubernetesのEnv変数がunset」などに変えて同じ流れを試す  
3. **チームドキュメントとの連携**  
   - Copilot が出力したMarkdownをWikiやConfluenceに貼り付けて、最終調整する

---

## まとめ

- **環境変数未設定**や**Firewallミス**など、環境起因のトラブルにも、**ログ内容**＆**状況**をCopilotに渡す → **Markdownの手順書**を自動生成  
- **Azure Portal**や**CLI**など具体的コマンドを例示 → 再発防止策も盛り込み  
- 最終的には**人間**がレビューして内容を整え、チームで共有 → **運用**がスムーズに  

こうして**運用フェーズ**のトラブルでも、**Copilot**を活用して**手順書作成**を効率化し、**二次トラブル**や**再発**を防げるようになります。