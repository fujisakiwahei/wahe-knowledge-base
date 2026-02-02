# Shopify開発ストア作成→Dawn環境構築マニュアル

Shopify開発ストアでDawnテーマのローカル開発環境を構築する手順書

## 前提条件

- Shopify CLI インストール済み
- Shopify Partnerアカウント登録済み（[https://partners.shopify.com](https://partners.shopify.com/)）
- 開発用ストア作成済み

## 1. 開発用ストアの作成（簡易版）

Partner Dashboard（[https://partners.shopify.com](https://partners.shopify.com/)）にログイン後、以下の手順で作成：

1. 「Stores」→「Add store」→「Create development store」
2. 「Create a store for a client」を選択
3. ストア名とURLを入力
4. 「Create development store」をクリック

※クライアント案件の場合は必ず「Create a store for a client」を選択することで、後からストア譲渡が可能になります

## 2. Dawnテーマの有効化

### 2-1. WebでDawnテーマを準備

1. Shopify管理画面にログイン
2. `オンラインストア > テーマ` に移動
3. `テーマライブラリを見る` または `テーマを追加` をクリック
4. **Dawn** を検索して `追加` をクリック
5. 追加されたDawnテーマの `公開する` をクリック（または公開せずに保持）

### 2-2. 作業ディレクトリに移動

ターミナルで開発プロジェクトを配置したい場所に移動します。

```
cd ~/projects/shopify/itoshou/dawn/
// など
```

### 2-3. Dawnテーマをローカルにダウンロード

```
shopify theme pull --store [ストアドメイン] --live

```

ここで案件名やクライアント名を入力してください（例：`tanaka-shop-2026`）。

## 4. 開発サーバーの起動

### 4-1. ストアに接続して開発サーバーを起動

```
shopify theme dev --store itoshou.myshopify.com

```

ストアドメインの指定方法：

- プレフィックスのみ：`example`
- 完全なURL：`example.myshopify.com`

### 4-2. 初回認証

初めて実行する場合、ブラウザが開いてShopify Partner/ストアへのログインが求められます。認証後、以下の情報がターミナルに表示されます：

```
✔ Synced theme #123456789 on example.myshopify.com

Please open this URL in your browser:
<http://127.0.0.1:9292>

Customize this theme in the Theme Editor:
<https://example.myshopify.com/admin/themes/123456789/editor>

Share this theme preview:
<https://example.myshopify.com/?preview_theme_id=123456789>

```

### 4-3. プレビューURLの確認

- **ローカルプレビュー**：`http://127.0.0.1:9292`（ホットリロード対応）
- **管理画面エディター**：テーマエディターへの直リンク
- **共有可能プレビュー**：チームメンバーと共有できるURL

ブラウザで `http://127.0.0.1:9292` にアクセスし、Dawnテーマが表示されれば環境構築完了です。

## よく使うコマンド

### テーマをストアにプッシュ

```
shopify theme push --store [ストア名]

```

### テーマをストアからプル

```
shopify theme pull --store [ストア名]

```

### ストア上のテーマ一覧を表示

```
shopify theme list --store [ストア名]

```

### 開発サーバーのオプション

```
# 自動的にブラウザで開く
shopify theme dev --store [ストア名] --open

# ポート番号を変更
shopify theme dev --store [ストア名] --port 3000

# ファイル削除を同期させない
shopify theme dev --store [ストア名] --nodelete

```

## 公式ドキュメント

- Shopify CLI 公式：[https://shopify.dev/docs/api/shopify-cli](https://shopify.dev/docs/api/shopify-cli)
- theme init コマンド：[https://shopify.dev/docs/api/shopify-cli/theme/theme-init](https://shopify.dev/docs/api/shopify-cli/theme/theme-init)
- theme dev コマンド：[https://shopify.dev/docs/api/shopify-cli/theme/theme-dev](https://shopify.dev/docs/api/shopify-cli/theme/theme-dev)
- テーマアーキテクチャ：[https://shopify.dev/docs/storefronts/themes/architecture](https://shopify.dev/docs/storefronts/themes/architecture)
- 開発用ストア作成：[https://shopify.dev/docs/apps/build/dev-dashboard/development-stores](https://shopify.dev/docs/apps/build/dev-dashboard/development-stores)

## 補足：テーマディレクトリの役割

各ディレクトリの詳細な役割は公式ドキュメント（[https://shopify.dev/docs/storefronts/themes/architecture](https://shopify.dev/docs/storefronts/themes/architecture)）を参照してください。

- `assets`：画像、CSS、JavaScriptファイルを格納
- `config`：テーマ設定のJSONファイル
- `layout`：全ページ共通のレイアウト（`theme.liquid`が必須）
- `locales`：多言語対応の翻訳ファイル
- `sections`：マーチャントがカスタマイズ可能な再利用モジュール
- `snippets`：小さな再利用可能なLiquidコード
- `templates`：各ページタイプのテンプレート