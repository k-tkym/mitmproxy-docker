# mitmproxy Docker Environment

Docker Compose を使用して、透過プロキシ（Transparent Mode）および Web UI（mitmweb）を備えた `mitmproxy` 環境を簡単に構築するためのリポジトリです。

## 特徴
- **Docker Compose** による一発起動
- **mitmweb** によるブラウザからのリアルタイムトラフィック監視（Web UI）
- 透過プロキシモード（Transparent Mode）および WireGuard ポートのサポート
- 自動生成される証明書や設定データをホスト側 (`mitmproxy_data/`) で永続化

## 構成ポート
- `8081`: mitmweb UI 用ポート
- `51820/udp`: WireGuard 用ポート

## 前提条件
- Docker
- Docker Compose

## セットアップと起動手順

### 1. 設定の調整
`docker-compose.yml` を開き、以下の箇所をご自身の環境に合わせて変更してください。

*   **Web UI のホストIP**:
    `--web-host [IP_ADDRESS]` の `[IP_ADDRESS]` を、Docker ホストマシンの IP アドレス（例: `192.168.x.x` や `127.0.0.1`）に変更します。
*   **Web UI のパスワード**:
    `--set web_password=password` の `password` を、任意の安全なパスワードに変更します。

### 2. コンテナの起動
ターミナルで以下のコマンドを実行して起動します。

```bash
docker compose up -d
```

起動すると、リポジトリ内に `mitmproxy_data/` ディレクトリが自動作成され、CA証明書や秘密鍵などのデータが自動生成されます。
> [!IMPORTANT]
> `mitmproxy_data/` に生成されるCA証明書や秘密鍵、設定ファイルなどの機密データは、セキュリティ保護のため `.gitignore` によって Git の管理対象外に設定されています。

### 3. Web UI へのアクセス
ブラウザから以下の URL にアクセスします。

```
http://<設定したIPアドレス>:8081
```

ステップ1で設定したパスワードを入力してログインしてください。

### 4. 証明書のインストール（HTTPS解析用）
HTTPS 通信のインターセプトと解析を行うには、対象のクライアント端末に `mitmproxy` のCA証明書をインストールする必要があります。
自動生成された証明書は `mitmproxy_data/` 内に保存されています。
*   `mitmproxy-ca-cert.pem`
*   `mitmproxy-ca-cert.p12`
*   `mitmproxy-ca-cert.cer`

また、プロキシ設定が完了した端末のブラウザから [http://mitm.it](http://mitm.it) にアクセスすることでも、各プラットフォーム用の証明書を簡単にダウンロード・インストールできます。

## コンテナの停止

コンテナを停止して削除するには、以下のコマンドを実行します。

```bash
docker compose down
```
