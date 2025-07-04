# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 第3章 ストレージサービス

### はじめに

第2章でコンピュートサービスについて学習しました。本章では、これらのコンピュートリソースを支える重要な基盤であるストレージサービスについて詳しく解説します。ストレージは、データの永続化、バックアップ、アーカイブなど、システム運用において不可欠な要素です。OCIでは、用途に応じて最適化された複数のストレージサービスを提供しています。

### ストレージの基本概念

#### データストレージの重要性

現代のITシステムにおいて、データは企業の最も重要な資産の一つです。適切なストレージ戦略なしには、以下のようなリスクが発生します：

```mermaid
graph TB
    A[不適切なストレージ戦略] --> B[データ損失リスク]
    A --> C[性能問題]
    A --> D[コスト増大]
    A --> E[コンプライアンス違反]
    
    B --> B1[ハードウェア障害]
    B --> B2[人的ミス]
    B --> B3[災害・事故]
    
    C --> C1[アプリケーション応答遅延]
    C --> C2[スループット不足]
    C --> C3[同時接続数制限]
    
    D --> D1[過剰なストレージ容量]
    D --> D2[不適切な性能レベル]
    D --> D3[非効率なデータ配置]
    
    E --> E1[データ保管期間違反]
    E --> E2[暗号化要件未対応]
    E --> E3[監査ログ不備]
    
    style A fill:#ffebee
    style B fill:#ffcdd2
    style C fill:#ffcdd2
    style D fill:#ffcdd2
    style E fill:#ffcdd2
```

#### ストレージの分類

ストレージは、アクセス方法と用途によって以下のように分類されます：

```mermaid
graph TB
    A[ストレージタイプ] --> B[ブロックストレージ]
    A --> C[ファイルストレージ]
    A --> D[オブジェクトストレージ]
    
    B --> B1[仮想ディスク]
    B --> B2[データベース]
    B --> B3[ファイルシステム]
    
    C --> C1[共有ファイル]
    C --> C2[ホームディレクトリ]
    C --> C3[アプリケーション設定]
    
    D --> D1[Webコンテンツ]
    D --> D2[バックアップ]
    D --> D3[ログファイル]
    D --> D4[メディアファイル]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

### 3.1 Block Storage（ブロックストレージ）

#### ブロックストレージとは

ブロックストレージは、データを固定サイズのブロック単位で管理するストレージ方式です。従来のハードディスクやSSDと同様の動作をし、オペレーティングシステムからは通常のディスクとして認識されます。

```mermaid
graph TB
    subgraph "ブロックストレージの構造"
        A[ブロック1<br/>4KB]
        B[ブロック2<br/>4KB]
        C[ブロック3<br/>4KB]
        D[ブロック4<br/>4KB]
        E[...]
        F[ブロックN<br/>4KB]
    end
    
    G[ファイルシステム] --> A
    G --> B
    G --> C
    
    H[データベース] --> D
    H --> E
    H --> F
    
    style A fill:#e3f2fd
    style B fill:#e3f2fd
    style C fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#e8f5e8
    style F fill:#e8f5e8
```

#### OCIブロックストレージの特徴

**1. 高性能**
- NVMe SSDベースの高速ストレージ
- 最大32,000 IOPS（Input/Output Operations Per Second）
- 低レイテンシ（1ms以下）

**2. 高可用性**
- 自動的な3重レプリケーション
- 99.95%の可用性SLA
- 自動障害復旧

**3. 柔軟性**
- 1GB〜32TBまでの容量
- オンラインでの容量拡張
- 性能レベルの動的変更

#### ブロックストレージの性能レベル

OCIでは、用途に応じて3つの性能レベルを提供しています：

```mermaid
graph TB
    A[ブロックストレージ性能レベル] --> B[Lower Cost<br/>低コスト]
    A --> C[Balanced<br/>バランス]
    A --> D[Higher Performance<br/>高性能]
    
    B --> B1[0-3 IOPS/GB]
    B --> B2[最大3,000 IOPS]
    B --> B3[コスト重視]
    
    C --> C1[10 IOPS/GB]
    C --> C2[最大25,000 IOPS]
    C --> C3[汎用用途]
    
    D --> D1[30-120 IOPS/GB]
    D --> D2[最大32,000 IOPS]
    D --> D3[高性能要求]
    
    style B fill:#fff3e0
    style C fill:#e8f5e8
    style D fill:#e3f2fd
