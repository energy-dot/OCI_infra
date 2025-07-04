# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 第4章 ネットワーキングサービス

### はじめに

第3章でストレージサービスについて学習しました。本章では、OCIの基盤となるネットワーキングサービスについて詳しく解説します。ネットワークは、クラウドインフラストラクチャの神経系とも言える重要な要素で、セキュリティ、性能、可用性のすべてに影響を与えます。

### ネットワーキングの基本概念

#### なぜクラウドネットワーキングが重要なのか

従来のオンプレミス環境では、物理的なネットワーク機器（スイッチ、ルーター、ファイアウォール）を購入・設置・設定する必要がありました。しかし、クラウド環境では、これらがすべてソフトウェアで定義され、APIやコンソールから管理できます。

```mermaid
graph TB
    subgraph "従来のオンプレミス"
        A1[物理スイッチ] --> A2[物理ルーター]
        A2 --> A3[物理ファイアウォール]
        A3 --> A4[インターネット]
        
        B1[設定の複雑さ]
        B2[ハードウェア調達]
        B3[物理配線]
        B4[障害対応]
    end
    
    subgraph "クラウドネットワーキング"
        C1[仮想スイッチ] --> C2[仮想ルーター]
        C2 --> C3[仮想ファイアウォール]
        C3 --> C4[インターネット]
        
        D1[API/コンソール設定]
        D2[即座のプロビジョニング]
        D3[ソフトウェア定義]
        D4[自動復旧]
    end
    
    style A1 fill:#ffebee
    style A2 fill:#ffebee
    style A3 fill:#ffebee
    style C1 fill:#e8f5e8
    style C2 fill:#e8f5e8
    style C3 fill:#e8f5e8
```

#### ネットワーキングの基本要素

クラウドネットワーキングを理解するために、まず基本的な概念を整理しましょう：

```mermaid
graph TB
    A[ネットワーキング基本要素] --> B[IPアドレス]
    A --> C[サブネット]
    A --> D[ルーティング]
    A --> E[セキュリティ]
    A --> F[負荷分散]
    
    B --> B1[パブリックIP]
    B --> B2[プライベートIP]
    B --> B3[CIDR記法]
    
    C --> C1[ネットワーク分割]
    C --> C2[ブロードキャストドメイン]
    C --> C3[セキュリティ境界]
    
    D --> D1[ルートテーブル]
    D --> D2[デフォルトゲートウェイ]
    D --> D3[静的/動的ルーティング]
    
    E --> E1[ファイアウォール]
    E --> E2[アクセス制御リスト]
    E --> E3[VPN]
    
    F --> F1[トラフィック分散]
    F --> F2[ヘルスチェック]
    F --> F3[フェイルオーバー]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
    style F fill:#f1f8e9
```

#### IPアドレスとCIDR記法の詳細解説

**IPアドレスの基本：**

IPアドレスは、ネットワーク上のデバイスを識別するための32ビット（IPv4）の数値です。通常は4つのオクテット（8ビットずつ）に分けて表記されます。

```mermaid
graph LR
    A[192.168.1.100] --> B[11000000.10101000.00000001.01100100]
    B --> C[ネットワーク部]
    B --> D[ホスト部]
    
    style C fill:#e3f2fd
    style D fill:#e8f5e8
```

**CIDR記法の理解：**

CIDR（Classless Inter-Domain Routing）記法は、ネットワークアドレスとサブネットマスクを組み合わせた表記法です。

| CIDR | サブネットマスク | 利用可能IP数 | 用途例 |
|------|----------------|-------------|--------|
| /8 | 255.0.0.0 | 16,777,214 | 大規模ネットワーク |
| /16 | 255.255.0.0 | 65,534 | 中規模ネットワーク |
| /24 | 255.255.255.0 | 254 | 小規模ネットワーク |
| /28 | 255.255.255.240 | 14 | 極小ネットワーク |

**サブネット設計例：**

