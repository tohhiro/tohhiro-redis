# Redis 学習プロジェクト

このプロジェクトは Redis の学習用環境です。

## 必要な環境

- Docker
- Docker Compose

## セットアップ

### Redis の起動

#### Redis-server の起動

```bash
docker-compose up
```

#### CLI で接続

```bash
docker exec -it redis-learning redis-cli
```

#### CLI から抜ける

> exit

#### CLI の終了（終了時に保存される）

> shutdown

#### データベースの選択

(WIP)

#### データの保存

（WIP）

#### GUI で接続（Redis Commander）

ブラウザで以下の URL にアクセス:

```
http://localhost:8081
```

### Redis の停止

```bash
docker-compose down
```

### データを含めて削除

```bash
docker-compose down -v
```

## サービス

- **Redis**: ポート `6379` でアクセス可能
- **Redis Commander**: ポート `8081` で Web ブラウザからアクセス可能な GUI ツール

## ディレクトリ構成

```
.
├── docker-compose.yml   # Docker Compose設定
├── redis.conf          # Redis設定ファイル
└── README.md          # このファイル
```

## データの永続化

- Redis のデータは `redis-data` という名前付きボリュームに保存されます
- AOF（Append Only File）による永続化が有効になっています

## カスタマイズ

### Redis 設定の変更

`redis.conf` ファイルを編集後、コンテナを再起動してください:

```bash
docker-compose restart redis
```

### パスワード認証の有効化

`redis.conf` の以下の行のコメントを外してパスワードを設定:

```
requirepass your_password_here
```

## トラブルシューティング

### ポートが既に使用されている場合

`docker-compose.yml` のポート番号を変更してください:

```yaml
ports:
  - "6380:6379" # ホスト側のポートを変更
```

### ログの確認

```bash
docker-compose logs redis
```

## 学習リソース

- [Redis 公式ドキュメント](https://redis.io/docs/)
- [Redis コマンドリファレンス](https://redis.io/commands/)
- [Try Redis](https://try.redis.io/) - インタラクティブなチュートリアル

## Database

- Key value
  example
  name "tohhiro"
  emai "tohhiro@gmail.com"
  score 120

# Data Type

- String: 個々の要素
- List: 順番に並べた複数の要素
  -- 時系列的なデータ

- Set: 順不同の複数の要素。重複を許さない
  -- タグ、ソーシャルグラフ

- Sorted Set: Set の特徴を持ちつつ、ここの要素にスコア付き
  -- ラインキング

- Hash
  -- 連想配列（ラベルと値のセット）
