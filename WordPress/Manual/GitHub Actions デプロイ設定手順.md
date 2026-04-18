## 手順 1：GitHub Secrets を登録する

ConoHa WING への SSH 鍵・接続情報は全プロジェクト共通。
変わるのは `REMOTE_PATH`（デプロイ先のテーマディレクトリ）だけ。

> デプロイが止まったら、レンタルサーバコントロールパネルのアクセス制限と シークレットのホスト名・鍵を真っ先に疑う

---

### 方法 A：CLI で登録（推奨）

ターミナルから GitHub CLI で一括登録できる。

#### 事前準備（初回のみ）

```bash
brew install gh
gh auth login
```

#### Secrets 登録コマンド（ステージング: ConoHa）
```bash
# リポジトリ名を変数にセット（都度変える）
REPO="your-github-username/your-repo-name"

# 共通の値（毎回同じ）
gh secret set STAGING_SSH_PRIVATE_KEY --body "$(cat ~/.ssh/conoha_deploy_rsa)" --repo $REPO
gh secret set STAGING_REMOTE_HOST --body "www1016.conoha.ne.jp" --repo $REPO
gh secret set STAGING_REMOTE_USER --body "c7161250" --repo $REPO

# プロジェクトごとに変わる値
gh secret set STAGING_REMOTE_PATH --body "/home/c7161250/public_html/ドメイン/wp-content/themes/テーマ名/" --repo $REPO
```

> **ヒント**: `REMOTE_HOST` と `REMOTE_USER` は毎回同じ値なので、
> シェルのヒストリーから呼び出して `REPO` と `REMOTE_PATH` だけ変えれば OK。

#### Secrets 登録コマンド（本番）
```bash
# 本番用共通の値（お名前.com RSプラン用）
gh secret set PRODUCTION_SSH_PRIVATE_KEY --body "$(cat ~/.ssh/[プロジェクトごとの秘密鍵のファイル名])" --repo $REPO
gh secret set PRODUCTION_REMOTE_HOST --body "[レンタルサーバのホスト名]" --repo $REPO
gh secret set PRODUCTION_REMOTE_USER --body "[レンタルサーバのユーザー名]" --repo $REPO   # ← ここを実際のユーザー名に置き換え
gh secret set PRODUCTION_REMOTE_PORT --body "[レンタルサーバのホスト]" --repo $REPO

# プロジェクトごとに変わる値（デプロイ先パス）
gh secret set PRODUCTION_REMOTE_PATH --body "/home/[レンタルサーバのユーザー名]/public_html/[ドメイン名]/" --repo $REPO
```

---

### 方法 B：ブラウザから登録

1. GitHub でリポジトリを開く
2. **Settings → Secrets and variables → Actions → New repository secret**
3. 以下を 4 つ登録

| Name | 値 | 毎回変わる？ |
|---|---|---|
| `SSH_PRIVATE_KEY` | `~/.ssh/conoha_deploy_rsa` の中身まるごと | いいえ |
| `REMOTE_HOST` | ConoHa WING の SSH ホスト名 | いいえ |
| `REMOTE_USER` | ConoHa WING の SSH ユーザー名 | いいえ |
| `REMOTE_PATH` | デプロイ先テーマのフルパス | **はい** |

#### REMOTE_PATH の形式

```
/home/ユーザー名/public_html/ドメイン名/wp-content/themes/テーマ名/
```

例：
```
/home/fujisakiwahei/public_html/example.com/wp-content/themes/my-theme/
```

> パスが不安な場合は SSH で確認：
> ```bash
> ssh -p 8022 ユーザー名@ホスト名
> ls ~/public_html/
> ```
