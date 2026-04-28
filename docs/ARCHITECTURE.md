# 데이터 플랫폼 아키텍처

## 1. 시스템 개요

### 1.1 아키텍처 다이어그램

```
┌─────────────────────────────────────────────────────────────────┐
│                        Data Sources                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐       │
│  │ REST API │  │   CSV    │  │  Kafka   │  │    S3    │       │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘       │
└───────┼─────────────┼─────────────┼─────────────┼──────────────┘
        │             │             │             │
        └─────────────┴─────────────┴─────────────┘
                      │
        ┌─────────────▼─────────────┐
        │   Ingestion Layer         │
        │  ┌──────────────────────┐ │
        │  │  Data Collectors     │ │
        │  │  - API Collector     │ │
        │  │  - File Collector    │ │
        │  │  - Stream Collector  │ │
        │  └──────────┬───────────┘ │
        └─────────────┼─────────────┘
                      │
        ┌─────────────▼─────────────┐
        │   Processing Layer        │
        │  ┌──────────────────────┐ │
        │  │  Data Processors     │ │
        │  │  - Validator         │ │
        │  │  - Transformer       │ │
        │  │  - Aggregator        │ │
        │  └──────────┬───────────┘ │
        └─────────────┼─────────────┘
                      │
        ┌─────────────▼─────────────┐
        │   Storage Layer           │
        │  ┌──────────────────────┐ │
        │  │  PostgreSQL          │ │
        │  │  (Structured Data)   │ │
        │  └──────────────────────┘ │
        │  ┌──────────────────────┐ │
        │  │  Parquet Files       │ │
        │  │  (Data Lake)         │ │
        │  └──────────────────────┘ │
        │  ┌──────────────────────┐ │
        │  │  Redis Cache         │ │
        │  └──────────────────────┘ │
        └─────────────┬─────────────┘
                      │
        ┌─────────────▼─────────────┐
        │   API Layer               │
        │  ┌──────────────────────┐ │
        │  │  FastAPI             │ │
        │  │  - Query API         │ │
        │  │  - Analytics API     │ │
        │  │  - Metadata API      │ │
        │  └──────────┬───────────┘ │
        └─────────────┼─────────────┘
                      │
        ┌─────────────▼─────────────┐
        │   Monitoring Layer        │
        │  ┌──────────────────────┐ │
        │  │  Prometheus          │ │
        │  │  Grafana             │ │
        │  │  Logging (ELK)       │ │
        │  └──────────────────────┘ │
        └───────────────────────────┘
```

## 2. 레이어별 상세 설계

### 2.1 Ingestion Layer (데이터 수집)

#### 책임
- 다양한 소스에서 데이터 수집
- 초기 데이터 검증
- 에러 핸들링 및 재시도

#### 컴포넌트

**APICollector**
```python
class APICollector:
    """REST API에서 데이터 수집"""
    
    def __init__(self, config: APIConfig):
        self.base_url = config.base_url
        self.auth = config.auth
        self.retry_policy = RetryPolicy(max_retries=3)
    
    async def collect(self, endpoint: str) -> List[Dict]:
        """데이터 수집 및 반환"""
        pass
```

**FileCollector**
```python
class FileCollector:
    """파일에서 데이터 수집"""
    
    def __init__(self, config: FileConfig):
        self.file_path = config.file_path
        self.file_type = config.file_type  # csv, json, parquet
        self.chunk_size = 1000
    
    def collect(self) -> Iterator[pd.DataFrame]:
        """청크 단위로 데이터 반환"""
        pass
```

#### 데이터 플로우
1. 소스에서 데이터 읽기
2. 기본 포맷 검증
3. 메타데이터 추가 (수집 시간, 소스 정보)
4. Processing Layer로 전달

### 2.2 Processing Layer (데이터 처리)

#### 책임
- 데이터 검증 및 정제
- 데이터 변환 및 집계
- 비즈니스 로직 적용

#### 컴포넌트

**DataValidator**
```python
class DataValidator:
    """스키마 기반 데이터 검증"""
    
    def __init__(self, schema: Schema):
        self.schema = schema
    
    def validate(self, data: pd.DataFrame) -> ValidationResult:
        """데이터 검증 및 결과 반환"""
        # 필수 필드 체크
        # 데이터 타입 검증
        # 범위 검증
        pass
```

