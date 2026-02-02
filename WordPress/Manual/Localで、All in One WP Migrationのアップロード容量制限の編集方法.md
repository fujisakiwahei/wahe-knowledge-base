# Localで、All in One WP Migrationのアップロード容量制限の編集方法

タイプ: スニペット, フロー
技術・ツール: WordPress

## ⚙️ php.ini.hbsの編集

ファイルパス ：

`text~/Local Sites/(サイト名)/conf/php/php.ini.hbs`

変更内容（例：1GB） ：[taco3suisui+1](https://taco3suisui.com/local-migration-limit-up/)

```php
textupload_max_filesize = 1000M
post_max_size = 1000M
```

保存後、Localでサイトを停止→再起動

## 🔧 1GB以上の場合の追加設定

ファイルパス ：[[qiita](https://qiita.com/ayame_hasegawa/items/30db12282c2ebc1c974a)]

`textwp-content/plugins/all-in-one-wp-migration/constants.php`

変更内容 ：[[qiita](https://qiita.com/ayame_hasegawa/items/30db12282c2ebc1c974a)]

```php
php// 変更前
define( 'AI1WM_MAX_FILE_SIZE', 536870912 );

// 変更後（5GBまで可能）
define( 'AI1WM_MAX_FILE_SIZE', 536870912 * 100 );
```