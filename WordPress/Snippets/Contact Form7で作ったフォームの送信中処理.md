# Contact Form7で作ったフォームの送信中処理

タイプ: スニペット

## 利用シーン（あれば画像）

送信失敗時

[https://gyazo.com/100094ecaaeb35e249902a5b040514b4](https://gyazo.com/100094ecaaeb35e249902a5b040514b4)

送信成功時

[https://gyazo.com/41faf112b0446886b61eba41b9e6ce72](https://gyazo.com/41faf112b0446886b61eba41b9e6ce72)

## コード

※クラス名は適宜変更

```jsx
/* ===============================================
 *  ウェイトリストフォームの送信処理
 =============================================== */
document.addEventListener("DOMContentLoaded", () => {
  // すべてのウェイトリストフォームを取得
  const waitListForms = document.querySelectorAll(".${フォームの親要素divのクラス名}");

  waitListForms.forEach((formWrapper) => {
    const form = formWrapper.querySelector(".wpcf7-form");
    const submitButton = formWrapper.querySelector('input[type="submit"]');

    if (!form || !submitButton) return;

    // 元のボタンテキストを保存
    const originalButtonText = submitButton.value;

    // Contact Form 7の送信開始時の処理（バリデーション成功時のみ）
    document.addEventListener("wpcf7beforesubmit", (event) => {
      if (event.detail.contactFormId === parseInt(form.closest(".wpcf7").querySelector('input[name="_wpcf7"]').value)) {
        // バリデーションが通った場合のみ送信中処理を実行
        submitButton.value = "送信中...";
        submitButton.disabled = true;
        submitButton.style.opacity = "0.7";
      }
    });

    // Contact Form 7のバリデーションエラー時の処理
    document.addEventListener("wpcf7invalid", (event) => {
      if (event.detail.contactFormId === parseInt(form.closest(".wpcf7").querySelector('input[name="_wpcf7"]').value)) {
        // バリデーションエラーの場合、ボタンを元に戻す
        submitButton.value = originalButtonText;
        submitButton.disabled = false;
        submitButton.style.opacity = "1";
      }
    });

    // Contact Form 7の送信失敗後の処理
    document.addEventListener("wpcf7mailfailed", (event) => {
      if (event.detail.contactFormId === parseInt(form.closest(".wpcf7").querySelector('input[name="_wpcf7"]').value)) {
        // 送信失敗後、ボタンを元に戻す
        submitButton.value = originalButtonText;
        submitButton.disabled = false;
        submitButton.style.opacity = "1";
      }
    });
  });
});
```

## 注意点