```

#### 性能レベル詳細比較

| 性能レベル | IOPS/GB | 最大IOPS | 最大スループット | 用途例 |
|-----------|---------|----------|----------------|--------|
| **Lower Cost** | 0-3 | 3,000 | 480 MB/s | ログ保存、アーカイブ |
| **Balanced** | 10 | 25,000 | 480 MB/s | 一般的なアプリケーション |
| **Higher Performance** | 30-120 | 32,000 | 480 MB/s | データベース、高負荷アプリ |

#### ブロックストレージの設計パターン

**1. 単一インスタンス構成**

```mermaid
graph LR
    A[コンピュートインスタンス] --> B[ブートボリューム<br/>50GB]
    A --> C[データボリューム1<br/>100GB]
    A --> D[データボリューム2<br/>500GB]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

**2. 高可用性構成**

```mermaid
graph TB
    subgraph "AD-1"
        A1[インスタンス1] --> B1[データボリューム1]
    end
    
    subgraph "AD-2"
        A2[インスタンス2] --> B2[データボリューム2]
    end
    
    B1 -.->|レプリケーション| B2
    
    C[ロードバランサー] --> A1
    C --> A2
    
    style A1 fill:#e8f5e8
    style A2 fill:#e8f5e8
    style B1 fill:#e3f2fd
    style B2 fill:#e3f2fd
```

#### バックアップとスナップショット

**スナップショット機能**

```mermaid
graph LR
    A[元ボリューム] --> B[スナップショット1<br/>日次]
    A --> C[スナップショット2<br/>週次]
    A --> D[スナップショット3<br/>月次]
    
    B --> E[復元ボリューム1]
    C --> F[復元ボリューム2]
    D --> G[復元ボリューム3]
    
    style A fill:#e8f5e8
    style B fill:#e3f2fd
    style C fill:#fff3e0
    style D fill:#fce4ec
```

**主要パラメータ：**
- **スナップショット頻度**: 手動、日次、週次、月次
- **保持期間**: 7日〜無制限
- **増分バックアップ**: 変更分のみ保存
- **クロスリージョンコピー**: 災害復旧対応

#### 暗号化とセキュリティ

**暗号化オプション：**

```mermaid
graph TB
    A[ブロックストレージ暗号化] --> B[Oracle管理キー]
    A --> C[顧客管理キー]
    
    B --> B1[デフォルト暗号化]
    B --> B2[管理不要]
    B --> B3[AES-256]
    
    C --> C1[Vault統合]
    C --> C2[キーローテーション]
    C --> C3[アクセス制御]
    
    style B fill:#e8f5e8
    style C fill:#e3f2fd
```

#### 使用例とベストプラクティス

**1. データベース用途**
```bash
# 高性能ブロックストレージの作成例
oci bv volume create \
  --compartment-id <compartment-id> \
  --display-name "database-volume" \
  --size-in-gbs 1000 \
  --vpus-per-gb 30 \
  --availability-domain <ad-name>
```

**2. ファイルシステム構成**
```bash
# ボリュームのアタッチ後
sudo mkfs.ext4 /dev/sdb
sudo mkdir /data
sudo mount /dev/sdb /data
echo '/dev/sdb /data ext4 defaults 0 2' >> /etc/fstab
```

### 3.2 Object Storage（オブジェクトストレージ）

#### オブジェクトストレージとは

オブジェクトストレージは、データをオブジェクトとして管理するストレージ方式です。各オブジェクトには、データ本体、メタデータ、一意のIDが含まれ、フラットな名前空間で管理されます。

