# Policy Repository - Vector Store Auto-Regeneration

このリポジトリは `team-mirai/policy` の代替テスト環境として、Vector Store の自動再生成機能をテストするために使用されます。

## 必要な環境設定一覧

### Repository Variables（必須）
- `FACT_CHECKER_REPO`: ターゲットリポジトリ名（例：`FMs-sugiyama/tmp-generator`）
  - **設定場所**: GitHub リポジトリの Settings → Secrets and variables → Actions → Variables タブ

### Repository Secrets（必須・設定済み）
- `FACT_CHECKER_PAT`: Fine-grained Personal Access Token（対象リポジトリへのアクセス権限付き）
  - **設定場所**: GitHub リポジトリの Settings → Secrets and variables → Actions → Secrets タブ

---

## セットアップ手順

### 1. GitHub Token の作成

**Fine-grained Personal Access Token を使用（推奨）**

1. GitHub.com → Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. **Generate new token** をクリック
3. 以下を設定：
   - **Token name**: `tmp-document-dispatch`
   - **Expiration**: 90 days（適切な期間）
   - **Resource owner**: `FMs-sugiyama` を選択
   - **Repository access**: **Selected repositories** を選択
   - **対象リポジトリ**: `FMs-sugiyama/tmp-generator` を明示的に追加
   - **Repository permissions**:
     - **Contents**: `Write`
     - **Metadata**: `Read`
4. **Generate token** をクリックしてトークンをコピー

### 2. Repository Variables の設定（🔴 新規設定が必要）

**`FACT_CHECKER_REPO` 変数の設定**:

1. https://github.com/FMs-sugiyama/tmp-document に移動
2. **Settings** タブをクリック
3. 左サイドバーの **Secrets and variables** → **Actions** をクリック
4. **Variables** タブをクリック（Secretsタブではありません）
5. **New repository variable** ボタンをクリック
6. 以下を入力：
   - **Name**: `FACT_CHECKER_REPO`
   - **Value**: `FMs-sugiyama/tmp-generator` （ターゲットリポジトリ名）
7. **Add variable** をクリック

この変数により、ワークフローが `repository_dispatch` を送信する先のリポジトリが決まります。

### 3. Repository Secret の設定（✅ 設定済み）

`FACT_CHECKER_PAT` シークレットは既に設定済みです。

参考：設定方法
1. https://github.com/FMs-sugiyama/tmp-document に移動
2. **Settings** タブをクリック
3. 左サイドバーの **Secrets and variables** → **Actions** をクリック
4. **Secrets** タブを選択
5. **New repository secret** ボタンをクリック
6. 以下を入力：
   - **Name**: `FACT_CHECKER_PAT`
   - **Secret**: 手順1で作成したトークン（Fine-grained PAT）
7. **Add secret** をクリック

### 4. ワークフローのテスト

Markdownファイルを追加・編集してpushすると、`.github/workflows/publish.yml` が自動実行され、`FACT_CHECKER_REPO` 変数で指定されたリポジトリに `repository_dispatch` イベントが送信されます。

## ワークフローの動作

1. `**/*.md` ファイルが変更されると `publish.yml` が実行
2. 変更されたファイルのリストを取得
3. `FACT_CHECKER_REPO` 変数で指定されたリポジトリに `repository_dispatch` イベントを送信
4. 対象リポジトリ側で Vector Store の再構築処理が開始

## トラブルシューティング

- **`GH_TOKEN` が空のエラー**: `FACT_CHECKER_PAT` シークレットが正しく設定されていることを確認
- **403 Forbidden エラー (Fine-grained token)**: 
  - `FMs-sugiyama/tmp-generator` が **Repository access** に明示的に追加されていることを確認
  - **Repository permissions** で `Actions: Write` が設定されていることを確認
  - トークンの有効期限が切れていないことを確認
- **API権限エラー**: 
  - Fine-grained token: `Actions: write` 権限が付与されていることを確認
  - Classic PAT: `repo` スコープが選択されていることを確認
- **リポジトリアクセスエラー**: トークンが `tmp-generator` リポジトリにアクセス権限を持っていることを確認

### よくある問題
- Fine-grained token で **Selected repositories** を選んだが、対象リポジトリを追加し忘れた
- **Repository permissions** の設定で `Actions` を `Write` にし忘れた