# All-in-One WP Migrationで、インポート容量を増やす

タイプ: フロー

## 利用シーン（あれば画像）

- タイトルの通り

## やること

1. レンタルサーバーの設定を開く
    1. ConoHaだと、サイト管理→応用設定
2. `.htaccess`を編集。末尾に追加。
    
    ```
    php_value memory_limit 1000M
    php_value post_max_size 1000M
    php_value upload_max_filesize 1000M
    ```
    
3. `php.ini`を編集。
    
    ```
    memory_limit = 1000M
    upload_max_filesize = 1000M
    post_max_size = 1000M
    ```
    

## リファレンス

https://kanrekigg.com/archives/3080#index_id8