```mermaid
graph TB
    A[VCN: 10.0.0.0/16] --> B[パブリックサブネット<br/>10.0.1.0/24]
    A --> C[プライベートサブネット<br/>10.0.2.0/24]
    A --> D[データベースサブネット<br/>10.0.3.0/24]
    
    B --> B1[Webサーバー<br/>10.0.1.10-50]
    C --> C1[アプリサーバー<br/>10.0.2.10-50]
    D --> D1[データベース<br/>10.0.3.10-20]
    
    style B fill:#e3f2fd
    style C fill:#fff3e0
    style D fill:#fce4ec
```

### 4.1 Virtual Cloud Networks (VCN)

#### VCNとは

Virtual Cloud Network（VCN）は、OCIにおける仮想ネットワークの基盤です。従来のデータセンターネットワークをクラウド上で再現し、完全に分離された専用ネットワーク環境を提供します。

```mermaid
graph TB
    subgraph "OCI テナンシー"
        subgraph "VCN 1 (本番環境)"
            A1[パブリックサブネット]
            A2[プライベートサブネット]
            A3[データベースサブネット]
        end
        
        subgraph "VCN 2 (開発環境)"
            B1[パブリックサブネット]
            B2[プライベートサブネット]
        end
        
        subgraph "VCN 3 (管理環境)"
            C1[管理サブネット]
            C2[監視サブネット]
        end
    end
    
    D[インターネット] --> A1
    D --> B1
    
    style A1 fill:#e3f2fd
    style A2 fill:#fff3e0
    style A3 fill:#fce4ec
    style B1 fill:#e3f2fd
    style B2 fill:#fff3e0
    style C1 fill:#e8f5e8
    style C2 fill:#f1f8e9
```

#### VCNの主要コンポーネント

**1. サブネット（Subnet）**

サブネットは、VCN内のIPアドレス範囲を分割したものです。セキュリティとトラフィック制御の基本単位となります。

```mermaid
graph TB
    A[サブネットタイプ] --> B[パブリックサブネット]
    A --> C[プライベートサブネット]
    
    B --> B1[インターネットゲートウェイ経由]
    B --> B2[パブリックIPアドレス]
    B --> B3[外部からアクセス可能]
    
    C --> C1[NATゲートウェイ経由]
    C --> C2[プライベートIPのみ]
    C --> C3[外部から直接アクセス不可]
    
    style B fill:#e3f2fd
    style C fill:#fff3e0
```

**サブネット設計のベストプラクティス：**

| 層 | サブネットタイプ | 用途 | セキュリティレベル |
|----|----------------|------|-------------------|
| Web層 | パブリック | ロードバランサー、Webサーバー | 中 |
| App層 | プライベート | アプリケーションサーバー | 高 |
| DB層 | プライベート | データベースサーバー | 最高 |
| 管理層 | プライベート | 踏み台サーバー、監視 | 高 |

**2. ルートテーブル（Route Table）**

ルートテーブルは、ネットワークトラフィックの経路を定義します。各サブネットには必ずルートテーブルが関連付けられます。

```mermaid
graph LR
    A[送信元] --> B[ルートテーブル]
    B --> C{宛先判定}
    
    C -->|0.0.0.0/0| D[インターネットゲートウェイ]
    C -->|10.0.0.0/16| E[ローカル]
    C -->|192.168.0.0/16| F[DRGゲートウェイ]
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#fff3e0
    style F fill:#fce4ec
```

**ルートテーブル設定例：**

```bash
# パブリックサブネット用ルートテーブル
宛先CIDR: 0.0.0.0/0 → ターゲット: インターネットゲートウェイ
宛先CIDR: 10.0.0.0/16 → ターゲット: ローカル

# プライベートサブネット用ルートテーブル
宛先CIDR: 0.0.0.0/0 → ターゲット: NATゲートウェイ
宛先CIDR: 10.0.0.0/16 → ターゲット: ローカル
宛先CIDR: 192.168.0.0/16 → ターゲット: DRG
```