```mermaid
graph TB
    A[オブジェクトストレージ] --> B[オブジェクト1]
    A --> C[オブジェクト2]
    A --> D[オブジェクト3]
    
    B --> B1[データ本体]
    B --> B2[メタデータ]
    B --> B3[オブジェクトID]
    
    C --> C1[データ本体]
    C --> C2[メタデータ]
    C --> C3[オブジェクトID]
    
    D --> D1[データ本体]
    D --> D2[メタデータ]
    D --> D3[オブジェクトID]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### OCIオブジェクトストレージの特徴

**1. 無制限スケーラビリティ**
- 容量制限なし
- 単一オブジェクト最大10TB
- 自動的な負荷分散

**2. 高耐久性**
- 99.999999999%（11 9's）の耐久性
- 複数のアベイラビリティドメインでレプリケーション
- 自動的なデータ整合性チェック

**3. 豊富なアクセス方法**
- REST API
- SDK（Java、Python、.NET等）
- CLI（Command Line Interface）
- Webコンソール

#### ストレージクラス

OCIオブジェクトストレージでは、アクセス頻度に応じて3つのストレージクラスを提供：

```mermaid
graph TB
    A[ストレージクラス] --> B[Standard<br/>標準]
    A --> C[Infrequent Access<br/>低頻度アクセス]
    A --> D[Archive<br/>アーカイブ]
    
    B --> B1[頻繁なアクセス]
    B --> B2[即座の取得]
    B --> B3[高コスト]
    
    C --> C1[月1回程度のアクセス]
    C --> C2[即座の取得]
    C --> C3[中コスト]
    
    D --> D1[年1回程度のアクセス]
    D --> D2[取得に時間要]
    D --> D3[低コスト]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### ストレージクラス詳細比較

| 項目 | Standard | Infrequent Access | Archive |
|------|----------|-------------------|---------|
| **用途** | 頻繁アクセス | 月次アクセス | 年次アクセス |
| **取得時間** | 即座 | 即座 | 1-4時間 |
| **最小保存期間** | なし | 31日 | 90日 |
| **ストレージ料金** | 高 | 中 | 低 |
| **取得料金** | 無料 | 有料 | 有料 |

#### バケット設計とネーミング

**バケット構成例：**

```mermaid
graph TB
    A[テナンシー] --> B[本番環境バケット]
    A --> C[開発環境バケット]
    A --> D[バックアップバケット]
    
    B --> B1[web-assets-prod]
    B --> B2[user-uploads-prod]
    B --> B3[logs-prod]
    
    C --> C1[web-assets-dev]
    C --> C2[user-uploads-dev]
    C --> C3[logs-dev]
    
    D --> D1[database-backup]
    D --> D2[system-backup]
    D --> D3[archive-data]
    
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#e3f2fd
```

**ネーミング規則例：**
- `{環境}-{用途}-{リージョン}-{年月}`
- `prod-web-assets-tokyo-202312`
- `dev-user-data-osaka-202312`

#### ライフサイクル管理

オブジェクトのライフサイクルを自動化することで、コストを最適化できます：

```mermaid
graph LR
    A[Standard<br/>作成時] --> B[Infrequent Access<br/>30日後]
    B --> C[Archive<br/>90日後]
    C --> D[削除<br/>7年後]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#ffebee
```

**ライフサイクルポリシー例：**
```json
{
  "rules": [
    {
      "name": "transition-to-ia",
      "action": "TRANSITION",
      "target": "InfrequentAccess",
      "timeAmount": 30,
      "timeUnit": "DAYS"
    },
    {
      "name": "transition-to-archive",
      "action": "TRANSITION", 
      "target": "Archive",
      "timeAmount": 90,
      "timeUnit": "DAYS"
    },
    {
      "name": "delete-old-objects",
      "action": "DELETE",
      "timeAmount": 7,
      "timeUnit": "YEARS"
    }
  ]
}
```

#### セキュリティ機能

**1. アクセス制御**

