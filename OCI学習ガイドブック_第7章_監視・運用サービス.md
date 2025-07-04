# Oracle Cloud Infrastructure (OCI) 学習ガイドブック

## 第7章 監視・運用サービス

### はじめに

第6章でセキュリティサービスについて学習しました。本章では、OCIシステムの安定運用に不可欠な監視・運用サービスについて詳しく解説します。現代のクラウドシステムでは、リアルタイムでの監視、迅速な問題検知、自動化された対応が求められます。OCIでは、包括的な監視・運用ツールを提供し、システムの可視性向上と運用効率化を実現します。

### 監視・運用の基本概念

#### なぜ監視・運用が重要なのか

現代のITシステムは複雑化・大規模化しており、適切な監視・運用なしには以下のような問題が発生します：

```mermaid
graph TB
    A[監視・運用不備] --> B[システム障害]
    A --> C[性能劣化]
    A --> D[セキュリティ侵害]
    A --> E[コスト増大]
    A --> F[コンプライアンス違反]
    
    B --> B1[サービス停止]
    B --> B2[データ損失]
    B --> B3[復旧遅延]
    
    C --> C1[応答時間悪化]
    C --> C2[スループット低下]
    C --> C3[ユーザー体験悪化]
    
    D --> D1[不正アクセス見逃し]
    D --> D2[データ漏洩]
    D --> D3[攻撃の拡大]
    
    E --> E1[リソース無駄遣い]
    E --> E2[緊急対応コスト]
    E --> E3[機会損失]
    
    F --> F1[監査ログ不備]
    F --> F2[規制違反]
    F --> F3[法的責任]
    
    style A fill:#ffebee
    style B fill:#ffcdd2
    style C fill:#ffcdd2
    style D fill:#ffcdd2
    style E fill:#ffcdd2
    style F fill:#ffcdd2
```

#### 監視・運用のフレームワーク

**1. SRE（Site Reliability Engineering）の概念**

```mermaid
graph TB
    A[SRE原則] --> B[SLI<br/>Service Level Indicator]
    A --> C[SLO<br/>Service Level Objective]
    A --> D[SLA<br/>Service Level Agreement]
    A --> E[Error Budget]
    
    B --> B1[応答時間]
    B --> B2[可用性]
    B --> B3[エラー率]
    B --> B4[スループット]
    
    C --> C1[目標値設定]
    C --> C2[99.9%可用性]
    C --> C3[100ms以下応答]
    
    D --> D1[顧客との合意]
    D --> D2[ペナルティ条項]
    
    E --> E1[許容エラー範囲]
    E --> E2[新機能開発バランス]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

**2. 可観測性（Observability）の3つの柱**

```mermaid
graph TB
    A[可観測性] --> B[メトリクス<br/>Metrics]
    A --> C[ログ<br/>Logs]
    A --> D[トレース<br/>Traces]
    
    B --> B1[数値データ]
    B --> B2[時系列データ]
    B --> B3[集約・統計]
    
    C --> C1[イベント記録]
    C --> C2[構造化データ]
    C --> C3[検索・分析]
    
    D --> D1[リクエスト追跡]
    D --> D2[分散システム]
    D --> D3[パフォーマンス分析]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

### 7.1 Monitoring Service（監視サービス）

#### Monitoring Serviceとは

OCI Monitoring Serviceは、OCIリソースのメトリクスを収集・可視化・アラート通知するサービスです。リアルタイムでシステムの状態を監視し、問題の早期発見と迅速な対応を可能にします。

