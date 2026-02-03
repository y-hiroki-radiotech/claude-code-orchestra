# Claude Code Orchestra

**マルチエージェント協調フレームワーク**

Claude Code が Gemini CLI（大規模リサーチ）を統合し、各エージェントの強みを活かして開発を加速する。

---

## Why This Exists

| Agent | Strength | Use For |
|-------|----------|---------|
| **Claude Code** | オーケストレーション、ユーザー対話 | 全体統括、タスク管理、実装 |
| **Gemini CLI** | 1Mトークン、深い推論、マルチモーダル、Web検索 | 設計判断、デバッグ、コードベース分析、ライブラリ調査、PDF/動画処理 |

**IMPORTANT**: 単体では難しいタスクも、2エージェントの協調で解決できる。

---

## Context Management (CRITICAL)

Claude Code のコンテキストは **200k トークン** だが、ツール定義等で **実質 70-100k** に縮小する。

**YOU MUST** サブエージェント経由で Gemini を呼び出す（出力が10行以上の場合）。

| 出力サイズ | 方法 | 理由 |
|-----------|------|------|
| 1-2文 | 直接呼び出しOK | オーバーヘッド不要 |
| 10行以上 | **サブエージェント経由** | メインコンテキスト保護 |
| 分析レポート | サブエージェント → ファイル保存 | 詳細は `.claude/docs/` に永続化 |

```
# MUST: サブエージェント経由（大きな出力）
Task(subagent_type="general-purpose", prompt="Geminiでリサーチし、要約を返して")

# OK: 直接呼び出し（小さな出力のみ）
Bash("gemini -p ... '1文で答えて'")
```

---

## Quick Reference

### Gemini を使う時

- **設計判断**（「どう実装？」「どのパターン？」）
- **デバッグ**（「なぜ動かない？」「エラーの原因は？」）
- **比較検討**（「AとBどちらがいい？」）
- **リサーチ**（「調べて」「最新の情報は？」）
- **大規模分析**（「コードベース全体を理解して」）
- **マルチモーダル**（「このPDF/動画を見て」）

→ 詳細: `.claude/rules/gemini-delegation.md`

---

## Workflow

```
/startproject <機能名>
```

1. Gemini がリポジトリ分析（サブエージェント経由）
2. Claude が要件ヒアリング・計画作成
3. Gemini が計画レビュー・リスク分析（サブエージェント経由）
4. Claude がタスクリスト作成

→ 詳細: `/startproject`, `/plan`, `/tdd` skills

---

## Tech Stack

- **Python** / **uv** (pip禁止)
- **ruff** (lint/format) / **ty** (type check) / **pytest**
- `poe lint` / `poe test` / `poe all`

→ 詳細: `.claude/rules/dev-environment.md`

---

## Documentation

| Location | Content |
|----------|---------|
| `.claude/rules/` | コーディング・セキュリティ・言語ルール |
| `.claude/docs/DESIGN.md` | 設計決定の記録 |
| `.claude/docs/research/` | Gemini調査結果 |
| `.claude/logs/cli-tools.jsonl` | Gemini入出力ログ |

---

## Language Protocol

- **思考・コード**: 英語
- **ユーザー対話**: 日本語
