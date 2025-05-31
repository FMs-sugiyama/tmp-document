# ワークフロー修正テスト

## 修正内容
- `gh api` コマンドで `-F` フラグから `--raw-field` に変更
- JSONエスケープ問題を解決
- `--method POST` を明示的に指定

## 期待される結果
- 422エラーが解消される
- repository_dispatch が正常に送信される
- tmp-generator で受信確認ができる

## テスト実行時刻
2025-05-31 12:40 JST