# microCMS / Nuxt 4 / Tailwind CSS v4 セットアップマニュアル

## 前提条件

- Node.js 20以上
- npm または pnpm
- microCMSアカウント
- GitHubアカウント

---

## 1. Git リポジトリの準備

### 1-1. GitHub でリポジトリを作成（GUI）

1. GitHub にログイン
2. 「New repository」をクリック
3. リポジトリ名を入力
4. **「Add a README file」のチェックを外す**（空のリポジトリにする）
5. 「Create repository」をクリック
6. 表示されるリポジトリURLをコピーしておく

> **ポイント**: READMEなしで作成することで、ローカルのNuxtプロジェクトと競合しない

---

## 2. Nuxt 4 プロジェクトの初期化

### 2-1. ローカルでプロジェクト作成

```bash
npx nuxi@latest init プロジェクト名
cd プロジェクト名
```

初期化時に「Nuxt 4」を選択。または `nuxt.config.ts` に以下を追加：

```typescript
export default defineNuxtConfig({
  future: { compatibilityVersion: 4 },
  compatibilityDate: '2025-07-15',
})
```

### 2-2. Node.js型定義の追加

```bash
npm i -D @types/node
```

### 2-3. .gitignore の確認・作成

プロジェクトルートの `.gitignore`:

```gitignore
# Nuxt dev/build outputs
.output
.data
.nuxt
.nitro
.cache

# Node dependencies
node_modules

# Logs
logs
*.log

# Misc
.DS_Store
.fleet
.idea

# Local env files
.env
.env.*
!.env.example

# VS Code / Cursor
.vscode/*
!.vscode/extensions.json
```

### 2-4. GitHub リポジトリと接続 & 初回コミット

```bash
git init
git add .
git commit -m "Initial commit: Nuxt 4 project setup"
git branch -M main
git remote add origin https://github.com/ユーザー名/リポジトリ名.git
git push -u origin main
```

---

## 3. Tailwind CSS v4 のセットアップ

### 3-1. パッケージのインストール

```bash
npm i tailwindcss @tailwindcss/vite
npm install -D sass-embedded
```

> **重要**: Tailwind v4 では `@nuxtjs/tailwindcss` モジュールは不要。Viteプラグインを直接使用する。

### 3-2. nuxt.config.ts の設定

```typescript
import tailwindcss from '@tailwindcss/vite'

export default defineNuxtConfig({
  future: { compatibilityVersion: 4 },
  compatibilityDate: '2025-07-15',
  devtools: { enabled: true },

  css: ['~/assets/css/main.css'],

  vite: {
    plugins: [tailwindcss() as any],
  },
})
```

### 3-3. main.css の作成

```bash
mkdir -p app/assets/css
touch app/assets/css/main.css
```

`app/assets/css/main.css`:

```css
@import 'tailwindcss';

@theme {
  /* カスタムフォント */
  --font-custom: 'Your Font', sans-serif;
  --font-sans: var(--font-custom), ui-sans-serif, system-ui, sans-serif;

  /* カスタムカラー */
  --color-primary: #000000;
  --color-secondary: #ffffff;
}

body {
  font-family: var(--font-sans);
  line-height: 1.8;
}
```

### 3-4. @theme とは？（Tailwind v4 の新機能）

Tailwind CSS v4 では、従来の `tailwind.config.js` が廃止され、**CSS内で直接カスタマイズ**する方式に変更された。

| 項目 | Tailwind v3（旧） | Tailwind v4（新） |
|------|------------------|------------------|
| 設定ファイル | `tailwind.config.js` | 不要 |
| カスタマイズ | JSで記述 | CSS内の `@theme` で記述 |
| インポート | `@tailwind base/components/utilities` | `@import 'tailwindcss'` |

**`@theme` で定義できるもの:**
- `--font-*`: フォントファミリー
- `--color-*`: カラーパレット
- `--spacing-*`: スペーシング
- `--tracking-*`: 文字間隔
- `--leading-*`: 行間
- など

定義した値は自動的にユーティリティクラスとして使用可能：

