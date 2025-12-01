---
title: ② ツール
categories: [GitHub Copilot, Agent Mode]
weight: 2
---

## 1. ツールとは？

GitHub Copilot のチャットでは、**`#`ツール名**を使って便利な機能を呼び出すことができます。
これらは「ツール」と呼ばれ、コードに関する情報を簡単に取得したり、様々な作業を自動化したりできます。

**身近な例で説明すると：**
- `#selection` → 今選択しているコードについて質問
- `#terminalSelection` → ターミナルのエラーメッセージについて質問
- `#fetch` → ウェブサイトの内容を取得して分析

> **ポイント**
>
> * **チャット変数の参照** … `#-mention構文`で関連コンテキストを効率的に参照
> * **Agent mode での自動実行** … 自律的なコーディングワークフローで必要に応じてツールが実行される
> * **拡張機能とMCPサーバー** … 追加のツールを提供し、チャットプロンプトで利用可能

---

## 2. 利用可能なツール一覧

| ツール                          | 目的・説明                            |
| ---------------------------- | -------------------------------- |
| `#changes`                   | Git 変更一覧を取得し、差分を参照に会話            |
| `#codebase`                  | ワークスペース全体をコード検索して関連箇所を抽出         |
| `#editFiles`                 | ファイルの新規作成・編集を行うツールセット            |
| `#extensions`                | VS Code 拡張機能を検索・質問               |
| `#fetch`                     | 指定 URL の HTML/JSON などを取得して内容を利用  |
| `#findTestFiles`             | テストファイルを検索して一覧化                  |
| `#githubRepo`                | GitHub 上の公開リポジトリをコード検索           |
| `#new`                       | 新しい VS Code ワークスペースをスキャフォールド     |
| `#openSimpleBrowser`         | VS Code 内ブラウザでローカル Web アプリをプレビュー |
| `#problems`                  | Problems パネルのエラー・警告を取得           |
| `#readCellOutput`            | Notebook のセル出力を読み取り              |
| `#runCommands`               | ターミナルコマンドを実行し出力を取得               |
| `#runNotebooks`              | Notebook のセルを実行し出力を取得            |
| `#runTasks`                  | `tasks.json` に定義したタスクを実行         |
| `#runTests`                  | テストスイートを実行し結果を取得                 |
| `#search` / `#searchResults` | ワークスペース内のファイル検索＆結果取得             |
| `#selection`                 | エディタで選択中のテキストをコンテキスト追加           |
| `#terminalSelection`         | ターミナルで選択中のテキスト（コマンドなど）を追加        |
| `#terminalLastCommand`       | 直近で実行したターミナルコマンドを追加              |
| `#testFailure`               | テスト失敗情報をコンテキストに追加                |
| `#usages`                    | シンボル参照・実装・定義情報をまとめて取得            |
| `#VSCodeAPI`                 | VS Code 拡張 API のリファレンスを検索        |

---

## 3. 具体的なツールの使い方

### :pen: 例題

よく使うツールの実際の使い方を見てみましょう。

### 💡 シナリオ1：ターミナルでエラーが出た時

**状況：** ターミナルでコマンドを実行したらエラーメッセージが表示された

**やり方：**
1. ターミナルのエラーメッセージをマウスで選択
2. チャットに以下のように入力

```text
#terminalSelection このエラーを解決する方法を教えて
```

**結果：** Copilotがエラーの原因と解決方法を詳しく説明してくれます

---

### 💡 シナリオ2：React公式サイトの情報が知りたい時

**状況：** Reactの最新情報をチェックしたい

**やり方：**
```text
#fetch https://react.dev Reactの最新バージョンについて教えて
```

**結果：** React公式サイトから最新バージョンの情報を取得し、要約してくれます

### 💡 シナリオ3：Next.jsのスタイリングライブラリを調べたい時

**状況：** Next.jsで使えるスタイリングライブラリの選択肢を知りたい

**やり方：**
```text
#githubRepo nextjsで使用できるスタイリング用のライブラリで使えそうなものをいくつか教えて
```

**結果：** Next.jsエコシステムから人気のスタイリングライブラリ（Tailwind CSS、Styled Components、Chakra UI等）とその特徴を教えてくれます

## 4. 分からない時は直接聞こう

**使い方が分からないツールがある時は、直接聞いてみましょう**

```text
#terminalSelection このツールの使い方を教えて
```

どのツールでも「このツールの使い方を教えて」と聞けば、詳しい説明をしてくれます。

---

## 5. Agent mode でのツール制御

Agent mode では、使用するツールを制御できます。


**Tool Pickerの使い方：**
![ツールの選択方法](../images/tool_select.png)
1. Agent mode のチャット入力欄右端の 🔧 アイコンをクリック
2. 使いたいツールだけにチェックを入れる

これにより、Agent mode が自動で作業する際に使用するツールを制限できます。

---

## 6. MCPサーバーでツールを拡張する

### 6.1 MCP とは

