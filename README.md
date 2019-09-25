![architecture-diagram-AWS-Developer-Center_mythical-mysfits-application-architecture-1@1.5x.22db78a48a57bd00cce92f44146e96d3312ab1e3.png](https://d1.awsstatic.com/Getting%20Started/build-modern-app-fargate-lambda-dynamodb-python/architecture-diagram-AWS-Developer-Center_mythical-mysfits-application-architecture-1@1.5x.22db78a48a57bd00cce92f44146e96d3312ab1e3.png)
## モジュール 2: ウェブサーバーでのアプリケーションのホスト
### モジュール 2A: コアインフラストラクチャのセットアップ
#### ステップ1 CloudFormationテンプレートのダウンロード
- Amazon VPC – 10.0.0.0/16 のプライベート IP 空間に 4 つのサブネット (パブリックとプライベートを 2 つずつ) を配置したネットワーク環境と、必要なすべてのルートテーブル設定。このネットワークのサブネットは別々の AWS アベイラビリティーゾーン (AZ) で作成され、AWS リージョン内の複数の物理的な施設全体で高可用性を実現できます。AZ で高可用性を実現する方法の詳細をご覧ください。
- 2つの NAT ゲートウェイ (各パブリックサブネットに 1 つ) – 最終的にプライベートサブネットにデプロイするコンテナがインターネットと通信して、必要なパッケージなどをダウンロードできるようにします。
- DynamoDB VPC エンドポイント – マイクロサービスバックエンドは、永続性を実現するため、最終的に Amazon DynamoDB と統合します (モジュール 3 で実施)。
- セキュリティグループ – Docker コンテナが、Network Load Balancer を経由してポート 8080 でインターネットからのトラフィックを受信できるようにします。
- IAM ロール – Identity and Access Management ロールが作成されます。ロールは、AWS のサービスや作成するリソースが他の AWS のサービス (DynamoDB、S3 など) にアクセスできるようにするために、ワークショップ全体を通じて使用します。

### モジュール 2B: AWS Fargate を使用したサービスのデプロイ
#### ステップ１Flaskサービスを作成する
#### ステップ2 Amazon ECSにサービスの前提条件を設定
- A: AWS Fargate クラスターを作成する
- B: AWS CloudWatch Logs グループを作成する
- C: ECS のタスク定義を登録する
#### ステップ3 ロードバランサーを有効にしたFargateサービスを有効にする
- A: Network Load Balancer を作成する
- B: ロードバランサーのターゲットグループを作成する
- C: ロードバランサーのリスナーを作成する
#### ステップ4 Fargateでサービスを作成する
- A: ECS のサービスにリンクされたロールを作成する
- B: サービスを作成する
- B: サービスをテストする
#### ステップ5 NLBを呼び出すようMythicalMysfitsを更新する
- A: API エンドポイントを置き換える
- B: S3 にアップロードする
### モジュール 2C: AWS のコードサービスを使用したデプロイの自動化
#### ステップ１:CICDパイプラインを作成する
- A: パイプラインアーティファクト用の S3 バケットを作成する
- B:CodeCommit リポジトリを作成する
- C: CodeBuild プロジェクトを作成する
- D: CodePipeline のパイプラインを作成する
- E: ECR イメージリポジトリへの自動アクセスを有効にする
#### ステップ2:CICDパイプラインのテストをする
- A: AWS CodeCommit で Git を使用する
- B: コードの変更をプッシュする

## モジュール 3 Mysfit 情報の保存
### ステップ1 Mythical MysfitsにNoSQLデータベースを作成する
- A: DynamoDB テーブルを作成する
- B: DynamoDB テーブルに項目を追加する
### ステップ2 最初の実際のコード変更をコミットする
- A: 更新された Flask サービスコードをコピーする
- B: 更新された Flask サービスコードをコピーする
### ステップ3 S3内のウェブサイトコンテンツを更新する


## モジュール 4 ユーザー登録の設定
### ステップ1 ウェブサイトユーザー用のユーザープールを追加する
- A: Cognito ユーザープールを作成する
- B: Cognito ユーザープールクライアントを作成する
### ステップ2 新しいREST APIとAmazon API Gatewayを追加する
- A: API Gateway VPC リンクを作成する
- B: Swagger を使用して REST API を作成する
- C: API をデプロイする
### ステップ3 Mythical Mysfitsウェブサイトを更新する
- A: Flask サービスバックエンドを更新する
- B: S3 内の Mythical Mysfits ウェブサイトを更新する
## モジュール 5 ユーザー行動の把握
### ステップ1 ストリーミングサービスのコードを作成する
- A: 新しい CodeCommit リポジトリを作成する
- B: ストリーミングサービスのコードベースをコピーする
- 
### ステップ2 Lambda関数のパッケージとコードを更新する
- A: pip を使用して Lambda 関数の依存関係をインストールする
- B: Lambda 関数コードを更新する
- C: コードを CodeCommit にプッシュする
### ステップ3 ストリーミングサービスのスタックを作成する
- A: Lambda 関数コードパッケージの S3 バケットを作成する
- B: SAM CLI を使用して Lambda のコードをパッケージ化する
- C: AWS CloudFormation を使用してスタックをデプロイする
### ステップ4 Mysfit プロフィールのクリックを新しいマイクロサービスに送信する
- A: ウェブサイトのコンテンツを更新する
- B: 新しいサイトのバージョンを S3 にプッシュする
### ステップ5 ワークショップのクリーンアップ
