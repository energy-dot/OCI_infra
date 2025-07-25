# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 第1章 OCI基礎概念

### はじめに

Oracle Cloud Infrastructure（OCI）は、Oracleが提供するエンタープライズグレードのクラウドプラットフォームです。この章では、OCIを理解するための基礎概念について、クラウド初心者でも理解できるよう詳しく解説します。

#### なぜクラウドが必要なのか

従来のIT環境では、企業は自社でサーバーやネットワーク機器を購入し、データセンターを構築・運用していました。しかし、この方式には以下のような課題がありました：

**従来のオンプレミス環境の課題とクラウドによる解決：**

## オンプレミスとクラウドの比較表

| 比較カテゴリ | オンプレミスの課題 | クラウドの利点 |
| :--- | :--- | :--- |
| **初期投資** | • 高額なサーバー購入<br>• ソフトウェアライセンス<br>• データセンター構築 | • 設備投資が不要<br>• 従量課金制<br>• 予測可能なコスト |
| **迅速性** | • 長期調達プロセス<br>• 数週間の設置・設定<br>• テスト・検証に時間 | • 数分でリソース調達<br>• 自動化された設定<br>• 即座のテスト環境 |
| **運用負荷** | • 24/7の自己監視<br>• 保守・メンテナンス<br>• セキュリティ対策<br>• バックアップ管理 | • プロバイダーが管理<br>• 自動パッチ適用<br>• 組み込みセキュリティ<br>• 自動バックアップ |
| **スケーラビリティ** | • ピーク時の性能不足<br>• 閑散期のリソース無駄<br>• 需要増への対応困難 | • 需要に応じた自動拡張・縮小<br>• グローバル展開 |
| **災害復旧** | • 複数拠点の構築・管理<br>• 複雑なデータ同期<br>• 複雑な復旧手順 | • 複数リージョンを容易に活用<br>• 自動レプリケーション<br>• ワンクリック復旧 |


#### クラウドサービスモデル

クラウドサービスは、提供される機能レベルによって3つの主要なモデルに分類されます：

```mermaid
graph TB
    subgraph "IaaS (Infrastructure as a Service)"
        A1[仮想マシン]
        A2[ストレージ]
        A3[ネットワーク]
        A4[ロードバランサー]
    end
    
    subgraph "PaaS (Platform as a Service)"
        B1[データベース]
        B2[アプリケーション実行環境]
        B3[開発ツール]
        B4[ミドルウェア]
    end
    
    subgraph "SaaS (Software as a Service)"
        C1[Webアプリケーション]
        C2[メール]
        C3[CRM]
        C4[ERP]
    end
    
    D[ユーザー管理範囲] --> A1
    E[プロバイダー管理範囲] --> B1
    F[完全管理] --> C1
    
    style A1 fill:#e3f2fd
    style B1 fill:#e8f5e8
    style C1 fill:#fff3e0
```

**各モデルの詳細比較：**

| 項目 | IaaS | PaaS | SaaS |
|------|------|------|------|
| **ユーザー管理** | OS、ミドルウェア、アプリ | アプリケーション | 設定のみ |
| **プロバイダー管理** | ハードウェア、仮想化 | OS、ミドルウェア、ランタイム | 全て |
| **柔軟性** | 最高 | 中 | 最低 |
| **管理負荷** | 最高 | 中 | 最低 |
| **導入速度** | 中 | 高 | 最高 |
| **カスタマイズ性** | 最高 | 中 | 最低 |

#### OCIの市場ポジション

OCIは、エンタープライズ向けクラウドサービス市場において独自のポジションを確立しています：

