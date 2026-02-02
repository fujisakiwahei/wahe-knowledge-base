# WordPress 開発環境構築

タイプ: フロー

## 利用シーン（あれば画像）

- 開発環境構築の基本フロー

## やること

1. Local を立ち上げ
   1. 開発メモに情報は記載しておく
2. Git のセットアップ
   1. ＋ボタンからリポジトリを作る
   2. 作業ディレクトリを作成したい場所の 1 階層上に移動
      1. Local を使用している場合は、theme ファイルに移動
   3. `git clone {リポジトリのURL}`でローカルにリポジトリをコピー
   4. 適当なファイル（readme など）を作成して、`git add .`でステージング
   5. `git commit -m “コメント”`で、ローカルのリポジトリに変更を保存
   6. `git push` でリモートにプッシュ
3. ホットリロードの導入
   1. [【BrowserSync】WordPress のローカル環境をホットリロードさせる方法](%E3%80%90BrowserSync%E3%80%91WordPress%E3%81%AE%E3%83%AD%E3%83%BC%E3%82%AB%E3%83%AB%E7%92%B0%E5%A2%83%E3%82%92%E3%83%9B%E3%83%83%E3%83%88%E3%83%AA%E3%83%AD%E3%83%BC%E3%83%89%E3%81%95%E3%81%9B%E3%82%8B%E6%96%B9%E6%B3%95%201b5df78c372e808da5a3e4dad93d5ffa.md)
4. WordPress 初期設定

   1. サイトの言語
   2. タイムゾーン
   3. 外観 → テーマ変更
   4. ホームページの表示設定
   5. プラグイン

      - Advanced Custom Fields
      - All in One SEO
      - All-in-One WP Migration and Backup
      - Autoptimize
      - Contact Form 7
      - Custom Post Type UI
      - EWWW Image Optimizer
      - SiteGuard WP Plugin
      - W3 Total Cache
      - Yoast Duplicate Post

      上記 9 つをインストールしてアクティブにするコマンド（Local > サイトを右クリック > Site shell で CLI を起動）

      SiteGuard WP Plugin だけ手動で入れる必要あり。

      ```bash
      wp plugin install advanced-custom-fields all-in-one-seo-pack all-in-one-wp-migration autoptimize contact-form-7 custom-post-type-ui ewww-image-optimizer w3-total-cache duplicate-post --activate
      ```

5. フォントの読み込み。Google フォントなど。

   ```bash
   @functions.php
   以下を別々に読み込んで

   Fugaz One
   Noto Sans JP
   ```
