# DB テーブル設計

## user or account テーブル (仮)
| カラム | 型 | デフォルト値 | 説明 |
| :--- | :--- | :--- | :--- |
| id | integer | - | ユーザーID (PK) |
| username | varchar | "anonymous" | ユーザー名 |
| password_hash | varchar | NULL | パスワードのハッシュ値 |
| private_path | varchar | "/" | 個人dirパス |

## DB リレーション (仮)
```mermaid
erDiagram
    USERS ||--o{ PERMISSIONS : "has"
    USERS {
        integer user_id PK
        varchar username
        varchar password_hash
        varchar private_path
    }
    PERMISSIONS {
        integer perm_id PK
        integer user_id FK
        varchar target_path
        boolean can_write
    }
```