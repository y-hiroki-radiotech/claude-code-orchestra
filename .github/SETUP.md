# セットアップガイド

このテンプレートを使い始めるためのガイドです。

## 1. Prerequisites

### Claude Code のインストール

```bash
npm install -g @anthropic-ai/claude-code
claude login
```

### Gemini CLI のインストール

```bash
npm install -g @google/gemini-cli
gemini login
```

## 2. プロジェクトのカスタマイズ

### 2.1 CLAUDE.md の更新

`CLAUDE.md` の以下のセクションをプロジェクトに合わせて更新してください：

```markdown
## Current Project: {プロジェクト名}

### Context
- Goal: {プロジェクトの目的}
- Key files: {重要なファイル一覧}
- Dependencies: {主要な依存関係}

### Decisions
- {決定事項1}: {理由}
```

### 2.2 pyproject.toml の更新

プロジェクト名と依存関係を更新：

```toml
[project]
name = "your-project-name"
version = "0.1.0"
description = "Your project description"
```

### 2.3 不要なファイルの削除

テンプレート固有のファイルを削除：

```bash
rm summary.png  # サンプル画像
rm .github/SETUP.md  # このファイル
```

## 3. Git の初期化

```bash
# 既存の履歴をクリア（オプション）
rm -rf .git
git init
git add .
git commit -m "Initial commit from claude-code-orchestra template"

# リモートリポジトリを設定
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

## 4. 最初のスキルを実行

```bash
# Claude Code を起動
claude

# プロジェクト開始
/startproject {機能名}
```

## 5. カスタマイズのヒント

### スキルの追加

`.claude/skills/` ディレクトリに新しいスキルを追加できます：

```bash
mkdir .claude/skills/my-custom-skill
# SKILL.md を作成
```

### ルールの調整

`.claude/rules/` でコーディングルールをカスタマイズ：

- `coding-principles.md` - コーディング原則
- `testing.md` - テスト戦略
- `security.md` - セキュリティガイドライン

### フックの有効化/無効化

`.claude/settings.json` でフックを調整できます。

## トラブルシューティング

### Gemini CLI が見つからない

```bash
which gemini
# パスが表示されない場合は再インストール
npm install -g @google/gemini-cli
```

### フックが動作しない

```bash
# Pythonが利用可能か確認
python3 --version

# フックのパーミッションを確認
chmod +x .claude/hooks/*.py
```

## 次のステップ

1. プロジェクトの要件を整理
2. `/startproject` で開発を開始
3. `/design-tracker` で設計決定を記録
4. `/checkpointing` で定期的にセッションを保存
