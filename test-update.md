# テスト更新文書

## トークン設定後のテスト

Fine-grained Personal Access Token を正しく設定した後のテストです。

### 設定内容
- Repository access: Selected repositories
- 対象: FMs-sugiyama/tmp-generator
- Contents: Write
- Metadata: Read

### 期待される動作
1. この文書の追加により publish.yml が実行される
2. tmp-generator に repository_dispatch が送信される
3. 403エラーが解消される

## 作成日時
2025-05-31 12:30 JST