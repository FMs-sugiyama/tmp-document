# Heredoc JSON 修正テスト

## 修正内容確認
- `cat <<EOF` を使用したJSON構築
- `--input -` での標準入力からの読み込み
- 適切なJSONオブジェクト形式での送信

## 検証項目
1. JSONパース エラー（422）が解消されるか
2. repository_dispatch が tmp-generator に正常送信されるか
3. client_payload が正しい形式で受信されるか

## 実行ログ確認ポイント
- JSON文字列がオブジェクトとして認識される
- エラーログに "is not an object" が出ない
- HTTP 200/201 で成功する

## 作成日時
2025-05-31 12:55 JST - Heredoc修正後テスト