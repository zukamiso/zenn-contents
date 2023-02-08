---
title: "boto3でS3の署名付きURLを発行する方法！"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["aws", "python"]
published: true
---

# 概要
Lambdaからboto3を使ってS3にあるファイルをダウンロードするためのコードが必要になったので備忘録として書いておきます。
LambdaのIAMポリシーなどの設定も必要なのでそれらに関連した内容も記載します。

# ポリシーの作成
Lambdaリソースの作成方法については省略します。
IAMロールは作成済みとして、そのIAMロールにポリシーをアタッチします。
必要なポリシーは`AmazonS3ReadOnlyAccess`です。
もう少し厳密に設定する場合は`s3:GetObject`の権限があるとOKです。

# ソースコード
サンプルのコードです。
例外などは適宜実装してください。

```python
def generate_s3_presigned_url(object_key, bucket_name, expiration):
    presigned_url = ''
    try:
        presigned_url = s3_client.generate_presigned_url('get_object',
                                                        Params={'Bucket': bucket_name, 'Key': object_key},
                                                        ExpiresIn=expiration)
    except Exception as e:
        raise e
    
    return presigned_url
```

# まとめ
S3のオブジェクトをWebアプリケーションで配信する場合によく使うのでまとめました。
誰かのお役に立てたら幸いです。
