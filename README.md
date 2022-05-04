# 概要

AWSのS3の静的ウェブサイトホスティングを利用してReactで作ったWebアプリをデプロイする｡

# 手順

1. Reactプロジェクトの作成

```javascript
npx create-react-app test_deploy
```

2. package.jsonの編集

```json
"homepage": "."
```

上記を追加する｡これにより､staticフォルダへの参照が相対的なものになる｡これをしないとindex.htmlからjavascriptファイルにアクセスすることができない｡

3. Reactプロジェクトをビルド

```javascript
npm run build
```

4. S3バケットの作成

`AWS>サービス>ストレージ>S3>バケット>バケットを作成`から新しいバケットを作成する｡

5. ビルドしたファイル・フォルダのアップロード

作成したバケットを開き､`オブジェクト`タブを開く｡その後､デプロイしたファイル・フォルダを選択する｡

6. アクセス許可を変更する

`アクセス許可`タブの`ブロックパブリックアクセス(バケット設定)`から以下の項目のみチェックをつける｡

- 新しいアクセスコントロールリスト (ACL) を介して付与されたバケットとオブジェクトへのパブリックアクセスをブロックする
- 任意のアクセスコントロールリスト (ACL) を介して付与されたバケットとオブジェクトへのパブリックアクセスをブロックする

7. オブジェクトを公開する

`プロパティ`タブの`静的ウェブサイトホスティング`を有効にする｡このとき､以下のように設定する｡

```javascript
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"PublicRead",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject","s3:GetObjectVersion"],
      "Resource":["arn:aws:s3:::awsexamplebucket1/*"]
    }
  ]
}
```
