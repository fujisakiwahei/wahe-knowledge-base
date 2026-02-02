# WordPressで投稿の日付が表示されない (1)

タイプ: トラブルシューティング

## 現象

WordPressで投稿の日付が表示されない。

## 原因

- `the_date` で日付を取得して表示させている。そうすると、同じ日付で複数の投稿をした場合重複するものは非表示になってしまう。

## 確認手順

1. `the_date` で日付を取得して表示させていないか確認。

## 解決方法

1. 一旦変数に格納してから、変数を用いて表示
    
    ```php
    $date = get_the_date('Y.m.d');
    ```
    
2. 変数を呼び出す形で表示
    
    ```php
    <h3 class="blogItemTitle"><?php the_title(); ?></h3>
    ```
    

## 参考リソース

- https://gimmicklog.com/wordpress/44/

## メモ