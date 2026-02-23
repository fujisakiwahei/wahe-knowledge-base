# microCMS データ取得マニュアル 

このプロジェクトで microCMS からデータを取得する際の手順と、トラブルシューティングのガイドです。

## 1. コンポーザブルの作成 (`app/composables/`)

新しいエンドポイント（例: `news`）を追加する場合は、`useMicroCMSData.ts` に関数を追加します。

```typescript
// app/composables/useMicroCMSData.ts

export const useNews = (queries: MicroCMSQueries = {}) => {
  return useMicroCMSGetList<News>({
    endpoint: 'news', // microCMSの「API識別子」と一致させる
    queries: {
      depth: 2, // 参照フィールド（カテゴリ等）がある場合は必須
      ...queries,
    },
  })
}
```

## 2. 型定義の作成 (`types/`)

取得するデータの構造を TypeScript で定義します。

```typescript
// types/news.ts

export type News = {
  title: string
  content: string
  thumbnail?: MicroCMSImage
  category: (MicroCMSListContent & Category)[] | null
}
```

## 3. ページでの利用 (`app/pages/`)

コンポーザブルを呼び出してデータを受け取ります。

```vue
<script setup lang="ts">
// 1. 基本的な取得（例：最新5件）
const { data: newsData } = useNews({ limit: 5 })

// 2. 特定の記事を取得（詳細ページなど）
const route = useRoute()
const { data: article } = useNewsDetail(route.params.id as string)

// 3. データの読み込み状態やエラーを扱う場合
const { data, pending, error, refresh } = useNews({ limit: 10 })
</script>

<template>
  <!-- 読み込み中 -->
  <div v-if="pending">Loading...</div>

  <!-- エラー発生時 -->
  <div v-else-if="error">データの取得に失敗しました</div>

  <!-- データ表示 -->
  <div v-else>
    <div v-for="post in data?.contents" :key="post.id">
      <h2>{{ post.title }}</h2>
      <div v-html="post.content"></div>
    </div>
  </div>
</template>
```

## 4. コンポーザブルの共通パターン

新しいプロジェクトでコンポーザブルを作成する際は、以下のパターンをコピーして使用してください。

```typescript
// app/composables/useMicroCMSData.ts

/**
 * リスト取得用 (一覧)
 */
export const use[EntityName] = (queries: MicroCMSQueries = {}) => {
  return useMicroCMSGetList<[TypeName]>({
    endpoint: '[endpoint_name]',
    queries: {
      depth: 2, // 必須：参照フィールドを解決するため
      ...queries,
    },
  })
}

/**
 * 詳細取得用 (個別)
 */
export const use[EntityName]Detail = (
  contentId: string,
  queries: MicroCMSQueries = {},
) => {
  return useMicroCMSGetListDetail<[TypeName]>({
    endpoint: '[endpoint_name]',
    contentId,
    queries: {
      depth: 2,
      ...queries,
    },
  })
}
```

---

## トラブルシューティング・チェックリスト

データが `undefined` になったり、正しく表示されない場合は以下を確認してください。

### ① API識別子（エンドポイント名）の確認
- `endpoint: 'works'` の名前が microCMS 管理画面の **「API識別子」** と完全に一致しているか。
- フィールド名（例: `categories`, `lead`）が **「フィールドID」** と一致しているか。

### ② `depth` パラメータ（階層の深さ）
- **症状**: カテゴリ名などが `undefined` になる、または ID しか取れない。
- **対策**: クエリに `depth: 2` を設定してください。コンテンツ参照（複数選択含む）を使用している場合は必須です。

### ③ `v-html` の使用
- **症状**: 画面に `<p>...</p>` などのタグがそのまま文字列として表示される。
- **対策**: `{{ }}` ではなく `v-html="post.content"` を使用してください。

### ④ 下書き・公開状態
- **症状**: 特定の記事だけが表示されない。
- **対策**: 
  - 記事が「公開」状態になっているか。
  - 参照しているカテゴリ等の記事自体も「公開」されているか。

### ⑤ ブラウザでの直接確認
- ブラウザの **デベロッパーツール > Networkタブ** を開き、`works?depth=2...` のようなリクエストを探します。
- **Response** を見て、JSONの中に期待するデータが含まれているか確認してください。ここになければ microCMS 側の設定ミスです。
