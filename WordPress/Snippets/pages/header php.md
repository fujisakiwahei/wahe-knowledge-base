# header.php

タイプ: スニペット

## 利用シーン（あれば画像）

- WordPressサイトのヘッダーやメタタグ

## コード

```php
<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width,initial-scale=1.0" />

    <title><?php echo wp_get_document_title(); ?></title>

    <?php wp_head(); ?>

    <header class="header">

    </header>
</head>
<body>
    <header class="header">
    </header>
    <main>
```

## 注意点