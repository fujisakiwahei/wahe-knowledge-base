# GSAP ScrollTrigger アニメーション実装マニュアル

Nuxt 4 プロジェクトで GSAP の ScrollTrigger を使ったスクロール連動アニメーションを実装する方法。

---

## 1. プラグインの設定（初回のみ）

`app/plugins/gsap.client.ts` で GSAP と ScrollTrigger をグローバルに登録する。

```ts
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

**ポイント**:
- `*.client.ts` でクライアントサイドのみ実行
- `provide` でアプリ全体から `$gsap`, `$ScrollTrigger` としてアクセス可能

---

## 2. コンポーネントでの使い方

### 2.1 インポートと ref の準備

```ts
import { onMounted, onUnmounted, ref } from 'vue'

// プラグインから GSAP を取得
const { $gsap: gsap, $ScrollTrigger: ScrollTrigger } = useNuxtApp()

// DOM 要素への参照
const sectionRef = ref<HTMLElement | null>(null)  // トリガーとなる要素
const imageRef = ref<HTMLElement | null>(null)    // アニメーションさせる要素

// GSAP コンテキスト（クリーンアップ用）
let ctx: gsap.Context
```

### 2.2 テンプレートで ref を設定

```vue
<template>
  <section ref="sectionRef">
    <img ref="imageRef" src="..." />
  </section>
</template>
```

### 2.3 アニメーションの実装

```ts
onMounted(() => {
  // 要素が存在しない場合は早期リターン
  if (!imageRef.value || !sectionRef.value) return

  ctx = gsap.context(() => {
    gsap.fromTo(
      imageRef.value,       // アニメーション対象
      { y: 100 },           // 開始状態
      {
        y: -50,             // 終了状態
        ease: 'none',       // イージング（none = 線形）
        scrollTrigger: {
          trigger: sectionRef.value,  // この要素を監視
          start: 'top bottom',        // 開始タイミング
          end: 'bottom top',          // 終了タイミング
          scrub: true,                // スクロール量に連動
        },
      },
    )
  })
})

// コンポーネント破棄時にアニメーションを解除
onUnmounted(() => {
  if (ctx) ctx.revert()
})
```

---

## 3. ScrollTrigger オプション解説

### 3.1 trigger

監視する要素。この要素が画面内に入ると/出るとアニメーションが動作する。

### 3.2 start / end

```
start: 'トリガー要素の位置 ビューポートの位置'
end:   'トリガー要素の位置 ビューポートの位置'
```

| 値 | 意味 |
|---|---|
| `top` | 要素/ビューポートの上端 |
| `bottom` | 要素/ビューポートの下端 |
| `center` | 要素/ビューポートの中央 |
| `50%` | パーセント指定も可能 |

**例**:
- `start: 'top bottom'` → 要素の上端がビューポートの下端に来た時に開始
- `end: 'bottom top'` → 要素の下端がビューポートの上端に来た時に終了

### 3.3 scrub

| 値 | 挙動 |
|---|---|
| `true` | スクロール位置に完全同期 |
| `false` | トリガー時に一度だけ再生 |
| `0.5` | 数値を指定すると追従の遅延（秒） |

### 3.4 その他のオプション

```ts
scrollTrigger: {
  markers: true,        // デバッグ用マーカー表示
  toggleActions: 'play pause resume reset',  // 各状態での動作
  pin: true,            // 要素を固定
  pinSpacing: false,    // 固定時のスペース調整
}
```

---

## 4. よく使うパターン

### 4.1 パララックス（上下移動）

```ts
gsap.fromTo(element, { y: 100 }, { y: -100, scrollTrigger: { ... } })
```

### 4.2 フェードイン

```ts
gsap.fromTo(
  element,
  { opacity: 0, y: 50 },
  {
    opacity: 1,
    y: 0,
    scrollTrigger: {
      trigger: element,
      start: 'top 80%',
      end: 'top 50%',
      scrub: true,
    },
  },
)
```

### 4.3 スケール変化

```ts
gsap.fromTo(
  element,
  { scale: 0.8 },
  {
    scale: 1,
    scrollTrigger: {
      trigger: element,
      start: 'top bottom',
      end: 'top center',
      scrub: true,
    },
  },
)
```

---

## 5. 注意点

1. **SSR 対策**: `onMounted` 内でのみ DOM 操作を行う
2. **クリーンアップ**: `onUnmounted` で必ず `ctx.revert()` を呼ぶ
3. **ref の null チェック**: アニメーション前に要素の存在確認
4. **gsap.context**: Vue のリアクティブシステムとの競合を防ぐ

---

## 6. 実装例（完全版）

```vue
<template>
  <section ref="sectionRef" class="relative">
    <img ref="imageRef" src="..." class="absolute bottom-0" />
    <div class="relative">コンテンツ</div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

const { $gsap: gsap } = useNuxtApp()

const sectionRef = ref<HTMLElement | null>(null)
const imageRef = ref<HTMLElement | null>(null)

let ctx: gsap.Context

onMounted(() => {
  if (!imageRef.value || !sectionRef.value) return

  ctx = gsap.context(() => {
    gsap.fromTo(
      imageRef.value,
      { y: 100 },
      {
        y: -50,
        ease: 'none',
        scrollTrigger: {
          trigger: sectionRef.value,
          start: 'top bottom',
          end: 'bottom top',
          scrub: true,
        },
      },
    )
  })
})

onUnmounted(() => {
  if (ctx) ctx.revert()
})
</script>
```