```mermaid
graph TB
    A[クラウド市場] --> B[AWS<br/>市場リーダー]
    A --> C[Microsoft Azure<br/>エンタープライズ強化]
    A --> D[Google Cloud<br/>AI/ML特化]
    A --> E[Oracle Cloud<br/>データベース特化]
    
    B --> B1[豊富なサービス]
    B --> B2[グローバル展開]
    B --> B3[エコシステム]
    
    C --> C1[Office 365統合]
    C --> C2[ハイブリッド対応]
    C --> C3[エンタープライズ営業]
    
    D --> D1[機械学習]
    D --> D2[ビッグデータ]
    D --> D3[Kubernetes]
    
    E --> E1[Oracle Database最適化]
    E --> E2[予測可能な料金]
    E --> E3[高性能インフラ]
    
    style E fill:#e8f5e8
```

**OCIの差別化要因：**

1. **Oracle Database最適化**
   - Exadata Cloud Service
   - Autonomous Database
   - 既存Oracle環境からの移行容易性

2. **予測可能な料金体系**
   - 複雑な料金計算なし
   - データ転送料金の大幅削減
   - 透明性の高い課金

3. **高性能インフラ**
   - ベアメタルインスタンス
   - 高速ネットワーク
   - NVMe SSDストレージ

### 1.1 OCIアーキテクチャ概要

#### クラウドコンピューティングとは

クラウドコンピューティングとは、インターネット経由でコンピューティングリソース（サーバー、ストレージ、データベース、ネットワーク、ソフトウェアなど）を提供するサービスモデルです。従来のオンプレミス（自社内設置）環境と比較して、以下のメリットがあります：

- **初期投資の削減**: 物理的なハードウェア購入が不要
- **スケーラビリティ**: 需要に応じてリソースを柔軟に増減
- **運用負荷の軽減**: インフラの保守・管理をクラウドプロバイダーが担当
- **高可用性**: 複数のデータセンターによる冗長化

#### OCIの特徴

OCIは以下の特徴を持つクラウドプラットフォームです：

```mermaid
graph TB
    A[Oracle Cloud Infrastructure] --> B[高性能]
    A --> C[セキュリティ]
    A --> D[コスト効率]
    A --> E[エンタープライズ対応]
    
    B --> B1[ベアメタルインスタンス]
    B --> B2[高速ネットワーク]
    B --> B3[NVMe SSD]
    
    C --> C1[デフォルト暗号化]
    C --> C2[ネットワーク分離]
    C --> C3[IAM統合]
    
    D --> D1[予測可能な料金]
    D --> D2[従量課金制]
    D --> D3[リザーブドインスタンス]
    
    E --> E1[Oracle Database最適化]
    E --> E2[エンタープライズSLA]
    E --> E3[24/7サポート]
```

#### OCIアーキテクチャの階層構造

OCIは以下の階層構造で構成されています：

```mermaid
graph TD
    A[テナンシー<br/>Tenancy] --> B[リージョン<br/>Region]
    B --> C[アベイラビリティドメイン<br/>Availability Domain]
    C --> D[フォルトドメイン<br/>Fault Domain]
    D --> E[コンパートメント<br/>Compartment]
    E --> F[リソース<br/>Resources]
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
    style F fill:#f1f8e9
```

### 1.2 リージョンとアベイラビリティドメイン

#### リージョン（Region）

リージョンは、地理的に分散した独立したデータセンターの集合体です。各リージョンは以下の特徴を持ちます：

**主要パラメータ：**
- **地理的位置**: 東京、大阪、シンガポール、フランクフルトなど
- **レイテンシ**: ユーザーとの物理的距離による通信遅延
- **データ主権**: 各国の法規制への対応
- **災害復旧**: 地理的分散による災害対策

```mermaid
graph LR
    A[アジア太平洋地域] --> A1[東京 ap-tokyo-1]
    A --> A2[大阪 ap-osaka-1]
    A --> A3[シンガポール ap-singapore-1]
    A --> A4[ソウル ap-seoul-1]
    
    B[北米地域] --> B1[アッシュバーン us-ashburn-1]
    B --> B2[フェニックス us-phoenix-1]
    B --> B3[サンノゼ us-sanjose-1]
    
    C[ヨーロッパ地域] --> C1[フランクフルト eu-frankfurt-1]
    C --> C2[ロンドン uk-london-1]
    C --> C3[アムステルダム eu-amsterdam-1]
```

