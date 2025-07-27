---
title: ④ Copilot のカスタム
categories: [GitHub Copilot, Agent Mode]
weight: 4
---

## 1. Copilot のカスタム方法は？

VS Code の GitHub Copilot は、**カスタム指示**や**プロンプトファイル**を使用してAIの応答をプロジェクトの要件に合わせてカスタマイズできます。毎回同じコンテキストを入力する代わりに、ファイルに保存して自動的にすべてのチャットリクエストに含めることが可能です。

> **ポイント**
>
> * **カスタム指示** … コーディング規約や技術スタックに関する共通ガイドライン（**どのように**作業を行うか）
> * **プロンプトファイル** … 特定タスク用の再利用可能なプロンプト（**何を**行うか）
> * **自動適用** … ワークスペース全体またはファイル固有の自動適用
> * **チーム共有** … バージョン管理でチーム全体の標準化

---

## 2. カスタムの種類と使い分け

VS Code では以下の3つの方法でCopilotをカスタマイズできます：

| 種類 | 用途 | ファイル形式 | 適用範囲 | 使い分けのポイント |
|------|------|-------------|----------|-------------------|
| **`.github/copilot-instructions.md`** | 全般的なコーディング規約 | Markdown | ワークスペース全体 | • プロジェクト全体の基本ルール<br>• すべてのチャットに自動適用<br>• チーム共有が容易 |
| **`.instructions.md`** | タスク・技術固有の指示 | Markdown | globパターンで指定 | • 特定ファイルタイプや領域の詳細ルール<br>• 条件付き自動適用<br>• 複数ファイルで分割管理 |
| **`.prompt.md`** | 再利用可能なプロンプト | Markdown | 手動実行 | • 複雑なタスクの標準化<br>• 変数を使った柔軟な実行<br>• 頻繁に行う作業の効率化 |

### 使い分けの指針

**📝 基本ルール → `.github/copilot-instructions.md`**
- プロジェクト全体で統一したいコーディング規約
- 技術スタックの指定（React, TypeScript等）
- 基本的なファイル構成やネーミング規則

**🎯 専門領域 → `.instructions.md`**
- フロントエンド/バックエンド固有のルール
- テスト専用の指示
- 特定のファイルパターンにのみ適用したい詳細ルール

**🚀 作業効率化 → `.prompt.md`**
- コンポーネント生成、API作成などの定型作業
- コードレビュー、セキュリティチェックなどの検査作業
- 複数のパラメータが必要な複雑なタスク

---

## 3. カスタム指示（Custom Instructions）

### 3.1 カスタム指示の作成方法

**手順：**
![ツールの選択方法](../images/custom-instructions-activate.png)
1. `github.copilot.chat.codeGeneration.useInstructionFiles` 設定を `true` に設定
2. ワークスペースルートに `.github` フォルダを作成（存在しない場合）
3. `.github/copilot-instructions.md` ファイルを作成
4. Markdown形式で指示内容を記述

### 3.2 ファイル構造

#### `.github/copilot-instructions.md`
```
プロジェクトルート/
├── .github/
│   └── copilot-instructions.md  # 📝 全般的なコーディング規約
└── ...
```

**ファイル内容の構造：**
- **ヘッダー（必須）**: なし（Front Matter不要）
- **本文**: Markdown形式で自然言語による指示
- **フォーマット**: 見出し、リスト、コードブロックが利用可能
- **適用範囲**: ワークスペース全体に自動適用

### 3.3 実践的な使い方

### :pen: 例題

基本的なカスタム指示ファイルを作成してみましょう。

```markdown
# TypeScript & React プロジェクトのコーディング規約

## TypeScript
- 厳密な型定義を使用してください
- interface よりも type エイリアスを優先してください
- 非同期処理には async/await を使用してください

## React
- 関数コンポーネントのみを使用してください
- スタイルは Tailwind CSS を使用してください
- カスタムフックでロジックを分離してください

## テスト
- すべての新機能にはテストを含めてください
- Jest と React Testing Library を使用してください
```

### :robot: 出力例

カスタム指示を設定後、Copilot が以下のように応答します：

**ユーザー:** 「Todoのフィルタリング機能コンポーネントを作成して」

