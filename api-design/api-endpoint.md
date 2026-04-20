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
| GET | `/contents` | ディレクトリ・ファイル一覧を取得 |
| POST | `/upload` | 特定のデータをアップロードする |
| GET | `/download` | 特定のデータをダウンロードする |

## エンドポイント詳細
- [api-fs-operations.md](api-fs-operations.md): ファイル操作系
  - `/contents`
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
| 500 | `Internal Server Error`: サーバ処理でのエラー発生時 | 処理中にサーバが通信不良で切れた場合 |

### レスポンスボディ
#### 正常

#### エラー ([RFC 9457](https://tex2e.github.io/rfc-translater/html/rfc9457.html) 準拠)
エラー発生時は `application/problem+json` 形式で詳細を返します。

| パラメータ | 型 | 値の例 | 説明 |
| :--- | :--- | :--- | :--- |
| type | `string` | `https://localhost:8080/v1/errors/not-found` | エラー分類を示すURL |
| instance | `string` | `/contents` | エラーが発生したエンドポイント |
| status | `int` | `404` | HTTPステータス値 |
| title | `string` | `"Not Found"` | エラーの概要 |
| detail | `string` | `"指定されたファイルが存在しません"` | ユーザ向けのエラー詳細 |


#### サンプル
```
{
    "type": "https://localhost:8080/v1/errors/not-found",
    "instance": "/contents",
    "status": 404,
    "title": "Not Found",
    "detail": "指定されたファイルが存在しません"
}
```

#### エラー定義一覧 (type 一覧)
各エラーの type に指定する識別子と、その発生条件を定義します。

| type(識別URL) | title | 発生条件 |
| :--- | :--- | :--- |
| `not-found` | `Not Found` | 指定されたパスにファイルやディレクトリが存在しない |
| `permission-denided` | `Forbidden` | 対象へのアクセス権が存在しない |
| `invalid-parameter` | `Bad Request` | リクエストパラメータの形式が不正 |
| `server-error` | `Internal Server Error` | サーバ側で予期せぬエラーが発生した |
