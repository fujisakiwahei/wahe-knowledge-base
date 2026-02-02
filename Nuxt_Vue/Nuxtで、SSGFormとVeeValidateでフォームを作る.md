# Nuxtで、SSGFormとVeeValidateでフォームを作る

タイプ: スニペット, フロー

このマニュアルでは、静的サイトジェネレーター（SSG）向けフォームサービス「SSGForm」をバックエンドとして利用し、Nuxt.js (Vue 3 Composition API `<script setup>`) と VeeValidate (`v4.15.0`) を使って、バリデーション付きのお問い合わせフォームを実装する手順を解説します。

**前提条件:**

- Node.js と npm (または yarn) がインストールされていること。
- Nuxt.js プロジェクトがセットアップされていること。
- VeeValidate のバージョンが `^4.15.0`、`@vee-validate/rules` のバージョンが `^4.15.0` であること。

---

## ステップ1: SSGフォームで受け皿を作成する

まず、フォームの送信データを受け取るためのエンドポイントを SSGForm で作成します。

1. **SSGForm にアクセス:** [https://ssgform.com/](https://ssgform.com/) にアクセスし、アカウント登録またはログインします。
2. **フォームを作成:** ダッシュボードで新しいフォームを作成します。フォーム名は任意で構いません（例: "お問い合わせフォーム"）。
3. **エンドポイントURLを取得:** フォーム作成後、表示される **エンドポイントURL** をコピーします。これは後ほど Nuxt.js のフォームの `action` 属性に設定します。
    - 例: `https://ssgform.com/s/xxxxxxxxxxxx` (提供されたコードでは `https://ssgform.com/s/B0Clp8lPbXye`)
4. **フィールド名の確認 (重要):** SSGForm は、フォームから送信される `name` 属性に基づいてデータを認識します。Nuxt.js 側で設定する `Field` コンポーネントの `name` 属性 (`会社名`, `お名前`, `メールアドレス`, `お問い合わせ内容`, `お問い合わせ内容（詳細）`) が、SSGForm で受信したいデータ項目と一致するようにしてください。SSGForm の設定画面で必要に応じてフィールド名を調整できます。

---

## ステップ2: Nuxt.js でフォームを記述・スタイリングする

次に、Nuxt.js プロジェクト内にフォームコンポーネントを作成し、見た目を整えます。

1. **コンポーネントの作成:**`components` ディレクトリなどにフォーム用の `.vue` ファイルを作成します（例: `components/ContactForm.vue`）。
2. **`<template>` でのフォーム構造:**`<template>` 内にフォームの骨組みを配置します。VeeValidate の `Form`, `Field`, `ErrorMessage` コンポーネントを使用します。
    
    ```
    <template>
      <div class="wrapper mx-auto max-w-[960px] px-5 py-10 md:py-20">
        <h2 class="text-3xl font-bold text-center mb-4">お問い合わせ</h2>
        <p class="mb-10 text-center md:-mt-4">
          お問い合わせを頂きましたら、<br class="md:hidden" />通常3日以内にご返信いたします。<br />
          些細なことでもお気軽に<br class="md:hidden" />お問い合わせお待ちしております！
        </p>
    
        <Form
          :action="ssgFormEndpoint" method="POST"
          v-slot="{ errors }" class="rounded-lg border border-green-500 p-5 shadow-lg md:p-16"
        >
          <div class="formItem">
            <div class="formContents">
              <label class="formItemLabel" for="company">会社名</label>
              <Field type="text" name="会社名" id="company" />
            </div>
            <ErrorMessage name="会社名" class="error-message" />
          </div>
    
          <div class="formItem">
            <div class="formContents">
              <label class="formItemLabel required" for="name">お名前</label>
              <Field rules="required" type="text" name="お名前" id="name" required />
            </div>
            <ErrorMessage name="お名前" class="error-message" />
          </div>
    
          <div class="formItem">
            <div class="formContents">
              <label class="formItemLabel required" for="email">メールアドレス</label>
              <Field
                :rules="{ required: true, email: true }"
                type="email"
                name="メールアドレス"
                id="email"
                required
              />
            </div>
            <ErrorMessage name="メールアドレス" class="error-message" />
          </div>
    
          <div class="formItem">
            <div class="formContents">
              <label class="formItemLabel required" for="inquiry">お問い合わせ内容</label>
              <div class="radio-group flex flex-wrap gap-4">
                <label class="flex items-center">
                  <Field
                    rules="required"
                    type="radio"
                    name="お問い合わせ内容"
                    value="制作のご相談"
                    id="inquiry-consult"
                  />
                  制作のご相談
                </label>
                <label class="flex items-center">
                  <Field
                    rules="required"
                    type="radio"
                    name="お問い合わせ内容"
                    value="ご質問"
                    id="inquiry-question"
                  />
                  ご質問
                </label>
                <label class="flex items-center">
                  <Field
                    rules="required"
                    type="radio"
                    name="お問い合わせ内容"
                    value="その他"
                    id="inquiry-other"
                  />
                  その他
                </label>
              </div>
            </div>
            <ErrorMessage name="お問い合わせ内容" class="error-message" />
          </div>
    
          <div class="formItem">
            <div class="formContents">
              <label class="formItemLabel required" for="inquiryDetail">お問い合わせ内容(詳細)</label>
              <Field
                rules="required"
                as="textarea"
                name="お問い合わせ内容（詳細）"
                id="inquiryDetail"
                required
              />
            </div>
            <ErrorMessage name="お問い合わせ内容（詳細）" class="error-message" />
          </div>
    
          <div class="formItem flex justify-center">
            <button
              type="submit"
              class="mx-auto h-14 w-40 items-center justify-center rounded-md border-[#333] bg-green-500 text-white shadow-md transition-all duration-300 hover:border-green-500 hover:bg-green-500 hover:text-white hover:shadow-lg md:border md:bg-white md:text-[#333]"
            >
              送信する
            </button>
          </div>
        </Form>
      </div>
    </template>
    
    ```
    
    **ポイント:**
    
    - `<Form>` コンポーネントでフォーム全体を囲み、`action` 属性に SSGForm のエンドポイントURL（これは `<script>` で定義します）をバインドします。`method="POST"` を指定します。
    - 各入力項目は `<Field>` コンポーネントで作成します。`name` 属性は SSGForm がデータを受け取る際のキーとなるため、**SSGForm で設定したフィールド名と一致させる** 必要があります。
    - `type` 属性で入力タイプ (`text`, `email`, `radio`) を指定します。
    - `as="textarea"` を指定すると `<textarea>` 要素としてレンダリングされます。
    - `<ErrorMessage>` コンポーネントは、対応する `name` 属性を持つ `<Field>` のバリデーションエラーメッセージを表示します。
    - `rules` 属性や `:rules` 属性でバリデーションルールを指定します（詳細はステップ3）。
3. **スタイリング:**`<style scoped lang="scss">` タグ内に SCSS を記述して、フォームの見た目を整えます。同時に Tailwind CSS のクラスも利用できます。
    
    ```scss
    <style scoped lang="scss">
    /* 提供されたSCSSコードをここに記述 */
    .wrapper {
      form {
        display: flex;
        flex-direction: column;
        .formItem:not(:last-child) {
          padding-bottom: 40px;
          margin-bottom: 40px;
          border-bottom: 1px solid #c5f2d6;
          @media (max-width: 768px) {
            padding-bottom: 24px;
            margin-bottom: 24px;
          }
          /* 最後の要素とその前の要素（詳細テキストエリア）の下線は不要 */
          &:last-child,
          &:nth-last-child(2) { /* 詳細テキストエリアの項目を特定 */
             border-bottom: none;
             margin-bottom: 0;
             padding-bottom: 0; /* paddingもリセット */
          }
          /* 送信ボタンの前の項目（詳細テキストエリア）のマージン調整 */
           &:nth-last-child(2) {
             margin-bottom: 40px; /* 送信ボタンとの間隔を確保 */
              @media (max-width: 768px) {
                 margin-bottom: 24px;
              }
           }
        }
        .formContents {
          display: flex;
          align-items: center;
          gap: 32px;
          @media (max-width: 768px) {
            flex-direction: column;
            align-items: flex-start; /* スマホ表示では左寄せ */
            gap: 16px;
          }
          label {
            flex-shrink: 0;
          }
          .formItemLabel {
            font-size: 14px;
            font-weight: 600;
            width: 220px;
            display: flex;
            align-items: center;
            &::before {
              content: '';
              display: block;
              width: 8px;
              height: 8px;
              background-color: #22c55e;
              border-radius: 50%;
              margin-right: 16px;
            }
            @media (max-width: 768px) {
              width: 100%;
              &::before {
                margin-right: 8px;
              }
            }
          }
          .required {
            &::after {
              content: '*';
              color: rgb(153, 0, 0);
              margin-left: 6px;
            }
          }
          input[type='text'], input[type='email'], textarea { /* inputとtextareaのスタイルを共通化 */
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc; /* 境界線を少し薄く */
            border-radius: 5px;
            transition: border-color 0.3s; /* フォーカス時のアニメーション */
             &:focus {
                outline: none;
                border-color: #22c55e; /* フォーカス時に緑色に */
             }
          }
          input[type='radio'] {
            cursor: pointer;
            margin-right: 4px; /* ラジオボタンとテキストの間隔 */
            width: 16px; /* サイズ指定 */
            height: 16px;
          }
          textarea {
            height: 200px;
            resize: vertical; /* 縦方向のリサイズのみ許可 */
          }
          .radio-group {
            display: flex;
            gap: 24px;
            flex-wrap: wrap;
            width: 100%; /* ラジオボタングループが幅全体を使うように */
            @media (max-width: 768px) {
              column-gap: 16px;
              row-gap: 8px;
            }
            label {
              cursor: pointer;
              width: fit-content;
              display: flex;
              align-items: center;
              gap: 0px; /* ラジオボタンとラベルテキストの間隔を詰める */
              font-weight: normal; /* ラベルの太字解除 */
              font-size: 1rem; /* ラベルのフォントサイズ調整 */
            }
          }
        }
      }
    }
    .error-message {
      display: block; /* block要素にして改行させる */
      color: rgb(153, 0, 0);
      font-size: 0.875rem; /* 14px */
      margin-top: 8px; /* フィールドとの間隔 */
      padding-left: 252px; /* PC表示でラベルの幅に合わせる */
       @media (max-width: 768px) {
         padding-left: 0; /* スマホ表示ではインデント解除 */
         margin-top: 4px;
       }
    }
    /* Tailwind CSS クラスも併用可能 */
    /* 例: ボタンのスタイルは主にTailwindで指定されている */
    </style>
    
    ```
    

---

## ステップ3: VeeValidate でバリデーションを実装する

VeeValidate をインストールし、`<script setup>` 内で設定を行い、各フィールドにバリデーションルールを適用します。

1. **必要なパッケージのインストール:**
VeeValidate と定義済みのバリデーションルールを提供する `@vee-validate/rules` をインストールします。
    
    ```bash
    npm install vee-validate@^4.15.0 @vee-validate/rules@^4.15.0
    
    ```
    
2. **インポート:** 必要な関数やコンポーネントを `vee-validate` と `@vee-validate/rules` からインポートします。
    
    ```tsx
    <script setup lang="ts">
    import { Form, Field, ErrorMessage, defineRule } from 'vee-validate'
    import { required, email } from '@vee-validate/rules' // 事前定義ルールをインポート
    
    // SSGFormのエンドポイントURL
    const ssgFormEndpoint = '[<https://ssgform.com/s/B0Clp8lPbXye>](<https://ssgform.com/s/B0Clp8lPbXye>)'; // あなたのエンドポイントURLに置き換えてください
    
    // ... (ルールの定義は以下)
    </script>
    
    ```
    
3. **日本語エラーメッセージの定義 (`defineRule`):**`defineRule` を使って、`@vee-validate/rules` のルール (`required`, `email` など) に対するデフォルトのエラーメッセージを日本語で上書きしたり、カスタムルールを定義したりします。
    
    ```tsx
    <script setup lang="ts">
    import { Form, Field, ErrorMessage, defineRule } from 'vee-validate'
    import { required, email } from '@vee-validate/rules' // required, emailルール本体はインポートが必要
    
    // SSGFormのエンドポイントURL
    const ssgFormEndpoint = '[<https://ssgform.com/s/B0Clp8lPbXye>](<https://ssgform.com/s/B0Clp8lPbXye>)';
    
    // 'required' ルールの日本語エラーメッセージを設定
    defineRule('required', (value: string | any[]) => { // 配列（ラジオボタンなど）も考慮
      if (!value || (typeof value === 'string' && !value.length) || (Array.isArray(value) && !value.length)) {
        return '必須項目です';
      }
      return true;
    });
    
    // 'email' ルールの日本語エラーメッセージを設定
    defineRule('email', (value: string) => {
      // 値がない場合はチェックしない（requiredルールでチェックするため）
      if (!value || !value.length) {
        return true;
      }
      // 簡単なメールアドレス形式の正規表現チェック
      if (!/^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$/.test(value)) {
        return '正しいメールアドレス形式で入力してください';
      }
      return true;
    });
    </script>
    
    ```
    
    - `defineRule('ルール名', バリデーション関数)` の形式で定義します。
    - バリデーション関数は、値が有効なら `true` を、無効なら **エラーメッセージ文字列** を返します。
    - `@vee-validate/rules` には多くの定義済みルールがあります。詳細は [VeeValidate Rules Documentation](https://vee-validate.logaretm.com/v4/guide/global-validators/) を参照してください。
4. **ルールの適用 (`Field` の `rules` 属性):**`<Field>` コンポーネントの `rules` 属性に、適用したいルール名を文字列で指定します。複数のルールを適用する場合は、`:rules` としてオブジェクト形式で記述します。（適用例はステップ2の `<template>` コードブロックを参照してください）
5. **エラーメッセージの表示 (`ErrorMessage` コンポーネント):**`<ErrorMessage>` コンポーネントは、対応する `<Field>` (同じ `name` 属性を持つ) でバリデーションエラーが発生した場合に、`defineRule` で設定したエラーメッセージを表示します。（表示例はステップ2の `<template>` コードブロックを参照してください）
    - `class="error-message"` でスタイルを適用します。

---

これで、SSGForm をバックエンドに、Nuxt.js と VeeValidate を使ったバリデーション付きフォームの実装が完了します。提供されたコードをベースに、各ステップでの設定箇所やポイントを解説しました。