**3. セキュリティリスト（Security List）**

セキュリティリストは、サブネットレベルでのファイアウォール機能を提供します。インバウンド（受信）とアウトバウンド（送信）のトラフィックを制御できます。

```mermaid
graph TB
    A[セキュリティリスト] --> B[インバウンドルール]
    A --> C[アウトバウンドルール]
    
    B --> B1[送信元IP/CIDR]
    B --> B2[プロトコル]
    B --> B3[ポート範囲]
    B --> B4[許可/拒否]
    
    C --> C1[宛先IP/CIDR]
    C --> C2[プロトコル]
    C --> C3[ポート範囲]
    C --> C4[許可/拒否]
    
    style B fill:#e8f5e8
    style C fill:#fff3e0
```

**セキュリティリスト設定例：**

| 方向 | プロトコル | 送信元/宛先 | ポート | 用途 |
|------|-----------|------------|--------|------|
| インバウンド | TCP | 0.0.0.0/0 | 80 | HTTP |
| インバウンド | TCP | 0.0.0.0/0 | 443 | HTTPS |
| インバウンド | TCP | 10.0.0.0/16 | 22 | SSH（内部のみ） |
| アウトバウンド | TCP | 0.0.0.0/0 | 80,443 | HTTP/HTTPS |
| アウトバウンド | TCP | 10.0.2.0/24 | 3306 | MySQL |

#### VCN設計パターン

**1. 単一VCN構成（小規模）**

```mermaid
graph TB
    subgraph "VCN: 10.0.0.0/16"
        A[パブリックサブネット<br/>10.0.1.0/24] --> B[プライベートサブネット<br/>10.0.2.0/24]
        B --> C[データベースサブネット<br/>10.0.3.0/24]
    end
    
    D[インターネット] --> A
    A --> E[Webサーバー]
    B --> F[アプリサーバー]
    C --> G[データベース]
    
    style A fill:#e3f2fd
    style B fill:#fff3e0
    style C fill:#fce4ec
```

**2. 複数VCN構成（大規模）**

```mermaid
graph TB
    subgraph "本番VCN: 10.0.0.0/16"
        A1[Web層: 10.0.1.0/24]
        A2[App層: 10.0.2.0/24]
        A3[DB層: 10.0.3.0/24]
    end
    
    subgraph "開発VCN: 10.1.0.0/16"
        B1[Web層: 10.1.1.0/24]
        B2[App層: 10.1.2.0/24]
        B3[DB層: 10.1.3.0/24]
    end
    
    subgraph "共通VCN: 10.2.0.0/16"
        C1[管理層: 10.2.1.0/24]
        C2[監視層: 10.2.2.0/24]
    end
    
    D[VCNピアリング] --> A1
    D --> B1
    D --> C1
    
    style A1 fill:#e8f5e8
    style B1 fill:#fff3e0
    style C1 fill:#e3f2fd
```

#### ゲートウェイの詳細

**1. インターネットゲートウェイ（Internet Gateway）**

インターネットゲートウェイは、VCNとインターネット間の通信を可能にします。

```mermaid
graph LR
    A[インターネット] <--> B[インターネットゲートウェイ]
    B <--> C[パブリックサブネット]
    C --> D[Webサーバー<br/>パブリックIP]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

**特徴：**
- VCNあたり1つまで
- パブリックIPアドレスが必要
- ステートフル通信

**2. NATゲートウェイ（NAT Gateway）**

NATゲートウェイは、プライベートサブネットからインターネットへの一方向通信を可能にします。

```mermaid
graph LR
    A[プライベートサブネット] --> B[NATゲートウェイ]
    B --> C[インターネット]
    C -.->|戻りトラフィックのみ| B
    B -.-> A
    
    style A fill:#fff3e0
    style B fill:#e3f2fd
    style C fill:#e8f5e8