```mermaid
graph TB
    A[Monitoring Service] --> B[メトリクス収集]
    A --> C[可視化]
    A --> D[アラート]
    A --> E[自動化]
    
    B --> B1[システムメトリクス]
    B --> B2[アプリケーションメトリクス]
    B --> B3[カスタムメトリクス]
    
    C --> C1[ダッシュボード]
    C --> C2[グラフ・チャート]
    C --> C3[リアルタイム表示]
    
    D --> D1[閾値監視]
    D --> D2[通知設定]
    D --> D3[エスカレーション]
    
    E --> E1[自動スケーリング]
    E --> E2[自動復旧]
    E --> E3[自動通知]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### 主要メトリクス

**1. コンピュートメトリクス**

| メトリクス | 説明 | 単位 | 推奨閾値 |
|-----------|------|------|----------|
| **CPU使用率** | プロセッサ使用率 | % | >80% |
| **メモリ使用率** | メモリ使用率 | % | >85% |
| **ディスクI/O** | ディスク読み書き | IOPS | 設計値の80% |
| **ネットワークI/O** | ネットワーク送受信 | Mbps | 設計値の80% |
| **ロードアベレージ** | システム負荷 | 数値 | CPU数×1.5 |

**2. データベースメトリクス**

```mermaid
graph TB
    A[データベースメトリクス] --> B[性能メトリクス]
    A --> C[リソースメトリクス]
    A --> D[接続メトリクス]
    A --> E[エラーメトリクス]
    
    B --> B1[応答時間]
    B --> B2[スループット]
    B --> B3[待機時間]
    
    C --> C1[CPU使用率]
    C --> C2[メモリ使用率]
    C --> C3[ストレージ使用率]
    
    D --> D1[アクティブ接続数]
    D --> D2[接続プール使用率]
    
    E --> E1[エラー率]
    E --> E2[デッドロック数]
    E --> E3[タイムアウト数]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

**3. ネットワークメトリクス**

| メトリクス | 説明 | 監視ポイント |
|-----------|------|-------------|
| **帯域使用率** | ネットワーク帯域の使用状況 | >70%で注意 |
| **パケット損失率** | パケットドロップ率 | >0.1%で警告 |
| **レイテンシ** | ネットワーク遅延 | 設計値の150% |
| **接続数** | 同時接続数 | 上限の80% |

#### ダッシュボード設計

**1. 階層化ダッシュボード**

```mermaid
graph TB
    A[ダッシュボード階層] --> B[エグゼクティブ<br/>Executive]
    A --> C[オペレーション<br/>Operations]
    A --> D[技術詳細<br/>Technical]
    
    B --> B1[SLA達成率]
    B --> B2[可用性]
    B --> B3[ユーザー満足度]
    
    C --> C1[システム全体状況]
    C --> C2[アラート一覧]
    C --> C3[性能サマリー]
    
    D --> D1[詳細メトリクス]
    D --> D2[ログ分析]
    D --> D3[トラブルシューティング]
    
    style B fill:#e3f2fd
    style C fill:#e8f5e8
    style D fill:#fff3e0
```

**2. ダッシュボード設計原則**

```mermaid
graph TB
    A[設計原則] --> B[5秒ルール]
    A --> C[階層化]
    A --> D[コンテキスト]
    A --> E[アクション指向]
    
    B --> B1[5秒で状況把握]
    B --> B2[重要情報を上部]
    
    C --> C1[概要→詳細]
    C --> C2[ドリルダウン]
    
    D --> D1[時間軸表示]
    D --> D2[比較データ]
    
    E --> E1[問題特定]
    E --> E2[対応手順]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### アラート設定

**1. アラート設計パターン**

```mermaid
graph LR
    A[メトリクス] --> B[閾値判定]
    B --> C[アラート発火]
    C --> D[通知送信]
    D --> E[エスカレーション]
    
    F[抑制ルール] --> C
    G[重複排除] --> D
    
    style C fill:#e3f2fd
    style D fill:#e8f5e8
    style E fill:#fff3e0
```

**2. アラート重要度分類**

| 重要度 | 説明 | 対応時間 | 通知方法 |
|--------|------|----------|----------|
| **Critical** | サービス停止 | 即座 | 電話、SMS、メール |
| **Warning** | 性能劣化 | 30分以内 | メール、Slack |
| **Info** | 情報通知 | 営業時間内 | メール |

#### 実装例

**1. カスタムメトリクス送信**

```python
import oci
from datetime import datetime