**Copilot応答:**
```typescript
// 📝 type エイリアスを使用した型定義
type TodoFilter = 'all' | 'active' | 'completed';

interface TodoFilterProps {
  currentFilter: TodoFilter;
  onFilterChange: (filter: TodoFilter) => void;
  activeTodosCount: number;
  completedTodosCount: number;
  onClearCompleted: () => void;
}

// 📝 関数コンポーネントとして実装
const TodoFilterComponent: React.FC<TodoFilterProps> = ({
  currentFilter,
  onFilterChange,
  activeTodosCount,
  completedTodosCount,
  onClearCompleted
}) => {
  const filters: { key: TodoFilter; label: string }[] = [
    { key: 'all', label: 'すべて' },
    { key: 'active', label: 'アクティブ' },
    { key: 'completed', label: '完了済み' },
  ];

  return (
    <div className="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-4">
      <div className="flex gap-1 bg-gray-100 dark:bg-gray-700 rounded-lg p-1">
        {filters.map(({ key, label }) => (
          <button
            key={key}
            onClick={() => onFilterChange(key)}
            className={`px-4 py-2 rounded-md text-sm font-medium transition-all ${
              currentFilter === key
                ? 'bg-white dark:bg-gray-600 text-blue-600 shadow-sm'
                : 'text-gray-600 hover:text-gray-900'
            }`}
          >
            {label}
            {key === 'active' && activeTodosCount > 0 && (
              <span className="ml-1 px-2 py-0.5 bg-blue-100 text-blue-800 rounded-full text-xs">
                {activeTodosCount}
              </span>
            )}
          </button>
        ))}
      </div>
    </div>
  );
};
```

> **💡 Tips: カスタム指示の自動生成**
>
> VS Code の新機能として、**`チャット: ワークスペース指示ファイルを生成する`** コマンドが利用できます。このコマンドは、既存のコードベースを分析して、プロジェクトに最適化されたカスタム指示を自動生成します。
>
> **使用方法:**
> - **コマンドパレット**から `チャット: ワークスペース指示ファイルを生成する` を実行
> ![Instructionsの作成](../images/generate-custom.png)
> - または **チャットビュー**の「歯車アイコン」→「指示の生成」を実行
> ![Instructionsの作成2](../images/generate-custom2.png)
>
> **機能:**
> - プロジェクトの構造、技術スタック、コーディングパターンを分析
> - `.github/copilot-instructions.md` ファイルを自動作成
> - 既存の指示ファイルがある場合は改善提案を生成
>
> 生成された指示は、チームの特定のニーズに合わせてカスタマイズできます。

---

## 4. Instructionsファイル（`.instructions.md`）

### 4.1 作成方法

**手順：**
1. **コマンドパレット**から `チャット: 手順の構成` を実行
![Instructionsの作成](../images/instruction-create.png)
1. または **チャットビュー**の「歯車アイコン」→「指示」→「新しい命令ファイル」
![Instructionsの作成2](../images/instruction-create2.png)
1. 保存場所を選択：
   - **Workspace**: `.github/instructions/` フォルダ（ワークスペース固有）
   - **User profile**: ユーザープロファイル（複数ワークスペース共通）
2. ファイル名を入力（例：`typescript.instructions.md`）

### 4.2 ファイル構造

#### ディレクトリ構造
```
プロジェクトルート/
├── .github/
│   ├── copilot-instructions.md     # 📝 全般指示
│   └── instructions/
│       ├── backend.instructions.md  # 📝 バックエンド専用
│       ├── frontend.instructions.md # 📝 フロントエンド専用
│       └── testing.instructions.md  # 📝 テスト専用
└── ...
```

#### ファイル内容の構造
```markdown
---
description: "TypeScript専用のコーディング指示"
applyTo: "**/*.ts,**/*.tsx"
---

# TypeScript専用指示

## 型定義
- 厳密な型定義を使用してください
- any型の使用を禁止します

## コーディングスタイル
- セミコロンを必須とします
```

**構造要素：**
- **Front Matter**（任意）:
  - `description`: 指示ファイルの説明
  - `applyTo`: 自動適用するファイルのglobパターン
- **本文**: Markdown形式の指示内容

### 4.2 Front Matter の設定項目

| 項目 | 説明 | 例 |
|------|------|-----|
| `description` | 指示ファイルの説明文 | `"TypeScript専用のコーディング指示"` |
| `applyTo` | 自動適用するファイルのglobパターン | `"**/*.ts,**/*.tsx"` |

### 4.3 実践的な使い方

### :pen: 例題

タスク固有のinstructionsファイルを作成してみましょう。