```mermaid
graph TB
    A[アクセス制御] --> B[IAMポリシー]
    A --> C[プリ認証リクエスト]
    A --> D[バケットポリシー]
    
    B --> B1[ユーザー/グループベース]
    B --> B2[リソースベース]
    
    C --> C1[期限付きURL]
    C --> C2[読み取り専用]
    C --> C3[アップロード専用]
    
    D --> D1[バケット単位制御]
    D --> D2[オブジェクト単位制御]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

**2. 暗号化**
- **転送時暗号化**: HTTPS/TLS
- **保存時暗号化**: AES-256
- **キー管理**: Oracle管理 or 顧客管理

#### 使用例

**1. 静的Webサイトホスティング**
```bash
# バケット作成
oci os bucket create \
  --name my-website \
  --compartment-id <compartment-id>

# 静的ウェブサイト設定
oci os bucket update \
  --bucket-name my-website \
  --public-access-type ObjectRead
```

**2. データレイク構築**
```mermaid
graph TB
    A[データソース] --> B[Object Storage<br/>Raw Data]
    B --> C[データ処理<br/>Analytics Service]
    C --> D[Object Storage<br/>Processed Data]
    D --> E[可視化<br/>BI Tools]
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
```

### 3.3 File Storage（ファイルストレージ）

#### ファイルストレージとは

ファイルストレージは、NFSv3プロトコルを使用した共有ファイルシステムサービスです。複数のコンピュートインスタンスから同時にアクセス可能で、従来のNASと同様の使用感を提供します。

```mermaid
graph TB
    A[File Storage] --> B[インスタンス1]
    A --> C[インスタンス2]
    A --> D[インスタンス3]
    
    B --> B1[/mnt/shared]
    C --> C1[/mnt/shared]
    D --> D1[/mnt/shared]
    
    E[共有データ] --> A
    
    style A fill:#e3f2fd
    style E fill:#e8f5e8
```

#### OCIファイルストレージの特徴

**1. 高可用性**
- 99.95%の可用性SLA
- 自動的な冗長化
- 複数ADでのレプリケーション

**2. 高性能**
- 最大2.3GB/sのスループット
- 低レイテンシアクセス
- 同時接続数制限なし

**3. 柔軟性**
- 8EB（エクサバイト）までの容量
- 動的な容量拡張
- スナップショット機能

#### ファイルシステム構成要素

```mermaid
graph TB
    A[ファイルシステム] --> B[マウントターゲット]
    A --> C[エクスポート]
    A --> D[エクスポートセット]
    
    B --> B1[VCN内のIPアドレス]
    B --> B2[セキュリティリスト]
    
    C --> C1[ファイルシステムパス]
    C --> C2[アクセス権限]
    
    D --> D1[エクスポートのグループ]
    D --> D2[NFSオプション]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### 設計パターン

**1. 共有アプリケーションデータ**

```mermaid
graph TB
    subgraph "Webサーバー群"
        A1[Web Server 1]
        A2[Web Server 2]
        A3[Web Server 3]
    end
    
    B[File Storage<br/>共有コンテンツ] --> A1
    B --> A2
    B --> A3
    
    C[ロードバランサー] --> A1
    C --> A2
    C --> A3
    
    style B fill:#e3f2fd
```

**2. 開発環境での共有**

```mermaid
graph TB
    A[File Storage<br/>プロジェクト共有] --> B[開発者1のVM]
    A --> C[開発者2のVM]
    A --> D[CI/CDサーバー]
    
    E[ソースコード] --> A
    F[ビルド成果物] --> A
    G[設定ファイル] --> A
    
    style A fill:#e3f2fd
    style E fill:#e8f5e8
    style F fill:#fff3e0
    style G fill:#fce4ec
```

#### セキュリティ設定

**1. ネットワークセキュリティ**
```bash
# セキュリティリストでNFS通信を許可
# TCP 111, 2048-2050 (NFS)
# UDP 111, 2048 (NFS)
```