**DataTransformer**
```python
class DataTransformer:
    """데이터 변환"""
    
    def transform(self, data: pd.DataFrame, rules: List[TransformRule]) -> pd.DataFrame:
        """변환 규칙 적용"""
        # 컬럼 추가/삭제/이름 변경
        # 데이터 타입 변환
        # NULL 값 처리
        pass
```

**DataAggregator**
```python
class DataAggregator:
    """데이터 집계"""
    
    def aggregate(self, data: pd.DataFrame, config: AggConfig) -> pd.DataFrame:
        """집계 수행"""
        # 그룹별 통계
        # 시계열 집계
        # 윈도우 함수
        pass
```

#### 처리 파이프라인
```python
class ProcessingPipeline:
    """데이터 처리 파이프라인"""
    
    def __init__(self):
        self.validator = DataValidator(schema)
        self.transformer = DataTransformer()
        self.aggregator = DataAggregator()
    
    def process(self, raw_data: pd.DataFrame) -> pd.DataFrame:
        # 1. 검증
        validation_result = self.validator.validate(raw_data)
        if not validation_result.is_valid:
            raise ValidationError(validation_result.errors)
        
        # 2. 변환
        transformed = self.transformer.transform(raw_data, transform_rules)
        
        # 3. 집계 (선택적)
        if needs_aggregation:
            aggregated = self.aggregator.aggregate(transformed, agg_config)
            return aggregated
        
        return transformed
```

### 2.3 Storage Layer (데이터 저장)

#### 책임
- 처리된 데이터 영구 저장
- 효율적인 데이터 조회
- 데이터 백업 및 복원

#### 컴포넌트

**PostgreSQL (Primary Storage)**
```python
class PostgreSQLStorage:
    """구조화된 데이터 저장"""
    
    def __init__(self, connection_string: str):
        self.engine = create_engine(connection_string)
    
    def save(self, data: pd.DataFrame, table_name: str):
        """데이터 저장"""
        # 테이블 자동 생성
        # 인덱스 생성
        # 파티셔닝 (날짜 기준)
        data.to_sql(table_name, self.engine, if_exists='append')
    
    def query(self, sql: str) -> pd.DataFrame:
        """데이터 조회"""
        return pd.read_sql(sql, self.engine)
```

**ParquetStorage (Data Lake)**
```python
class ParquetStorage:
    """대용량 데이터 저장"""
    
    def __init__(self, base_path: str):
        self.base_path = base_path
    
    def save(self, data: pd.DataFrame, dataset: str, partition_cols: List[str]):
        """파티션 구조로 저장"""
        # year/month/day 파티션
        # snappy 압축
        # 메타데이터 저장
        pass
```

**RedisCache**
```python
class RedisCache:
    """자주 조회되는 데이터 캐싱"""
    
    def __init__(self, redis_url: str):
        self.redis = redis.from_url(redis_url)
    
    def get(self, key: str) -> Optional[Dict]:
        """캐시 조회"""
        pass
    
    def set(self, key: str, value: Dict, ttl: int = 3600):
        """캐시 저장 (TTL: 1시간)"""
        pass
```

#### 저장 전략
- **Hot Data** (최근 7일): PostgreSQL + Redis
- **Warm Data** (최근 30일): PostgreSQL
- **Cold Data** (30일 이상): Parquet (Data Lake)

### 2.4 API Layer (데이터 접근)

#### 책임
- REST API 제공
- 인증 및 권한 관리
- 요청 검증 및 응답 포맷팅

#### 엔드포인트 설계

**Query API**
```python
@app.get("/api/v1/data/{dataset}")
async def query_data(
    dataset: str,
    filters: Optional[str] = None,
    order_by: Optional[str] = None,
    limit: int = 100,
    offset: int = 0
) -> DataResponse:
    """데이터 조회"""
    # 1. 캐시 확인
    # 2. 캐시 미스 시 DB 조회
    # 3. 결과 캐싱
    # 4. 응답 반환
    pass
```

**Analytics API**
```python
@app.get("/api/v1/analytics/{dataset}")
async def analyze_data(
    dataset: str,
    group_by: List[str],
    metrics: List[str],
    time_range: TimeRange
) -> AnalyticsResponse:
    """데이터 분석"""
    # 1. 집계 쿼리 생성
    # 2. 실행 및 결과 반환
    pass
```