MCP（Model Context Protocol）は、**AIと外部システム（API/DB/ファイル/クラウド等）を安全に接続するための標準プロトコル**です。VS Code の Copilot Chat に MCP サーバーを追加すると、そのサーバーが提供する"ツール"が **Tool Picker** に現れ、Agent mode から呼び出せます。

**導入メリット:**
* **手作業のコマンド/API呼び出しを会話化**（例：GitHubやAzure操作）
* **再利用・持ち運びが効く**：MCPはクライアント非依存（VS Code/JetBrains/Visual Studio 等）で同じサーバーを使い回せる
* **構成の見える化**：`mcp.json` で「どのツールを使って良いか」を明文化し、リポジトリに同梱可能

### 6.2 MCPサーバーのインストール方法

MCPサーバーをインストールする方法は3つあります。

#### 方法1: VS Code 拡張機能からインストール

**手順:**
1. 拡張機能ビュー（⇧⌘X / Ctrl+Shift+X）を開く
2. MCP サーバー名を検索（例：「Azure MCP」「GitHub MCP」）
3. 「Install」ボタンをクリック

![MCP 拡張機能](../images/mcp-extension.png)

#### 方法2: MCP Marketplace からインストール（プレビュー機能）

> **Note**: 設定で `chat.mcp.gallery.enabled` を `true` に設定する必要があります。

**手順:**
1. 拡張機能ビュー（⇧⌘X / Ctrl+Shift+X）を開く
2. 検索ボックスに `@mcp` と入力
3. 必要な MCP サーバーを選択してインストール

![MCP Marketplace](../images/mcp-marketplace.png)

#### 方法3: 手動で mcp.json を編集

`.vscode/mcp.json` ファイルを直接編集する方法です。

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/directory"]
    }
  }
}
```

> **💡 Tips**: [公式サンプル集](https://github.com/modelcontextprotocol/servers/tree/main)に、様々な MCP サーバーの一覧と設定方法が記載されています。

### 6.3 例題 - Azure MCP ServerでBicepテンプレート作成

**目的:** Storage AccountのBicepテンプレートを最新のAPI仕様で作成したい

**手順:**

1. [拡張機能のリンク](vscode:extension/ms-azuretools.vscode-azure-mcp-server) を開いてインストール
   ![Azure MCP Server 拡張機能](../images/azuremcp.png)

2. Copilot Chat → Agent を選択し、🔧 **Tools** で "Azure MCP Server" のツールを有効化
   ![Azure MCP Server 有効化](../images/azuremcpactivate.png)

3. チャットに以下のプロンプトを入力：

```text
#bicepschema
最新のapiVersionでStorage AccountのBicepテンプレートを作成してください。

要件：
- SKU: Standard_LRS
- 既存VNet統合は不要
- 冪等性を保つ設計
- 説明コメントも含める
```

### :robot: 出力例

Azure MCP Serverが最新のAPI仕様を取得し、以下のようなBicepテンプレートを生成：

```bicep
@description('Storage Accountの名前')
@minLength(3)
@maxLength(24)
param storageAccountName string

@description('リージョン')
param location string = resourceGroup().location

@description('Storage Account')
resource storageAccount 'Microsoft.Storage/storageAccounts@2025-01-01' = {
  name: storageAccountName
  location: location
  sku: {
    name: 'Standard_LRS'
  }
  kind: 'StorageV2'
  properties: {
    supportsHttpsTrafficOnly: true
    minimumTlsVersion: 'TLS1_2'
    allowBlobPublicAccess: false
  }
}

output storageAccountId string = storageAccount.id
output blobEndpoint string = storageAccount.properties.primaryEndpoints.blob
```

**その他の活用例:**
```text
#bicepschema Microsoft.KeyVault/vaults の最新 apiVersion を教えて
```

---

## :memo: 練習

以下の練習で実際にツールを使ってみましょう：

1. **エラー解析の練習**
   何かコマンドを実行してエラーを出し、`#terminalSelection`で解決方法を聞いてみる

2. **情報収集の練習**
   `#fetch`で好きなウェブサイトの情報を取得・要約してもらう

3. **コード研究の練習**
   `#githubRepo`で興味のあるプロジェクトのコードを検索してもらう

4. **MCP拡張の練習（上級者向け）**
   - `.vscode/mcp.json` を作成してシンプルなMCPサーバーを追加してみる
   - Tool Pickerで新しいツールが表示されることを確認する

---

## 7. まとめ

* **`#`記号** でツールを簡単に呼び出せる
* **シナリオ別** に使い分けることで効率的な開発が可能
* **分からない時は直接聞く** ことで新しいツールも覚えやすい
* **Agent mode** では自動でツールが選択されるが、Tool Pickerで制御も可能
* **MCPサーバー** で外部システム連携ツールを追加可能（Azure、GitHub等）
* **実際に使ってみる** ことで、各ツールの便利さが実感できる

> MCPサーバーを活用することで、GitHub Copilotの基本ツールを大幅に拡張し、プロジェクト固有のワークフローに最適化された開発環境を構築できます。