**2. エクスポートオプション**
- **root_squash**: rootユーザーのアクセス制限
- **all_squash**: 全ユーザーのアクセス制限
- **anonuid/anongid**: 匿名ユーザーのUID/GID設定

#### 使用例

**マウント設定例：**
```bash
# マウントポイント作成
sudo mkdir /mnt/shared

# NFSマウント
sudo mount -t nfs -o vers=3 \
  <mount-target-ip>:/<export-path> /mnt/shared

# 永続化設定
echo '<mount-target-ip>:/<export-path> /mnt/shared nfs vers=3,defaults 0 0' \
  | sudo tee -a /etc/fstab
```

### 3.4 Archive Storage（アーカイブストレージ）

#### アーカイブストレージとは

アーカイブストレージは、長期保存とコンプライアンス要件に特化した超低コストストレージサービスです。頻繁にアクセスしないデータの長期保管に最適です。

```mermaid
graph TB
    A[データライフサイクル] --> B[アクティブデータ<br/>Block/File Storage]
    B --> C[低頻度アクセス<br/>Object Storage IA]
    C --> D[アーカイブデータ<br/>Archive Storage]
    
    E[コスト] --> F[高]
    E --> G[中]
    E --> H[低]
    
    F --> B
    G --> C
    H --> D
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### アーカイブストレージの特徴

**1. 超低コスト**
- 標準ストレージの1/10のコスト
- 長期保存に最適化
- 最小保存期間90日

**2. 高耐久性**
- 99.999999999%（11 9's）の耐久性
- 地理的分散レプリケーション
- 自動データ整合性チェック

**3. コンプライアンス対応**
- WORM（Write Once Read Many）
- 法的保持要件対応
- 監査ログ

#### 取得プロセス

```mermaid
graph LR
    A[アーカイブ要求] --> B[復元処理<br/>1-4時間]
    B --> C[一時的利用可能<br/>24時間]
    C --> D[自動削除]
    
    style A fill:#e3f2fd
    style B fill:#fff3e0
    style C fill:#e8f5e8
    style D fill:#ffebee
```

#### 使用例

**1. 法的要件対応**
- 財務記録の7年保存
- 医療記録の長期保存
- 監査ログの保管

**2. バックアップ戦略**
```mermaid
graph TB
    A[日次バックアップ] --> B[Block Storage<br/>7日保持]
    B --> C[Object Storage<br/>30日保持]
    C --> D[Archive Storage<br/>7年保持]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

### 3.5 データ移行サービス

#### データ移行の課題

大容量データのクラウド移行には、以下のような課題があります：

```mermaid
graph TB
    A[データ移行の課題] --> B[ネットワーク帯域制限]
    A --> C[移行時間の長期化]
    A --> D[セキュリティリスク]
    A --> E[コスト増大]
    
    B --> B1[インターネット回線の制限]
    B --> B2[帯域幅の競合]
    
    C --> C1[業務への影響]
    C --> C2[ダウンタイム]
    
    D --> D1[データ漏洩リスク]
    D --> D2[転送中の暗号化]
    
    E --> E1[データ転送料金]
    E --> E2[時間コスト]
    
    style A fill:#ffebee
```

#### OCIデータ移行サービス

**1. Data Transfer Service**

```mermaid
graph LR
    A[オンプレミス] --> B[データ転送アプライアンス]
    B --> C[物理輸送]
    C --> D[OCIデータセンター]
    D --> E[Object Storage]
    
    style B fill:#e3f2fd
    style C fill:#fff3e0
    style E fill:#e8f5e8
```

**特徴：**
- 150TB/480TBアプライアンス
- 暗号化された物理輸送
- ペタバイト規模の移行対応

**2. Storage Gateway**

```mermaid
graph TB
    A[オンプレミス] --> B[Storage Gateway]
    B --> C[インターネット/専用線]
    C --> D[Object Storage]
    
    E[既存アプリケーション] --> B
    F[NFS/SMBプロトコル] --> B
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
```

