## フォームのコード

```html
<div class="contactFormInner">
  <!-- テキスト入力 (必須) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">お名前</span>
      <span class="contactFormItemRequired">必須</span>
    </label>
    <div class="contactFormItemContent">[text* your-name class:contactFormInput placeholder "例：山田 太郎"]</div>
  </div>

  <!-- メールアドレス (必須) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">メールアドレス</span>
      <span class="contactFormItemRequired">必須</span>
    </label>
    <div class="contactFormItemContent">[email* your-email class:contactFormInput placeholder "例：info@example.com"]</div>
  </div>

  <!-- セレクトボックス (任意) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">お問い合わせ項目</span>
      <span class="contactFormItemOptional">任意</span>
    </label>
    <div class="contactFormItemContent">[select menu-reason class:contactFormInput "制作について" "採用について" "その他"]</div>
  </div>

  <!-- ラジオボタン (任意) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">ご予算感</span>
      <span class="contactFormItemOptional">任意</span>
    </label>
    <div class="contactFormItemContent">[radio budget use_label_element default:1 "〜30万円" "30〜100万円" "100万円〜" "未定"]</div>
  </div>

  <!-- チェックボックス (任意) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">ご希望の連絡方法</span>
      <span class="contactFormItemOptional">任意</span>
    </label>
    <div class="contactFormItemContent">[checkbox contact-method use_label_element "メール" "電話" "Zoom"]</div>
  </div>

  <!-- テキストエリア (必須) -->
  <div class="contactFormItem">
    <label class="contactFormItemTitle">
      <span class="contactFormItemTitleText">お問い合わせ内容</span>
      <span class="contactFormItemRequired">必須</span>
    </label>
    <div class="contactFormItemContent">[textarea* your-message class:contactFormInput x8 placeholder "ご自由にご記入ください"]</div>
  </div>

  <!-- 送信ボタン -->
    <div class="contactFormItem submitWrapper">
		    <div class="contactFormItemButton">[submit class:contactFormBtn "Send"]</div>
		    <div class="contactFormItemText" aria-hidden="true">Send</div>
		    <span class="material-symbols-rounded" aria-hidden="true">send</span>
		</div>
</div>

```