**リージョン選択の判断基準：**

**1. レイテンシ要件**
   - エンドユーザーに最も近いリージョンを選択
   - 一般的に100ms以下が推奨
   - リアルタイム処理では50ms以下が理想

```mermaid
graph LR
    A[東京ユーザー] -->|5-10ms| B[東京リージョン]
    A -->|150-200ms| C[シンガポールリージョン]
    A -->|180-250ms| D[フランクフルトリージョン]
    
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#ffebee
```

**2. データ主権・コンプライアンス**
   - **個人情報保護法**: 日本の個人情報は日本国内保管が推奨
   - **GDPR**: EU市民のデータはEU内保管が必要
   - **業界固有規制**: 金融、医療、政府系の特別要件
   - **データローカライゼーション**: 各国の法的要求事項

```mermaid
graph TB
    A[データ主権要件] --> B[日本の個人情報保護法]
    A --> C[EU GDPR]
    A --> D[米国SOX法]
    A --> E[中国サイバーセキュリティ法]
    
    B --> B1[日本リージョン必須]
    C --> C1[EU圏内リージョン必須]
    D --> D2[監査ログ保管要件]
    E --> E1[中国本土データ保管]
    
    style B1 fill:#e8f5e8
    style C1 fill:#e3f2fd
    style D2 fill:#fff3e0
    style E1 fill:#fce4ec
```

**3. 災害復旧要件**
   - プライマリとセカンダリリージョンの組み合わせ
   - 地理的分散による自然災害対策
   - RTO（Recovery Time Objective）とRPO（Recovery Point Objective）の要件

**災害復旧設計パターン：**

```mermaid
graph TB
    subgraph "プライマリリージョン（東京）"
        A1[本番システム]
        A2[リアルタイムデータ]
    end
    
    subgraph "セカンダリリージョン（大阪）"
        B1[スタンバイシステム]
        B2[レプリケーションデータ]
    end
    
    A1 -.->|データ同期| B1
    A2 -.->|レプリケーション| B2
    
    C[災害発生] --> D[フェイルオーバー]
    D --> B1
    
    style A1 fill:#e8f5e8
    style B1 fill:#fff3e0
    style C fill:#ffebee
```

**4. サービス可用性**
   - 利用したいサービスがリージョンで提供されているか
   - 新サービスの展開タイミング
   - 特殊なハードウェア要件（GPU、高メモリなど）

**5. コスト要件**
   - リージョンごとの料金差異
   - データ転送コスト
   - 為替レートの影響

**リージョン選択マトリックス例：**

| 要件 | 東京 | 大阪 | シンガポール | フランクフルト |
|------|------|------|-------------|---------------|
| **日本ユーザー向けレイテンシ** | ◎ | ◎ | △ | × |
| **日本の法規制対応** | ◎ | ◎ | × | × |
| **災害復旧ペア** | 大阪 | 東京 | 東京 | ロンドン |
| **サービス豊富さ** | ◎ | ○ | ◎ | ◎ |
| **コスト** | 標準 | 標準 | やや高 | やや高 |

#### アベイラビリティドメイン（Availability Domain: AD）

アベイラビリティドメインは、リージョン内の独立したデータセンターです。

```mermaid
graph TB
    subgraph "東京リージョン (ap-tokyo-1)"
        AD1[AD-1<br/>独立電源・ネットワーク]
        AD2[AD-2<br/>独立電源・ネットワーク]
        AD3[AD-3<br/>独立電源・ネットワーク]
    end
    
    AD1 -.->|高速専用線| AD2
    AD2 -.->|高速専用線| AD3
    AD3 -.->|高速専用線| AD1
    
    style AD1 fill:#e3f2fd
    style AD2 fill:#e8f5e8
    style AD3 fill:#fff3e0
```

