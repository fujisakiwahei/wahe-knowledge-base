# common.scss

タイプ: スニペット

## 利用シーン（あれば画像）

- ページ全体で利用するパーツのスタイリング。
    - ボタンやセクションタイトル、ページのフォントなど。

## コード

```scss
@use "./reset";
@use "./var" as *;

// フォント
* {
  font-feature-settings: "palt";
}

body {
  font-family: sans-serif;
  font-weight: 400;
  font-size: 16px;
  letter-spacing: 0.05em;
  line-height: 1.5;
  color: $colorText;
}

main {
  position: relative;
}

.inner{
	width: 100%;
	max-width: 1000px;
	margin-inline: auto;
	padding-inline: 20px;
}

.containar--full{
	width: 100%;
}

/* ===============================================
     aタグ
  =============================================== */
a {
  cursor: pointer;
  text-decoration: none;
}

/* ===============================================
     セクション
  =============================================== */

/* ===============================================
     セクションタイトル
  =============================================== */

/* ===============================================
     ページタイトル
  =============================================== */

/* ===============================================
       ボタン
  =============================================== */

/* ===============================================
       改行
  =============================================== */
/* ===============================================
   *  デバイスサイズに応じた表示切り替え
  =============================================== */
.pc-only {
  @media screen and (max-width: $tabletBreakPoint) {
    display: none !important;
  }
}

.over-tablet-only {
  @media screen and (max-width: calc($tabletBreakPoint)) {
    display: none !important;
  }
}

.under-tablet-only {
  @media screen and (min-width: calc($tabletBreakPoint + 1px)) {
    display: none !important;
  }
}

.sp-only {
  @media screen and (min-width: calc($spBreakPoint + 1px)) {
    display: none !important;
  }
}

/* ===============================================
       グローバルナビゲーション
  =============================================== */

```

## 注意点

- 以下のコードは、同階層にリセットと変数のscssファイルを作っている前提。
    
    ```scss
    @use "./reset";
    @use "./var" as *;
    ```