```scss
/* ===============================================
 *  フォーム
 =============================================== */

.wpcf7 {
  width: 100%;
  margin-inline: auto;

  & * {
    box-sizing: border-box;
  }

  .wpcf7-form {
    width: 100%;
    margin-inline: auto;
  }

  /* インナーコンテナ */
  .contactFormInner {
    display: flex;
    flex-direction: column;
    gap: 32px; /* 行間の余白 */
  }

  /* 各行のラッパー */
  .contactFormItem {
    flex-shrink: 0;
    display: flex;
    // flex-direction: column; // 2カラムの時は削除
    gap: 40px;

    @media (max-width: 768px) {
      gap: 8px;
      flex-direction: column;
    }

    /* 送信ボタンエリア用の修飾クラス */
    .submitWrapper {
      margin-top: 24px;
      border-bottom: none;
      align-items: center;
      justify-content: center;

      .contactFormItemContent {
        width: auto; /* ボタンの幅に合わせる */
        flex: 0 0 auto;
      }
    }
  }

  /* 見出し（ラベル）エリア */
  .contactFormItemTitle {
    //2カラムの時は width: ⚪︎px;
    width: 200px;
    flex-shrink: 0;
    font-weight: 700;
    font-size: 16px;
    line-height: 1.5;
    display: flex;
    align-items: center;
    gap: 12px;
  }

  /* 必須・任意バッジ */
  .contactFormItemRequired,
  .contactFormItemOptional {
    font-size: 12px;
    padding: 6px 8px 4px 8px;
    border-radius: 4px;
    line-height: 1;
    white-space: nowrap;
  }

  .contactFormItemRequired {
    background-color: #e03131;
    color: #fff;
  }

  .contactFormItemOptional {
    background-color: #f1f3f5;
    color: #868e96;
    border: 1px solid #dee2e6;
  }

  /* 入力エリアのラッパー */
  .contactFormItemContent {
    flex-grow: 1;
    width: 100%;
    position: relative;

    .wpcf7-not-valid-tip {
      font-size: 12px;
      color: #e03131;
      margin-top: 4px;
      display: block;
    }
  }

  .wpcf7-form-control-wrap {
    display: block;
    width: 100%;
    height: 100%;
  }

  /* ==========================================================================
      Input Elements Styling (タグ別の基本スタイル)
========================================================================== */

  input {
    width: 100%;
    box-sizing: border-box;

    /* ラジオボタン・チェックボックス以外にappearance: noneを適用 */
    &:not([type="radio"]):not([type="checkbox"]) {
      appearance: none;
    }
  }

  /* テキスト入力系 (text, email, url, tel, password) */
  input[type="text"],
  input[type="email"],
  input[type="url"],
  input[type="tel"],
  input[type="password"],
  textarea,
  select {
    width: 100%;
    padding: 12px 16px;
    border: 1px solid #ddd;
    border-radius: 4px;
    background-color: #fff;
    font-size: 16px;
    line-height: 1.5;
    letter-spacing: 0.05em;
    appearance: none; /* ブラウザ標準スタイルをリセット */
    transition: border-color 0.2s ease, box-shadow 0.2s ease;

    &::placeholder {
      color: #adb5bd;
    }

    &:focus {
      outline: none;
      border-color: #339af0;
      box-shadow: 0 0 0 3px rgba(51, 154, 240, 0.1);
    }
  }

  /* テキストエリア固有 */
  textarea {
    min-height: 160px;
    letter-spacing: 0.05em;
    resize: vertical; /* 縦方向のみリサイズ許可 */
  }

  /* セレクトボックス - デフォルトスタイルへ戻す */
  select {
    appearance: auto; /* OS標準の矢印を表示 */
    cursor: pointer;
  }

  /* チェックボックスのラッパー (CF7が出力するspan) */
  .wpcf7-checkbox {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 8px;
    cursor: pointer;
  }

  /* ラジオボタンのラッパー (CF7が出力するspan) */
  .wpcf7-radio {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    gap: 8px;
    cursor: pointer;
  }

  /* チェックボックス・ラジオボタンのアイテムのラッパー (CF7が出力するspan) */
  .wpcf7-list-item {
    margin-left: 0;
    label {
      display: flex;
      align-items: center;
      gap: 8px;
      cursor: pointer;
    }
  }

  /* チェックボックス & ラジオボタン本体 */
  input[type="checkbox"],
  input[type="radio"] {
    box-sizing: border-box;
    width: 20px;
    height: 20px;
    margin: 0;
    cursor: pointer;
    accent-color: red;

    /* 標準の外観を保持（重要！） */
    appearance: auto;

    /* カスタムスタイルは最小限に */
    flex-shrink: 0; /* サイズ固定 */
  }

  /* 送信ボタン */
  .submitWrapper {
    position: relative;
    cursor: pointer;
    width: fit-content;
    display: flex;
    align-items: center;
    flex-direction: row;
    gap: 10px;
    padding: 0 20px;
    height: 44px;
    border-radius: 0vh;
    font-size: 16px;
    letter-spacing: 0.1em;
    font-weight: 400 !important;
    font-family: $fontEn !important;
    background: transparent;
    color: $colorGreen;
    border: 1px solid $colorGreen;
    transition: all 0.5s;

    // ★ フォーカススタイル（キーボード操作用）
    &:focus-within {
      border-radius: 24px;
      background: $colorGreen !important ;
      color: $colorWhite !important;
    }

    @media (hover: none) {
      border-radius: 24px;
      background: $colorGreen !important ;
      color: $colorWhite !important;

      &:active {
        opacity: 0.7;
        scale: 0.98;
      }
    }

    @media (max-width: $tabletBreakPoint) {
      margin-inline: auto;
    }

    @media (hover: hover) {
      &:hover {
        border-radius: 24px;
        background: $colorGreen;
        color: $colorWhite;

        .material-symbols-rounded {
          transform: translateX(10px);
        }
      }
    }

    input[type="submit"],
    button[type="submit"] {
      letter-spacing: 0.1em;
    }

    .contactFormItemButton {
      display: flex;
      align-items: center;
      justify-content: center;
      position: absolute;
      width: 100%;
      height: 100%;
      top: 0;
      right: 0;
      opacity: 0;

      input {
        height: 100%;
        width: 100%;
      }
    }

    .material-symbols-rounded {
      pointer-events: none;
      font-size: 24px;
      font-weight: 300;
      transition: all 0.5s $easeOutQuad;
      transform: translateX(0);
    }

    &:active {
      transform: translateY(1px);
    }

    &.submitting {
      opacity: 0.7;
      cursor: wait;
    }
  }
}

/* ===============================================
 *  メッセージ
 =============================================== */
.wpcf7-response-output {
  display: block;
  font-size: 14px !important;
  margin: 0 !important;
  padding: 0 !important;
  margin-top: 16px !important;
  color: #333;
  border: none !important;
  padding: 0;
}

.invalid {
  .wpcf7-response-output {
    color: red;
  }
}

.wpcf7-spinner {
  display: none !important;
}

/* ===============================================
 *  Turnstileの認証
 =============================================== */
.wpcf7-turnstile {
  margin-bottom: 24px;
}

```