**設計要素：**
- **独立性**: 電源、冷却、ネットワークが完全に独立
- **低レイテンシ**: AD間の通信は2ms以下
- **高可用性**: 単一ADの障害が他ADに影響しない設計

#### フォルトドメイン（Fault Domain）

フォルトドメインは、AD内のさらに細かい障害分離単位です。

```mermaid
graph TB
    subgraph "アベイラビリティドメイン"
        FD1[フォルトドメイン 1<br/>独立ラック・電源]
        FD2[フォルトドメイン 2<br/>独立ラック・電源]
        FD3[フォルトドメイン 3<br/>独立ラック・電源]
    end
    
    style FD1 fill:#e1f5fe
    style FD2 fill:#f3e5f5
    style FD3 fill:#e8f5e8
```

### 1.3 コンパートメント設計

#### コンパートメントとは

コンパートメントは、OCIリソースを論理的に分離・整理するための仕組みです。セキュリティ、管理、課金の観点から重要な概念です。

```mermaid
graph TD
    A[ルートコンパートメント<br/>Root Compartment] --> B[本番環境<br/>Production]
    A --> C[開発環境<br/>Development]
    A --> D[共通サービス<br/>Shared Services]
    
    B --> B1[Webアプリケーション]
    B --> B2[データベース]
    B --> B3[ネットワーク]
    
    C --> C1[開発用VM]
    C --> C2[テスト用DB]
    
    D --> D1[監視サービス]
    D --> D2[ログ管理]
    D --> D3[バックアップ]
    
    style A fill:#ffebee
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#e3f2fd
```

#### コンパートメント設計パターン

**1. 環境別分離パターン**
```mermaid
graph LR
    A[組織] --> B[本番環境]
    A --> C[ステージング環境]
    A --> D[開発環境]
    A --> E[検証環境]
```

**2. 部門別分離パターン**
```mermaid
graph LR
    A[組織] --> B[営業部門]
    A --> C[開発部門]
    A --> D[人事部門]
    A --> E[財務部門]
```

**3. プロジェクト別分離パターン**
```mermaid
graph LR
    A[組織] --> B[プロジェクトA]
    A --> C[プロジェクトB]
    A --> D[プロジェクトC]
    A --> E[共通基盤]
```

#### コンパートメント設計のベストプラクティス

**主要パラメータ：**
- **階層の深さ**: 最大6階層まで（推奨は3-4階層）
- **命名規則**: 一貫性のある命名パターン
- **アクセス制御**: 最小権限の原則
- **課金管理**: コスト配分とチャージバック

**設計要素：**

1. **セキュリティ境界**
   - 機密度レベルによる分離
   - アクセス権限の最小化
   - 監査ログの分離

2. **運用管理**
   - 責任範囲の明確化
   - 変更管理プロセス
   - 障害対応の分離

3. **コスト管理**
   - 部門別コスト配分
   - プロジェクト別予算管理
   - リソース使用量の可視化

### 1.4 IAM（Identity and Access Management）

#### IAMの基本概念

IAMは、OCIリソースへのアクセスを制御するセキュリティサービスです。「誰が」「何に」「どのような操作を」行えるかを定義します。

```mermaid
graph TB
    A[IAM] --> B[プリンシパル<br/>Principal]
    A --> C[リソース<br/>Resource]
    A --> D[アクション<br/>Action]
    A --> E[ポリシー<br/>Policy]
    
    B --> B1[ユーザー<br/>User]
    B --> B2[グループ<br/>Group]
    B --> B3[インスタンスプリンシパル<br/>Instance Principal]
    B --> B4[動的グループ<br/>Dynamic Group]
    
    C --> C1[コンピュートインスタンス]
    C --> C2[ストレージバケット]
    C --> C3[データベース]
    C --> C4[ネットワーク]
    
    D --> D1[読み取り<br/>Read]
    D --> D2[書き込み<br/>Write]
    D --> D3[削除<br/>Delete]
    D --> D4[管理<br/>Manage]
```

#### IAMコンポーネント詳細

