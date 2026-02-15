# Dockerまとめ（心得 + 使い方 + 罠）

## 心得
- **イメージは不変、コンテナは使い捨て**: 状態は外に逃がす。
- **小さく作る**: 1コンテナ1役割、最小ベースイメージ。
- **再現性が命**: Dockerfileとタグ運用を厳密に。
- **セキュリティは最初から**: 非root、脆弱性スキャン。
- **ローカルと本番を近づける**: 環境差分を減らす。

---

## 1. 基本用語
- **Image**: 実行環境の設計図（不変）
- **Container**: Imageから作られる実行単位
- **Registry**: Imageの保管場所（Docker Hub / ECR など）
- **Volume**: 永続データ領域
- **Network**: コンテナ間通信

---

## 2. 基本コマンド
```bash
# イメージ取得
Docker pull nginx

# コンテナ起動
Docker run -d -p 8080:80 --name web nginx

# 起動中コンテナ確認
Docker ps

# すべてのコンテナ
Docker ps -a

# 停止・削除
Docker stop web
Docker rm web

# イメージ削除
Docker rmi nginx
```

---

## 3. Dockerfile 基本
```Dockerfile
FROM python:3.12-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

### よく使う命令
- `FROM`, `RUN`, `COPY`, `ADD`, `CMD`, `ENTRYPOINT`, `ENV`, `EXPOSE`

---

## 4. ベストプラクティス
- **マルチステージビルド**でサイズ削減
- **layer最適化**（変更頻度の低い命令を上に）
- `COPY`より`ADD`は慎重に
- **非root実行**
- `CMD`/`ENTRYPOINT`の違いを理解

---

## 5. コンテナネットワーク
```bash
# ネットワーク作成
Docker network create app-net

# ネットワーク指定で起動
Docker run -d --name db --network app-net mysql
Docker run -d --name api --network app-net my-api
```

---

## 6. Volume（永続化）
```bash
# ボリューム作成
Docker volume create data-vol

# ボリュームマウント
Docker run -d -v data-vol:/var/lib/mysql mysql
```

---

## 7. docker-compose
```yaml
version: "3.9"
services:
  app:
    build: .
    ports:
      - "8080:8080"
  db:
    image: postgres:16
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:
```

```bash
Docker compose up -d
Docker compose down
```

---

## 8. よくある罠
- **Dockerfileのキャッシュが効かない**（順序が悪い）
- **rootで動かす**（権限リスク）
- **巨大なイメージ**（無駄な依存）
- **ログがファイルに出る**（コンテナ標準出力に出す）
- **データの永続化忘れ**（Volumeなし）

---

## 9. セキュリティ
- **最小権限**: `USER`指定
- **脆弱性スキャン**: Trivy など
- **秘密情報は埋め込まない**: ENVではなくシークレット管理

---

## 10. 運用
- **タグ運用**: `latest`依存は避ける
- **ログ集約**: Fluentd / Loki など
- **監視**: cAdvisor, Prometheus
- **イメージの棚卸し**

---

## 11. もっと深めたい
- Kubernetes連携
- BuildKit / buildx
- Rootless Docker
- OCI規格

---

## まとめ
Dockerは「再現性のある実行環境」を作るための最重要ツール。イメージは不変、コンテナは使い捨て、状態は外部化。この原則を守ると運用が安定する。
