# アンチパターン

## Lambda関数を呼び出すLambda関数
現象
- 呼び出した関数の実行が終わるまで、もう片方も終われない
起こりうる問題
- 同時実行数を圧迫する
- 実行時間が長くなるので、料金が高くなる(実行時間に応じて料金が計算されるため)
- エラー処理がより複雑化する
回避アプローチ
- 2つの関数を分離し、SQSを使って呼び出し先のLambdaにメッセージを発行する
- StepFunctionを使う

## 単一Lambda内での同期的な待機
LambdaからS3にput_objectした後DynamoDBに同期するみたいな処理
![](../image/Pasted%20image%2020251110125748.png)
問題
- レスポンスを待つ時間ができてしまう(同時実行できないので料金がかさむ)
回避アプローチ
- 2番目のタスクが最初のタスクに依存するのであれば、最初のタスクの実行イベントをトリガーに別の関数を呼び出してやればいい
![](../image/Pasted%20image%2020251110130305.png)

## モノリス的なサービス
例)APIGatewayの色々なルート処理を1つのLambda関数が行う
![](../image/Pasted%20image%2020251110131510.png)
問題
- パスが増えるとその分コードが大きくなるため、サイズも大きくなるし保守が難しくなる
- 最小権限の適応が難しくなる
- テストを作るのが難しくなる
- コードの再利用が難しい
回避アプローチ
- パスごとの処理を別々のLambda関数に分ける
![](../image/Pasted%20image%2020251110132911.png)

参考
```embed
title: "Operating Lambda: イベント駆動型アーキテクチャにおけるアンチパターン – Part 3 | Amazon Web Services"
image: "https://d2908q01vomqb2.cloudfront.net/b3f0c7f6bb763af1be91d9e74eabfeb199dc1f1f/2021/03/08/Lambda.jpg"
description: "Operating Lambda シリーズでは、AWS Lambda ベースのアプリケーションを管理している開 […]"
url: "https://aws.amazon.com/jp/blogs/news/compute-operating-lambda-anti-patterns-in-event-driven-architectures-part-3/"
favicon: ""
aspectRatio: "50"
```

# コード署名
署名無しのコードのデプロイをブロックしたりできるらしい
# StepFunction
複数のLambda関数をステートマシンと呼ばれるサーバーレスワークフローに接続できるオーケストレーションサービス
![](../image/Pasted%20image%2020251111125141.png)
これらをGUIで複雑に設定できるらしい
Lambdaとはまた別のサービス