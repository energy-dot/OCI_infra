# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 第6章 セキュリティサービス

### はじめに

第5章でデータベースサービスについて学習しました。本章では、OCIの包括的なセキュリティサービスについて詳しく解説します。クラウド環境におけるセキュリティは、従来のオンプレミス環境とは異なる考え方とアプローチが必要です。OCIでは、多層防御の概念に基づいた包括的なセキュリティサービスを提供し、企業の重要な資産を保護します。

### セキュリティの基本概念

#### クラウドセキュリティの重要性

現代のサイバー脅威は日々進化し、企業にとって深刻なリスクとなっています。適切なセキュリティ対策なしには、以下のような重大な被害が発生する可能性があります：

```mermaid
graph TB
    A[セキュリティ脅威] --> B[データ漏洩]
    A --> C[システム停止]
    A --> D[金銭的損失]
    A --> E[信頼失墜]
    A --> F[法的責任]
    
    B --> B1[個人情報流出]
    B --> B2[企業機密漏洩]
    B --> B3[知的財産盗用]
    
    C --> C1[ランサムウェア]
    C --> C2[DDoS攻撃]
    C --> C3[システム破壊]
    
    D --> D1[復旧費用]
    D --> D2[賠償金]
    D --> D3[売上機会損失]
    
    E --> E1[顧客離れ]
    E --> E2[ブランド毀損]
    E --> E3[株価下落]
    
    F --> F1[GDPR違反]
    F --> F2[個人情報保護法違反]
    F --> F3[業界規制違反]
    
    style A fill:#ffebee
    style B fill:#ffcdd2
    style C fill:#ffcdd2
    style D fill:#ffcdd2
    style E fill:#ffcdd2
    style F fill:#ffcdd2
```

#### 責任共有モデル

クラウドセキュリティでは、クラウドプロバイダーと顧客の間で責任を分担する「責任共有モデル」が基本となります：

```mermaid
graph TB
    subgraph "顧客責任範囲"
        A1[データ暗号化]
        A2[アクセス管理]
        A3[ネットワーク設定]
        A4[OS・アプリケーション]
        A5[データ分類]
    end
    
    subgraph "共有責任範囲"
        B1[パッチ管理]
        B2[設定管理]
        B3[監査ログ]
    end
    
    subgraph "OCI責任範囲"
        C1[物理セキュリティ]
        C2[インフラストラクチャ]
        C3[ネットワーク制御]
        C4[ホストOS]
        C5[仮想化レイヤー]
    end
    
    style A1 fill:#e3f2fd
    style B1 fill:#fff3e0
    style C1 fill:#e8f5e8
```

#### セキュリティフレームワーク

OCIのセキュリティは、業界標準のフレームワークに基づいて設計されています：

```mermaid
graph TB
    A[セキュリティフレームワーク] --> B[NIST Cybersecurity Framework]
    A --> C[ISO 27001/27002]
    A --> D[SOC 2 Type II]
    A --> E[PCI DSS]
    A --> F[GDPR]
    
    B --> B1[識別（Identify）]
    B --> B2[保護（Protect）]
    B --> B3[検知（Detect）]
    B --> B4[対応（Respond）]
    B --> B5[復旧（Recover）]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
    style F fill:#e1f5fe
```

### 6.1 Web Application Firewall (WAF)

#### WAFとは

Web Application Firewall（WAF）は、Webアプリケーションを標的とした攻撃からシステムを保護するセキュリティサービスです。従来のネットワークファイアウォールがネットワーク層での保護を行うのに対し、WAFはアプリケーション層（Layer 7）での保護を提供します。

```mermaid
graph TB
    A[インターネット] --> B[WAF]
    B --> C[ロードバランサー]
    C --> D[Webサーバー]
    
    E[攻撃トラフィック] --> B
    B -->|ブロック| F[攻撃遮断]
    
    G[正常トラフィック] --> B
    B -->|許可| C
    
    style B fill:#e3f2fd
    style F fill:#ffebee
    style C fill:#e8f5e8
```

#### WAFが防御する攻撃

**1. OWASP Top 10攻撃**

