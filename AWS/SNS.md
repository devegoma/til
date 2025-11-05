# AWS Simple Notification Service(SNS)
A2A、A2Pメッセージング用サービス
![](../image/Pasted%20image%2020251105174759.png)
(引用元: https://docs.aws.amazon.com/ja_jp/sns/latest/dg/welcome.html)

料金
https://aws.amazon.com/jp/sns/pricing/
SMSは料金形態が違うらしい
https://aws.amazon.com/jp/sns/sms-pricing/

## 機能
- A2Aメッセージング
	- メッセージング内容をS3や、Redshiftなどの分析テーブルに入れることも可
- A2Pメッセージング
- メッセージのフィルター処理
- 標準及びFIFOトピック
- メッセージ耐久性
	- 公開済みのメッセージは複数のサーバー、データセンター間で保存される
	- 配信の再試行ポリシーが設定できる
	- デッドレターキューで再試行ポリシー実行前のメッセージを保持
- メッセージ属性
- メッセージのフィルター処理
- メッセージセキュリティ(KMSの暗号化キーを用いた暗号化)