```

**用途：**
- ソフトウェア更新
- 外部APIアクセス
- ログ送信

**3. サービスゲートウェイ（Service Gateway）**

サービスゲートウェイは、OCIサービスへのプライベート接続を提供します。

```mermaid
graph LR
    A[プライベートサブネット] --> B[サービスゲートウェイ]
    B --> C[Object Storage]
    B --> D[Autonomous Database]
    B --> E[その他OCIサービス]
    
    style A fill:#fff3e0
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#e8f5e8
    style E fill:#e8f5e8
```

**利点：**
- インターネット経由不要
- 高速・低レイテンシ
- データ転送料金なし

#### VCN作成の実践例

**1. VCN作成（CLI）**

```bash
# VCN作成
oci network vcn create \
  --compartment-id <compartment-id> \
  --display-name "production-vcn" \
  --cidr-block "10.0.0.0/16" \
  --dns-label "prodvcn"

# インターネットゲートウェイ作成
oci network internet-gateway create \
  --compartment-id <compartment-id> \
  --vcn-id <vcn-id> \
  --display-name "production-igw" \
  --is-enabled true

# パブリックサブネット作成
oci network subnet create \
  --compartment-id <compartment-id> \
  --vcn-id <vcn-id> \
  --display-name "public-subnet" \
  --cidr-block "10.0.1.0/24" \
  --route-table-id <route-table-id> \
  --security-list-ids '["<security-list-id>"]' \
  --dns-label "public"
```

**2. セキュリティリスト設定**

```bash
# HTTPアクセス許可ルール追加
oci network security-list update \
  --security-list-id <security-list-id> \
  --ingress-security-rules '[
    {
      "protocol": "6",
      "source": "0.0.0.0/0",
      "tcpOptions": {
        "destinationPortRange": {
          "min": 80,
          "max": 80
        }
      }
    }
  ]'
```

### 4.2 Load Balancer（ロードバランサー）

#### ロードバランサーとは

ロードバランサーは、複数のサーバーに対してトラフィックを分散し、システムの可用性と性能を向上させるサービスです。単一障害点を排除し、スケーラビリティを実現します。

```mermaid
graph TB
    A[ユーザー] --> B[ロードバランサー]
    B --> C[Webサーバー1]
    B --> D[Webサーバー2]
    B --> E[Webサーバー3]
    
    F[ヘルスチェック] --> C
    F --> D
    F --> E
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#e8f5e8
    style E fill:#e8f5e8
```

#### ロードバランサーの必要性

**従来の単一サーバー構成の問題：**

```mermaid
graph TB
    A[問題点] --> B[単一障害点]
    A --> C[性能限界]
    A --> D[スケーラビリティ不足]
    A --> E[メンテナンス時の停止]
    
    B --> B1[サーバー障害で全停止]
    C --> C1[処理能力の上限]
    D --> D1[急激な負荷増に対応困難]
    E --> E1[計画停止が必要]
    
    style A fill:#ffebee
    style B fill:#ffcdd2
    style C fill:#ffcdd2
    style D fill:#ffcdd2
    style E fill:#ffcdd2
```

**ロードバランサーによる解決：**

```mermaid
graph TB
    A[解決効果] --> B[高可用性]
    A --> C[性能向上]
    A --> D[自動スケーリング]
    A --> E[無停止メンテナンス]
    
    B --> B1[サーバー障害時の自動切り替え]
    C --> C1[複数サーバーでの負荷分散]
    D --> D1[需要に応じたサーバー追加]
    E --> E1[ローリングメンテナンス]
    
    style A fill:#e8f5e8
    style B fill:#c8e6c8
    style C fill:#c8e6c8
    style D fill:#c8e6c8
    style E fill:#c8e6c8
```

#### OCIロードバランサーの種類

**1. Network Load Balancer（Layer 4）**

```mermaid
graph TB
    A[Network Load Balancer] --> B[特徴]
    A --> C[用途]
    A --> D[性能]
    
    B --> B1[TCP/UDPレベル]
    B --> B2[IPアドレス保持]
    B --> B3[超低レイテンシ]
    
    C --> C1[高性能アプリケーション]
    C --> C2[ゲームサーバー]
    C --> C3[IoTデータ収集]
    
    D --> D1[数百万接続]
    D --> D2[マイクロ秒レベル]
    D --> D3[線形スケーリング]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