```mermaid
graph TB
    A[OWASP Top 10] --> B[SQLインジェクション]
    A --> C[XSS<br/>クロスサイトスクリプティング]
    A --> D[CSRF<br/>クロスサイトリクエストフォージェリ]
    A --> E[ディレクトリトラバーサル]
    A --> F[コマンドインジェクション]
    
    B --> B1[データベース不正操作]
    C --> C1[悪意あるスクリプト実行]
    D --> D1[不正な操作実行]
    E --> E1[システムファイルアクセス]
    F --> F1[システムコマンド実行]
    
    style A fill:#e3f2fd
    style B fill:#ffebee
    style C fill:#ffebee
    style D fill:#ffebee
    style E fill:#ffebee
    style F fill:#ffebee
```

**2. DDoS攻撃**

```mermaid
graph LR
    A[大量リクエスト] --> B[WAF]
    B --> C[レート制限]
    C --> D[正常トラフィック]
    
    E[ボットネット] --> B
    B --> F[攻撃検知・遮断]
    
    style B fill:#e3f2fd
    style F fill:#ffebee
    style D fill:#e8f5e8
```

#### OCI WAFの特徴

**1. 多層防御アーキテクチャ**

```mermaid
graph TB
    A[OCI WAF] --> B[エッジ保護]
    A --> C[アプリケーション保護]
    A --> D[ボット管理]
    A --> E[レート制限]
    
    B --> B1[グローバルエッジ配信]
    B --> B2[DDoS軽減]
    B --> B3[地理的ブロック]
    
    C --> C1[OWASP保護]
    C --> C2[カスタムルール]
    C --> C3[シグネチャベース検知]
    
    D --> D1[ボット検知]
    D --> D2[CAPTCHA統合]
    D --> D3[行動分析]
    
    E --> E1[リクエスト制限]
    E --> E2[帯域制限]
    E --> E3[同時接続制限]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

**2. 機械学習による脅威検知**

```mermaid
graph LR
    A[トラフィック分析] --> B[機械学習エンジン]
    B --> C[異常検知]
    C --> D[自動ブロック]
    
    E[学習データ] --> B
    F[脅威インテリジェンス] --> B
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#ffebee
```

#### WAF保護ルール

**1. 事前定義ルール**

| カテゴリ | 保護対象 | 例 |
|---------|----------|-----|
| **SQLインジェクション** | データベース攻撃 | `' OR 1=1 --` |
| **XSS** | スクリプト攻撃 | `<script>alert('xss')</script>` |
| **ディレクトリトラバーサル** | ファイルアクセス攻撃 | `../../../etc/passwd` |
| **コマンドインジェクション** | システム攻撃 | `; rm -rf /` |
| **プロトコル攻撃** | HTTP異常 | 不正なHTTPヘッダー |

**2. カスタムルール**

```bash
# カスタムルール例：特定IPからのアクセス制限
{
  "name": "block-suspicious-ip",
  "conditions": [
    {
      "field": "source_ip",
      "operator": "equals",
      "value": "192.168.1.100"
    }
  ],
  "action": "BLOCK"
}

# カスタムルール例：特定パターンのブロック
{
  "name": "block-admin-access",
  "conditions": [
    {
      "field": "request_uri",
      "operator": "contains",
      "value": "/admin"
    },
    {
      "field": "source_country",
      "operator": "not_equals",
      "value": "JP"
    }
  ],
  "action": "BLOCK"
}
```

#### ボット管理

**1. ボット分類**

```mermaid
graph TB
    A[ボット分類] --> B[良性ボット]
    A --> C[悪性ボット]
    A --> D[不明ボット]
    
    B --> B1[検索エンジン]
    B --> B2[監視ツール]
    B --> B3[API クライアント]
    
    C --> C1[スクレイピング]
    C --> C2[DDoS攻撃]
    C --> C3[不正ログイン]
    
    D --> D1[行動分析]
    D --> D2[CAPTCHA チャレンジ]
    D --> D3[レート制限]
    
    style B fill:#e8f5e8
    style C fill:#ffebee
    style D fill:#fff3e0
```

**2. ボット検知技術**

```mermaid
graph TB
    A[ボット検知技術] --> B[シグネチャベース]
    A --> C[行動分析]
    A --> D[デバイスフィンガープリンティング]
    A --> E[機械学習]
    
    B --> B1[既知ボットパターン]
    C --> C1[アクセスパターン分析]
    D --> D1[ブラウザ特性分析]
    E --> E1[異常行動検知]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### 実装例

**1. WAFポリシー作成**

```bash
# WAFポリシー作成
oci waas waas-policy create \
  --compartment-id <compartment-id> \
  --display-name "production-waf" \
  --domain "example.com" \
  --origins '[{
    "label": "primary",
    "uri": "https://backend.example.com"
  }]'

