# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 付録

### はじめに

本付録では、OCI学習ガイドブックの補完情報として、サービス比較表、料金計算例、移行チェックリスト、参考資料などを提供します。これらの情報は、実際のプロジェクトでOCIを活用する際の実践的なリファレンスとしてご活用ください。

### 付録A: サービス比較表

#### A.1 コンピュートサービス比較

| 項目 | VM Instances | Bare Metal | OKE | Container Instances | Functions |
|------|-------------|------------|-----|-------------------|-----------|
| **起動時間** | 1-2分 | 5-10分 | 3-5分 | 10-30秒 | 1-5秒 |
| **最小課金単位** | 1分 | 1分 | 1分 | 1秒 | 100ms |
| **スケーラビリティ** | 手動/自動 | 手動 | 自動 | 自動 | 自動 |
| **管理負荷** | 中 | 高 | 中 | 低 | 最低 |
| **カスタマイズ性** | 高 | 最高 | 中 | 低 | 最低 |
| **適用シーン** | 汎用 | 高性能 | マイクロサービス | バッチ処理 | イベント処理 |
| **最大CPU** | 128 OCPU | 128 OCPU | クラスター規模 | 64 vCPU | N/A |
| **最大メモリ** | 2048 GB | 2048 GB | クラスター規模 | 1024 GB | 1024 MB |
| **永続ストレージ** | ○ | ○ | ○ | × | × |
| **ネットワーク分離** | ○ | ○ | ○ | ○ | ○ |

#### A.2 ストレージサービス比較

| 項目 | Block Storage | Object Storage | File Storage | Archive Storage |
|------|--------------|----------------|-------------|----------------|
| **アクセス方法** | ブロックレベル | REST API | NFS | REST API |
| **最大容量** | 32 TB/ボリューム | 無制限 | 8 EB | 無制限 |
| **最大IOPS** | 32,000 | N/A | 7,000 | N/A |
| **レイテンシ** | <1ms | 10-100ms | 1-5ms | 1-4時間 |
| **耐久性** | 99.95% | 99.999999999% | 99.95% | 99.999999999% |
| **同時アクセス** | 1インスタンス | 無制限 | 無制限 | 無制限 |
| **用途** | OS、DB | Webコンテンツ | 共有ファイル | 長期保存 |
| **料金（GB/月）** | $0.0255 | $0.0255 | $0.30 | $0.0012 |
| **データ転送料金** | 無料 | 有料 | 無料 | 有料 |

#### A.3 データベースサービス比較

| 項目 | Autonomous DB | Database Cloud | MySQL Service | NoSQL Service |
|------|--------------|----------------|---------------|---------------|
| **データモデル** | リレーショナル | リレーショナル | リレーショナル | ドキュメント |
| **管理レベル** | フルマネージド | 一部マネージド | フルマネージド | フルマネージド |
| **自動チューニング** | ○ | × | ○ | ○ |
| **自動スケーリング** | ○ | × | ○ | ○ |
| **最大ストレージ** | 128 TB | 無制限 | 64 TB | 無制限 |
| **高可用性** | 組み込み | 手動設定 | 組み込み | 組み込み |
| **バックアップ** | 自動 | 手動/自動 | 自動 | 自動 |
| **適用シーン** | OLTP/OLAP | 既存Oracle移行 | Web/モバイル | スケーラブルアプリ |

#### A.4 ネットワークサービス比較

| 項目 | Load Balancer | WAF | FastConnect | VPN Connect |
|------|--------------|-----|-------------|-------------|
| **レイヤー** | L4/L7 | L7 | L1/L2 | L3 |
| **最大帯域** | 8 Gbps | 無制限 | 10 Gbps | 1.2 Gbps |
| **可用性** | 99.95% | 99.95% | 99.95% | 99.9% |
| **セットアップ時間** | 数分 | 数分 | 数週間 | 数分 |
| **月額料金** | $18.25 | $20 | $500+ | $36.50 |
| **用途** | 負荷分散 | セキュリティ | 専用線接続 | VPN接続 |

### 付録B: 料金計算例

#### B.1 小規模Webアプリケーション

**構成：**
- VM.Standard3.Flex (2 OCPU, 16GB RAM) × 2台
- Block Storage 100GB × 2台
- Load Balancer 1台
- Object Storage 50GB

**月額料金計算：**

