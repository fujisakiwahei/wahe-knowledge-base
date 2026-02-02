# WPサイト、サーバーへのアップ（初回・修正）

タイプ: フロー

## All in One WP Migrationのプラグインデータ

[https://drive.google.com/file/d/1XVu1rOHe5_9CcolYXt5chdH_yDujRpFM/view?usp=drive_link](https://drive.google.com/file/d/1XVu1rOHe5_9CcolYXt5chdH_yDujRpFM/view?usp=drive_link)

## テストサーバー・本番サーバーへの初回アップ

- [ ]  PHPのバージョンをチェック
    - ローカルのPHPのバージョン：
    - サーバーのPHPのバージョン：
- [ ]  ローカルから、All-in-One WP Migrationのデータをエクスポート
- [ ]  [Googleドライブのprojectのディレクトリ](https://drive.google.com/drive/folders/14X_9sgDhZqrXTgBsEmaqtQik5ifzlbKy?usp=drive_link)に案件フォルダを作ってAll-in-One WP Migrationのデータを保存
    
    [例](https://gyazo.com/442ca2cf274b24685dfdce29f3069683)
    
    例
    
- [ ]  アップするサーバにログインして、WordPressをインストール
- [ ]  WordPressの管理画面から、All-in-One WP Migrationをインストール
- [ ]  ローカルからエクスポートしたファイルをインポート
- [ ]  動作チェック

## テストサーバー・本番サーバーへの修正アップ

- [ ]  本番サーバから、All-in-One WP Migrationのデータをエクスポート
- [ ]  [Googleドライブのprojectのディレクトリ](https://drive.google.com/drive/folders/14X_9sgDhZqrXTgBsEmaqtQik5ifzlbKy?usp=drive_link)にAll-in-One WP Migrationのデータを保存
- [ ]  ローカルサーバに、All-in-One WP Migrationのデータをインポート
    - 管理画面からの変更をしている可能性が高いので、環境を揃えておく
- [ ]  ローカルで修正
- [ ]  **管理画面の変更がある場合→**All-in-One WP Migrationでテストサーバにアップロード
- [ ]  **テーマファイルだけの変更の場合→**FTPで、変更したファイルをテストサーバにアップロード
- [ ]  動作確認ができたら↑と同じ手順で本番アップ