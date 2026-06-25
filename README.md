# Frigate NVR

2台のRTSPカメラストリームをAI物体検知付きで監視するFrigate設定。

## 構成

- カメラ2台（mediamtx経由のRTSPストリーム）
- AI物体検知（CPU推論）: 人、車、猫、犬
- 常時録画（1日保持）
- Web UI: LAN内からアクセス可能

## セットアップ

### 1. 環境変数の設定

```bash
cp .env.example .env
```

`.env` を編集して値を設定する。

```
FRIGATE_RTSP_USER=       # RTSPユーザー名
FRIGATE_RTSP_PASSWORD=   # RTSPパスワード
FRIGATE_CAMERA1_IP=      # カメラ1のIPアドレス
FRIGATE_CAMERA2_IP=      # カメラ2のIPアドレス
```

### 2. 起動

```bash
docker compose up -d
```

### 3. アクセス

```
http://<ホストのIP>:8080
```

## ラズパイで動かす場合

カメラ1と同じラズパイ上で動かす場合は `.env` の `FRIGATE_CAMERA1_IP` を `localhost` に設定する。

```
FRIGATE_CAMERA1_IP=localhost
```

## ディレクトリ構成

```
.
├── config/
│   └── config.yml       # Frigate設定ファイル
├── storage/             # 録画データ（.gitignore対象）
├── .env                 # 環境変数（.gitignore対象）
├── .env.example         # 環境変数サンプル
└── docker-compose.yml
```

## パフォーマンス

Apple Silicon MacではDockerコンテナ内からGPUが使えないためCPU推論になる。
負荷が高い場合は `config/config.yml` の `fps` を下げて対応する。

Google Coral USB TPUを追加することで大幅な高速化が可能。
