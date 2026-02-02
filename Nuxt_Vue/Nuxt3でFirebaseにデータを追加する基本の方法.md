# Nuxt3でFirebaseにデータを追加する基本の方法

タイプ: フロー

## やっていること

このコードは、Firebaseのデータベース（Firestore）にデータを追加するための基本的なフォームを実装しています。主な機能は以下の通りです：

- **フォームの表示：** 画面中央に固定配置された入力フォームを表示します。document_idとquestion_textの2つの入力フィールドがあります。
- **データの入力管理：** v-modelディレクティブを使用して、入力値をVueコンポーネントのdataプロパティにバインドしています。
- **Firestoreへのデータ追加処理：**
    - Submit ボタンクリック時に addNewQuestion メソッドが実行されます
    - 指定されたdocument_idを使用して、"questions_for_values"コレクションに新しいドキュメントを作成
    - ドキュメントには question_text と value_id の2つのフィールドが保存されます
- **エラーハンドリング：**
    - Firestoreが利用可能かどうかのチェック
    - ドキュメントIDが指定されているかのチェック
    - try-catchによるエラー処理の実装

このコードは、特定のIDを持つドキュメントを作成または更新するための基本的な実装例となっています。

## コード

```html
<template>
  <div
    class="fixed left-1/2 top-1/2 flex h-60 w-96 -translate-x-1/2 -translate-y-1/2 flex-col items-center justify-center gap-4 rounded-lg border border-green-500 px-6 shadow-lg"
  >
    <label class="flex w-full items-center justify-between gap-2">
      <span> document_id: </span>
      <input
        type="text"
        v-model="document_id"
        class="ml-4 rounded-md border border-green-500 p-2"
      />
    </label>
    <label class="flex w-full items-center justify-between gap-2">
      <span> question_text: </span>
      <input
        type="text"
        v-model="question_text"
        class="ml-4 rounded-md border border-green-500 p-2"
      />
    </label>
    <button
      class="rounded-md bg-green-500 p-2 text-white"
      type="button"
      @click="addNewQuestion"
    >
      Submit
    </button>
  </div>
</template>

<script>
import { collection, doc, setDoc } from "firebase/firestore";

export default {
  data() {
    return {
      document_id: "",
      question_text: "",
      value_id: "",
    };
  },

  methods: {
    async addNewQuestion() {
      // プラグインから提供された Firestore インスタンスを使用
      const db = this.$firestore;

      // Firestore が利用可能か念のためチェック
      if (!db) {
        console.error("Firestoreが利用できません。");
        return;
      }

      // --- ここから ID 指定の処理 ---

      // 1. 使用するドキュメントIDを決定 (例: dataプロパティから、または固定値)
      const docIdToUse = this.document_id; // もしくは this.value_id など、IDとして使いたい値

      if (!docIdToUse) {
        console.error("ドキュメントIDが指定されていません。");
        return;
      }

      try {
        // 2. コレクション名と指定IDでドキュメント参照を作成
        const docRef = doc(db, "questions_for_values", docIdToUse); // "questions_for_values" は実際のコレクション名に合わせる

        // 3. setDoc でデータを書き込み
        await setDoc(docRef, {
          question_text: this.question_text,
          value_id: this.document_id, // value_id はフィールドとしても保存
        });

        console.log("ドキュメントを追加/更新しました (指定ID):", docIdToUse);
        // 必要に応じてフォームをクリア
        this.question_text = "";
        this.value_id = "";
      } catch (error) {
        console.error("ドキュメント追加エラー:", error);
      }
    },
  },
};
</script>

```