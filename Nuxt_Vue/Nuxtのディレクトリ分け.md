# Nuxtのディレクトリ分け

技術・ツール: Nuxt

```scss
my-nuxt-app/
├─ app.vue          ← アプリ全体のエントリポイント
│                    （ここに <NuxtLayout><NuxtPage/></NuxtLayout> を置く）
│
├─ components/      ← 小さな部品を置く
│   ├─ AppHeader.vue   ヘッダー
│   └─ AppFooter.vue   フッター
│
├─ layouts/         ← ページ全体のテンプレート
│   └─ default.vue     全ページ共通（ヘッダー/フッターをここに配置）
│                      definePageMeta({ layout: "..." }) で切替可能
│
└─ pages/           ← 実際のページ（ルーティングに対応）
    ├─ index.vue       →  /
    ├─ about.vue       →  /about
    └─ posts/
        ├─ index.vue   →  /posts
        └─ [slug].vue  →  /posts/:slug
```

1. Layoutsでテンプレートを作る
2. appにLayoutsを読み込む

---

- **app.vue**
    
    Nuxtアプリの入り口。ここで <NuxtLayout> と <NuxtPage> を描画するだけ。
    
    （基本的にここに UI は直接書かない）
    
- **components/**
    
    ページやレイアウトに組み込む「部品」。
    
    例: AppHeader.vue, AppFooter.vue, ボタン、カードなど。
    
- **layouts/**
    
    ページ全体の共通枠。
    
    例: default.vue にヘッダー＆フッターを入れて「どのページでも共通UI」を持たせる。
    
    ページごとに definePageMeta({ layout: '...' }) で切替可能。
    
- **pages/**
    
    実際のページ。ファイルやディレクトリ構造がそのまま URL ルーティングに変換される。
    
    例: pages/posts/[slug].vue → /posts/任意のslug