# Monitoring クライアント初期化
monitoring_client = oci.monitoring.MonitoringClient(config)

# カスタムメトリクス送信
def send_custom_metric(metric_name, value, dimensions=None):
    if dimensions is None:
        dimensions = {}
    
    metric_data = oci.monitoring.models.PostMetricDataDetails(
        metric_data=[
            oci.monitoring.models.MetricDataDetails(
                namespace="custom_app",
                name=metric_name,
                dimensions=dimensions,
                datapoints=[
                    oci.monitoring.models.Datapoint(
                        timestamp=datetime.utcnow(),
                        value=float(value)
                    )
                ]
            )
        ]
    )
    
    response = monitoring_client.post_metric_data(
        post_metric_data_details=metric_data
    )
    return response

# 使用例
send_custom_metric(
    metric_name="active_users",
    value=1250,
    dimensions={
        "application": "web_app",
        "environment": "production"
    }
)
```

**2. アラーム作成**

```bash
# CPU使用率アラーム作成
oci monitoring alarm create \
  --compartment-id <compartment-id> \
  --display-name "High CPU Usage" \
  --metric-compartment-id <compartment-id> \
  --namespace "oci_computeagent" \
  --query "CpuUtilization[1m].mean() > 80" \
  --severity "WARNING" \
  --destinations '["<notification-topic-id>"]' \
  --is-enabled true
```

### 7.2 Logging Service（ログサービス）

#### Logging Serviceとは

OCI Logging Serviceは、OCIリソースとアプリケーションからのログを一元的に収集・保存・分析するサービスです。構造化ログと非構造化ログの両方をサポートし、高速検索と長期保存を提供します。

```mermaid
graph TB
    A[Logging Service] --> B[ログ収集]
    A --> C[ログ保存]
    A --> D[ログ分析]
    A --> E[ログ転送]
    
    B --> B1[システムログ]
    B --> B2[アプリケーションログ]
    B --> B3[監査ログ]
    B --> B4[セキュリティログ]
    
    C --> C1[長期保存]
    C --> C2[圧縮保存]
    C --> C3[暗号化保存]
    
    D --> D1[リアルタイム検索]
    D --> D2[ログ分析]
    D --> D3[パターン検出]
    
    E --> E1[外部SIEM]
    E --> E2[分析ツール]
    E --> E3[アーカイブ]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### ログの種類と用途

**1. システムログ**

```mermaid
graph TB
    A[システムログ] --> B[OS ログ]
    A --> C[ミドルウェアログ]
    A --> D[インフラログ]
    
    B --> B1[/var/log/messages]
    B --> B2[/var/log/secure]
    B --> B3[/var/log/cron]
    
    C --> C1[Apache/Nginx]
    C --> C2[データベース]
    C --> C3[アプリケーションサーバー]
    
    D --> D1[ネットワーク機器]
    D --> D2[ロードバランサー]
    D --> D3[ファイアウォール]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

**2. アプリケーションログ**

| ログレベル | 用途 | 例 |
|-----------|------|-----|
| **ERROR** | エラー情報 | 例外、システムエラー |
| **WARN** | 警告情報 | 非推奨API使用、リソース不足 |
| **INFO** | 一般情報 | 処理開始/終了、設定変更 |
| **DEBUG** | デバッグ情報 | 詳細な処理フロー |
| **TRACE** | トレース情報 | 関数呼び出し、変数値 |

#### 構造化ログ

**1. JSON形式ログ**

```json
{
  "timestamp": "2023-12-01T10:30:00.000Z",
  "level": "INFO",
  "service": "user-service",
  "version": "1.2.3",
  "trace_id": "abc123def456",
  "span_id": "789ghi012jkl",
  "user_id": "user123",
  "action": "login",
  "result": "success",
  "duration_ms": 150,
  "ip_address": "192.168.1.100",
  "user_agent": "Mozilla/5.0...",
  "message": "User login successful"
}
```

**2. 構造化ログの利点**

```mermaid
graph TB
    A[構造化ログ利点] --> B[高速検索]
    A --> C[自動分析]
    A --> D[可視化]
    A --> E[アラート]
    
    B --> B1[フィールド検索]
    B --> B2[インデックス活用]
    
    C --> C1[統計分析]
    C --> C2[トレンド分析]
    
    D --> D1[ダッシュボード]
    D --> D2[グラフ化]
    
    E --> E1[条件ベースアラート]
    E --> E2[異常検知]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### ログ分析パターン

