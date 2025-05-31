# Policy Repository - Vector Store Auto-Regeneration

このリポジトリは `team-mirai/policy` の代替テスト環境として、Vector Store の自動再生成機能をテストするために使用されます。

## セットアップ手順

### 1. GitHub Token の作成

**推奨: Fine-grained Personal Access Token を使用**

1. GitHub.com → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. **Generate new token** をクリック
3. 以下を設定：
   - **Resource owner**: `FMs-sugiyama`
   - **Repository access**: `tmp-generator` を選択
   - **Permissions**: `Actions: write` を付与
4. トークンを生成してコピー

**代替案: Classic Personal Access Token**

1. GitHub.com → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. **Generate new token (classic)** をクリック
3. **Scopes**: `repo` を選択
4. トークンを生成してコピー

※ Fine-grained token の方がセキュリティ上推奨されます

### 2. Repository Secret の設定

1. https://github.com/FMs-sugiyama/tmp-document に移動
2. **Settings** タブをクリック
3. 左サイドバーの **Secrets and variables** → **Actions** をクリック
4. **New repository secret** ボタンをクリック
5. 以下を入力：
   - **Name**: `FACT_CHECKER_PAT`
   - **Secret**: 手順1で作成したトークン（Fine-grained または Classic PAT）
6. **Add secret** をクリック

### 3. ワークフローのテスト

Markdownファイルを追加・編集してpushすると、`.github/workflows/publish.yml` が自動実行され、`tmp-generator` リポジトリに `repository_dispatch` イベントが送信されます。

## ワークフローの動作

1. `**/*.md` ファイルが変更されると `publish.yml` が実行
2. 変更されたファイルのリストを取得
3. `FMs-sugiyama/tmp-generator` に `repository_dispatch` イベントを送信
4. `tmp-generator` 側で Vector Store の再構築処理が開始

## トラブルシューティング

- **`GH_TOKEN` が空のエラー**: `FACT_CHECKER_PAT` シークレットが正しく設定されていることを確認
- **API権限エラー**: 
  - Fine-grained token: `Actions: write` 権限が付与されていることを確認
  - Classic PAT: `repo` スコープが選択されていることを確認
- **リポジトリアクセスエラー**: トークンが `tmp-generator` リポジトリにアクセス権限を持っていることを確認