**2. Application Load Balancer（Layer 7）**

```mermaid
graph TB
    A[Application Load Balancer] --> B[特徴]
    A --> C[用途]
    A --> D[機能]
    
    B --> B1[HTTP/HTTPSレベル]
    B --> B2[コンテンツベース]
    B --> B3[SSL終端]
    
    C --> C1[Webアプリケーション]
    C --> C2[マイクロサービス]
    C --> C3[API Gateway]
    
    D --> D1[パスベースルーティング]
    D --> D2[ホストベースルーティング]
    D --> D3[セッション維持]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### ロードバランシングアルゴリズム

```mermaid
graph TB
    A[ロードバランシングアルゴリズム] --> B[Round Robin<br/>順番]
    A --> C[Least Connections<br/>最少接続]
    A --> D[IP Hash<br/>IPハッシュ]
    A --> E[Weighted Round Robin<br/>重み付き順番]
    
    B --> B1[均等分散]
    B --> B2[シンプル]
    B --> B3[セッション考慮なし]
    
    C --> C1[負荷考慮]
    C --> C2[動的分散]
    C --> C3[接続数監視]
    
    D --> D1[セッション維持]
    D --> D2[同一クライアント]
    D --> D3[ハッシュベース]
    
    E --> E1[サーバー性能差考慮]
    E --> E2[重み設定]
    E --> E3[柔軟な分散]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
```

#### ヘルスチェック機能

ヘルスチェックは、バックエンドサーバーの健全性を監視し、障害サーバーを自動的に切り離す機能です。

```mermaid
graph LR
    A[ロードバランサー] --> B[ヘルスチェック]
    B --> C[サーバー1<br/>正常]
    B --> D[サーバー2<br/>異常]
    B --> E[サーバー3<br/>正常]
    
    F[トラフィック] --> C
    F --> E
    F -.->|除外| D
    
    style C fill:#e8f5e8
    style D fill:#ffebee
    style E fill:#e8f5e8
```

**ヘルスチェック設定パラメータ：**

| パラメータ | 説明 | 推奨値 |
|-----------|------|--------|
| **プロトコル** | HTTP/HTTPS/TCP | HTTP |
| **ポート** | チェック対象ポート | 80/443 |
| **パス** | チェック対象URL | /health |
| **間隔** | チェック間隔（秒） | 30 |
| **タイムアウト** | 応答待ち時間（秒） | 5 |
| **再試行回数** | 失敗判定までの回数 | 3 |
| **成功回数** | 復旧判定までの回数 | 2 |

#### SSL/TLS終端

ロードバランサーでSSL/TLS終端を行うことで、バックエンドサーバーの負荷を軽減できます。

```mermaid
graph LR
    A[クライアント] -->|HTTPS| B[ロードバランサー]
    B -->|HTTP| C[サーバー1]
    B -->|HTTP| D[サーバー2]
    B -->|HTTP| E[サーバー3]
    
    F[SSL証明書] --> B
    
    style B fill:#e3f2fd
    style F fill:#e8f5e8
```

**SSL設定の利点：**
1. **サーバー負荷軽減**: SSL処理をロードバランサーで実行
2. **証明書管理の簡素化**: 1箇所での証明書管理
3. **セキュリティ強化**: 最新のTLSプロトコル対応

#### 実装例

**1. Application Load Balancer作成**

```bash
# ロードバランサー作成
oci lb load-balancer create \
  --compartment-id <compartment-id> \
  --display-name "web-app-lb" \
  --shape-name "100Mbps" \
  --subnet-ids '["<subnet-id-1>", "<subnet-id-2>"]' \
  --is-private false