# 保護ルール設定
oci waas protection-rule update \
  --waas-policy-id <policy-id> \
  --key "981176" \
  --action "BLOCK" \
  --exclusions '[]'
```

**2. カスタムルール設定**

```json
{
  "customProtectionRules": [
    {
      "displayName": "Block Admin Access from Outside Japan",
      "description": "Block access to admin pages from non-Japanese IPs",
      "template": "SecRule REQUEST_URI \"@contains /admin\" \"id:1001,phase:1,block,msg:'Admin access blocked',logdata:'Blocked admin access from %{REMOTE_ADDR}'\""
    }
  ]
}
```

### 6.2 Vault（キー管理）

#### Vaultとは

Vaultは、暗号化キー、シークレット、証明書などの機密情報を安全に管理するサービスです。ハードウェアセキュリティモジュール（HSM）を使用して、最高レベルのセキュリティを提供します。

```mermaid
graph TB
    A[OCI Vault] --> B[キー管理]
    A --> C[シークレット管理]
    A --> D[証明書管理]
    
    B --> B1[暗号化キー生成]
    B --> B2[キーローテーション]
    B --> B3[キーバージョン管理]
    
    C --> C1[パスワード]
    C --> C2[APIキー]
    C --> C3[接続文字列]
    
    D --> D1[SSL/TLS証明書]
    D --> D2[コード署名証明書]
    D --> D3[証明書更新]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### 暗号化の基本概念

**1. 対称暗号化と非対称暗号化**

```mermaid
graph TB
    subgraph "対称暗号化"
        A1[平文] --> A2[同一キーで暗号化]
        A2 --> A3[暗号文]
        A3 --> A4[同一キーで復号化]
        A4 --> A5[平文]
    end
    
    subgraph "非対称暗号化"
        B1[平文] --> B2[公開キーで暗号化]
        B2 --> B3[暗号文]
        B3 --> B4[秘密キーで復号化]
        B4 --> B5[平文]
    end
    
    style A2 fill:#e3f2fd
    style B2 fill:#e8f5e8
    style B4 fill:#fff3e0
```

**2. 暗号化アルゴリズム**

| アルゴリズム | タイプ | キー長 | 用途 |
|-------------|--------|--------|------|
| **AES** | 対称 | 128/192/256 bit | データ暗号化 |
| **RSA** | 非対称 | 2048/3072/4096 bit | キー交換、デジタル署名 |
| **ECDSA** | 非対称 | 256/384/521 bit | デジタル署名 |
| **SHA-256** | ハッシュ | 256 bit | データ整合性 |

#### HSM（Hardware Security Module）

**1. HSMの利点**

```mermaid
graph TB
    A[HSM利点] --> B[ハードウェアベース]
    A --> C[改ざん検知]
    A --> D[高性能]
    A --> E[コンプライアンス]
    
    B --> B1[物理的セキュリティ]
    B --> B2[キー抽出不可]
    
    C --> C1[物理的攻撃検知]
    C --> C2[自動キー削除]
    
    D --> D1[専用ハードウェア]
    D --> D2[並列処理]
    
    E --> E1[FIPS 140-2 Level 3]
    E --> E2[Common Criteria]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

**2. HSMアーキテクチャ**

```mermaid
graph TB
    A[アプリケーション] --> B[Vault API]
    B --> C[Vault Service]
    C --> D[HSM Cluster]
    
    D --> D1[HSM Node 1]
    D --> D2[HSM Node 2]
    D --> D3[HSM Node 3]
    
    E[キー複製] --> D1
    E --> D2
    E --> D3
    
    style C fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#fff3e0
```

#### キー管理ライフサイクル

**1. キーライフサイクル**

```mermaid
graph LR
    A[キー生成] --> B[キー配布]
    B --> C[キー使用]
    C --> D[キーローテーション]
    D --> E[キー廃棄]
    
    F[バックアップ] --> C
    G[監査] --> C
    
    style A fill:#e3f2fd
    style C fill:#e8f5e8
    style E fill:#ffebee
```

**2. キーローテーション**

```mermaid
graph TB
    A[自動ローテーション] --> B[新キー生成]
    B --> C[新キーでの暗号化開始]
    C --> D[旧キーでの復号化継続]
    D --> E[旧データの再暗号化]
    E --> F[旧キーの無効化]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style F fill:#fff3e0
