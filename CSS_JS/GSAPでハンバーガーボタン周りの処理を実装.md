# ハンバーガーメニュー テンプレート

GSAP を使ったハンバーガーメニューの構造と実装パターンです。

---

## 1. HTML 構造

```html
<header class="header">
  <div class="header__inner">
    <!-- ロゴなど -->
    <nav class="header__nav over-tablet-only">
      <!-- 通常のナビ（PC） -->
    </nav>
    <div class="header__hamburger-button under-tablet-only" aria-label="メニュー">
      <span class="header__hamburger-button__line"></span>
      <span class="header__hamburger-button__line"></span>
      <span class="header__hamburger-button__line"></span>
    </div>
  </div>
</header>

<!-- グローバルナビ（header の外、固定表示） -->
<div class="global-nav">
  <nav class="global-nav__nav">
    <ul class="global-nav__nav__list">
      <li class="global-nav__nav__list__item global-nav-animation">
        <a href="#">項目1</a>
      </li>
      <li class="global-nav__nav__list__item global-nav-animation">
        <a href="#">項目2</a>
      </li>
      <!-- アニメーションさせたい要素には .global-nav-animation を付与 -->
    </ul>
  </nav>
</div>
```

### ポイント

- `header__hamburger-button`：クリック対象。`under-tablet-only` で SP のみ表示。
- `global-nav`：header の兄弟要素。`position: fixed` で全画面オーバーレイ。
- `.global-nav-animation`：個別アニメーション（stagger など）を適用する要素に付与。

---

## 2. CSS 構造

```scss
.global-nav {
  z-index: 100;
  position: fixed;
  top: 0;
  left: 0;
  // 必要に応じてスタイリング
  display: none; // 初期状態（GSAPと同期）
  opacity: 0; // 初期状態（GSAP と同期）
  visibility: hidden; // 初期状態（GSAP の autoAlpha と同期）

  // transition は GSAP が担当するため不要
}
```

### ポイント

- **初期状態**：`display: none`、`opacity: 0`、`visibility: hidden` で JS 実行前のちらつきを防止。
- **transition**：GSAP がアニメーションを制御するため、CSS の `transition` は書かない。

---

## 3. GSAP の仕組み

### 3.1 用語

| 用語              | 説明                                                                                           |
| ----------------- | ---------------------------------------------------------------------------------------------- |
| `gsap.set()`      | 即座にスタイルを適用（アニメーションなし）                                                     |
| `gsap.to()`       | 指定した値までアニメーション                                                                   |
| `autoAlpha`       | `opacity` + `visibility` をまとめて制御。0 で `visibility: hidden`、1 で `visibility: visible` |
| `gsap.timeline()` | 複数のアニメーションを時系列で管理                                                             |
| `stagger`         | 複数要素のアニメーション開始をずらす（負の値で逆順）                                           |

### 3.2 display とアニメーションの関係

- `display: none` の要素はアニメーションできない。
- **開く**：先に `display: flex` を `gsap.set()` で指定 → その後 `autoAlpha` を 0→1 でアニメーション。
- **閉じる**：`autoAlpha` を 1→0 でアニメーション → `onComplete` で `display: none` を設定。

---

## 4. JS テンプレート

```javascript
document.addEventListener("DOMContentLoaded", () => {
  const hamburgerButton = document.querySelector(".header__hamburger-button");
  const globalNav = document.querySelector(".global-nav");
  const animatedElements = document.querySelectorAll(".global-nav-animation");

  if (!hamburgerButton || !globalNav || !animatedElements.length) return;

  // 初期状態をセット
  gsap.set(globalNav, { autoAlpha: 0, display: "none" });
  gsap.set(animatedElements, {
    /* 初期値 */
  });

  hamburgerButton.addEventListener("click", () => {
    const isOpen = hamburgerButton.classList.contains("is-open");

    if (isOpen) {
      // 閉じる
      gsap
        .timeline({
          onComplete: () => {
            gsap.set(globalNav, { display: "none" });
            hamburgerButton.classList.remove("is-open");
          },
        })
        .to(animatedElements, {
          /* 閉じるときのアニメーション */
        })
        .to(globalNav, { autoAlpha: 0, duration: 0.3, ease: "power2.inOut" }, "<");
    } else {
      // 開く
      hamburgerButton.classList.add("is-open");
      gsap.set(globalNav, { display: "flex" });

      gsap
        .timeline()
        .to(globalNav, {
          autoAlpha: 1,
          duration: 0.3,
          ease: "power2.inOut",
        })
        .to(
          animatedElements,
          {
            /* 開くときのアニメーション（stagger など） */
          },
          "-=0.15",
        );
    }
  });
});
```

### 4.1 アニメーションを入れる箇所（ダミー）

**初期状態（gsap.set）**

```javascript
gsap.set(animatedElements, {
  // 例: y: 100, opacity: 0
});
```

**開くとき（.to）**

```javascript
.to(animatedElements, {
  // 目標値
  // duration: 0.5,
  // stagger: 0.08,  // 複数要素をずらす場合
  // ease: "power2.inOut",
}, "-=0.15")  // 前のアニメと少し重ねる
```

**閉じるとき（.to）**

```javascript
.to(animatedElements, {
  // 初期状態に戻す値
  // stagger: -0.08,  // 逆順にしたい場合
}, "<")  // "<" で同時開始、">" で順次
```

### 4.2 タイムラインの位置指定

3つ目の引数で開始タイミングを指定：

- `"<"`：直前のアニメと同時開始
- `">"`：直前のアニメ終了後に開始（デフォルト）d
- `"-=0.15"`：直前のアニメより 0.15 秒前に開始

---

## 5. 依存関係

`inc/enqueue.php` で main.js に GSAP を依存させる：

```php
wp_enqueue_script(
  "mainJs",
  get_theme_file_uri('/assets/js/main.js'),
  array('gsap'),
  null,
  true
);
```
