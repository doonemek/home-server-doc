# フォルダ構成設計

## フォルダ構成 (仮)
```
root (/)
├── data/ (データ格納場所)
|   ├── shared/ (共有フォルダ: 全員操作可)
|       ├── google_drive/ (GoogleDrive 共有フォルダ)
|   :
|
|   └── users/ (個人フォルダ群)
|       ├── user1/ (user1 のみアクセス可)
|       ├── user2/ (user2 のみアクセス可)
|       :
|
├── system/ (管理用)
|   ├── database/ (DBファイル保存先)
|   ├── scripts/ (batch,cronなど)
|   :
|
└── apps/ (Docker起動ファイルなど)
```