```css
@theme {
  --color-brand: #ff6600;
}
```

↓ 自動的に使えるようになる

```html
<div class="bg-brand text-brand">...</div>
```

---

## 4. Prettier の設定

### 4-1. パッケージのインストール

```bash
npm i -D prettier prettier-plugin-tailwindcss
```

### 4-2. .prettierrc の作成（プロジェクトルート）

**プロジェクトルート**に `.prettierrc` を作成：

```
プロジェクト/
├── .prettierrc  ← ここ
├── app/
├── nuxt.config.ts
└── package.json
```

`.prettierrc`:

```json
{
  "semi": false,
  "singleQuote": true,
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### 4-3. 保存時の自動フォーマット設定（VSCode / Cursor）

プロジェクトルートに `.vscode/settings.json` を作成：

```bash
mkdir -p .vscode
touch .vscode/settings.json
```

`.vscode/settings.json`:

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[vue]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

> **必要な拡張機能**: [Prettier - Code formatter](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) をインストールしておくこと

### 4-4. ESLint の設定（任意）

Lintも保存時に自動実行したい場合は、ESLintを追加：

```bash
npm i -D eslint @nuxt/eslint
```

`nuxt.config.ts` に追加：

```typescript
export default defineNuxtConfig({
  modules: ['@nuxt/eslint'],
})
```

`.vscode/settings.json` に追加：

```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": "explicit"
  }
}
```

これで**保存時に Prettier でフォーマット + ESLint で自動修正**が実行される。

---

## 5. microCMS のセットアップ

### 5-1. microCMS側の準備

1. microCMSでサービスを作成
2. 必要なAPIを作成（例: `works`, `blog`）
3. 管理画面から **サービスドメイン** と **APIキー** を取得

### 5-2. モジュールのインストール

```bash
npm i nuxt-microcms-module
```

### 5-3. 環境変数の設定

プロジェクトルートに `.env` を作成：

```env
MICROCMS_SERVICE_DOMAIN=your-service-domain
MICROCMS_API_KEY=your-api-key
```

チーム用に `.env.example` も作成（こちらはコミットする）：

```env
MICROCMS_SERVICE_DOMAIN=your-service-domain
MICROCMS_API_KEY=your-api-key
```

### 5-4. nuxt.config.ts に追加

```typescript
export default defineNuxtConfig({
  // ...既存の設定

  modules: ['nuxt-microcms-module'],

  microCMS: {
    serviceDomain: process.env.MICROCMS_SERVICE_DOMAIN,
    apiKey: process.env.MICROCMS_API_KEY,
  },
})
```

### 5-5. 型定義ファイルの作成

プロジェクトルートに `types/` ディレクトリを作成し、APIごとに型を定義：

```bash
mkdir types
```

`types/works.ts`:

```typescript
import type { MicroCMSImage, MicroCMSListContent } from 'microcms-js-sdk'
import type { WorksCategory } from './worksCategory'

export type Works = {
  title?: string
  content?: string
  eyecatch?: MicroCMSImage
  category: (MicroCMSListContent & WorksCategory) | null
}
```

`types/worksCategory.ts`:

```typescript
export type WorksCategory = {
  name?: string
}
```

---

## 6. ページの作成

### 6-1. app.vue の設定

`app/app.vue`:

```vue
<template>
  <ClientOnly>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </ClientOnly>
</template>
```

### 6-2. 一覧ページの作成

`app/pages/works/index.vue`:

```vue
<script setup lang="ts">
import type { Works } from '~/types/works'

const { data: worksData } = await useMicroCMSGetList<Works>({
  endpoint: 'works',
})
</script>

<template>
  <div>
    <h1>Works</h1>
    <ul v-if="worksData">
      <li v-for="work in worksData.contents" :key="work.id">
        <NuxtLink :to="`/works/${work.id}`">
          {{ work.title }}
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>
```

### 6-3. 詳細ページの作成

`app/pages/works/[id].vue`:

```vue
<script setup lang="ts">
import type { Works } from '~/types/works'

