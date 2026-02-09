---
description: ユーザーの要望に基づいてAgent Skillsを作成し、distディレクトリに出力する
---

# Agent Skill 作成

ユーザーの要望: $ARGUMENTS

## 手順

### 1. ドキュメントの読み込み（必須）

**スキル作成前に、`docs/` ディレクトリ以下のすべてのドキュメントを読むこと。**

必ず読むドキュメント:
- `docs/agent-skills/` 以下のすべてのファイル（標準仕様）
- `docs/claude-code/` 以下のすべてのファイル
- `docs/gemini-cli/` 以下のすべてのファイル
- `docs/openai-codex/` 以下のすべてのファイル

### 2. 要件の分析

ユーザーの要望から以下を抽出する:
- スキル名（小文字、ハイフン区切り）
- スキルの目的と機能
- いつこのスキルが使われるべきか（トリガー条件）
- いつこのスキルが使われるべき**でない**か（スコープの境界）
- 必要なスクリプトやリソース

### 3. description の設計（最重要）

**`description` はスキルの成否を決める最も重要なフィールドである。**

以下を必ず含める:
1. **何ができるか**: スキルの機能を具体的に記述
2. **いつ使うべきか**: トリガー条件を明示（`Use when...` / `〜のときに使用する`）
3. **いつ使うべきでないか**: スコープと境界を明確に

#### 良い例
```yaml
description: |
  PDFファイルからテキストとテーブルを抽出し、フォームに入力し、複数のPDFをマージする。
  PDFドキュメントを扱うとき、またはユーザーがPDF、フォーム、ドキュメント抽出について言及したときに使用する。
```

#### 悪い例
```yaml
description: PDFを扱う  # 曖昧すぎる、いつ使うかが不明
```

### 4. ファイルの作成

`dist/<skill-name>/` ディレクトリに以下を作成:

```
dist/<skill-name>/
├── SKILL.md          # 必須
├── scripts/          # 必要な場合のみ
├── references/       # 必要な場合のみ
└── assets/           # 必要な場合のみ
```

### 5. SKILL.md の作成

以下のフォーマットで作成:

```yaml
---
name: <skill-name>
description: |
  このスキルの目的と機能。
  〜のときに使用する。
---

# <スキル名>

## 概要
このスキルが解決する問題...

## いつ使うか
- ユーザーが〜したいとき
- 〜という状況のとき

## 手順

### 1. <ステップ1>
具体的な指示...

### 2. <ステップ2>
具体的な指示...

## 注意事項
- エッジケースの対処法
- よくある間違い
```

### 6. Claude Code 用 frontmatter フィールド

標準フィールド（`name`, `description`）に加えて、Claude Code特有のフィールドを必要に応じて追加:

#### `allowed-tools`（推奨）
スキル実行中に許可なく使えるツールを指定。UXを大きく左右するため、必要なツールがあれば積極的に追加。

```yaml
allowed-tools: Read, Grep, Glob, Bash(git *)
```

#### `disable-model-invocation`
`true`にするとClaudeが自動呼び出しせず、`/name`での手動呼び出しのみになる。
デプロイ、送信、削除など**副作用のあるスキルには必須**。

```yaml
disable-model-invocation: true
```

#### `argument-hint`（必要に応じて）
オートコンプリート時に表示されるヒント。引数を受け取るスキルに追加。

```yaml
argument-hint: [issue-number]
```

#### `context`（必要に応じて）
`fork`を指定するとサブエージェントとして独立実行。リサーチ系など重い処理に有用。

```yaml
context: fork
agent: Explore  # Explore, Plan, general-purpose などを指定
```

### 7. 検証

- [ ] name がディレクトリ名と一致しているか
- [ ] description が1024文字以内か
- [ ] description に「いつ使うか」が明記されているか
- [ ] description が十分に具体的か（他のスキルと混同されないか）
- [ ] SKILL.md が500行以内か
- [ ] 手順が明確で実行可能か
- [ ] 副作用のあるスキルに `disable-model-invocation: true` が設定されているか
- [ ] 頻繁に使うツールがあれば `allowed-tools` が設定されているか