| サービス | 数量 | 単価 | 月額料金 |
|---------|------|------|----------|
| VM.Standard3.Flex | 2台 × 730時間 | $0.0425/OCPU/時間 | $124.10 |
| Block Storage | 200GB | $0.0255/GB/月 | $5.10 |
| Load Balancer | 1台 | $18.25/月 | $18.25 |
| Object Storage | 50GB | $0.0255/GB/月 | $1.28 |
| **合計** | | | **$148.73** |

#### B.2 中規模エンタープライズアプリケーション

**構成：**
- VM.Standard3.Flex (4 OCPU, 32GB RAM) × 4台
- Autonomous Database (4 OCPU)
- Block Storage 500GB × 4台
- Load Balancer 1台
- Object Storage 500GB
- WAF 1台

**月額料金計算：**

| サービス | 数量 | 単価 | 月額料金 |
|---------|------|------|----------|
| VM.Standard3.Flex | 4台 × 730時間 × 4 OCPU | $0.0425/OCPU/時間 | $496.40 |
| Autonomous Database | 4 OCPU × 730時間 | $0.36/OCPU/時間 | $1,051.20 |
| Block Storage | 2000GB | $0.0255/GB/月 | $51.00 |
| Load Balancer | 1台 | $18.25/月 | $18.25 |
| Object Storage | 500GB | $0.0255/GB/月 | $12.75 |
| WAF | 1台 | $20/月 | $20.00 |
| **合計** | | | **$1,649.60** |

#### B.3 大規模データ分析基盤

**構成：**
- OKE クラスター (10ノード、各4 OCPU)
- Autonomous Data Warehouse (16 OCPU)
- Object Storage 10TB
- Data Science Service (8 OCPU)
- Big Data Service (5ノード)

**月額料金計算：**

| サービス | 数量 | 単価 | 月額料金 |
|---------|------|------|----------|
| OKE ワーカーノード | 10台 × 730時間 × 4 OCPU | $0.0425/OCPU/時間 | $1,241.00 |
| Autonomous DW | 16 OCPU × 730時間 | $0.36/OCPU/時間 | $4,204.80 |
| Object Storage | 10TB | $0.0255/GB/月 | $261.12 |
| Data Science | 8 OCPU × 100時間 | $0.36/OCPU/時間 | $288.00 |
| Big Data Service | 5ノード × 730時間 | $0.17/時間/ノード | $620.50 |
| **合計** | | | **$6,615.42** |

### 付録C: 移行チェックリスト

#### C.1 移行前準備チェックリスト

**□ 現状分析**
- [ ] 既存システムの棚卸し完了
- [ ] 依存関係マップ作成完了
- [ ] 性能要件の文書化完了
- [ ] セキュリティ要件の文書化完了
- [ ] コンプライアンス要件の確認完了

**□ 移行計画**
- [ ] 移行戦略の決定（リフト&シフト/リファクタリング）
- [ ] 移行順序の決定
- [ ] ダウンタイム要件の確認
- [ ] ロールバック計画の策定
- [ ] リスク評価と軽減策の策定

**□ 技術準備**
- [ ] OCIアカウントの設定
- [ ] ネットワーク接続の確立（FastConnect/VPN）
- [ ] IAMポリシーの設定
- [ ] セキュリティ設定の実装
- [ ] 監視・ログ設定の準備

**□ チーム準備**
- [ ] 移行チームの編成
- [ ] 役割と責任の明確化
- [ ] OCIトレーニングの実施
- [ ] 移行手順書の作成
- [ ] 緊急連絡先の整備

#### C.2 移行実行チェックリスト

**□ データ移行**
- [ ] データバックアップの実行
- [ ] データ移行ツールの準備
- [ ] データ整合性チェック手順の確認
- [ ] 移行データの検証
- [ ] データ同期の確認

**□ アプリケーション移行**
- [ ] アプリケーションの停止
- [ ] 設定ファイルの移行
- [ ] 依存関係の確認
- [ ] アプリケーションの起動
- [ ] 機能テストの実行

**□ ネットワーク設定**
- [ ] DNS設定の更新
- [ ] ロードバランサー設定
- [ ] セキュリティグループ設定
- [ ] ファイアウォール設定
- [ ] 接続テストの実行

**□ 監視・運用**
- [ ] 監視設定の有効化
- [ ] アラート設定の確認
- [ ] ログ収集の開始
- [ ] バックアップ設定の確認
- [ ] 運用手順書の更新

#### C.3 移行後検証チェックリスト

**□ 機能検証**
- [ ] 全機能の動作確認
- [ ] パフォーマンステスト
- [ ] セキュリティテスト
- [ ] 災害復旧テスト
- [ ] ユーザー受け入れテスト

