# NuxtのプロジェクトにGSAPを導入

タイプ: スニペット, フロー

### **やること**

1. GSAPのインストール:
    
    プロジェクトのルートディレクトリで以下のコマンドを実行し、GSAPをインストールします。
    
    ```bash
    npm install gsap
    # または
    yarn add gsap
    # または
    pnpm add gsap
    ```
    
2. プラグインファイルの作成:
    
    plugins ディレクトリを作成し、その中に gsap.client.ts という名前のファイルを作成します。このファイルはクライアントサイドでのみ実行されるようにします。
    
    ```bash
    mkdir plugins
    touch plugins/gsap.client.ts
    ```
    
    ```jsx
    // plugins/gsap.client.ts
    import gsap from 'gsap';
    import { ScrollTrigger } from 'gsap/ScrollTrigger'; // 必要に応じて
    
    // ScrollTriggerプラグインを登録 (GSAPのプラグインは個別に登録が必要)
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
    
    **解説:**
    
    - `import gsap from 'gsap';`: GSAPのコアライブラリをインポートします。
    - `import { ScrollTrigger } from 'gsap/ScrollTrigger';`: 必要に応じて、GSAPのプラグイン（ここではScrollTrigger）をインポートします。他のプラグインも同様にインポートできます。
    - `gsap.registerPlugin(ScrollTrigger);`: インポートしたプラグインをGSAPに登録します。
    - `defineNuxtPlugin(() => { ... });`: Nuxtのプラグイン定義です。
    - `provide: { gsap: gsap, ScrollTrigger: ScrollTrigger, },`: これにより、コンポーネント内で `$gsap` や `$ScrollTrigger` としてGSAPのインスタンスやプラグインを利用できるようになります。
3. プラグインの登録（nuxt.config.ts）:
    
    nuxt.config.ts ファイルの plugins セクションに作成したプラグインを追加します。
    
    ```jsx
    // nuxt.config.ts
    export default defineNuxtConfig({
        // ... その他の設定 ...
        plugins: [
            { src: '~/plugins/gsap.client.ts', mode: 'client' } // クライアントサイドでのみ実行
        ]
    })
    ```
    
    **解説:**
    
    - `plugins: [...]`: 登録するプラグインの配列です。
    - `{ src: '~/plugins/gsap.client.ts', mode: 'client' }`: 作成したプラグインファイルのパスと、クライアントサイドでのみ実行することを指定します。
4. コンポーネントでの使用:
    
    コンポーネントの `<script setup>` 内で `useNuxtApp` を使って `$gsap` や `$ScrollTrigger` にアクセスし、GSAPのアニメーションを記述します。
    
    ```bash
    import { ref, onMounted, useNuxtApp } from 'vue';
    
    const { $gsap, $ScrollTrigger } = useNuxtApp();
    const animate = ref(null);
    ```
    
    ```jsx
    <template>
      <div ref="animate">Hello GSAP</div>
    </template>
    
    <script setup>
    import { ref, onMounted, useNuxtApp } from 'vue';
    
    const { $gsap, $ScrollTrigger } = useNuxtApp();
    const animate = ref(null);
    
    onMounted(() => {
      // 基本的なアニメーション
      $gsap.to(animate.value, { x: 100, duration: 1 });
    
      // ScrollTriggerを使ったアニメーション (必要に応じて)
      $ScrollTrigger.create({
        trigger: animate.value,
        start: 'top center',
        end: 'bottom center',
        markers: true,
        onEnter: () => $gsap.to(animate.value, { scale: 1.2, duration: 0.5 }),
        onLeaveBack: () => $gsap.to(animate.value, { scale: 1, duration: 0.5 }),
      });
    });
    </script>
    ```
    
    **解説:**
    
    - `import { ref, onMounted, useNuxtApp } from 'vue';`: Vueの必要な関数と `useNuxtApp` をインポートします。
    - `const { $gsap, $ScrollTrigger } = useNuxtApp();`: `useNuxtApp` を呼び出すことで、プラグインで `provide` した `$gsap` と `$ScrollTrigger` にアクセスできます。
    - `const box = ref(null);`: アニメーションを適用するDOM要素の参照を作成します。
    - `onMounted(() => { ... });`: コンポーネントがマウントされた後にアニメーションを実行します。
    - `$gsap.to(box.value, { x: 100, duration: 1 });`: `box` 要素をX方向に100px移動させるアニメーションを1秒かけて実行します。
    - `$ScrollTrigger.create({...});`: ScrollTriggerを使ったアニメーションの例です。指定した要素がビューポートに入ったり出たりする際にアニメーションを実行できます。

### **リファレンス**

- **GSAP公式ドキュメント:** [https://greensock.com/docs/v3/](https://greensock.com/docs/v3/)
- **Nuxt プラグイン:** [https://nuxt.com/docs/guide/directory-structure/plugins](https://nuxt.com/docs/guide/directory-structure/plugins)