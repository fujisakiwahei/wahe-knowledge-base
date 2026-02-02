# NuxtプロジェクトにTailwindを導入

タイプ: フロー

## 利用シーン（あれば画像）

- タイトルの通り

## やること

1. 必要なパッケージのインストールyarn add -D @nuxtjs/tailwindcss を実行
2. nuxt.config.ts の設定modules に @nuxtjs/tailwindcss を追加：
    
    ```tsx
    export default defineNuxtConfig({
      modules: [
        '@nuxtjs/tailwindcss'
      ]
    })
    ```
    
3. tailwind.config.js の作成プロジェクトルートに以下の設定ファイルを作成：
    
    ```jsx
    /** @type {import('tailwindcss').Config} */
    module.exports = {
      content: [
        "./components/**/*.{js,vue,ts}",
        "./layouts/**/*.vue",
        "./pages/**/*.vue",
        "./plugins/**/*.{js,ts}",
        "./nuxt.config.{js,ts}",
        "./app.vue",
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    }
    ```
    
4. 開発サーバーの再起動設定変更を反映させるため npm run dev を実行し直す

## 注意点

その後は、Prettierの設定をしたい

[TailwindプロジェクトにPrettierを導入](Tailwind%E3%83%97%E3%83%AD%E3%82%B8%E3%82%A7%E3%82%AF%E3%83%88%E3%81%ABPrettier%E3%82%92%E5%B0%8E%E5%85%A5%20195df78c372e80c186a9f2836d70376d.md)