**1. テスト専用指示ファイル（`testing.instructions.md`）:**
```markdown
---
description: "テスト作成時の指示"
applyTo: "**/*.test.ts,**/*.spec.ts"
---

# テスト作成指示

## テストライブラリ
- Jest と React Testing Library を使用
- テストケースは describe と it で構造化

## テスト内容
- 正常系と異常系の両方をテスト
- ユーザーの操作に基づいたテストを優先

## アサーション
- toBeInTheDocument(), toHaveTextContent() などのユーザー視点のアサーションを使用
```

**2. バックエンド専用指示ファイル（`backend.instructions.md`）:**
```markdown
---
description: "バックエンドAPI開発指示"
applyTo: "**/api/**/*.ts,**/server/**/*.ts"
---

# バックエンドAPI開発指示

## エラーハンドリング
- 適切なHTTPステータスコードを返す
- エラーレスポンスは統一フォーマットを使用

## バリデーション
- 入力値の検証を必須とする
- Zodなどのスキーマバリデーションライブラリを使用
```

**使用方法：**
- **自動適用**: `applyTo` で指定したファイルで自動的に適用
- **手動適用**: チャットで `Add Context > Instructions` から選択

---

## 5. Promptsファイル（`.prompt.md`）

### 5.1 作成方法

**手順：**
1. **コマンドパレット**から `チャット: プロンプトファイルの構成` を実行
![Promptsの作成](../images/prompt-create.png)
2. または **チャットビュー**の「歯車アイコン」→「プロンプト」→「新しいプロンプトファイル」
![Promptsの作成](../images/prompt-create2.png)
3. 保存場所を選択：
   - **Workspace**: `.github/prompts/` フォルダ（ワークスペース固有）
   - **User profile**: ユーザープロファイル（複数ワークスペース共通）
4. ファイル名を入力（例：`create-component.prompt.md`）

### 5.2 ファイル構造

#### ディレクトリ構造
```
プロジェクトルート/
├── .github/
│   └── prompts/
│       ├── create-component.prompt.md    # 📝 コンポーネント生成用
│       ├── api-review.prompt.md          # 📝 API レビュー用
│       └── security-check.prompt.md      # 📝 セキュリティチェック用
└── ...
```

#### ファイル内容の構造
```markdown
---
mode: agent
model: gpt-4
description: "Reactコンポーネント生成プロンプト"
tools: ["editFiles", "runTests"]
---

# React コンポーネントの生成

${input:componentName:コンポーネント名} コンポーネントを作成してください。

## 要件
- TypeScript対応
- styled-components使用
- テストファイルも生成
```

**構造要素：**
- **Front Matter**（任意）:
  - `mode`: チャットモード（`ask`, `edit`, `agent`）
  - `model`: 使用するAIモデル
  - `description`: プロンプトの説明
  - `tools`: エージェントモードで使用可能なツール
- **本文**: プロンプト内容（変数、他ファイル参照可能）

### 5.2 Front Matter の設定項目

| 項目 | 説明 | 例 |
|------|------|-----|
| `mode` | チャットモード | `"agent"`, `"ask"`, `"edit"` |
| `model` | 使用するAIモデル | `"gpt-4"`, `"Claude Sonnet 4"` |
| `description` | プロンプトの説明文 | `"Reactコンポーネント生成プロンプト"` |
| `tools` | 使用可能なツール・ツールセット | `["editFiles", "runTests"]` |

### 5.3 実践的な使い方

### :pen: 例題

再利用可能なpromptsファイルを作成してみましょう。

**1. コンポーネント生成プロンプト（`create-component.prompt.md`）:**
```markdown
---
mode: agent
description: "Reactコンポーネント生成"
tools: ["editFiles", "runTests"]
---

# React コンポーネントの生成

${input:componentName:コンポーネント名} という名前でReactコンポーネントを作成してください。

## 要件
- ${input:componentType:コンポーネントタイプ（例：form, list, modal）} として機能
- TypeScript対応
- styled-components使用
- React Testing Library を使用したテストファイルも生成

## ファイル構成
- `${componentName}.tsx` - メインコンポーネント
- `${componentName}.test.tsx` - テストファイル
- `index.ts` - エクスポート用
```

**2. APIレビュープロンプト（`api-review.prompt.md`）:**
```markdown
---
mode: ask
description: "REST APIのセキュリティレビュー"
---

# REST API セキュリティレビュー

選択されたAPIコードに対してセキュリティレビューを実行してください。

## チェック項目
- 入力値の検証
- 認証・認可の実装
- SQLインジェクション対策
- XSS対策
- レート制限の実装

## 出力形式
- **セキュリティリスク**: 発見された問題点
- **推奨対策**: 具体的な修正方法
- **優先度**: High/Medium/Low
```

