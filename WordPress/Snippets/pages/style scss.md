# style.scss

タイプ: スニペット

## 利用シーン（あれば画像）

- プロジェクト立ち上げ時に作成。
- `assets/scss/` に作成して、コンパイル後のcssはルートに生成されているとよい。

## コード

- WPの場合
    
    ```scss
    @charset "UTF-8";
    
    /*
    Theme Name: My Awesome Theme  // テーマ名
    Author: Fujisaki Wahei
    Description: 合同会社SakiのHPリニューアル
    Version: 1.0
    */
    
    // 個別ファイル読み込み ベース
    @use "./reset";
    @use "./common";
    @use "./var";
    
    // 個別ファイル読み込み 固定
    @use "./header";
    @use "./footer";
    @use "./pages/front";
    
    // 個別ファイル読み込み 追加
    
    ```
    
- WPじゃない場合
    
    ```scss
    @charset "UTF-8";
    
    // 個別ファイル読み込み ベース
    @use "./reset";
    @use "./common";
    @use "./var";
    
    // 個別ファイル読み込み 固定
    @use "./header";
    @use "./footer";
    
    // 個別ファイル読み込み 追加
    @use "./front-page";
    ```
    

## 注意点

- ファイルのパスや読み込むファイルは、必要に応じて変更

## AIにお願いするプロンプト

```html
assets/scss/ 内にあるscssファイルを読み込むコードを最下部に追加して。

## 例 
```
@use "./pages/front";
```
```