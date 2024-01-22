# ホンダ プロンプトエンジニアリング資料

## 環境構築

1. docker-composeを使って、環境を構築します。

```bash
$ docker-compose up -d
```

2. ブラウザで http://localhost:1313/ にアクセスします。

ドキュメントを編集すると、自動的にブラウザがリロードされます。

## 本番環境のビルド

```bash
$ npm run build:production
```

## ビルドディレクトリのクリーンアップ

```bash
$ npm run clean
```

## ビルドテスト
    
```bash
$ npm run test
```
