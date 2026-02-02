# Nuxt 3 & Tailwind CSS セットアップマニュアル 📖

タイプ: スニペット, フロー
技術・ツール: Nuxt, Tailwind

このマニュアルでは、Nuxt 3プロジェクトをゼロから立ち上げ、Tailwind CSSを導入して開発を始めるまでの手順を、ステップバイステップで解説します。

### 1. 🚀 プロジェクトの新規作成

まず、Nuxt 3プロジェクトの雛形を作成します。

1. ターミナルを開き、好きな場所に移動して以下のコマンドを実行します。`<project-name>`はあなたのプロジェクト名に置き換えてください。Bash
    
    `npx nuxi@latest init <project-name>`
    
2. 作成されたプロジェクトのディレクトリに移動し、基本的な依存パッケージをインストールします。Bash
    
    `cd <project-name>
    npm install`
    

---

### 2. 🎨 Tailwind CSSモジュールの導入

次に、Nuxt 3と連携するための公式Tailwind CSSモジュールをインストールします。

1. プロジェクトのルートディレクトリで、以下のコマンドを実行します。Bash
    
    `npm install -D @nuxtjs/tailwindcss`
    
    - `D` は、このパッケージが開発時にのみ必要な「開発用依存関係」であることを示します。

---

### 3. ⚙️ Nuxtの設定

インストールしたモジュールをNuxtに読み込ませます。また、今回はSPA（シングルページアプリケーション）として動作させます。

1. プロジェクトのルートにある `nuxt.config.ts` ファイルを開き、以下のように編集します。
    
    ```jsx
    // nuxt.config.ts
    
    export default defineNuxtConfig({
      // SSRモードを有効にする
      ssr: true,
    
      // Tailwind CSSモジュールを追加
      modules: ["@nuxtjs/tailwindcss"],
      
      // CSSファイルを読み込み
      css: ["~/assets/css/main.css"],
    
      // 開発ツールを有効にする
      devtools: { enabled: true },
    });
    ```
    
    **ポイント**: `@nuxtjs/tailwindcss`モジュールが必要なCSSを自動で読み込むため、`css: [...]`という記述を追加する必要はありません。
    

---

### 4. 📝 Tailwind CSS設定ファイルの生成

Tailwind CSS自体の設定ファイル (`tailwind.config.js`) を作成します。

1. ターミナルで以下のコマンドを実行します。Bash
    
    `npx tailwindcss init`
    
    **ポイント**: プロジェクト内にインストールしたコマンドを実行するため、必ず`npx`を先頭に付けます。
    
2. 上記コマンドにより、プロジェクトのルートに `tailwind.config.js` が作成されます。このファイルで、Tailwind CSSを適用するファイルの範囲を指定します。JavaScript
    
    ```jsx
    // tailwind.config.js
    
    /** @type {import('tailwindcss').Config} */
    export default {
      content: [
        "./components/**/*.{js,vue,ts}",
        "./layouts/**/*.vue",
        "./pages/**/*.vue",
        "./plugins/**/*.{js,ts}",
        "./app.vue",
        "./error.vue",
      ],
      theme: {
        extend: {},
      },
      plugins: [],
    };
    ```
    
3. `app/assets/css/main.css` を作成

```scss
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

### 5. 🏠 メインファイルの編集と動作確認

Tailwind CSSのクラスが適用されるか確認するために、アプリケーションのエントリーポイントである `app.vue` を編集します。

1. **プロジェクトのルートディレクトリ**にある `app.vue` を開きます。（`app`フォルダの中ではなく、`nuxt.config.ts` と同じ階層です）
2. 中身を一度空にして、以下のように書き換えます。Tailwind CSSのクラスをいくつか使ってスタイルを当ててみましょう。

```jsx
<template>
  <div class="bg-slate-900 text-white min-h-screen flex items-center justify-center">
    <h1 class="text-4xl font-bold">
      Hello, Nuxt 3 with Tailwind CSS! 👋
    </h1>
  </div>
</template>
```

---

### 6. 🏃‍♂️ 開発サーバーの起動

全ての準備が整いました。開発サーバーを起動して、ブラウザで表示を確認します。

1. ターミナルで以下のコマンドを実行します。Bash
    
    `npm run dev`
    
2. ターミナルに表示されたURL（通常は `http://localhost:3000`）をブラウザで開きます。

黒い背景に白い大きな文字で「Hello, Nuxt 3 with Tailwind CSS! 👋」と表示されれば、セットアップは成功です！

---

### **7. 💅 Tailwind CSSクラスの自動整形**

開発を進めていくと、class属性に多くのクラスが並び、順序がバラバラになりがちです。ここでは、コード保存時にTailwind CSSのクラスを自動で推奨順に並び替える設定を追加し、開発体験を向上させましょう。

1. **必要なパッケージをインストール**

コードフォーマッターのPrettierと、そのTailwind CSS用プラグインをインストールします。

`npm install -D prettier prettier-plugin-tailwindcss`

1. **Prettierの設定ファイルを作成**

プロジェクトのルートに、Prettierの設定ファイル .prettierrc.cjs を作成し、プラグインを有効にします。

- ファイル名: .prettierrc.cjs
- 内容:
    
    ```jsx
    module.exports = {
    	plugins: ["prettier-plugin-tailwindcss"],
    };
    ```
    
1. **VS Codeで保存時に自動フォーマット**

最後に、ファイルを保存するたびに自動で整形が実行されるよう、VS Codeを設定します。

**ポイント**: この設定には、VS Codeの拡張機能「Prettier - Code formatter」が必要です。あらかじめインストールしておいてください。

1. プロジェクトのルートに .vscode というフォルダを作成します。
2. その中に settings.json というファイルを作成し、以下の内容を記述します。
- ファイルパス: .vscode/settings.json
- 内容:json
    
    ```jsx
    {
      "editor.formatOnSave": true,
      "editor.defaultFormatter": "esbenp.prettier-vscode",
      "[vue]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      },
      "[typescript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
      }
    }
    ```
    

**ポイント**: Vueファイル (.vue) などで言語別のフォーマッター（Volarなど）と競合するのを防ぐため、ファイルタイプごとにPrettierを指定しています。