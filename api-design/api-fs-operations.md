# ファイル情報取得 API (v1)

## ファイル・フォルダ取得
指定されたパスにあるファイル・フォルダ情報を取得する

### リクエスト
- **URL:** `/v1/contents`
- **Method:** `GET`

#### クエリパラメータ

| パラメータ名 | 必須 | 型 | フォーマット | デフォルト値 | 値の例 | 説明 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| `path` | 〇 | `string` | 1文字以上の文字列 | - | `"data/sample"` | `DATA_ROOT` からの相対パス<br>以下の場合にはエラーとなる<br>・空文字<br>・絶対パス (`/`で始まる)<br>・`../`を含む |
| `recursive` | - | `string` | true, falseのいずれか | `false` | `"false"` | フォルダを再帰的に検索するかどうかを判断する |

### レスポンス
- **201 OK:**
  - `contents`: リクエストされた `path` 内のファイル・フォルダ一覧 (内容以下)

- `contents` データクラス
  - `name`: ファイル・フォルダ名
  - `path`: ファイル・フォルダのパス
  - `type`: `file` or `directory`
  - `size`: ファイルのサイズ、フォルダの場合は `null`
  - `createdAt`: 作成日時 (ISO 8601 / UTC 準拠) [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339)
  - `updatedAt`: 更新日時 (ISO 8601 / UTC 準拠) [RFC 3339](https://datatracker.ietf.org/doc/html/rfc3339)
  - `author`: 作成者・更新者(未実装)
  - `children`: 再帰取得する場合に、フォルダ内にあるファイル・フォルダを格納
    - `children` の内容は 同一データクラスとする
    - ファイルの場合は `null`

#### レスポンスボディ例
**recursive = false** の場合
```
[
    {
        "name": "sample.txt",
        "path": "data/sample.txt",
        "type": "file",
        "size": 100,
        "createdAt": "2026-04-15T13:55:44.137Z",
        "updatedAt": "2026-04-15T13:55:44.137Z",
        "author": "admin",
        children: null
    },
    {
        "name": "upload",
        "path": "data/upload",
        "type": "directory",
        "size": null,
        "createdAt": "2026-05-02T08:05:27.874Z",
        "updatedAt": "2026-05-02T08:05:27.874Z",
        "children": null
    },
]
```
**recursive = true** の場合
```
[
    {
        "name": "sample.txt",
        "path": "data/sample.txt",
        "type": "file",
        "size": 100,
        "createdAt": "2026-04-15T13:55:44.137Z",
        "updatedAt": "2026-04-15T13:55:44.137Z",
        "author": "admin",
        children: null
    },
    {
        "name": "upload",
        "path": "data/upload",
        "type": "directory",
        "size": null,
        "createdAt": "2026-05-02T08:05:27.874Z",
        "updatedAt": "2026-05-02T08:05:27.874Z",
        "children": [
            {
                "name": "to",
                "path": "data/upload/to",
                "type": "directory",
                "size": null,
                "createdAt": "2026-05-02T14:56:57.685Z",
                "updatedAt": "2026-05-02T14:56:57.685Z",
                "children": []
            }
        ]
    },
]
```

- **Error:** [共通エラー形式](./api-endpoint.md) に基づく JSON を返却。
  - `type` 値は以下参照

#### このエンドポイント固有のエラー識別子
`type`:  URLの末尾には以下の識別子が設定されます。

| 識別子 (type末尾) | ステータス | 理由 |
| :--- | :--- | :--- |
| `invalid-path-format` | 400 | リクエストされた path が不正な形式であった |
| `not-exist-directory` | 404 | 対象フォルダが存在しない |
