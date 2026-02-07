# Contact Form7で問い合わせフォームを作成

タイプ: スニペット, フロー

## 利用シーン（あれば画像）

- コンタクトフォームの作成

## コード

- 概要：https://shimizu-create.com/2023/07/982/

```html
<label class="formBlock">
    <div class="formLabel required">お名前</div>
    [text* your-name class:formField placeholder "例）姓名"]
</label>
<label class="formBlock">
    <div class="formLabel required">ふりがな</div>
    [text* your-name-furigana autocomplete:name class:formField placeholder "例）セイメイ"]
</label>
<label class="formBlock">
    <div class="formLabel">郵便番号</div>
    [text post_code class:formField placeholder "例）683-0009"]
</label>
<label class="formBlock">
    <div class="formLabel">住所</div>
    [text address class:formField]
</label>
<label class="formBlock">
    <div class="formLabel">お電話番号</div>
    [tel tel autocomplete:tel class:formField placeholder "例）0859-33-1319"]
</label>
<label class="formBlock">
    <div class="formLabel required">メールアドレス</div>
    [email* email autocomplete:email class:formField placeholder "例）info@shimazugumi.jp"]
</label>
<label class="formBlock">
    <div class="formLabel required">メールアドレス（確認）</div>
    [email* email-check autocomplete:email class:formField placeholder "例）info@shimazugumi.jp"]
</label>
<div class="formBlock">
    <div class="formLabel">年代</div>
    [select age class:formField first_as_label "選択してください" "10代" "20代" "30代" "40代" "50代" "60代"]
</div>
<div class="formBlock">
    <div class="formLabel">お客様のご状況</div>
    [radio request class:formField use_label_element "新築を検討中" "リノベーションを検討中" "その他"]
</div>
<div class="formBlock">
    <div class="formLabel required">お問い合わせ項目</div>
    [checkbox* purpose class:formField use_label_element "設計のご相談" "見積のご相談" "資金計画のご相談" "補助金のご相談" "その他お問い合せ"]
    
</div>
<div class="formBlock">
    <div class="formLabel">土地（物件）をお探し中ですか？</div>
    [radio land class:formField use_label_element "探している" "既に持っている" "その他"]
</div>
<div class="formBlock">
    <div class="formLabel">弊社を知ったきっかけ</div>
    [checkbox how_found class:formField use_label_element "ご紹介" "Instagram" "雑誌" "口コミ" "Facebook" "新聞広告" "インターネット" "CM" "看板" "イベント" "その他"]
</div>
<label class="formBlock">
    <div class="formLabel required">お問い合わせ内容</div>
    [textarea* detail class:formField]
</label>
<div class="submitWrapper button wide">
    <span class="buttonCopy">
      入力内容を確認する
    </span>
    <div class="buttonArrow">
      <span class="buttonArrowText">
        →
      </span>
    </div>
  </a>
    <input type="submit" value="送信">
</div>
```

## 注意点

- コード最下部は、送信ボタンを自作にしている。
- 従来のデザインに、不透明度を下げて絶対配置したボタンを置くことで機能にしている

```php
<div class="submitWrapper button wide">
    <span class="buttonCopy">
      入力内容を確認する
    </span>
    <div class="buttonArrow">
      <span class="buttonArrowText">
        →
      </span>
    </div>
  </a>
    <input type="submit" value="送信">
</div>
```

## リファレンス

https://ikel.co.jp/column/4208.html
