# GitHubへのアップロード手順

## 1. GitHubで新しいリポジトリを作成

1. https://github.com/new にアクセス
2. リポジトリ名を入力（例: `claude-code-orchestra`）
3. Public/Private を選択
4. **「Add a README file」のチェックを外す**（既にREADMEがあるため）
5. **「Template repository」にチェックを入れる**（テンプレートとして使えるようにする）
6. Create repository をクリック

## 2. ローカルリポジトリをプッシュ

GitHubで表示される指示に従ってプッシュ：

```bash
# リモートリポジトリを追加
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# mainブランチにプッシュ
git branch -M main
git push -u origin main
```

または、既存のリポジトリの場合：

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

## 3. テンプレートリポジトリの設定を確認

リポジトリのSettings タブで以下を確認：

1. **General** セクション
   - 「Template repository」にチェックが入っているか確認

2. **Description** を追加
   ```
   Multi-Agent AI Development Environment with Claude Code & Gemini CLI
   ```

3. **Topics** を追加（検索性向上）
   ```
   claude-code, gemini, ai, development-environment, template,
   multi-agent, automation, python
   ```

## 4. リポジトリの説明を追加

リポジトリのトップページで「About」セクションの歯車アイコンをクリック：

- **Description**: `マルチエージェントAI開発環境テンプレート（Claude Code + Gemini CLI）`
- **Website**: （あれば）
- **Topics**: `claude-code`, `gemini`, `ai-development`, `template`

## 5. 完了確認

- [ ] リポジトリが公開されている
- [ ] 「Template repository」が有効
- [ ] README.md が表示されている
- [ ] 「Use this template」ボタンが表示されている

## テンプレートの使い方（他の人向け）

他の人がこのテンプレートを使う場合：

1. リポジトリページの「Use this template」ボタンをクリック
2. 新しいリポジトリ名を入力
3. クローンして開発開始

```bash
git clone https://github.com/THEIR_USERNAME/THEIR_REPO_NAME.git
cd THEIR_REPO_NAME
claude
```

## トラブルシューティング

### プッシュに失敗する場合

```bash
# リモートURLを確認
git remote -v

# URLが間違っている場合は削除して再追加
git remote remove origin
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
git push -u origin main
```

### 「Template repository」が表示されない

Settings > General > Template repository にチェックを入れてください。

## 次のステップ

1. リポジトリの説明を充実させる
2. Issues テンプレートを追加（.github/ISSUE_TEMPLATE/）
3. Pull Request テンプレートを追加（.github/pull_request_template.md）
4. GitHub Actions でCI/CDを設定（オプション）
