# Zabbix-with-Docker
ZabbixをDocker Compose + CFTunnelで簡単に構築

このリポジトリは、Docker Compose と環境変数ファイル（`.env`）を使って、Zabbix（PostgreSQL + Nginx + CloudFlareTunnel）を手軽に立ち上げるための設定をまとめています  

---

## 📦 前提条件

- Docker
- Docker Compose
- Linux / macOS / Windows 環境
- Cloudflare Tunnel（`cloudflared`）利用のためのトークン取得済み

---

## 📂 ディレクトリ構成

```
.
├── docker-compose.yml    # docker compose
├── .env                  # 環境変数ファイル
└── postgres/             # PostgreSQL データ永続化用バインド
```

---

## 🔧 環境変数（.env）

```ini
# Database
POSTGRES_USER=zabbix
POSTGRES_PASSWORD=zabbix-db-password
POSTGRES_DB=zabbix

# Zabbix Server
DB_SERVER_HOST=postgres

# Zabbix Web
ZBX_SERVER_HOST=zabbix-server
DB_SERVER_HOST=postgres

# 言語・ロケール・タイムゾーン
ZBX_DEFAULT_LANGUAGE=ja_JP      # Web UI デフォルト言語
LANG=ja_JP.UTF-8                # OS ロケール（UTF-8）
LC_ALL=ja_JP.UTF-8              # OS ロケール（全域適用）
PHP_TZ=Asia/Tokyo               # PHP タイムゾーン
TZ=Asia/Tokyo                   # コンテナ OS タイムゾーン

# Cloudflared Tunnel
TUNNEL_TOKEN=your-tunnel-token          # Tunnel トークン
TUNNEL_TRANSPORT_PROTOCOL=http2         # 通信プロトコル
```  

※ 必要に応じて `POSTGRES_PASSWORD` や `TUNNEL_TOKEN` を書き換えてください

---

## 🚀 構築手順

1. リポジトリをクローン

```bash
git clone git@github.com:kuwacom/Zabbix-with-Docker.git
cd Zabbix-with-Docker
```  

2. 環境変数ファイルを作成/編集

```bash
cp .env.example .env
# 好みに合わせて .env の値を編集
```  

3. コンテナを起動

```bash
docker-compose up -d
```  

4. 動作確認

- Zabbix Web:
    - HTTP  : http://localhost:8080
    - HTTPS : https://localhost:8443
    - 初期ログイン情報
    - ユーザー: Admin
    - パスワード: zabbix

- Zabbix Server ポート: 10051

---

## 🛠️ 停止・削除

- 停止
```bash
docker-compose stop
```

---

## 📝 注意点

- PostgreSQL データは `./postgres` フォルダに永続化されます  誤って消去しないよう注意してください
- Cloudflare Tunnel を使用しない場合は、`cloudflared` docker composeサービスセクションを削除して構いません
- タイムゾーンやロケールは `.env` で自由に変更可能です

---