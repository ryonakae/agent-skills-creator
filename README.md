# Agent Skills Creator

[Agent Skills](https://agentskills.io) 標準に準拠したスキルを作成するためのプロジェクト。

## ディレクトリ構成

```
agent-skills-creator/
├── docs/                    # 参照ドキュメント
│   ├── agent-skills/        # Agent Skills 標準仕様
│   ├── claude-code/         # Claude Code のスキル拡張機能
│   ├── gemini-cli/          # Gemini CLI の Agent Skills
│   └── openai-codex/        # OpenAI Codex の Agent Skills
├── dist/                    # 生成されたスキルの出力先
└── .claude/commands/        # Claude Code カスタムコマンド
```

## 使い方

### スキルを作成する

```
/create-skill <作りたいスキルの説明>
```

例:
```
/create-skill PDFファイルからテキストを抽出するスキル
/create-skill コードレビューを行い、改善点を指摘するスキル
```

コマンドを実行すると、`docs/` 以下のドキュメントを参照しながら `dist/` ディレクトリにスキルを出力する。
