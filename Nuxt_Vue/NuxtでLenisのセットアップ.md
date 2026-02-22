# Nuxt 3/4 Lenis 導入マニュアル

Nuxt プロジェクトに Lenis（慣性スクロール）を導入する最短手順です。

## 1. インストール

```bash
npm install lenis
```

## 2. プラグインの作成

`app/plugins/lenis.client.ts` を作成:

```typescript
import Lenis from 'lenis'

export default defineNuxtPlugin(() => {
  const lenis = new Lenis()

  function raf(time: number) {
    lenis.raf(time)
    requestAnimationFrame(raf)
  }
  requestAnimationFrame(raf)

  return {
    provide: {
      lenis,
    },
  }
})
```

## 3. スタイルの追加（任意）

`app/assets/css/main.css` に追記:

```css
html.lenis, html.lenis body {
  height: auto;
}

.lenis.lenis-smooth {
  scroll-behavior: auto !important;
}

.lenis.lenis-smooth [data-lenis-prevent] {
  overscroll-behavior: contain;
}

.lenis.lenis-stopped {
  overflow: hidden;
}
```

## 4. コンポーネントでの使用（任意）

特定のコンポーネントで Lenis インスタンスにアクセスしたい場合:

```vue
<script setup lang="ts">
const { $lenis } = useNuxtApp()

// スクロールを一時停止
$lenis.stop()

// スクロールを再開
$lenis.start()

// 特定位置へスクロール
$lenis.scrollTo('#section-id')
</script>
```

## 5. GSAP ScrollTrigger との連携

Lenis と GSAP ScrollTrigger を併用する場合、`lenis.client.ts` を以下のように修正:

```typescript
import Lenis from 'lenis'
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

export default defineNuxtPlugin(() => {
  gsap.registerPlugin(ScrollTrigger)

  const lenis = new Lenis()

  lenis.on('scroll', ScrollTrigger.update)

  gsap.ticker.add((time) => {
    lenis.raf(time * 1000)
  })
  gsap.ticker.lagSmoothing(0)

  return {
    provide: {
      lenis,
    },
  }
})
```

## 注意事項

- **クライアントサイド限定**: `.client.ts` にすることでSSRエラーを回避しています。
- **1回だけ初期化**: プラグインはアプリ起動時に1度だけ実行されるため、全ページに自動適用されます。
- **スクロール位置の復元**: ページ遷移時にスクロール位置をリセットしたい場合は `$lenis.scrollTo(0, { immediate: true })` を使用してください。