**1. エラー分析**

```bash
# エラーログ検索例
search "level:ERROR" 
| stats count by service, error_type
| sort -count

# 特定期間のエラー率
search "service:user-service" 
| eval error_rate = if(level="ERROR", 1, 0)
| stats avg(error_rate) as error_rate by _time
| timechart span=1h avg(error_rate)
```

**2. パフォーマンス分析**

```bash
# 応答時間分析
search "action:api_call" 
| stats avg(duration_ms), p95(duration_ms), p99(duration_ms) by endpoint
| sort -avg(duration_ms)

# スループット分析
search "service:web-app" 
| stats count as requests by _time
| timechart span=1m count
```

#### 実装例

**1. カスタムログ送信**

```python
import oci
import json
from datetime import datetime

# Logging クライアント初期化
logging_client = oci.logging.LoggingManagementClient(config)
log_client = oci.loggingingestion.LoggingClient(config)

# 構造化ログ送信
def send_log(log_group_id, log_id, log_data):
    log_entry = oci.loggingingestion.models.LogEntry(
        data=json.dumps(log_data),
        id=str(uuid.uuid4()),
        time=datetime.utcnow()
    )
    
    put_logs_details = oci.loggingingestion.models.PutLogsDetails(
        specversion="1.0",
        log_entry_batches=[
            oci.loggingingestion.models.LogEntryBatch(
                entries=[log_entry],
                source="custom_application",
                type="application_log",
                defaultlogentrytime=datetime.utcnow()
            )
        ]
    )
    
    response = log_client.put_logs(
        log_id=log_id,
        put_logs_details=put_logs_details
    )
    return response

# 使用例
log_data = {
    "level": "INFO",
    "service": "user-service",
    "action": "user_registration",
    "user_id": "user123",
    "result": "success",
    "duration_ms": 250
}

send_log(log_group_id, log_id, log_data)
```

### 7.3 Application Performance Monitoring (APM)

#### APMとは

Application Performance Monitoring（APM）は、アプリケーションの性能を詳細に監視・分析するサービスです。分散トレーシング、エラー追跡、ユーザー体験監視を通じて、アプリケーションの問題を迅速に特定・解決できます。

```mermaid
graph TB
    A[APM] --> B[分散トレーシング]
    A --> C[エラー追跡]
    A --> D[性能監視]
    A --> E[ユーザー体験監視]
    
    B --> B1[リクエスト追跡]
    B --> B2[サービス間通信]
    B --> B3[ボトルネック特定]
    
    C --> C1[例外監視]
    C --> C2[エラー率追跡]
    C --> C3[根本原因分析]
    
    D --> D1[応答時間]
    D --> D2[スループット]
    D --> D3[リソース使用率]
    
    E --> E1[ページロード時間]
    E --> E2[ユーザーセッション]
    E --> E3[満足度指標]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### 分散トレーシング

**1. トレーシングの概念**

```mermaid
graph LR
    A[ユーザーリクエスト] --> B[Web Server]
    B --> C[App Server]
    C --> D[Database]
    C --> E[External API]
    
    F[Trace ID: abc123] --> B
    F --> C
    F --> D
    F --> E
    
    G[Span 1: Web] --> B
    H[Span 2: App] --> C
    I[Span 3: DB] --> D
    J[Span 4: API] --> E
    
    style F fill:#e3f2fd
    style G fill:#e8f5e8
    style H fill:#fff3e0
    style I fill:#fce4ec
    style J fill:#f1f8e9
