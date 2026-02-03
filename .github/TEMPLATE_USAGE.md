# テンプレート使用ガイド

## このテンプレートについて

Claude Code Orchestra は、Claude Code と Gemini CLI を統合したマルチエージェント開発環境のテンプレートです。

## 主な機能

- **マルチエージェント協調**: Claude と Gemini が連携して開発をサポート
- **コンテキスト管理**: サブエージェントパターンでメインコンテキストを節約
- **再利用可能なスキル**: プロジェクト開始、TDD、設計追跡などの標準ワークフロー
- **自動化フック**: lint、テスト、エージェントルーティングを自動化

## 使い方

### 1. テンプレートから新規リポジトリを作成

GitHubで「Use this template」ボタンをクリック

### 2. クローンして起動

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
cd YOUR_REPO_NAME
claude
```

### 3. プロジェクトをカスタマイズ

詳細は [SETUP.md](./SETUP.md) を参照してください。

## テンプレートの構成

```
.claude/
├── agents/          # サブエージェント定義
├── skills/          # 再利用可能なワークフロー
├── hooks/           # 自動化スクリプト
├── rules/           # 開発ガイドライン
└── docs/            # プロジェクトドキュメント

.gemini/             # Gemini CLI設定
CLAUDE.md            # システムドキュメント
```

## カスタマイズポイント

1. **CLAUDE.md** - プロジェクト固有のコンテキストを追加
2. **pyproject.toml** - プロジェクト名と依存関係を更新
3. **.claude/skills/** - カスタムスキルを追加
4. **.claude/rules/** - コーディングルールを調整

## サポート

- Issues: プロジェクトのIssuesタブで質問してください
- Discussions: 使い方の議論や提案

## ライセンス

MIT License - 詳細は LICENSE を参照
