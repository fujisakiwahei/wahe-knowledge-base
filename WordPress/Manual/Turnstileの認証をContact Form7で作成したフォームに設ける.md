# Turnstileの認証をContact Form7で作成したフォームに設ける

タイプ: フロー
技術・ツール: WordPress

## 🚀 ステップ1：Cloudflare Dashboardアクセス

1. *[https://dash.cloudflare.com/**](https://dash.cloudflare.com/**) にログイン
2. 左サイドバー → 「**Turnstile**」をクリック
3. 「**Add site**」または「**サイトを追加**」ボタンをクリック

---

## 🏷️ ステップ2：サイト情報入力

| 項目 | 入力内容（例） | 注意点 |
| --- | --- | --- |
| **サイト名** | `WordPress_Contact` | 管理用の名前 |
| **ドメイン** | `example.com` | **メインドメインのみ** |
| **モード** | `Managed` | ✅ UX最良の推奨モード |

> ✅ 正： example.com
❌ 誤： www.example.com, *.example.com
> 

---

## 🔑 ステップ3：キー取得

1. 「**Create**」をクリック
2. 以下のキーをコピーして保存します。
    - 📌 **サイトキー：** `1xxxxxxxxxxxxxxxx` (公開鍵)
    - 📌 **シークレットキー：** `0xxxxxxxxxxxxxxxx` (秘密鍵)

---

## ⚙️ ステップ4：登録内容の確認

1. Turnstile管理画面 → 作成したサイトをクリック
2. 「**Hostnames**」が `example.com` となっていることを確認します。
3. 必要に応じて、`www.example.com` やサブドメインを**ここから**追加します。

---

## 🖊️ ステップ5：Contact Form7のインテグレーションに設定

1. WordPressダッシュボード → 「**お問い合わせ**」 → 「**インテグレーション**」をクリック
2. 「**Cloudflare Turnstile**」の「**インテグレーションを追加**」をクリック
3. 取得した**サイトキー**と**シークレットキー**を入力して保存します。