```

**2. トレースデータ構造**

```json
{
  "trace_id": "abc123def456ghi789",
  "spans": [
    {
      "span_id": "span001",
      "parent_span_id": null,
      "operation_name": "http_request",
      "service_name": "web-server",
      "start_time": "2023-12-01T10:30:00.000Z",
      "end_time": "2023-12-01T10:30:00.500Z",
      "duration_ms": 500,
      "tags": {
        "http.method": "GET",
        "http.url": "/api/users",
        "http.status_code": 200
      }
    },
    {
      "span_id": "span002",
      "parent_span_id": "span001",
      "operation_name": "database_query",
      "service_name": "user-service",
      "start_time": "2023-12-01T10:30:00.100Z",
      "end_time": "2023-12-01T10:30:00.300Z",
      "duration_ms": 200,
      "tags": {
        "db.statement": "SELECT * FROM users WHERE id = ?",
        "db.type": "mysql"
      }
    }
  ]
}
```

#### エラー追跡

**1. エラー分類**

```mermaid
graph TB
    A[エラー分類] --> B[アプリケーションエラー]
    A --> C[システムエラー]
    A --> D[ネットワークエラー]
    A --> E[外部サービスエラー]
    
    B --> B1[例外]
    B --> B2[ビジネスロジックエラー]
    B --> B3[バリデーションエラー]
    
    C --> C1[メモリ不足]
    C --> C2[ディスク容量不足]
    C --> C3[CPU過負荷]
    
    D --> D1[接続タイムアウト]
    D --> D2[パケット損失]
    
    E --> E1[API エラー]
    E --> E2[サービス停止]
    
    style A fill:#e3f2fd
    style B fill:#ffebee
    style C fill:#ffcdd2
    style D fill:#fff3e0
    style E fill:#fce4ec
```

#### 実装例

**1. APMエージェント設定**

```java
// Java アプリケーションでのAPM設定
@RestController
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/users/{id}")
    @Traced(operationName = "get_user")
    public ResponseEntity<User> getUser(@PathVariable String id) {
        try {
            Span span = GlobalTracer.get().activeSpan();
            span.setTag("user.id", id);
            
            User user = userService.findById(id);
            
            span.setTag("user.found", user != null);
            return ResponseEntity.ok(user);
            
        } catch (Exception e) {
            Span span = GlobalTracer.get().activeSpan();
            span.setTag("error", true);
            span.log(Map.of("error.message", e.getMessage()));
            throw e;
        }
    }
}
```

### 7.4 Operations Insights（運用インサイト）

#### Operations Insightsとは

Operations Insightsは、機械学習を活用してシステムの性能を分析し、問題の予測と最適化の推奨を行うサービスです。過去のデータから学習し、将来の問題を予測して事前対応を可能にします。

```mermaid
graph TB
    A[Operations Insights] --> B[性能分析]
    A --> C[容量計画]
    A --> D[異常検知]
    A --> E[最適化推奨]
    
    B --> B1[リソース使用率分析]
    B --> B2[ボトルネック特定]
    B --> B3[トレンド分析]
    
    C --> C1[将来需要予測]
    C --> C2[リソース計画]
    C --> C3[コスト最適化]
    
    D --> D1[パターン学習]
    D --> D2[異常パターン検知]
    D --> D3[早期警告]
    
    E --> E1[設定最適化]
    E --> E2[アーキテクチャ改善]
    E --> E3[運用改善]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### 機械学習による分析

**1. 異常検知アルゴリズム**

```mermaid
graph LR
    A[履歴データ] --> B[機械学習モデル]
    B --> C[正常パターン学習]
    C --> D[リアルタイム監視]
    D --> E[異常検知]
    E --> F[アラート生成]
    
    style B fill:#e3f2fd
    style E fill:#ffebee
```