**□ 運用検証**
- [ ] 監視システムの動作確認
- [ ] アラート通知の確認
- [ ] バックアップ・復元テスト
- [ ] 運用手順の検証
- [ ] ドキュメントの更新

**□ 最適化**
- [ ] リソース使用率の確認
- [ ] コスト分析
- [ ] 性能チューニング
- [ ] セキュリティ設定の見直し
- [ ] 運用プロセスの改善

### 付録D: トラブルシューティングガイド

#### D.1 よくある問題と解決方法

**接続問題**

| 問題 | 原因 | 解決方法 |
|------|------|----------|
| インスタンスにSSH接続できない | セキュリティリスト設定 | ポート22の許可確認 |
| Webサイトにアクセスできない | セキュリティリスト設定 | ポート80/443の許可確認 |
| データベースに接続できない | ネットワーク設定 | プライベートサブネット確認 |

**性能問題**

| 問題 | 原因 | 解決方法 |
|------|------|----------|
| アプリケーション応答が遅い | リソース不足 | CPU/メモリ使用率確認 |
| データベースクエリが遅い | インデックス不足 | 実行計画確認、インデックス追加 |
| ネットワークが遅い | 帯域不足 | ネットワーク使用率確認 |

**セキュリティ問題**

| 問題 | 原因 | 解決方法 |
|------|------|----------|
| 不正アクセス検知 | 設定不備 | Cloud Guard確認 |
| データ漏洩リスク | 暗号化未設定 | 暗号化設定確認 |
| 権限エラー | IAM設定 | ポリシー設定確認 |

#### D.2 診断コマンド集

**ネットワーク診断**
```bash
# 接続確認
ping <target-ip>
telnet <target-ip> <port>
nslookup <hostname>

# ネットワーク設定確認
ip addr show
ip route show
netstat -tuln

# ファイアウォール確認
sudo firewall-cmd --list-all
sudo iptables -L
```

**システム診断**
```bash
# リソース使用率確認
top
htop
free -h
df -h

# ログ確認
sudo journalctl -u <service-name>
tail -f /var/log/messages
dmesg

# プロセス確認
ps aux | grep <process-name>
systemctl status <service-name>
```

**データベース診断**
```sql
-- Oracle Database
SELECT * FROM V$SESSION WHERE STATUS = 'ACTIVE';
SELECT * FROM V$SQL_MONITOR WHERE STATUS = 'EXECUTING';
SELECT * FROM V$SYSTEM_EVENT ORDER BY TOTAL_WAITS DESC;

-- MySQL
SHOW PROCESSLIST;
SHOW ENGINE INNODB STATUS;
SELECT * FROM performance_schema.events_waits_summary_global_by_event_name;
```

### 付録E: 参考資料・公式ドキュメント

#### E.1 公式ドキュメント

