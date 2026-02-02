# 【BrowserSync】WordPressのローカル環境をホットリロードさせる方法

タイプ: フロー

## 利用シーン（あれば画像）

- WordPressのローカル環境をホットリロードさせる。開発効率UP

## やること

以下記事の通り

https://fukulog.net/browsersync-wordpress/

## メモ

### 手順

```bash
npm init -y
```

```bash
npm i -D browser-sync
```

```bash
npx browser-sync init
```

---

```
module.exports = {
  proxy: "poweron.local", // localで設定したURL
  files: ["*.css", "**/*.css", "assets/scss/*.scss", "assets/scss/**/*.scss", "assets/js/*.js", "**/*.php", "*.php", "*.html"],
};

```

---

```
"browser": "browser-sync start --config bs-config.js"
```

---

```
npm run browser
```

```
module.exports = {
  proxy: "poweron.local", // localで設定したURL
  files: ["*.css", "**/*.css", "assets/scss/*.scss", "assets/scss/**/*.scss", "assets/js/*.js", "**/*.php", "*.php", "*.html"],
};

```