```

#### 実装例

**1. Vault作成**

```bash
# Vault作成
oci kms vault create \
  --compartment-id <compartment-id> \
  --display-name "production-vault" \
  --vault-type "DEFAULT"

# マスターキー作成
oci kms key create \
  --compartment-id <compartment-id> \
  --display-name "database-encryption-key" \
  --key-shape '{
    "algorithm": "AES",
    "length": 256
  }' \
  --management-endpoint <vault-endpoint>
```

**2. データ暗号化**

```python
import oci
import base64

# Vault クライアント初期化
kms_vault_client = oci.key_management.KmsVaultClient(config)
kms_crypto_client = oci.key_management.KmsCryptoClient(
    config, 
    service_endpoint=vault_endpoint
)

# データ暗号化
plaintext = "機密データ"
plaintext_bytes = plaintext.encode('utf-8')
plaintext_b64 = base64.b64encode(plaintext_bytes).decode('utf-8')

encrypt_response = kms_crypto_client.encrypt(
    encrypt_data_details=oci.key_management.models.EncryptDataDetails(
        key_id=key_id,
        plaintext=plaintext_b64
    )
)

ciphertext = encrypt_response.data.ciphertext

# データ復号化
decrypt_response = kms_crypto_client.decrypt(
    decrypt_data_details=oci.key_management.models.DecryptDataDetails(
        key_id=key_id,
        ciphertext=ciphertext
    )
)

decrypted_b64 = decrypt_response.data.plaintext
decrypted_bytes = base64.b64decode(decrypted_b64)
decrypted_text = decrypted_bytes.decode('utf-8')
```

### 6.3 Security Zones（セキュリティゾーン）

#### Security Zonesとは

Security Zonesは、セキュリティベストプラクティスを自動的に適用し、設定ミスを防ぐサービスです。事前定義されたセキュリティポリシーに基づいて、リソースの作成・変更を制御します。

```mermaid
graph TB
    A[Security Zones] --> B[自動ポリシー適用]
    A --> C[設定ミス防止]
    A --> D[コンプライアンス確保]
    A --> E[継続的監視]
    
    B --> B1[暗号化強制]
    B --> B2[パブリックアクセス制限]
    B --> B3[ログ有効化強制]
    
    C --> C1[不適切設定ブロック]
    C --> C2[リアルタイム検証]
    
    D --> D1[業界標準準拠]
    D --> D2[規制要件対応]
    
    E --> E1[設定変更監視]
    E --> E2[違反検知]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### セキュリティポリシー

**1. 暗号化ポリシー**

```mermaid
graph TB
    A[暗号化ポリシー] --> B[保存時暗号化]
    A --> C[転送時暗号化]
    A --> D[キー管理]
    
    B --> B1[Block Storage暗号化]
    B --> B2[Object Storage暗号化]
    B --> B3[Database暗号化]
    
    C --> C1[HTTPS強制]
    C --> C2[TLS 1.2以上]
    
    D --> D1[顧客管理キー]
    D --> D2[キーローテーション]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

**2. ネットワークセキュリティポリシー**

```mermaid
graph TB
    A[ネットワークポリシー] --> B[パブリックアクセス制限]
    A --> C[セキュリティリスト強制]
    A --> D[VPN/FastConnect推奨]
    
    B --> B1[Database非公開]
    B --> B2[管理ポート制限]
    
    C --> C1[最小権限原則]
    C --> C2[デフォルト拒否]
    
    D --> D1[専用接続]
    D --> D2[暗号化通信]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

#### 実装例

**1. Security Zone作成**

```bash
# Security Zone作成
oci cloud-guard security-zone create \
  --compartment-id <compartment-id> \
  --display-name "production-security-zone" \
  --security-zone-recipe-id <recipe-id>

# セキュリティポリシー確認
oci cloud-guard security-zone-recipe get \
  --security-zone-recipe-id <recipe-id>
```

### 6.4 Cloud Guard（クラウド監視）

#### Cloud Guardとは

Cloud Guardは、OCIリソースの設定とアクティビティを継続的に監視し、セキュリティ脅威を検知・対応するサービスです。機械学習を活用して異常を検知し、自動的な修復アクションを実行できます。