# バックエンドセット作成
oci lb backend-set create \
  --load-balancer-id <lb-id> \
  --name "web-backend-set" \
  --policy "ROUND_ROBIN" \
  --health-checker-protocol "HTTP" \
  --health-checker-port 80 \
  --health-checker-url-path "/health"

# バックエンド追加
oci lb backend create \
  --load-balancer-id <lb-id> \
  --backend-set-name "web-backend-set" \
  --ip-address "10.0.1.10" \
  --port 80 \
  --weight 1
```

**2. リスナー設定**

```bash
# HTTPリスナー作成
oci lb listener create \
  --load-balancer-id <lb-id> \
  --name "http-listener" \
  --default-backend-set-name "web-backend-set" \
  --port 80 \
  --protocol "HTTP"

# HTTPSリスナー作成（SSL証明書必要）
oci lb listener create \
  --load-balancer-id <lb-id> \
  --name "https-listener" \
  --default-backend-set-name "web-backend-set" \
  --port 443 \
  --protocol "HTTP" \
  --ssl-certificate-name "my-ssl-cert"
```

### 4.3 DNS Management（DNS管理）

#### DNSとは

DNS（Domain Name System）は、人間が理解しやすいドメイン名（例：www.example.com）をコンピューターが理解できるIPアドレス（例：192.168.1.100）に変換するシステムです。

```mermaid
graph LR
    A[ユーザー] -->|www.example.com| B[DNSリゾルバー]
    B -->|DNS問い合わせ| C[DNSサーバー]
    C -->|192.168.1.100| B
    B -->|IPアドレス| A
    A -->|HTTP接続| D[Webサーバー<br/>192.168.1.100]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### DNS階層構造

DNSは階層構造になっており、効率的な名前解決を実現しています：

```mermaid
graph TB
    A["ルートドメイン ."] --> B["トップレベルドメイン .com"]
    A --> C["トップレベルドメイン .org"]
    A --> D["トップレベルドメイン .jp"]
    
    B --> E["セカンドレベルドメイン example.com"]
    D --> F["セカンドレベルドメイン example.co.jp"]
    
    E --> G["サブドメイン www.example.com"]
    E --> H["サブドメイン mail.example.com"]
    E --> I["サブドメイン api.example.com"]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style E fill:#fff3e0
    style G fill:#fce4ec
```

#### OCIのDNS管理機能

**1. パブリックDNS**

```mermaid
graph TB
    A[OCI DNS Management] --> B[ゾーン管理]
    A --> C[レコード管理]
    A --> D[トラフィック管理]
    
    B --> B1[プライマリゾーン]
    B --> B2[セカンダリゾーン]
    
    C --> C1[Aレコード]
    C --> C2[CNAMEレコード]
    C --> C3[MXレコード]
    C --> C4[TXTレコード]
    
    D --> D1[地理的ルーティング]
    D --> D2[重み付きルーティング]
    D --> D3[ヘルスチェック]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

**2. プライベートDNS**

```mermaid
graph TB
    A[プライベートDNS] --> B[VCN内名前解決]
    A --> C[カスタムドメイン]
    A --> D[内部サービス発見]
    
    B --> B1[インスタンス名]
    B --> B2[サービス名]
    
    C --> C1[internal.company.com]
    C --> C2[dev.internal.com]
    
    D --> D1[マイクロサービス]
    D --> D2[データベース接続]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### DNSレコードタイプ

| レコードタイプ | 用途 | 例 |
|---------------|------|-----|
| **A** | IPv4アドレス | www.example.com → 192.168.1.100 |
| **AAAA** | IPv6アドレス | www.example.com → 2001:db8::1 |
| **CNAME** | 別名 | blog.example.com → www.example.com |
| **MX** | メールサーバー | example.com → mail.example.com |
| **TXT** | テキスト情報 | SPF、DKIM設定 |
| **SRV** | サービス情報 | _http._tcp.example.com |

#### トラフィック管理

OCIのDNSトラフィック管理機能により、高度なルーティング制御が可能です：

