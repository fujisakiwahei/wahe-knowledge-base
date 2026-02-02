# WordPressで、ページネーションを実装

タイプ: スニペット
技術・ツール: WordPress

## 利用シーン（あれば画像）

## コード

```php
 <div class="news-pagination">
    <?php
    echo paginate_links(array(
        'total' => $query->max_num_pages,
        'current' => max(1, get_query_var('paged')),
        'format' => '?paged=%#%',
        'show_all' => false,
        'end_size' => 1,
        'mid_size' => 1,
        'prev_next' => true,
    ));
    ?>
</div>
```

```scss

```

## 注意点

| パラメーター | 初期値 | 意味 |
| --- | --- | --- |
| show_all | false | すべてのページを表示するかどうか |
| end_size | 1 | リストの開始端と終了端のいずれかにある数字の数 |
| mid_size | 2 | 現在のページの両側にある数字の数 |
| prev_next | true | リストに前と次のリンクを含めるかどうか |
| prev_text | « 前へ | 前のページのテキスト |
| next_text | 次へ » | 次のページのテキスト |
| type | plain | 戻り値の形式を制御します |
| before_page_number | なし | ページ番号の前に表示される文字列 |
| after_page_number | なし | ページ番号の後に追加する文字列 |