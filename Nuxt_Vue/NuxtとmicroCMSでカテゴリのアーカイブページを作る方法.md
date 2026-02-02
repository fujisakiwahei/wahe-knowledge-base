# NuxtとmicroCMSでカテゴリのアーカイブページを作る方法

タイプ: フロー

## 利用シーン（あれば画像）

- タイトルの通り

![image.png](Nuxt%E3%81%A8microCMS%E3%81%A7%E3%82%AB%E3%83%86%E3%82%B4%E3%83%AA%E3%81%AE%E3%82%A2%E3%83%BC%E3%82%AB%E3%82%A4%E3%83%96%E3%83%9A%E3%83%BC%E3%82%B8%E3%82%92%E4%BD%9C%E3%82%8B%E6%96%B9%E6%B3%95/image.png)

## やること

1. カテゴリのアーカイブページを作る
    1. `pages/works/category/[id].vue`
2. カテゴリをクリックした際のリンク先を編集
    
    ```
    <NuxtLink
      :to="`/works/category/${category.id}`"
      class="flex h-10 items-center justify-center rounded-full border border-black bg-white px-4 text-sm shadow-sm transition-all duration-200 hover:border-green-500 hover:bg-green-500 hover:text-[white]"
    >
      {{ category.name }}
    </NuxtLink>
    ```
    
3. microCMSのリクエストにフィルターをかける
    
    ```jsx
    const route = useRoute()
    const id = route.params.id
    const { data } = await useMicroCMSGetList<Work>({
      endpoint: 'works',
      queries: {
        filters: `categories[contains]${id}`,
      },
    })
    ```
    
4. 現在有効なカテゴリに対してクラスを付与する
    
    ```jsx
    :class="[
      // 基本のクラス
      'flex h-10 items-center justify-center rounded-full border px-4 text-sm shadow-sm transition-all duration-200',
      // 現在のカテゴリと一致する場合とそれ以外で分岐
      route.params.id === category.id
        ? 'pointer-events-none cursor-default border-green-500 bg-green-500 text-white' // アクティブ時のスタイル
        : 'border-black bg-white hover:border-green-500 hover:bg-green-500 hover:text-[white]', // 非アクティブ時のスタイル
    ]"
    ```