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
または
docker compose exec redis redis-cli
```

#### CLI から抜ける

> exit

#### CLI の終了（終了時に保存される）

> shutdown

#### データベースの選択

> select

例: 1 番目の db へ接続する場合

> select 1

memo: デフォルトは 0

#### データの保存

> bgsave

memo: コマンドを打つと、`Background saving started`が表示（サーバー側もログが出る）

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

# 値のセットと取得

## セット

> set key value
> mset key value key value ...

## 有効期限付きでセット

> setex key time value

例: 5 秒間の有効期限でセット

> setex score 5 100

### 残り時間を確認

ttl key

## 取得

> get key
> mget key key key ...

#　数値の増減の操作
インクリメント

> incr key

例: 元の score の値に+1 される

> incr score

> incrby key number

例: 元の score の値に+10 される

> incrby score 10

デクリメント

> decr key

例: 元の score の値に-1 される

> decr score

> decrby key

例: 元の score の値に-10 される

> decrby score 10

# key の一覧取得

> keys pattern

例: 全てのキーを取得

> keys \*

例: m を含むキーを取得

> keys \*m\*

# ランダムに key を表示

> randomkey

# key の存在確認

> exists key

例: score という key が存在するか

> exists score

# key 名を変更

> rename old_name new_name

例: key 名を old_name から new_name へ変更

> rename old_name new_name

# key を削除

> del key

例: score の key を削除

> del score

# List（配列）

## 末尾に追加

> rpush list_name value

## 末尾の要素を削除

> rpop list_name

## 先頭の要素を削除

> lpop list_name

## 確認

memo: ここでの「l」は left、right の「l」ではなく、List の「l」

> lrange list_name index_top index_end

例: mycolor の 0 番目から 2 番目を取得

> lrange mycolor 0 2

### 先頭から 2 番目を取得

> lindex list_name number

### 要素数

> llen list_name

### トリム

> ltrim list_name index_start index_end

# Set（順不同）

## 追加

> sadd set_name value

## 確認

> smembers set_name

## 値の削除

> srem set_name value

## 和集合（全てを結合）

> sunion set_name1 set_name2

## 積集合（共通の値のみ）

> sinter set_name1 set_name2

## 差集合

> sdiff set_name1 set_name2

## 集合から新たな Set を作成

例: 和集合から新たな Set を作成する

> sunionstore new_set_name set_name1 set_name2

# Sorted Set

## 追加

> zadd set_name number value

## 数値が低いものからソート

> zrange set_name 0 -1

## 数値が高いものからソート

> zrevrange set_name 0 -1

## 数値が低いものからランキング

> zrank set_name value

## 数値が高いものからランキング

> zrevrank set_name value