**1. 地理的ルーティング**

```mermaid
graph TB
    A[グローバルユーザー] --> B[DNS問い合わせ]
    B --> C{地理的判定}
    
    C -->|日本| D[東京リージョン<br/>asia.example.com]
    C -->|アメリカ| E[アッシュバーンリージョン<br/>us.example.com]
    C -->|ヨーロッパ| F[フランクフルトリージョン<br/>eu.example.com]
    
    style C fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#fff3e0
    style F fill:#fce4ec
```

**2. 重み付きルーティング**

```mermaid
graph LR
    A[DNS問い合わせ] --> B{重み付き分散}
    
    B -->|70%| C[メインサーバー]
    B -->|20%| D[サブサーバー1]
    B -->|10%| E[サブサーバー2]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
```

#### DNS設定例

**1. ゾーン作成**

```bash
# DNSゾーン作成
oci dns zone create \
  --compartment-id <compartment-id> \
  --name "example.com" \
  --zone-type "PRIMARY"

# Aレコード追加
oci dns record update \
  --zone-name-or-id "example.com" \
  --domain "www.example.com" \
  --rtype "A" \
  --rdata "192.168.1.100" \
  --ttl 300
```

**2. ロードバランサーとの統合**

```bash
# ロードバランサーのIPアドレスをDNSに登録
oci dns record update \
  --zone-name-or-id "example.com" \
  --domain "app.example.com" \
  --rtype "A" \
  --rdata "<load-balancer-ip>" \
  --ttl 60
```

### 4.4 FastConnect（専用線接続）

#### FastConnectとは

FastConnectは、オンプレミス環境とOCI間を専用線で接続するサービスです。インターネットを経由しない安全で高速な接続を提供します。

```mermaid
graph LR
    A[オンプレミス] --> B[専用線]
    B --> C[FastConnect]
    C --> D[OCI VCN]
    
    E[インターネット] -.->|迂回| A
    E -.->|迂回| D
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### FastConnectの利点

```mermaid
graph TB
    A[FastConnect利点] --> B[高速性]
    A --> C[安定性]
    A --> D[セキュリティ]
    A --> E[コスト効率]
    
    B --> B1[最大10Gbps]
    B --> B2[低レイテンシ]
    B --> B3[帯域保証]
    
    C --> C1[専用回線]
    C --> C2[SLA保証]
    C --> C3[冗長化対応]
    
    D --> D1[プライベート接続]
    D --> D2[暗号化不要]
    D --> D3[ネットワーク分離]
    
    E --> E1[データ転送料金削減]
    E --> E2[予測可能なコスト]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fce4ec
```

#### 接続オプション

**1. 専用ポート（Dedicated Port）**

```mermaid
graph LR
    A[顧客データセンター] --> B[通信事業者]
    B --> C[OCIデータセンター]
    C --> D[専用ポート]
    D --> E[VCN]
    
    style D fill:#e3f2fd
    style E fill:#e8f5e8
```

**2. 共有ポート（Shared Port）**

```mermaid
graph TB
    A[顧客A] --> B[パートナー]
    C[顧客B] --> B
    D[顧客C] --> B
    
    B --> E[共有ポート]
    E --> F[OCI]
    
    style B fill:#e3f2fd
    style E fill:#e8f5e8
    style F fill:#fff3e0
```

#### 冗長化設計

```mermaid
graph TB
    subgraph "オンプレミス"
        A1[ルーター1]
        A2[ルーター2]
    end
    
    subgraph "FastConnect"
        B1[接続1]
        B2[接続2]
    end
    
    subgraph "OCI"
        C1[DRG1]
        C2[DRG2]
        D[VCN]
    end
    
    A1 --> B1
    A2 --> B2
    B1 --> C1
    B2 --> C2
    C1 --> D
    C2 --> D
    
    style B1 fill:#e3f2fd
    style B2 fill:#e8f5e8
    style D fill:#fff3e0
