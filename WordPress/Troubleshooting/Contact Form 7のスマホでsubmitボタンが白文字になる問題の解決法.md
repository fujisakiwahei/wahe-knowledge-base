# Contact Form 7のスマホでsubmitボタンが白文字になる問題の解決法

タイプ: トラブルシューティング

## 🔧 解決方法

```css
.wpcf7-submit {
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    color: ${カラーコード} !important;
}

```

## 📝 原因

- スマホブラウザが独自のスタイルを強制適用
- `webkit-appearance` によるOSネイティブスタイルの優先
- PCの検証ツールでは再現されない

## 🎯 ポイント

- `webkit-appearance: none` でブラウザの既定スタイルをリセットすることが重要！

解決おめでとうございます！🎉