**1. ユーザー（User）**
- 個人または外部システムを表すエンティティ
- 認証情報（パスワード、APIキー）を持つ
- 直接的な権限付与は非推奨

**2. グループ（Group）**
- 複数ユーザーをまとめる論理的な集合
- 役割ベースのアクセス制御（RBAC）を実現
- 権限管理の簡素化

**3. ポリシー（Policy）**
- アクセス権限を定義するルール
- 自然言語に近い構文で記述

```
Allow group <group_name> to <verb> <resource_type> in compartment <compartment_name>
```

#### ポリシー構文の詳細

**基本構文：**
```
Allow <subject> to <verb> <resource-type> in <location> where <conditions>
```

**主要パラメータ：**

1. **Subject（主体）**
   - `user <user_name>`
   - `group <group_name>`
   - `dynamic-group <dynamic_group_name>`
   - `any-user`

2. **Verb（動詞）**
   - `inspect`: リソースの一覧表示
   - `read`: リソースの詳細情報取得
   - `use`: リソースの使用
   - `manage`: 全ての操作（作成、更新、削除）

3. **Resource-type（リソースタイプ）**
   - `instances`: コンピュートインスタンス
   - `buckets`: オブジェクトストレージ
   - `databases`: データベース
   - `all-resources`: 全リソース

4. **Location（場所）**
   - `tenancy`: テナンシー全体
   - `compartment <name>`: 特定コンパートメント

#### IAM設計パターン

**1. 役割ベースアクセス制御（RBAC）**

```mermaid
graph TB
    A[管理者グループ] --> A1[全権限]
    B[開発者グループ] --> B1[開発環境の管理権限]
    B --> B2[本番環境の読み取り権限]
    C[運用者グループ] --> C1[本番環境の運用権限]
    C --> C2[監視・ログ権限]
    D[監査者グループ] --> D1[全環境の読み取り権限]
    D --> D2[監査ログアクセス権限]
```

**2. 最小権限の原則**

```mermaid
graph LR
    A[ユーザー] --> B[必要最小限の権限]
    B --> C[定期的な権限見直し]
    C --> D[不要権限の削除]
    D --> A
```

#### セキュリティベストプラクティス

**認証強化：**
1. **多要素認証（MFA）**
   - 管理者アカウントは必須
   - TOTP（Time-based One-Time Password）
   - SMS認証（非推奨）

2. **フェデレーション**
   - Active Directory統合
   - SAML 2.0対応
   - シングルサインオン（SSO）

**アクセス制御：**
1. **ネットワーク制限**
   - 許可IPアドレスからのみアクセス
   - VPN経由のアクセス強制

2. **時間制限**
   - 業務時間内のみアクセス許可
   - 定期的なパスワード変更

### 1.5 課金体系とコスト管理

#### OCIの課金モデル

OCIは従量課金制を基本とし、使用した分だけ支払う仕組みです。

```mermaid
graph TB
    A[OCI課金モデル] --> B[従量課金<br/>Pay-as-you-go]
    A --> C[月額固定<br/>Monthly Flex]
    A --> D[年額契約<br/>Annual Flex]
    
    B --> B1[時間単位課金]
    B --> B2[使用量課金]
    B --> B3[リクエスト課金]
    
    C --> C1[月額コミット]
    C --> C2[割引適用]
    C --> C3[柔軟な使用]
    
    D --> D1[年額コミット]
    D --> D2[最大割引]
    D --> D3[予算確定]
```

#### 主要サービスの課金体系

**1. コンピュート（Compute）**
- **課金単位**: 時間単位（秒単位で計算）
- **課金要素**: 
  - CPU数（OCPU: Oracle CPU）
  - メモリ容量（GB）
  - インスタンス稼働時間

**2. ストレージ（Storage）**
- **Block Storage**: 容量（GB/月）+ IOPS
- **Object Storage**: 容量（GB/月）+ リクエスト数
- **File Storage**: 容量（GB/月）

