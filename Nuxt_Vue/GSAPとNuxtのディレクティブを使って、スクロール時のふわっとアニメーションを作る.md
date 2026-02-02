# GSAPとNuxtのディレクティブを使って、スクロール時のふわっとアニメーションを作る

タイプ: スニペット, フロー

### 利用シーン

Webサイトのコンテンツがスクロールされて画面に現れる際に、要素をふわっと表示させる

### やること

1. **GSAPのインストール**
2. **GSAPとScrollTriggerのプラグイン作成 (`plugins/gsap.client.ts`):**`plugins` ディレクトリに `gsap.client.ts` ファイルを作成し、GSAPとScrollTriggerを登録します。
    
    ```tsx
    // plugins/gsap.client.ts
    import gsap from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger';
    
    gsap.registerPlugin(ScrollTrigger);
    
    export default defineNuxtPlugin(() => {
      return {
        provide: {
          gsap: gsap,
          ScrollTrigger: ScrollTrigger,
        },
      };
    });
    ```
    
3. **アピアランスアニメーション ディレクティブの作成 (`directives/appearanceAnimation.ts`):**`directives` ディレクトリを作成し、`appearanceAnimation.ts` ファイルに以下のコードを記述します。
    1. gsapを使って、クラスのつけ外しでアニメーションを実装。
    
    ```tsx
    // directives/appearanceAnimation.ts
    import { useNuxtApp } from '#app'
    import { gsap } from 'gsap'
    import type { Directive } from 'vue'
    
    export const appearanceAnimation: Directive = {
      mounted(el) {
        const { $ScrollTrigger } = useNuxtApp()
    
        gsap.set(el, {
          onComplete: () => {
            el.classList.add('appearanceBefore')
          },
        })
    
        $ScrollTrigger.create({
          trigger: el,
          start: 'top 80%',
          once: true,
          markers: false,
          onEnter: () => {
            el.classList.add('appearanceAfter')
          },
        })
      },
    ```
    
4. **ディレクティブ登録用プラグインの作成 (`plugins/directives.ts`):**`plugins` ディレクトリに `directives.ts` ファイルを作成し、作成したディレクティブをグローバルに登録します。
    
    ```tsx
    // plugins/directives.ts
    import { defineNuxtPlugin } from '#app';
    import { appearanceAnimation } from '~/directives/appearanceAnimation';
    
    export default defineNuxtPlugin((nuxtApp) => {
      nuxtApp.vueApp.directive('appearance-animation', appearanceAnimation);
    });
    ```
    
5. **`nuxt.config.ts` の編集:**
作成したプラグインを `nuxt.config.ts` の `plugins` セクションに登録します。
    
    ```tsx
    // nuxt.config.ts
    export default defineNuxtConfig({
      plugins: [
        '~/plugins/gsap.client.ts',
        '~/plugins/directives.ts'
      ]
    });
    ```
    
6. **app.vueにCSSの定義**
フェードイン効果のためのCSSを定義します。
    
    ```scss
    <style lang="scss">
    .appearanceBefore {
      opacity: 0;
      transform: translateY(50px);
      transition: all 0.5s ease-in-out;
    }
    
    .appearanceAfter {
      opacity: 1;
      transform: translateY(0);
    }
    </style>
    ```
    
7. **テンプレートで使用**
    
    ディレクティブを、アニメーションさせたい要素に記述するだけ
    
    ```html
    <template>
      <div>
        <section class="fade-in-section">
          <div v-appearance-animation>
            <h2>タイトル</h2>
            <p>コンテンツ</p>
          </div>
          <div v-appearance-animation-stagger>
            <p>要素1</p>
            <p>要素2</p>
            <p>要素3</p>
          </div>
        </section>
      </div>
    </template>
    ```
    

### 追記：staggerで配列を階段状にアニメーションさせたい場合

1. アニメーションのロジックを`directives/appearanceAnimationStagger.ts`に記述。
    
    基本的なやっていることは`appearanceAnimation` と同じ。配列にして、`setTimeout`を使って1要素0.1秒ずらしているだけ。
    
    ```bash
    import { useNuxtApp } from '#app'
    import { gsap } from 'gsap'
    import type { Directive } from 'vue'
    
    export const appearanceAnimationStagger: Directive = {
      mounted(el) {
        const { $ScrollTrigger } = useNuxtApp()
        const children = Array.from(el.children) as HTMLElement[]
    
        gsap.set(children, {
          onComplete: () => {
            children.forEach((child, index) => {
              child.classList.add('appearanceBefore')
            })
          },
        })
    
        $ScrollTrigger.create({
          trigger: el,
          start: 'top 80%',
          once: true,
          markers: false,
          onEnter: () => {
            children.forEach((child, index) => {
              setTimeout(() => {
                child.classList.add('appearanceAfter')
              }, index * 100)
            })
          },
        })
      },
    }
    ```
    
2. ディレクティブ登録用プラグインの作成 
    
    [**ディレクティブ登録用プラグインの作成 (`plugins/directives.ts`):**`plugins` ディレクトリに `directives.ts` ファイルを作成し、作成したディレクティブをグローバルに登録します。](GSAP%E3%81%A8Nuxt%E3%81%AE%E3%83%87%E3%82%A3%E3%83%AC%E3%82%AF%E3%83%86%E3%82%A3%E3%83%96%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%80%81%E3%82%B9%E3%82%AF%E3%83%AD%E3%83%BC%E3%83%AB%E6%99%82%E3%81%AE%E3%81%B5%E3%82%8F%E3%81%A3%E3%81%A8%E3%82%A2%E3%83%8B%E3%83%A1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E3%82%92%E4%BD%9C%E3%82%8B%201c4df78c372e8009970bc75d16cfb1f9.md)