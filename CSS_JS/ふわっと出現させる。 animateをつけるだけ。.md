# ふわっと出現させる。.animate をつけるだけ。

タイプ: スニペット

## 利用シーン（あれば画像）

- 手軽に、要素をふわっと出現させたい時

## コード

1. 動かしたい要素に、.animate クラスをつける。
2. js を記述

   ```jsx
   /* ===============================================
    *  全体をふわっと出現
    =============================================== */

   // すべての.boxを取得
   let animateElements = document.querySelectorAll(".animate");

   document.addEventListener("DOMContentLoaded", function () {
     const windowHeight = window.innerHeight; // 画面の高さを取得

     // 画面内に表示されている要素にクラスを追加
     animateElements.forEach((element) => {
       const elementTop = element.getBoundingClientRect().top;

       // 要素が画面内にある場合、即座にアニメーションを適用
       if (elementTop < windowHeight) {
         element.classList.add("isFadeIn");
       }
     });
   });

   window.addEventListener("scroll", function () {
     // スクロール量を取得
     const scroll = window.scrollY;
     console.log(scroll);

     // 画面の高さを取得
     const windowHeight = window.innerHeight;

     animateElements.forEach((element) => {
       const elementTop = element.getBoundingClientRect().top + window.pageYOffset;

       if (scroll > elementTop - windowHeight / 1.2) {
         element.classList.add("isFadeIn");
       }
     });
   });
   ```

3. CSS を記述（`common.scss`など共通のファイルがよき）

   ```scss
   /* ===============================================
    *  ふわっと出てくるアニメーション
    =============================================== */
   .animate {
     opacity: 0;
     transform: translateY(50px);
   }

   .isFadeIn {
     animation: fadeIn 0.8s ease-in-out;
     animation-fill-mode: forwards;
   }

   @keyframes fadeIn {
     0% {
       opacity: 0;
       transform: translateY(50px);
     }
     100% {
       opacity: 1;
       transform: translateY(0);
     }
   }
   ```

## 注意点
