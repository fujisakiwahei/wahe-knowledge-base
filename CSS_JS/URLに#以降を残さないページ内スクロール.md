# URLに#以降を残さないページ内スクロール

タイプ: スニペット

```jsx
/* ===============================================
 *  ページ内スクロール（#を残さない）
 =============================================== */
document.addEventListener("DOMContentLoaded", () => {
  const anchorLinks = document.querySelectorAll('a[href^="#"]');

  anchorLinks.forEach((link) => {
    link.addEventListener("click", (e) => {
      e.preventDefault();

      const targetId = link.getAttribute("href");
      if (targetId === "#") return; // href="#"を無視

      const targetElement = document.querySelector(targetId);

      if (targetElement) {
        // URLから#を削除
        const currentUrl = window.location.href.split("#")[0];
        history.replaceState(null, null, currentUrl);

        // スムーズスクロール
        const offset = 80; // ヘッダーの高さ分オフセット
        const elementPosition = targetElement.getBoundingClientRect().top;
        const offsetPosition = elementPosition + window.pageYOffset - offset;

        window.scrollTo({
          top: offsetPosition,
          behavior: "smooth",
        });
      }
    });
  });

  // ページ読み込み時に#がある場合
  if (window.location.hash) {
    const targetElement = document.querySelector(window.location.hash);
    if (targetElement) {
      // URLから#を削除
      const currentUrl = window.location.href.split("#")[0];
      history.replaceState(null, null, currentUrl);

      // スムーズスクロール
      setTimeout(() => {
        const offset = 150; // ヘッダーの高さ分オフセット
        const elementPosition = targetElement.getBoundingClientRect().top;
        const offsetPosition = elementPosition + window.pageYOffset - offset;

        window.scrollTo({
          top: offsetPosition,
          behavior: "smooth",
        });
      }, 100);
    }
  }
});

```

## header.php例

```php
<div class="headerNavInner">
    <li><a href="#features">主要機能</a></li>
    <li><a href="#benefits">導入メリット</a></li>
    <li><a href="#useCase">ユースケース</a></li>
    <li><a href="#pricing">プラン</a></li>
    <li><a href="https://www.google.com/">English Site</a></li> <!-- TODO: 英語版サイトへのリンク -->
</div>
```