# API エンドポイント設計 (仮)
自宅サーバのサーバサイド API の設計
- GET: サーバ内のファイル情報の取得、サーバからのダウンロード
- POST: APP からのアップロード
- PATCH/PUT: ファイル情報の更新

## 基本情報
- データ形式: `JSON`
- リクエストURL: https://localhost:8080/v1/{`エンドポイント`}

## エンドポイント一覧 (仮)
| メソッド | エンドポイント | 内容 | ドキュメント |
| :--- | :--- | :--- | :--- |
| GET | `/contents` | ファイル・ディレクトリ一覧を取得 | api-fs-operations.md |
| GET | `/metadata` | ファイル・ディレクトリ詳細情報取得 | api-fs-operations.md |
| GET | `/file` | 指定されたデータをダウンロード | [api-transfer.md](api-transfer.md) |
| POST | `/file` | 指定されたデータをアップロード | [api-transfer.md](api-transfer.md) |

## レスポンスフォーマット
### 形式
- Content-Type: `application/json`
- 文字コード: `UTF-8`

### HTTP ステータスコード
| ステータスコード | 説明 | レスポンス事例 |
| :--- | :--- | :--- |
| 200 | `OK`: 正常処理 | ファイルの取得や更新が成功した場合 |
| 201 | `Created`: アップロード完了 | データのアップロードに成功した場合 |
| 204 | `No Content`: 返却値がない | 対象データが削除された場合 |
| 400 | `Bad Request`: リクエスト値が期待されたものではない | パラメータフォーマットが期待されたものではない場合 |
| 401 | `Unauthorized`: ログインしていない | ログイン認証に一致しない場合 |
| 403 | `Forbidden`: 適切な権限がない | ファイルにアクセスする権限がない場合 |
| 404 | `Not Found`: データが見つからない | 存在しないエンドポイントやデータにアクセスしようとした場合 |
| 405 | `Method Not Allowed`: 許可されていないメソッドでリクエスト実施 | 対象パスで許可されていないメソッドでリクエストした場合 |
| 409 | `Conflict`: 競合 | 登録・更新しようとしたデータが既存であった場合 |
| 413 | `Payload Too Large`: サイズ超過 | 登録しようとしたデータのサイズが規定以上だった場合 |
| 415 | `Unsupported Media Type`: 許可されていないファイル形式 | ブラックリストにあるファイル形式の場合 |
| 500 | `Internal Server Error`: サーバ処理でのエラー発生時 | 処理中にサーバが通信不良で切れた場合 |

### レスポンスボディ
#### 正常
各種エンドポイントに準ずる
- [api-transfer.md](api-transfer.md)

#### エラー ([RFC 9457](https://tex2e.github.io/rfc-translater/html/rfc9457.html) 準拠)
エラー発生時は `application/problem+json` 形式で詳細を返します。

| パラメータ | 型 | 値の例 | 説明 |
| :--- | :--- | :--- | :--- |
| type | `string` | `https://localhost:8080/v1/errors/not-found` | エラー分類を示すURL <br>詳細は各種ドキュメントに記載|
| instance | `string` | `/contents` | エラーが発生したエンドポイント |
| status | `int` | `404` | HTTPステータス値 |
| title | `string` | `"Not Found"` | エラーの概要 |
| detail | `string` | `"The requested file could not be found."` | ユーザ向けのエラー詳細 |


#### サンプル
```
{
    "type": "https://localhost:8080/v1/errors/not-found",
    "instance": "/contents",
    "status": 404,
    "title": "Not Found",
    "detail": "The requested file could not be found."
}
```