```mermaid
graph TB
    A[Cloud Guard] --> B[脅威検知]
    A --> C[設定監査]
    A --> D[自動修復]
    A --> E[レポート生成]
    
    B --> B1[異常アクティビティ]
    B --> B2[不正アクセス]
    B --> B3[データ漏洩兆候]
    
    C --> C1[セキュリティ設定]
    C --> C2[コンプライアンス]
    C --> C3[ベストプラクティス]
    
    D --> D1[自動ブロック]
    D --> D2[設定修正]
    D --> D3[アラート送信]
    
    E --> E1[セキュリティダッシュボード]
    E --> E2[コンプライアンスレポート]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### 検知ルール

**1. アクティビティ検知**

| 検知項目 | 説明 | 重要度 |
|---------|------|--------|
| **異常ログイン** | 通常と異なる場所・時間からのアクセス | 高 |
| **権限昇格** | 管理者権限の不正取得 | 最高 |
| **大量データアクセス** | 通常を超えるデータアクセス | 中 |
| **設定変更** | 重要なセキュリティ設定の変更 | 高 |
| **リソース削除** | 重要リソースの削除 | 最高 |

**2. 設定監査**

```mermaid
graph TB
    A[設定監査] --> B[IAM設定]
    A --> C[ネットワーク設定]
    A --> D[ストレージ設定]
    A --> E[データベース設定]
    
    B --> B1[過度な権限]
    B --> B2[未使用ユーザー]
    B --> B3[MFA未設定]
    
    C --> C1[パブリックアクセス]
    C --> C2[セキュリティリスト]
    C --> C3[暗号化設定]
    
    D --> D1[パブリックバケット]
    D --> D2[暗号化無効]
    
    E --> E1[パブリックアクセス]
    E --> E2[暗号化無効]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

### 6.5 セキュリティベースライン

#### セキュリティベースラインとは

セキュリティベースラインは、組織が満たすべき最低限のセキュリティ要件を定義したものです。OCIでは、業界標準に基づいたセキュリティベースラインを提供し、継続的なセキュリティ向上を支援します。

```mermaid
graph TB
    A[セキュリティベースライン] --> B[アクセス制御]
    A --> C[データ保護]
    A --> D[ネットワークセキュリティ]
    A --> E[監視・ログ]
    A --> F[インシデント対応]
    
    B --> B1[最小権限原則]
    B --> B2[多要素認証]
    B --> B3[定期的権限見直し]
    
    C --> C1[保存時暗号化]
    C --> C2[転送時暗号化]
    C --> C3[データ分類]
    
    D --> D1[ネットワーク分離]
    D --> D2[ファイアウォール]
    D --> D3[侵入検知]
    
    E --> E1[包括的ログ]
    E --> E2[リアルタイム監視]
    E --> E3[異常検知]
    
    F --> F1[インシデント計画]
    F --> F2[対応手順]
    F --> F3[復旧計画]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
    style F fill:#e1f5fe
```

#### 実装チェックリスト

**1. IAMセキュリティ**

- [ ] 管理者アカウントでのMFA有効化
- [ ] 最小権限の原則適用
- [ ] 定期的なアクセス権限見直し
- [ ] サービスアカウントの適切な管理
- [ ] パスワードポリシーの設定

**2. ネットワークセキュリティ**

- [ ] VCNでのネットワーク分離
- [ ] セキュリティリストの適切な設定
- [ ] パブリックサブネットの最小化
- [ ] WAFの導入
- [ ] DDoS保護の有効化

**3. データ保護**

- [ ] 保存時暗号化の有効化
- [ ] 転送時暗号化の強制
- [ ] バックアップの暗号化
- [ ] データ分類の実施
- [ ] アクセスログの記録

**4. 監視・ログ**

- [ ] Cloud Guardの有効化
- [ ] 包括的ログ収集
- [ ] リアルタイム監視
- [ ] アラート設定
- [ ] インシデント対応計画

### まとめ

第6章では、OCIの包括的なセキュリティサービスについて詳しく解説しました。多層防御の概念に基づいた包括的なセキュリティ対策が重要です。

**重要ポイント：**
1. **WAF**: Webアプリケーションレベルでの攻撃防御
2. **Vault**: 暗号化キーとシークレットの安全な管理
3. **Security Zones**: 自動的なセキュリティポリシー適用
4. **Cloud Guard**: 継続的な監視と脅威検知
5. **セキュリティベースライン**: 組織全体でのセキュリティ標準化
6. **責任共有モデル**: クラウドプロバイダーと顧客の責任分担理解

次章では、これらのセキュリティ対策を含むシステム全体の監視・運用サービスについて学習します。