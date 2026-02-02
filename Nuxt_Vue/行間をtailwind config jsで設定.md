# 行間をtailwind.config.jsで設定

タイプ: スニペット

## 利用シーン（あれば画像）

- タイトル通り
- `main.css`で行間を設定しても、`text-sm`などの中にある`line-height`に上書きされてしまう。

## コード

```jsx
module.exports = {
  theme: {
    extend: {
      fontSize: {
        sm: ['0.875rem', { lineHeight: '1.6' }], // デフォルトのline-heightを変更
        base: ['1rem', { lineHeight: '1.6' }],
        lg: ['1.125rem', { lineHeight: '1.6' }],
        // 他のサイズも必要に応じて設定
      },
    },
  },
}
```

## 注意点