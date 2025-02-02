# Graylog Logging System with Docker Compose

このリポジトリでは、Graylogのログ管理システムをDocker Composeを使用して構築する方法を提供します。

## 構成
このシステムは以下の4つの主要なサービスで構成されています。

1. **MongoDB** (`mongo`) - Graylogのメタデータを保存するデータベース
2. **Elasticsearch** (`elasticsearch`) - Graylogのログデータを保存・検索するエンジン
3. **Graylog** (`graylog`) - ログを収集、解析、可視化するメインアプリケーション
4. **Filebeat** (`filebeat`) - ログをGraylogに転送するためのエージェント

## 使用技術
- MongoDB 4.4.18
- Elasticsearch 7.10.2 (OSS版)
- Graylog 4.1
- Filebeat

## 環境変数
Graylogのコンテナは以下の環境変数を使用します。

- `GRAYLOG_PASSWORD_SECRET` - パスワード暗号化のための秘密キー（任意の長い文字列を設定）
- `GRAYLOG_ROOT_PASSWORD_SHA2` - SHA2でハッシュ化された管理者パスワード
- `GRAYLOG_HTTP_EXTERNAL_URI` - Graylog Webインターフェースの外部URL

## ポート設定
以下のポートがホストマシンに公開されます。

- `9000:9000` - Graylog Web UI
- `5044:5044` - Filebeatログ転送用ポート
- `12201:12201/udp` - Syslog UDP入力用ポート

## セットアップ手順
1. `docker-compose up -d` を実行し、すべてのサービスを起動します。
2. ブラウザで [http://127.0.0.1:9000](http://127.0.0.1:9000) にアクセスします。
3. 管理者アカウントでログイン（デフォルトは `admin` / 設定した `GRAYLOG_ROOT_PASSWORD_SHA2`）
4. 必要に応じてGraylogの設定をカスタマイズします。

## ファイル構成
```
.
├── docker-compose.yml   # Docker Compose定義ファイル
├── filebeat/            # FilebeatのDockerビルド用ディレクトリ
│   ├── Dockerfile       # FilebeatのDockerfile
│   ├── filebeat.yml     # Filebeatの設定ファイル
```

## 停止方法
システムを停止するには、以下のコマンドを実行してください。
```sh
docker-compose down
```

## 注意点
- `GRAYLOG_ROOT_PASSWORD_SHA2` にはSHA2でハッシュ化したパスワードを設定してください。
- Elasticsearch 7.10.2を使用しているため、それ以外のバージョンでは互換性に注意してください。
- `filebeat` は `cloudflared.log` のログをGraylogに転送するよう設定されています。必要に応じて変更してください。

## 参考リンク
- [Graylog公式ドキュメント](https://docs.graylog.org/)
- [Elasticsearch公式ドキュメント](https://www.elastic.co/guide/en/elasticsearch/reference/7.10/index.html)
- [Filebeat公式ドキュメント](https://www.elastic.co/guide/en/beats/filebeat/index.html)