**2. 予測分析**

```mermaid
graph TB
    A[予測分析] --> B[容量予測]
    A --> C[性能予測]
    A --> D[障害予測]
    
    B --> B1[CPU使用率予測]
    B --> B2[メモリ使用率予測]
    B --> B3[ストレージ使用率予測]
    
    C --> C1[応答時間予測]
    C --> C2[スループット予測]
    
    D --> D1[障害確率計算]
    D --> D2[メンテナンス推奨]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
```

### 7.5 統合監視戦略とアラート設計

#### 統合監視アーキテクチャ

**1. 階層化監視**

```mermaid
graph TB
    A[統合監視] --> B[インフラ層]
    A --> C[プラットフォーム層]
    A --> D[アプリケーション層]
    A --> E[ビジネス層]
    
    B --> B1[サーバー監視]
    B --> B2[ネットワーク監視]
    B --> B3[ストレージ監視]
    
    C --> C1[データベース監視]
    C --> C2[ミドルウェア監視]
    C --> C3[コンテナ監視]
    
    D --> D1[APM]
    D --> D2[ログ監視]
    D --> D3[エラー追跡]
    
    E --> E1[SLA監視]
    E --> E2[ユーザー体験]
    E --> E3[ビジネスKPI]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

#### アラート設計ベストプラクティス

**1. アラート疲れ対策**

```mermaid
graph TB
    A[アラート疲れ対策] --> B[適切な閾値設定]
    A --> C[重複排除]
    A --> D[抑制ルール]
    A --> E[エスカレーション]
    
    B --> B1[統計的閾値]
    B --> B2[動的閾値]
    B --> B3[複数条件]
    
    C --> C1[同一問題の統合]
    C --> C2[時間窓での集約]
    
    D --> D1[メンテナンス時間]
    D --> D2[依存関係考慮]
    
    E --> E1[段階的通知]
    E --> E2[自動エスカレーション]
    
    style A fill:#e3f2fd
    style B fill:#e8f5e8
    style C fill:#fff3e0
    style D fill:#fce4ec
    style E fill:#f1f8e9
```

**2. 通知チャネル設計**

| 重要度 | 1次通知 | 2次通知 | 3次通知 |
|--------|---------|---------|---------|
| **Critical** | 電話 | SMS | メール |
| **High** | SMS | メール | Slack |
| **Medium** | メール | Slack | - |
| **Low** | Slack | - | - |

#### 運用自動化

**1. 自動修復（Auto-remediation）**

```mermaid
graph LR
    A[問題検知] --> B[自動診断]
    B --> C[修復アクション実行]
    C --> D[結果確認]
    D --> E[成功]
    D --> F[失敗]
    F --> G[エスカレーション]
    
    style C fill:#e3f2fd
    style E fill:#e8f5e8
    style F fill:#ffebee
```

**2. 自動化シナリオ例**

| 問題 | 自動修復アクション |
|------|-------------------|
| **高CPU使用率** | インスタンス追加、プロセス再起動 |
| **メモリ不足** | メモリ増設、キャッシュクリア |
| **ディスク容量不足** | ログローテーション、一時ファイル削除 |
| **サービス停止** | サービス再起動、ヘルスチェック |

### まとめ

第7章では、OCIの包括的な監視・運用サービスについて詳しく解説しました。効果的な監視・運用は、システムの安定性と性能を確保する上で不可欠です。

**重要ポイント：**
1. **Monitoring Service**: リアルタイムメトリクス監視とアラート
2. **Logging Service**: 一元的なログ管理と分析
3. **APM**: アプリケーション性能の詳細監視
4. **Operations Insights**: 機械学習による予測分析
5. **統合監視**: 階層化された包括的監視戦略
6. **自動化**: 問題の自動検知と修復

次章では、これらの監視・運用を支える開発・運用の自動化（DevOps）について学習します。