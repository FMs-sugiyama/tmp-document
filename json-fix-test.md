# JSON修正テスト

## 修正方法の変更
- フラグベースのJSON構築から、標準入力でのJSON送信に変更
- `cat <<EOF` heredocを使用してきれいなJSONを構築
- `--input -` で標準入力からJSONを読み込み

## 期待される結果
- JSONオブジェクトが正しく解析される
- 422エラーが解消される
- repository_dispatch が正常に送信される

## テスト実行
2025-05-31 12:50 JST