const route = useRoute()
const id = route.params.id as string

const { data: workData } = await useMicroCMSGetListDetail<Works>({
  endpoint: 'works',
  contentId: id,
})
</script>

<template>
  <article v-if="workData">
    <h1>{{ workData.title }}</h1>
    <div v-html="workData.content" />
  </article>
</template>
```

---

## 7. ディレクトリ構成（完成形）

```
プロジェクト/
├── .vscode/
│   └── settings.json      ← 自動フォーマット設定
├── app/
│   ├── assets/
│   │   └── css/
│   │       └── main.css   ← Tailwind + @theme
│   ├── components/
│   ├── layouts/
│   ├── pages/
│   │   ├── index.vue
│   │   ├── works/
│   │   │   ├── index.vue
│   │   │   └── [id].vue
│   │   └── blog/
│   │       ├── index.vue
│   │       └── [id].vue
│   └── app.vue
├── types/
│   ├── works.ts
│   └── worksCategory.ts
├── public/
├── .env                   ← Git除外
├── .env.example           ← コミットする
├── .gitignore
├── .prettierrc            ← ルートに配置
├── nuxt.config.ts
├── package.json
└── tsconfig.json
```

---

## 8. 開発サーバーの起動

```bash
npm run dev
```


---

## 9. レイアウトの設定（ヘッダー・フッター）

### 9-1. レイアウトファイルの作成

`app/layouts/default.vue`:

```vue
<template>
  <div class="flex min-h-screen flex-col">
    <NuxtHeader />
    <main class="flex-1">
      <slot />
    </main>
    <NuxtFooter />
  </div>
</template>
```

### 9-2. コンポーネントの作成

```bash
mkdir -p app/components
touch app/components/NuxtHeader.vue
touch app/components/NuxtFooter.vue
```

`app/components/NuxtHeader.vue`:

```vue
<template>
  <header class="border-b bg-white">
    <nav class="container mx-auto flex items-center justify-between px-4 py-4">
      <NuxtLink to="/" class="text-xl font-bold">
        LOGO
      </NuxtLink>
      <ul class="flex gap-8">
        <li><NuxtLink to="/works">Works</NuxtLink></li>
        <li><NuxtLink to="/blog">Blog</NuxtLink></li>
        <li><NuxtLink to="/services">Services</NuxtLink></li>
        <li><NuxtLink to="/contact">Contact</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>
```

`app/components/NuxtFooter.vue`:

```vue
<template>
  <footer class="bg-gray-900 text-white">
    <div class="container mx-auto px-4 py-8">
      <p class="text-center">© 2026 Your Company</p>
    </div>
  </footer>
</template>
```

### 9-3. 仕組み

| ファイル | 役割 |
|---------|------|
| `app/app.vue` | `<NuxtLayout>` でレイアウトを読み込む |
| `app/layouts/default.vue` | 共通レイアウト（ヘッダー・フッター・slot） |
| `app/components/NuxtHeader.vue` | ヘッダーコンポーネント |
| `app/components/NuxtFooter.vue` | フッターコンポーネント |

> **ポイント**: `components/` 内のファイルは自動インポートされるため、`import` 文は不要

### 9-4. 特定ページで別レイアウトを使う場合

`app/layouts/home.vue` など別レイアウトを作成し、ページで指定：

```vue
<script setup lang="ts">
definePageMeta({
  layout: 'home'
})
</script>
```


---

## よくあるトラブルと対処法

| 問題 | 原因 | 対処法 |
|------|------|--------|
| 保存時にフォーマットされない | Prettier拡張機能が未インストール | VSCode/Cursorで「Prettier - Code formatter」をインストール |
| Tailwindのクラスが効かない | Viteプラグイン未設定 | `@tailwindcss/vite` をインストールし、`nuxt.config.ts` に追加 |
| microCMSのデータが取得できない | 環境変数の読み込みエラー | `.env` のキー名を確認、サーバー再起動 |
| 型エラーが出る | `import type` を使っていない | `import type { ... }` に修正 |