**3. Database Migration Service**
- Oracle Database専用移行ツール
- ゼロダウンタイム移行
- 自動データ同期

#### 移行戦略の選択

| データ量 | 推奨方法 | 移行時間 | コスト |
|---------|----------|----------|--------|
| ~1TB | インターネット | 数時間-1日 | 低 |
| 1-10TB | 専用線 | 1-3日 | 中 |
| 10-100TB | Data Transfer Service | 1-2週間 | 中 |
| 100TB+ | Data Transfer Service | 2-4週間 | 高 |

### 3.6 パフォーマンス特性と選定指針

#### ストレージ性能比較

```mermaid
graph TB
    A[性能要件] --> B[レイテンシ]
    A --> C[スループット]
    A --> D[IOPS]
    A --> E[同時接続数]
    
    B --> B1[Block: <1ms]
    B --> B2[File: 1-5ms]
    B --> B3[Object: 10-100ms]
    
    C --> C1[Block: 480MB/s]
    C --> C2[File: 2.3GB/s]
    C --> C3[Object: 無制限]
    
    D --> D1[Block: 32,000]
    D --> D2[File: 7,000]
    D --> D3[Object: N/A]
    
    E --> E1[Block: 1]
    E --> E2[File: 無制限]
    E --> E3[Object: 無制限]
    
    style B1 fill:#e8f5e8
    style C2 fill:#e8f5e8
    style C3 fill:#e8f5e8
    style E2 fill:#e8f5e8
    style E3 fill:#e8f5e8
```

#### 用途別推奨ストレージ

**1. データベース**
```mermaid
graph LR
    A[データベース要件] --> B[Block Storage<br/>Higher Performance]
    B --> C[30-120 IOPS/GB]
    B --> D[低レイテンシ]
    B --> E[高可用性]
```

**2. Webアプリケーション**
```mermaid
graph TB
    A[Webアプリケーション] --> B[静的コンテンツ]
    A --> C[アプリケーションファイル]
    A --> D[ユーザーアップロード]
    A --> E[ログファイル]
    
    B --> F[Object Storage]
    C --> G[File Storage]
    D --> H[Object Storage]
    E --> I[Object Storage IA]
    
    style F fill:#e3f2fd
    style G fill:#e8f5e8
    style H fill:#e3f2fd
    style I fill:#fff3e0
```

**3. ビッグデータ分析**
```mermaid
graph LR
    A[Raw Data] --> B[Object Storage<br/>Standard]
    B --> C[処理エンジン]
    C --> D[Object Storage<br/>結果データ]
    D --> E[Archive Storage<br/>長期保存]
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#fff3e0
```

#### コスト最適化戦略

**1. ストレージクラス最適化**

```mermaid
graph TB
    A[コスト最適化] --> B[ライフサイクル管理]
    A --> C[重複排除]
    A --> D[圧縮]
    A --> E[適切なサイジング]
    
    B --> B1[自動階層化]
    B --> B2[古いデータの削除]
    
    C --> C1[同一データの統合]
    
    D --> D1[データ圧縮]
    D --> D2[転送量削減]
    
    E --> E1[使用量監視]
    E --> E2[容量計画]
    
    style A fill:#e8f5e8
```

**2. 監視とアラート**
- 使用量の定期監視
- 閾値アラートの設定
- コスト分析レポート

### まとめ

第3章では、OCIのストレージサービスについて詳しく解説しました。各ストレージサービスの特徴を理解し、用途に応じた適切な選択が重要です。

**重要ポイント：**
1. **Block Storage**: 高性能が必要なデータベースやアプリケーション
2. **Object Storage**: スケーラブルで耐久性の高いデータ保存
3. **File Storage**: 複数インスタンスでの共有ファイルシステム
4. **Archive Storage**: 長期保存とコンプライアンス対応
5. **適切な選択**: 性能、コスト、可用性要件のバランス

次章では、これらのストレージサービスを含むシステム全体を支えるネットワーキングサービスについて学習します。