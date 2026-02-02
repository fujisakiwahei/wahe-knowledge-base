# Nuxtのプロジェクトでページ遷移アニメーションを実装

タイプ: スニペット, フロー

## 利用シーン（あれば画像）

- タイトルの通り

## やること

1. nuxt.config.tsでページトランジションの設定を行う
    - app.pageTransitionオプションで名前とモードを設定
    - name: トランジションのクラス名のプレフィックス
    - mode: 'out-in'で古いページが消えてから新しいページが表示される
    
    ```tsx
    // nuxt.config.ts
    export default defineNuxtConfig({
      app: {
        pageTransition: {
          name: 'page',
          mode: 'out-in'
        }
      }
    })
    ```
    
2. app.vueにトランジションのスタイルを実装
    - enter-active/leave-active: トランジションの動作時間と種類を設定
    - enter-from/leave-to: 開始時と終了時のスタイルを設定
    - opacity と transform で fade & slide効果を実現
        
        ```
        <!-- app.vue -->
        <template>
          <NuxtPage />
        </template>
        
        <style>
        .page-enter-active,
        .page-leave-active {
          transition: all 0.4s;
        }
        .page-enter-from,
        .page-leave-to {
          opacity: 0;
          transform: translateY(20px);
        }
        </style>
        
        ```
        

## 注意点

- NuxtPageコンポーネントが必要
- トランジション名とスタイルのクラス名を一致させる
- モードの選択（out-in/in-out）でアニメーションの挙動が変わる