**基本ドキュメント**
- [OCI Documentation](https://docs.oracle.com/en-us/iaas/Content/home.htm)
- [OCI Architecture Center](https://docs.oracle.com/solutions/)
- [OCI Best Practices](https://docs.oracle.com/en-us/iaas/Content/General/Reference/aqswhitepapers.htm)

**サービス別ドキュメント**
- [Compute Service](https://docs.oracle.com/en-us/iaas/Content/Compute/home.htm)
- [Networking](https://docs.oracle.com/en-us/iaas/Content/Network/home.htm)
- [Database](https://docs.oracle.com/en-us/iaas/Content/Database/home.htm)
- [Security](https://docs.oracle.com/en-us/iaas/Content/Security/home.htm)

**API・SDK**
- [OCI REST API](https://docs.oracle.com/en-us/iaas/api/)
- [OCI CLI](https://docs.oracle.com/en-us/iaas/Content/API/CLIReference/cli.htm)
- [OCI SDK](https://docs.oracle.com/en-us/iaas/Content/API/SDKDocs/sdks.htm)

#### E.2 学習リソース

**オンライン学習**
- [Oracle University](https://education.oracle.com/oracle-cloud-infrastructure)
- [OCI Learning Path](https://mylearn.oracle.com/ou/learning-path/become-an-oci-architect-associate/79960)
- [OCI Hands-on Labs](https://oracle.github.io/learning-library/oci-library/)

**認定資格**
- OCI Foundations Associate
- OCI Architect Associate
- OCI Architect Professional
- OCI Developer Associate

**コミュニティ**
- [OCI Community](https://community.oracle.com/customerconnect/categories/oci)
- [Oracle Cloud Infrastructure Blog](https://blogs.oracle.com/cloud-infrastructure/)
- [OCI YouTube Channel](https://www.youtube.com/c/OracleCloudInfrastructure)

#### E.3 ツール・ユーティリティ

**開発・運用ツール**
- [OCI CLI](https://github.com/oracle/oci-cli)
- [Terraform OCI Provider](https://registry.terraform.io/providers/oracle/oci/latest)
- [Ansible OCI Collection](https://galaxy.ansible.com/oracle/oci)

**監視・管理ツール**
- [OCI Monitoring](https://docs.oracle.com/en-us/iaas/Content/Monitoring/home.htm)
- [OCI Logging](https://docs.oracle.com/en-us/iaas/Content/Logging/home.htm)
- [OCI Cloud Guard](https://docs.oracle.com/en-us/iaas/cloud-guard/home.htm)

**移行ツール**
- [OCI Database Migration Service](https://docs.oracle.com/en-us/iaas/database-migration/home.htm)
- [OCI Data Transfer Service](https://docs.oracle.com/en-us/iaas/Content/DataTransfer/home.htm)
- [OCI Storage Gateway](https://docs.oracle.com/en-us/iaas/Content/StorageGateway/home.htm)

### 付録F: 用語集

#### F.1 基本用語

**A-C**
- **AD (Availability Domain)**: アベイラビリティドメイン、独立したデータセンター
- **API**: Application Programming Interface、アプリケーション間の通信インターフェース
- **CIDR**: Classless Inter-Domain Routing、IPアドレス表記法
- **CLI**: Command Line Interface、コマンドライン操作ツール
- **Compartment**: コンパートメント、リソースの論理的分離単位

**D-I**
- **DRG**: Dynamic Routing Gateway、動的ルーティングゲートウェイ
- **FQDN**: Fully Qualified Domain Name、完全修飾ドメイン名
- **IAM**: Identity and Access Management、認証・認可管理
- **IOPS**: Input/Output Operations Per Second、秒間入出力操作数

**J-O**
- **JSON**: JavaScript Object Notation、データ交換フォーマット
- **OCPU**: Oracle CPU、Oracleの仮想CPU単位
- **OCID**: Oracle Cloud Identifier、OCIリソースの一意識別子

**P-S**
- **REST**: Representational State Transfer、Webサービスアーキテクチャスタイル
- **SDK**: Software Development Kit、ソフトウェア開発キット
- **SLA**: Service Level Agreement、サービス品質保証契約

**T-Z**
- **Tenancy**: テナンシー、OCIアカウントの最上位組織単位
- **VCN**: Virtual Cloud Network、仮想クラウドネットワーク
- **YAML**: YAML Ain't Markup Language、設定ファイル記述言語

#### F.2 技術用語

**クラウド・インフラ**
- **Auto Scaling**: 自動スケーリング、負荷に応じたリソース自動調整
- **Load Balancer**: ロードバランサー、負荷分散装置
- **Fault Domain**: フォルトドメイン、障害分離単位
- **High Availability**: 高可用性、システムの継続稼働性

**セキュリティ**
- **Encryption**: 暗号化、データの秘匿化
- **Firewall**: ファイアウォール、ネットワーク防御システム
- **MFA**: Multi-Factor Authentication、多要素認証
- **Zero Trust**: ゼロトラスト、「信頼しない、検証する」セキュリティモデル

**データ・分析**
- **Data Lake**: データレイク、多様なデータの統合保存領域
- **ETL**: Extract, Transform, Load、データ抽出・変換・読み込み
- **Machine Learning**: 機械学習、データからパターンを学習する技術
- **Big Data**: ビッグデータ、大容量・多様・高速データ

### まとめ

本付録では、OCI学習ガイドブックの補完情報として、実践的なリファレンス情報を提供しました。これらの情報を活用して、効果的なOCIプロジェクトの実現にお役立てください。

**付録の活用方法：**
1. **サービス比較表**: 要件に応じた適切なサービス選択
2. **料金計算例**: プロジェクト予算策定の参考
3. **移行チェックリスト**: 確実な移行プロジェクト実行
4. **トラブルシューティング**: 問題発生時の迅速な解決
5. **参考資料**: 継続的な学習とスキル向上
6. **用語集**: 技術用語の正確な理解

OCIの技術は日々進歩しているため、最新情報については公式ドキュメントを参照し、継続的な学習を心がけてください。