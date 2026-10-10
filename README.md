# FDE


| FDE開発ライフサイクル | クライアント / AIエージェント層 | MCPサーバー層（アダプター） | インフラストラクチャ層（サーバーレス） | セキュリティ層（AgentCore / Guardrails） |
| --- | --- | --- | --- | --- |
| **1. 要件定義・スコープ策定**<br><br>(Discovery & Scoping) | 利用するAIクライアントや自社UIの選定、エージェントの推論要件の定義<br>（**Amazon Bedrock Agents**） | 連携対象となる社内システム（DB、SaaS、API）の仕様・データ構造の分析<br>（**Bedrock Action Groups**） | 閉域網・オンプレ・クラウドといったインフラ環境要件、ネットワーク設計<br>（**Amazon VPC**） | 複数バックエンドの集約・ルーティング設計、認証・認可境界、機密情報要件の定義<br>（**AgentCore Identity**, **AWS IAM**） |
| **2. プロトタイピング & 検証**<br><br>(Prototyping & Validation) | 開発者によるプロンプトの挙動テストやオーケストレーションの動作確認<br>（**Bedrock Agents Test UI**, **Bedrock Console**） | 個別システムにアクセスする最小限のモックやAPIラッパーの実装と単体テスト<br>（**AWS Lambda**） | 自前サーバーを構築しない、マネージド環境を利用した即時検証の開始<br>（**AWS Serverless Application Model (SAM)**） | 簡易的な疎通テスト、テスト用のNGワード設定・入力バリデーション確認<br>（**Bedrock Guardrails Test Console**） |
| **3. ゲートウェイ・ブリッジ開発**<br><br>(Gateway & Bridge Development) | クライアント側から見た接続先の抽象化・単一化のインターフェース設計<br>（**Amazon Bedrock Agents**） | 各バックエンドごとの固有ロジック、エラーハンドリング、OpenAPIスキーマの整形<br>（**AWS Lambda**） | IaCを用いたサーバーレスリソースの定義とデプロイパイプラインの作成<br>（**AWS CDK**, **AWS CloudFormation**） | 複数ターゲットの集約・ルーティング、入力ガードレール、ポリシーによる認可制御の実装<br>（**Amazon Bedrock AgentCore**, **Bedrock Guardrails**, **Cedar Policies**） |
| **4. 本番インテグレーション**<br><br>(Production Hardening) | 検証用環境から、本番用の自社製WebアプリやカスタムUIへの統合<br>（**AWS SDK**, **Amazon API Gateway**） | バックエンドの仕様変更やAPI障害に対するレジリエンス（耐障害性）の強化<br>（**AWS Step Functions**, **AWS Lambda**） | フルマネージド環境への本番デプロイ、サーバーレスによるスケーラビリティの担保<br>（**AWS Lambda**） | ターゲットへの安全な権限伝搬（On-Behalf-Of）、監査ログの有効化、実運用ポリシーの適用<br>（**AgentCore Token Vault**, **AWS CloudTrail**） |
| **5. 運用・拡張**<br><br>(Operation & Evolution) | 新しい基盤モデルへの切り替え、プロンプトの継続的なチューニング<br>（**Amazon Bedrock**） | 追加される社内システムやAPI変更に伴うアダプターロジックのメンテナンス・拡張<br>（**Bedrock Action Groups**, **AWS Lambda**） | マネージドリソースのメトリクス監視、ログの集約、オートスケーリング（自動追従）<br>（**Amazon CloudWatch**） | 新規追加ツールの動的ルーティング管理、分散トレーシング、異常アクセスの継続監視<br>（**AWS Security Hub**, **AWS X-Ray**） |
