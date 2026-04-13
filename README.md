# Home Server Project

家族のためのファイルサーバーと、それを操作するAndroidアプリの開発リポジトリです。

## 全体構成
```mermaid
graph TD
    App[Android App] -->|HTTPS/Tailscale| API[API Server]
    API -->|File Access| FS[File System]
    API -->|Query| DB[(SQLite DB)]