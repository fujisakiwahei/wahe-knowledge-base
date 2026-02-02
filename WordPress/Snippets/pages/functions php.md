# functions.php

タイプ: スニペット

## 利用シーン（あれば画像）

- フォントやプラグインの読み込みをする
- 定型で、サムネイルの有効化も行っている

## コード

```php
<?php

/* ===============================================
  スクリプトとスタイルシートの読み込み
=============================================== */
function my_theme_scripts()
{
    // Google Fonts
    wp_enqueue_style('googleIcons', "https://fonts.googleapis.com/css2?family=Material+Symbols+Rounded:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200");
    
    // Google Icons
    wp_enqueue_style('googleFonts', "https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@100..900&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");

    // Swiper
    wp_enqueue_style('swiperStyle', "https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.css");
    wp_enqueue_script('swiperScript', "https://cdn.jsdelivr.net/npm/swiper@11/swiper-bundle.min.js");
    
    // 他にもプラグインなどあれば追加

    // assets
    wp_enqueue_style("mainStyle", get_theme_file_uri('style.css'));
    wp_enqueue_script("mainJs", get_theme_file_uri('/assets/js/main.js'));
}

add_action('wp_enqueue_scripts', 'my_theme_scripts');

/* ===============================================
  サムネイルを有効化
=============================================== */
add_theme_support('post-thumbnails');

/* ===============================================
 *  bodyに固定ページのクラスを追加
 =============================================== */
function add_slug_to_body_class($classes) {
    if (is_page()) {
        global $post;
        if (isset($post->post_name)) {
            $classes[] = 'page-' . $post->post_name;
        }
    }
    return $classes;
}
add_filter('body_class', 'add_slug_to_body_class');

/* ===============================================
  抜粋の区切り文字を変更
=============================================== */
function custom_excerpt_more($more) {
  return '...';
}
add_filter('excerpt_more', 'custom_excerpt_more');

```

## 注意点

- assetsの読み込みには、依存関係を作っている。プラグインに応じて調整する必要あり。
    - style：https://thewppress.com/libraries/enqueue-styles/
    - script：https://thewppress.com/libraries/enqueue-scripts/

```php
// assets
    wp_enqueue_style("mainStyle", get_theme_file_uri('style.css'), array('googleFonts', 'googleIcons', 'swiperStyle'), false, 'all');
```