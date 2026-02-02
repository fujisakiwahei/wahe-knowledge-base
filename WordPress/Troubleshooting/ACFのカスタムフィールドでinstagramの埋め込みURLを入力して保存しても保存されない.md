# ACFのカスタムフィールドでinstagramの埋め込みURLを入力して保存しても保存されない

タイプ: トラブルシューティング

## 現象

タイトルの通り。ローカルでは反映されるがConoHaのテストサーバーでは不可。

https://gyazo.com/a904961e3dac5a5536b9a0a087ff236f

→再度開いた時に表示されるリンクは、以前入れていたもの。変更が更新されない。

## 原因

複数考えられる原因があった。特定できず

1. instagramの埋め込みコードが6,000文字程度と長い
2. 途中で分割してみて、1,000文字程度にしたら保存が反映されたので文字数が原因かも
3. ACFの文字数制限
4. php.iniの設定：[https://www.netimpact.co.jp/diary/23622/](https://www.netimpact.co.jp/diary/23622/)

## 解決方法

1. 埋め込みではなく、他の人が作成した埋め込み用のコードを使用したら期待通りの動作をした。記述するURLは埋め込み用ではなくただの投稿URL。
    
    ```php
    〜ここから下〜
    <blockquote class="instagram-media" data-instgrm-version="5" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:658px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);">
    
    <a href=“ここにリンクを入れる” target="_blank"></a>
    
    </blockquote>
    
    <script async defer src="//platform.instagram.com/en_US/embeds.js"></script>
    〜ここまで〜
    ```
    

## 参考リソース

- https://noritoikioi.hatenablog.com/entry/insta_2