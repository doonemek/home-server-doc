# API エンドポイント設計 (仮)
自宅サーバのサーバサイド API の設計
- GET: サーバ内のファイル情報の取得、サーバからのダウンロード
- POST: APP からのアップロード
- PATCH/PUT: ファイル情報の更新

## 基本情報
- データ形式: `JSON`
- リクエストURL: https://localhost:8080/v1/{`エンドポイント`}

## エンドポイント一覧 (仮)
| メソッド | エンドポイント | 内容 |
| :--- | :--- | :--- |
| GET | `/dirlist` | ディレクトリファイル一覧を取得 |
| POST | `/upload` | 特定のデータをアップロードする |
| GET | `/download` | 特定のデータをダウンロードする |

## エンドポイント詳細
- [api-fs-operations.md](api-fs-operations.md): ファイル操作系
  - `/dirlist`
    - GET: ファイル・ディレクトリ一覧取得
  - `/file`
    - GET: ファイルの詳細情報取得
    - PATCH/PUT: ファイルの更新
    - DELETE: ファイルの削除
- [api-transfer.md](api-transfer.md): データ転送系
  - `/upload`
    - POST: ファイル/フォルダのアップロード
  - `/download`
    - GET: ファイル/フォルダのダウンロード
      - フォルダ指定時は `.zip` で返却予定