**実行方法：**
1. **コマンドパレット**: `Chat: Run Prompt` → プロンプト選択
2. **チャット内**: `/create-component` や `/api-review`
3. **エディタ**: プロンプトファイルの再生ボタンをクリック

### :robot: 出力例

**プロンプト実行:**
`/create-component` 入力後、以下のダイアログが表示：
- コンポーネント名: `UserProfile`
- コンポーネントタイプ: `form`

**Copilot応答:**
```typescript
// 📝 UserProfile.tsx
import React from 'react';
import styled from 'styled-components';

type UserProfileProps = {
  onSubmit: (data: UserData) => void;
  initialData?: UserData;
};

export const UserProfile: React.FC<UserProfileProps> = ({
  onSubmit,
  initialData
}) => {
  // フォーム実装
};

// 📝 UserProfile.test.tsx
import { render, screen } from '@testing-library/react';
import { UserProfile } from './UserProfile';

describe('UserProfile', () => {
  it('should render form fields', () => {
    // テスト実装
  });
});
```

---

## 6. 実践的な使用パターン

### 6.1 チーム標準の共有

**推奨ディレクトリ構造:**
```
.github/
├── copilot-instructions.md           # 📝 全般的なコーディング規約
├── instructions/
│   ├── backend.instructions.md       # 📝 バックエンド専用指示
│   ├── frontend.instructions.md      # 📝 フロントエンド専用指示
│   ├── testing.instructions.md       # 📝 テスト専用指示
│   └── database.instructions.md      # 📝 データベース専用指示
└── prompts/
    ├── create-api.prompt.md          # 📝 API作成用プロンプト
    ├── create-component.prompt.md    # 📝 コンポーネント作成用
    ├── code-review.prompt.md         # 📝 コードレビュー用
    └── security-check.prompt.md      # 📝 セキュリティチェック用
```

### 6.2 段階的導入のベストプラクティス

**フェーズ1**: 基本的なカスタム指示
1. `.github/copilot-instructions.md` で基本ルールを設定
2. プロジェクト全体に適用される最低限の規約を記述

**フェーズ2**: 専門分野別の指示
1. `frontend.instructions.md`, `backend.instructions.md` を作成
2. 技術領域ごとの詳細な指示を分割

**フェーズ3**: タスク別プロンプト
1. 頻繁に行うタスク用の `.prompt.md` ファイルを作成
2. チーム内で共通化できるプロンプトを蓄積

### :memo: 練習

1. **基本設定**: プロジェクトに `.github/copilot-instructions.md` を作成し、基本的なコーディング規約を定義してください

2. **専門指示**: 使用している技術スタック用の `.instructions.md` ファイルを作成してください
   - TypeScript/React用
   - テスト用
   - API用

3. **再利用プロンプト**: よく使用するタスク用の `.prompt.md` ファイルを作成してください
   - コンポーネント作成用
   - コードレビュー用

4. **動作確認**: 作成したファイルが正しく機能することを確認してください
   - チャットで指示が自動適用されるか
   - プロンプトが正しく実行されるか

---

## 7. まとめ

Copilotカスタマイゼーションの3つのアプローチ：

| ファイルタイプ | 用途 | 作成方法 | 実践ポイント |
|---------------|------|----------|-------------|
| **`.github/copilot-instructions.md`** | 全般的なコーディング規約 | 手動作成またはワークスペース生成 | • プロジェクト全体の基本ルール<br>• 自動適用<br>• チーム共有しやすい |
| **`.instructions.md`** | タスク・技術固有の指示 | コマンドパレットまたはチャットUI | • globパターンで自動適用<br>• 複数ファイルで分割管理<br>• ワークスペース/ユーザー別 |
| **`.prompt.md`** | 再利用可能なプロンプト | コマンドパレットまたはチャットUI | • 変数使用可能<br>• 手動実行<br>• 複雑なタスクの標準化 |

**成功のポイント:**
- **段階的導入**: 基本→専門→タスク別の順で導入
- **チーム標準化**: バージョン管理でファイルを共有
- **継続的改善**: 使用状況に応じて指示を調整
- **適切な分割**: 1ファイル1目的で管理しやすく
