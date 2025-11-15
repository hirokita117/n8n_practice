# n8n Local Development Environment

このリポジトリは、n8nをローカル環境でDocker Composeを使って動かすためのセットアップです。

## 概要

- **n8n**: ワークフロー自動化ツール
- **ポート**: `127.0.0.1:5678` でアクセス可能
- **データ永続化**: `./n8n-data` ディレクトリにn8nのデータを保存
- **ファイル共有**: `./local-files` ディレクトリをn8nコンテナ内の `/files` にマウント

## セットアップ手順

### 1. リポジトリのクローン

```bash
git clone https://github.com/hirokita117/n8n_practice.git
cd n8n_practice
```

### 2. 環境変数の設定

`.env.example` をコピーして `.env` ファイルを作成します：

```bash
cp .env.example .env
```

### 3. N8N_ENCRYPTION_KEY の生成

n8nでは、認証情報などの機密データを暗号化するために `N8N_ENCRYPTION_KEY` が必要です。

以下のコマンドで32バイトのランダムな暗号化キーを生成できます：

```bash
openssl rand -hex 32
```

生成されたキーを `.env` ファイルの `N8N_ENCRYPTION_KEY` に設定してください：

```
N8N_ENCRYPTION_KEY=生成されたキーをここに貼り付け
GENERIC_TIMEZONE=Asia/Tokyo
```

**重要**:
- この暗号化キーは**絶対に変更しないでください**。変更すると既存の認証情報が使えなくなります。
- `.env` ファイルは `.gitignore` に含まれているため、Gitにコミットされません。
- 本番環境では、このキーを安全に管理してください。

### 4. n8nの起動

```bash
docker-compose up -d
```

### 5. アクセス

ブラウザで以下のURLにアクセスします：

```
http://localhost:5678
```

初回アクセス時にアカウント設定が求められます。

## 基本的な使い方

### コンテナの状態確認

```bash
docker-compose ps
```

### ログの確認

```bash
docker-compose logs -f n8n
```

### コンテナの停止

```bash
docker-compose down
```

### コンテナの再起動

```bash
docker-compose restart
```

## ディレクトリ構成

```
.
├── docker-compose.yml   # Docker Compose設定ファイル
├── .env                 # 環境変数（gitignoreされています）
├── .env.example         # 環境変数のサンプル
├── n8n-data/           # n8nのデータ永続化ディレクトリ
└── local-files/        # n8nからアクセス可能なファイル置き場
```

## トラブルシューティング

### ポートが既に使用されている

ポート5678が既に使用されている場合は、`docker-compose.yml` のポート設定を変更してください：

```yaml
ports:
  - "127.0.0.1:8080:5678"  # 例: 8080に変更
```

### データのリセット

n8nのデータを完全にリセットしたい場合：

```bash
docker-compose down
rm -rf n8n-data/
docker-compose up -d
```

**注意**: この操作により、すべてのワークフローと設定が削除されます。

## 参考リンク

- [n8n公式ドキュメント](https://docs.n8n.io/)
- [n8n GitHub](https://github.com/n8n-io/n8n)