```

### 4.5 VPN Connect（VPN接続）

#### VPN Connectとは

VPN Connectは、インターネット経由でオンプレミス環境とOCIを安全に接続するサービスです。IPsecプロトコルを使用して暗号化された通信を提供します。

```mermaid
graph LR
    A[オンプレミス] -->|暗号化トンネル| B[インターネット]
    B -->|IPsec VPN| C[OCI VPN Gateway]
    C --> D[VCN]
    
    style A fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### VPNの種類

**1. Site-to-Site VPN**

```mermaid
graph LR
    A[本社ネットワーク] --> B[VPNゲートウェイ]
    B --> C[インターネット]
    C --> D[OCI VPN Gateway]
    D --> E[VCN]
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
```

**2. Client VPN（リモートアクセス）**

```mermaid
graph TB
    A[リモートワーカー] --> B[VPNクライアント]
    B --> C[インターネット]
    C --> D[OCI VPN Gateway]
    D --> E[VCN]
    
    style B fill:#e3f2fd
    style D fill:#e8f5e8
```

#### VPN設定パラメータ

| パラメータ | 説明 | 推奨値 |
|-----------|------|--------|
| **暗号化アルゴリズム** | データ暗号化方式 | AES-256 |
| **認証アルゴリズム** | データ整合性確認 | SHA-256 |
| **DH Group** | 鍵交換方式 | Group 14 |
| **PFS** | Perfect Forward Secrecy | 有効 |
| **Dead Peer Detection** | 接続監視 | 有効 |

#### VPN設定例

```bash
# VPN接続作成
oci network ip-sec-connection create \
  --compartment-id <compartment-id> \
  --cpe-id <cpe-id> \
  --drg-id <drg-id> \
  --display-name "office-vpn" \
  --static-routes '["192.168.0.0/16"]'

# トンネル設定確認
oci network ip-sec-connection-tunnel get \
  --ipsc-id <ipsec-connection-id> \
  --tunnel-id <tunnel-id>
```

### 4.6 ネットワーク設計パターン

#### 基本的な3層アーキテクチャ

```mermaid
graph TB
    subgraph "パブリックサブネット"
        A[ロードバランサー]
        B[Webサーバー]
    end
    
    subgraph "プライベートサブネット"
        C[アプリケーションサーバー]
    end
    
    subgraph "データベースサブネット"
        D[データベースサーバー]
    end
    
    E[インターネット] --> A
    A --> B
    B --> C
    C --> D
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### ハイブリッドクラウド構成

```mermaid
graph TB
    subgraph "オンプレミス"
        A[既存システム]
        B[データベース]
    end
    
    subgraph "OCI"
        C[新規アプリケーション]
        D[バックアップシステム]
    end
    
    A -.->|FastConnect/VPN| C
    B -.->|データ同期| D
    
    style A fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

#### マルチリージョン構成

```mermaid
graph TB
    subgraph "東京リージョン"
        A1[プライマリシステム]
        A2[プライマリDB]
    end
    
    subgraph "大阪リージョン"
        B1[セカンダリシステム]
        B2[セカンダリDB]
    end
    
    C[グローバルロードバランサー] --> A1
    C --> B1
    A2 -.->|レプリケーション| B2
    
    style C fill:#e3f2fd
    style A1 fill:#e8f5e8
    style B1 fill:#fff3e0
```

### まとめ

第4章では、OCIのネットワーキングサービスについて詳しく解説しました。適切なネットワーク設計は、システムの性能、セキュリティ、可用性を決定する重要な要素です。

**重要ポイント：**
1. **VCN**: 仮想ネットワークの基盤設計
2. **Load Balancer**: 高可用性と性能向上
3. **DNS**: 名前解決とトラフィック制御
4. **FastConnect**: 高速・安全な専用線接続
5. **VPN**: インターネット経由の暗号化接続
6. **設計パターン**: 要件に応じた適切なアーキテクチャ選択

次章では、これらのネットワーク上で動作するデータベースサービスについて学習します。