# Nuxt 3/4 GSAP 導入マニュアル

Nuxt プロジェクトに GSAP (ScrollTrigger 含む) を導入する最短手順です。

## 1. インストール

```bash
npm install gsap
```

## 2. プラグインの作成

Nuxt で GSAP をグローバルに利用可能にし、クライアントサイドでのみ実行されるように設定します。

`app/plugins/gsap.client.ts` を作成（Nuxt 4 の場合。Nuxt 3 なら `plugins/gsap.client.ts`）:

```typescript
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

export default defineNuxtPlugin(() => {
  if (import.meta.client) {
    gsap.registerPlugin(ScrollTrigger)
  }

  return {
    provide: {
      gsap,
      ScrollTrigger,
    },
  }
})
```

## 3. コンポーネントでの使用方法

`script setup` 内でインポートして使用します。

```vue
<script setup lang="ts">
import { gsap } from 'gsap'
import { ScrollTrigger } from 'gsap/ScrollTrigger'

const box = ref(null)

onMounted(() => {
  // プラグインで登録済みだが、型定義や補完のために再インポートを推奨
  gsap.to(box.value, {
    x: 100,
    scrollTrigger: {
      trigger: box.value,
      start: 'top center',
      scrub: true
    }
  })
})
```

## 4. 注意事項

- **クライアントサイド限定**: GSAP は DOM を操作するため、必ず `onMounted` 内で実行するか、`if (import.meta.client)` で囲ってください。
- **プラグインの命名**: ファイル名を `.client.ts` にすることで、サーバーサイドでの実行を自動的に避けることができます。
- **メモリリーク防止**: コンポーネントが破棄される際は `ScrollTrigger.getAll().forEach(t => t.kill())` などでリセットを検討してください。
