# microCMS / Nuxt / Tailwindのセットアップ

タイプ: フロー

## 利用シーン（あれば画像）

- タイトルの通り

## やること

1. GitHubリポジトリの作成
2. 要件定義のテンプレートをコピペして埋める
3. NuxtとmicroCMSのセットアップ
    1. https://blog.microcms.io/nuxt3-jamstack-blog/
        1. Nuxtのセットアップ
        2. microCMSの準備
        3. `.env` にAPIキーの記述
        4. microCMSモジュールの導入 
        5. microCMSで取得できるコンテンツの型情報を作成
            1. APIの分だけ作成
        6. 注意事項
            1. `import type { Work } from "~/types/work";` のように、typeを明示して読み込む必要あり
            2. ドキュメントとは異なり、`pages/works/[id].vue` としているのでリンクも要調整
    2. `app.vue`は以下にする
        
        ```jsx
        <template>
          <ClientOnly>
            <NuxtLayout>
              <NuxtPage />
            </NuxtLayout>
          </ClientOnly>
        </template>
        ```
        
    3. アーカイブページを作る
        1. `/pages/{api名}.vue` を作成
        2. https://arc.net/l/quote/ofpfnhzk
        3. APIの分だけ作成
    4. 詳細ページを作る
        1. `pages/works/[{api名}].vue` を作成
4. Tailwindの導入
    1. https://arc.net/l/quote/zzamouhf
    2. talwind Prettierの導入
        1. **必要なパッケージのインストール**:
            
            ```bash
            npm install -D prettier prettier-plugin-tailwindcss
            ```
            
        2. **Prettierの設定ファイルの作成**:
            
            ```json
            {
              "semi": false,
              "singleQuote": true,
              "plugins": ["prettier-plugin-tailwindcss"]
            }
            
            ```
            
        3. **package.jsonにフォーマットスクリプトを追加**（オプション）:
            
            ```json
            {
              "scripts": {
                "format": "prettier --write ."
              }
            }
            
            ```
            
5. main.cssの作成→フォント指定まで
    1. `main.css` を作成
        
        ```bash
        mkdir -p assets/css
        touch assets/css/main.css
        ```
        
    2. `main.css` にグローバルスタイルを記述。フォントもここで指定
        
        ```css
        @tailwind base;
        @tailwind components;
        @tailwind utilities;
        
        /* グローバルスタイル */
        body {
          font-family:
          color:
          /* など */
        }
        ```
        
    3. `nuxt.config.js` にCSSファイルを登録
        
        ```jsx
          css: [
            '@/assets/css/main.css'
          ]
        ```