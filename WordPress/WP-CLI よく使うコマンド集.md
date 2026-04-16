# WP-CLI よく使うコマンド集

公式リファレンス: https://developer.wordpress.org/cli/commands/

> Local Sites の場合は「Open Site Shell」からコマンドを実行する

---

## 投稿（post）

```bash
# 一覧表示
wp post list
wp post list --post_status=publish --fields=ID,post_title,post_date

# 作成
wp post create --post_title="タイトル" --post_content="本文" --post_status=publish

# カテゴリ付きで作成
wp post create --post_title="タイトル" --post_content="本文" --post_status=publish --post_category=3

# 下書きで作成
wp post create --post_title="タイトル" --post_content="本文" --post_status=draft

# 更新
wp post update 55 --post_title="新しいタイトル"

# 削除（ゴミ箱へ）
wp post delete 55

# 完全削除（ゴミ箱をスキップ）
wp post delete 55 --force

# 複数削除
wp post delete 55 56 57 --force

# 全投稿を削除
wp post delete $(wp post list --field=ID) --force
```

---

## カテゴリ・タグ（term）

```bash
# カテゴリ一覧
wp term list category --fields=term_id,name,slug

# タグ一覧
wp term list post_tag --fields=term_id,name,slug

# カテゴリ作成
wp term create category "カテゴリ名" --slug=slug-name

# タグ作成
wp term create post_tag "タグ名" --slug=slug-name

# 投稿にカテゴリを割り当て
wp post term set 55 category 3

# 投稿にタグを割り当て
wp post term set 55 post_tag 10

# カテゴリ削除
wp term delete category 3

# カテゴリ名を変更
wp term update category 3 --name="新しい名前"
```

---

## 固定ページ（page）

```bash
# 一覧表示
wp post list --post_type=page --fields=ID,post_title,post_status

# 作成
wp post create --post_type=page --post_title="ページ名" --post_status=publish

# テンプレート指定で作成
wp post create --post_type=page --post_title="ページ名" --post_status=publish --page_template=page-contact.php
```

---

## メディア（media）

```bash
# 一覧
wp media list --fields=ID,title,file

# 画像をインポート（ローカルファイル）
wp media import /path/to/image.jpg

# URLから画像をインポート
wp media import https://example.com/image.jpg

# サムネイル再生成
wp media regenerate --yes
```

---

## ユーザー（user）

```bash
# 一覧
wp user list --fields=ID,user_login,user_email,roles

# 作成
wp user create username email@example.com --role=editor --user_pass=password

# パスワード変更
wp user update 1 --user_pass=newpassword

# 削除
wp user delete 2 --reassign=1
```

---

## プラグイン（plugin）

```bash
# 一覧
wp plugin list

# インストール
wp plugin install plugin-slug

# インストール＆有効化
wp plugin install plugin-slug --activate

# 有効化 / 無効化
wp plugin activate plugin-slug
wp plugin deactivate plugin-slug

# 更新
wp plugin update plugin-slug
wp plugin update --all

# 削除
wp plugin delete plugin-slug
```

---

## テーマ（theme）

```bash
# 一覧
wp theme list

# 有効化
wp theme activate theme-slug

# 更新
wp theme update theme-slug
wp theme update --all
```

---

## オプション・設定（option）

```bash
# サイトURL確認
wp option get siteurl
wp option get home

# サイト名
wp option get blogname

# 値を変更
wp option update blogname "新しいサイト名"

# 投稿一覧ページの設定を確認
wp option get page_for_posts
wp option get page_on_front
wp option get show_on_front
```

---

## データベース（db）

```bash
# データベースのエクスポート
wp db export backup.sql

# データベースのインポート
wp db import backup.sql

# SQLクエリを実行
wp db query "SELECT * FROM wp_posts LIMIT 5"

# 検索・置換（ドメイン変更時など）
wp search-replace 'old-domain.com' 'new-domain.com' --dry-run
wp search-replace 'old-domain.com' 'new-domain.com'
```

---

## キャッシュ・トランジェント

```bash
# キャッシュクリア
wp cache flush

# トランジェント削除
wp transient delete --all
```

---

## リライト（パーマリンク）

```bash
# パーマリンク構造を再生成
wp rewrite flush
```

---

## サイト情報の確認

```bash
# WordPress バージョン
wp core version

# PHP バージョン
wp cli info

# 全体的な診断
wp cli info
wp core verify-checksums
```
