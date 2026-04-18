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
4. プラグインファイルを編集
[All in One WP Migrationのアップロード容量制限の編集方法.md](https://github.com/fujisakiwahei/wahe-knowledge-base/blob/master/WordPress/Manual/Local%E3%81%A7%E3%80%81All%20in%20One%20WP%20Migration%E3%81%AE%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%89%E5%AE%B9%E9%87%8F%E5%88%B6%E9%99%90%E3%81%AE%E7%B7%A8%E9%9B%86%E6%96%B9%E6%B3%95.md)

## リファレンス

https://kanrekigg.com/archives/3080#index_id8