**Metadata API**
```python
@app.get("/api/v1/metadata/datasets")
async def list_datasets() -> List[DatasetInfo]:
    """데이터셋 목록"""
    pass

@app.get("/api/v1/metadata/datasets/{dataset}/schema")
async def get_schema(dataset: str) -> SchemaInfo:
    """스키마 정보"""
    pass
```

### 2.5 Monitoring Layer (모니터링)

#### 책임
- 시스템 메트릭 수집
- 로그 수집 및 분석
- 알림 발송

#### 컴포넌트

**Metrics Collector**
```python
from prometheus_client import Counter, Histogram, Gauge

# 메트릭 정의
ingestion_counter = Counter('data_ingestion_total', 'Total ingested records')
processing_duration = Histogram('data_processing_duration_seconds', 'Processing time')
storage_size = Gauge('data_storage_size_bytes', 'Storage size')

# 메트릭 수집
def collect_metrics():
    ingestion_counter.inc(batch_size)
    processing_duration.observe(duration)
    storage_size.set(current_size)
```

**Logging**
```python
import structlog

logger = structlog.get_logger()

# 구조화된 로깅
logger.info(
    "data_ingested",
    dataset="users",
    records=1000,
    duration=1.5,
    source="api"
)
```

## 3. 데이터 모델

### 3.1 메타데이터 스키마

**datasets 테이블**
```sql
CREATE TABLE datasets (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) UNIQUE NOT NULL,
    description TEXT,
    schema JSONB NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);
```

**ingestion_logs 테이블**
```sql
CREATE TABLE ingestion_logs (
    id SERIAL PRIMARY KEY,
    dataset_id INTEGER REFERENCES datasets(id),
    source VARCHAR(50) NOT NULL,
    records_count INTEGER NOT NULL,
    status VARCHAR(20) NOT NULL,
    error_message TEXT,
    started_at TIMESTAMP NOT NULL,
    completed_at TIMESTAMP,
    duration_seconds FLOAT
);
```

### 3.2 데이터 스키마 예시

**users 데이터셋**
```json
{
  "name": "users",
  "fields": [
    {"name": "user_id", "type": "integer", "required": true},
    {"name": "username", "type": "string", "required": true},
    {"name": "email", "type": "string", "required": true},
    {"name": "created_at", "type": "timestamp", "required": true},
    {"name": "last_login", "type": "timestamp", "required": false}
  ],
  "indexes": ["user_id", "email"],
  "partition_by": "created_at"
}
```

## 4. 확장성 고려사항

### 4.1 수평 확장
- **Worker 추가**: 처리 워커를 여러 개 실행
- **로드 밸런싱**: Nginx를 통한 API 요청 분산
- **데이터베이스 샤딩**: 데이터셋별 또는 날짜별 샤딩

### 4.2 성능 최적화
- **배치 처리**: 청크 단위 처리 (1000 rows)
- **병렬 처리**: 멀티프로세싱/멀티스레딩
- **인덱싱**: 자주 조회되는 컬럼에 인덱스
- **캐싱**: Redis를 통한 결과 캐싱

### 4.3 장애 대응
- **재시도 로직**: Exponential backoff
- **Circuit Breaker**: 연속 실패 시 일시 중단
- **Fallback**: 대체 데이터 소스 사용
- **Health Check**: 주기적인 상태 확인

## 5. 보안 아키텍처

### 5.1 인증/인가
- **JWT 토큰**: API 인증
- **RBAC**: 역할 기반 접근 제어
- **API Key**: 외부 시스템 연동

### 5.2 데이터 보안
- **암호화**: TLS (in transit), AES (at rest)
- **민감 데이터 마스킹**: PII 데이터 처리
- **감사 로그**: 모든 접근 기록

## 6. 배포 아키텍처

### 6.1 Docker Compose (개발/테스트)
```yaml
version: '3.8'
services:
  api:
    build: .
    ports:
      - "8000:8000"
    depends_on:
      - postgres
      - redis
  
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: dataplatform
      POSTGRES_USER: admin
      POSTGRES_PASSWORD: secret
  
  redis:
    image: redis:7
    ports:
      - "6379:6379"
  
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
  
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

### 6.2 Kubernetes (프로덕션)
- **API**: Deployment (3 replicas)
- **Worker**: Deployment (5 replicas)
- **PostgreSQL**: StatefulSet
- **Redis**: StatefulSet
- **Ingress**: Nginx Ingress Controller