**3. ネットワーク（Network）**
- **アウトバウンド転送**: 月10TBまで無料、以降従量課金
- **インバウンド転送**: 無料
- **リージョン間転送**: 従量課金

#### コスト最適化戦略

**1. リソースサイジング**

```mermaid
graph LR
    A[現在のリソース] --> B[使用率監視]
    B --> C[適正サイズ判定]
    C --> D[リサイズ実行]
    D --> E[コスト削減]
    
    style A fill:#ffebee
    style E fill:#e8f5e8
```

**主要パラメータ：**
- **CPU使用率**: 平均70-80%が目安
- **メモリ使用率**: 平均80-90%が目安
- **ストレージ使用率**: 80%以下を維持

**2. 自動スケーリング**

```mermaid
graph TB
    A[負荷監視] --> B{閾値判定}
    B -->|高負荷| C[スケールアウト]
    B -->|低負荷| D[スケールイン]
    C --> E[インスタンス追加]
    D --> F[インスタンス削除]
    E --> A
    F --> A
```

**3. 予約インスタンス**

| 契約期間 | 割引率 | 適用シーン |
|---------|--------|-----------|
| 1年 | 20-30% | 安定稼働システム |
| 3年 | 40-50% | 長期運用システム |

**4. スケジューリング**

```mermaid
gantt
    title リソーススケジューリング例
    dateFormat HH:mm
    axisFormat %H:%M
    
    section 開発環境
    稼働時間    :active, dev, 09:00, 18:00
    停止時間    :done, 18:00, 09:00
    
    section 検証環境
    稼働時間    :active, test, 08:00, 20:00
    停止時間    :done, 20:00, 08:00
```

#### コスト監視とアラート

**1. 予算設定**
- 月額予算の設定
- 閾値アラート（80%, 100%, 120%）
- 部門別予算配分

**2. コスト分析**
- サービス別コスト内訳
- 時系列でのコスト推移
- 予実管理とレポート

**3. タグ戦略**

```mermaid
graph TB
    A[リソースタグ] --> B[環境タグ]
    A --> C[プロジェクトタグ]
    A --> D[所有者タグ]
    A --> E[コストセンタータグ]
    
    B --> B1[Production]
    B --> B2[Development]
    B --> B3[Staging]
    
    C --> C1[Project-A]
    C --> C2[Project-B]
    
    D --> D1[team-web]
    D --> D2[team-db]
    
    E --> E1[CC-001]
    E --> E2[CC-002]
```

#### 他クラウドとの比較

**主要クラウドプロバイダー比較：**

| 項目 | OCI | AWS | Azure | GCP |
|------|-----|-----|-------|-----|
| 課金単位 | 秒 | 秒 | 分 | 秒 |
| 無料枠 | Always Free | 12ヶ月 | 12ヶ月 | 12ヶ月 |
| 予約割引 | 最大50% | 最大75% | 最大72% | 最大57% |
| データ転送 | 10TB/月無料 | 1GB/月無料 | 5GB/月無料 | 1GB/月無料 |

**選定判断基準：**

1. **既存システム連携**
   - Oracle Database使用 → OCI有利
   - Microsoft製品中心 → Azure有利
   - オープンソース中心 → GCP有利

2. **コスト要件**
   - 予測可能な料金 → OCI
   - 最安値重視 → 各社比較必要
   - 複雑な割引体系 → AWS

3. **技術要件**
   - ベアメタル性能 → OCI
   - 豊富なサービス → AWS
   - AI/ML重視 → GCP

### まとめ

第1章では、OCIの基礎概念について詳しく解説しました。次章では、これらの基礎知識を踏まえて、具体的なコンピュートサービスについて学習していきます。

**重要ポイント：**
1. リージョン・AD・コンパートメントの階層構造理解
2. IAMによる適切なアクセス制御設計
3. コスト最適化を考慮した運用計画
4. セキュリティベストプラクティスの実装

次章では、OCIの中核となるコンピュートサービスについて、サービス選定基準や設計パターンを含めて詳しく解説します。