## 注意点

> 入力必須版の radio (“radio*”) はありません。Contact Form 7 が “radio*” を提供しない理由は、そもそもラジオボタンとは入力必須のものだからです。
https://contactform7.com/ja/checkboxes-radio-buttons-and-menus/ より
> 

## フォームメッセージ翻訳

- **Thank you for your message. It has been sent.**
    
    ```
    お問い合わせありがとうございます。メッセージが正常に送信されました。
    ```
    
- **There was an error trying to send your message. Please try again later.**
    
    ```
    メッセージの送信中にエラーが発生しました。恐れ入りますが、しばらく経ってから再度お試しください。
    ```
    
- **One or more fields have an error. Please check and try again.**
    
    ```
    ひとつまたは複数の入力項目に不備があります。内容をご確認の上、再度お試しください。
    ```
    
- **There was an error trying to send your message. Please try again later.** (スパムと見なされた場合など)
    
    ```
    メッセージの送信中にエラーが発生しました。恐れ入りますが、しばらく経ってから再度お試しください。
    ```
    
- **You must accept the terms and conditions before sending your message.**
    
    ```
    送信前に、利用規約（またはプライバシーポリシー）への同意が必要です。
    ```
    
- **Please fill out this field.**
    
    ```
    この項目は必須です。ご入力ください。
    ```
    
- **This field has a too long input.**
    
    ```
    入力内容が長すぎます。制限文字数以内に短縮してください。
    ```
    
- **This field has a too short input.**
    
    ```
    入力内容が短すぎます。指定された文字数以上でご入力ください。
    ```
    
- **There was an unknown error uploading the file.**
    
    ```
    ファイルのアップロード中に予期せぬエラーが発生しました。
    ```
    
- **You are not allowed to upload files of this type.**
    
    ```
    この形式（タイプ）のファイルはアップロードできません。
    ```
    
- **The uploaded file is too large.**
    
    ```
    アップロードされたファイルのサイズが大きすぎます。
    ```
    
- **There was an error uploading the file.** (PHPエラーなど)
    
    ```
    ファイルのアップロード中にエラーが発生しました。
    ```
    

## 送信者・受信者へのメールの設定

```
## 💡 Contact Form 7 メール設定プロンプト

### 🎯 目的

Contact Form 7 のフォーム設定用コードに基づき、**管理者宛メール（受信者用）**と**フォーム送信者宛メール（自動返信用）**の2種類のメールを設定するための具体的なマニュアルを出力する。

### 📋 入力情報

コンタクトフォーム7の**フォーム設定用コード**（メールタグを含む）を提供します。

### ⚙️ 出力形式

* Contact Form 7の管理画面の「**メール**」タブの設定項目に沿った、段階的なマニュアル形式で出力してください。
* 「**メール 1**」（管理者宛）と「**メール (2)**」（自動返信宛）の項目を明確に分けて解説してください。
* 提供されたフォームコードから抽出される**メールタグの使用例**を含めてください。
```
