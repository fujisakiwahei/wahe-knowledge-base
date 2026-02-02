# WordPressのカテゴリを取得する

タイプ: スニペット

## 利用シーン（あれば画像）

- カテゴリのリストを取得して出力する時など

## コード

```php
<?php
$terms = get_terms(array(
    'taxonomy' => 'blogs_category',
    'hide_empty' => false,
));
?>
<ul class="blogSidebarCategoryList">
    <?php foreach ($terms as $term) : ?>
        <li class="blogSidebarCategoryItem">
            <a href="<?php echo get_term_link($term); ?>"><?php echo $term->name; ?></a>
        </li>
    <?php endforeach; ?>
</ul>
```

## 注意点