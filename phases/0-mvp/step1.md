# Step 1: 데이터 모델 및 스키마 정의

## 목표
데이터 플랫폼에서 사용할 데이터 모델, 스키마, 그리고 데이터베이스 테이블을 정의합니다.

## 작업 내용

### 1. Pydantic 모델 정의 (src/models/)
다음 모델들을 생성하세요:

#### src/models/__init__.py
```python
from .dataset import Dataset, DatasetCreate, DatasetSchema
from .ingestion import IngestionLog, IngestionStatus
from .data_record import DataRecord

__all__ = [
    "Dataset",
    "DatasetCreate", 
    "DatasetSchema",
    "IngestionLog",
    "IngestionStatus",
    "DataRecord",
]
```

#### src/models/dataset.py
```python
from datetime import datetime
from typing import List, Dict, Any, Optional
from pydantic import BaseModel, Field

class FieldSchema(BaseModel):
    """데이터 필드 스키마"""
    name: str
    type: str  # integer, string, float, timestamp, boolean
    required: bool = True
    description: Optional[str] = None

class DatasetSchema(BaseModel):
    """데이터셋 스키마"""
    fields: List[FieldSchema]
    indexes: List[str] = []
    partition_by: Optional[str] = None

class DatasetCreate(BaseModel):
    """데이터셋 생성 요청"""
    name: str = Field(..., min_length=1, max_length=100)
    description: Optional[str] = None
    schema: DatasetSchema

class Dataset(BaseModel):
    """데이터셋 모델"""
    id: int
    name: str
    description: Optional[str]
    schema: DatasetSchema
    created_at: datetime
    updated_at: datetime
    
    class Config:
        from_attributes = True
```

#### src/models/ingestion.py
```python
from datetime import datetime
from enum import Enum
from typing import Optional
from pydantic import BaseModel

class IngestionStatus(str, Enum):
    """수집 상태"""
    PENDING = "pending"
    RUNNING = "running"
    COMPLETED = "completed"
    FAILED = "failed"

class IngestionLog(BaseModel):
    """데이터 수집 로그"""
    id: int
    dataset_id: int
    source: str  # api, csv, stream
    records_count: int
    status: IngestionStatus
    error_message: Optional[str] = None
    started_at: datetime
    completed_at: Optional[datetime] = None
    duration_seconds: Optional[float] = None
    
    class Config:
        from_attributes = True
```

#### src/models/data_record.py
```python
from typing import Dict, Any
from datetime import datetime
from pydantic import BaseModel

class DataRecord(BaseModel):
    """일반 데이터 레코드"""
    data: Dict[str, Any]
    ingested_at: datetime
    source: str
```

### 2. SQLAlchemy 모델 정의 (src/storage/models.py)

```python
from datetime import datetime
from sqlalchemy import Column, Integer, String, Text, DateTime, Float, JSON, Enum as SQLEnum
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.sql import func
from src.models.ingestion import IngestionStatus

Base = declarative_base()

class DatasetModel(Base):
    """데이터셋 테이블"""
    __tablename__ = "datasets"
    
    id = Column(Integer, primary_key=True, index=True)
    name = Column(String(100), unique=True, nullable=False, index=True)
    description = Column(Text, nullable=True)
    schema = Column(JSON, nullable=False)
    created_at = Column(DateTime, default=func.now(), nullable=False)
    updated_at = Column(DateTime, default=func.now(), onupdate=func.now(), nullable=False)

class IngestionLogModel(Base):
    """수집 로그 테이블"""
    __tablename__ = "ingestion_logs"
    
    id = Column(Integer, primary_key=True, index=True)
    dataset_id = Column(Integer, nullable=False, index=True)
    source = Column(String(50), nullable=False)
    records_count = Column(Integer, nullable=False)
    status = Column(SQLEnum(IngestionStatus), nullable=False, index=True)
    error_message = Column(Text, nullable=True)
    started_at = Column(DateTime, nullable=False, index=True)
    completed_at = Column(DateTime, nullable=True)
    duration_seconds = Column(Float, nullable=True)
```

### 3. 데이터베이스 초기화 스크립트 (src/storage/database.py)

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, Session
from config.settings import Settings
from src.storage.models import Base

settings = Settings()

engine = create_engine(
    settings.database_url,
    pool_pre_ping=True,
    pool_size=10,
    max_overflow=20
)

SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

def get_db() -> Session:
    """데이터베이스 세션 생성"""
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

def init_db():
    """데이터베이스 초기화"""
    Base.metadata.create_all(bind=engine)
```

### 4. 스키마 검증 유틸리티 (src/processing/validator.py)

```python
from typing import Dict, Any, List
from pydantic import BaseModel, ValidationError
from src.models.dataset import DatasetSchema, FieldSchema

class ValidationResult(BaseModel):
    """검증 결과"""
    is_valid: bool
    errors: List[str] = []

class SchemaValidator:
    """스키마 기반 데이터 검증"""
    
    def __init__(self, schema: DatasetSchema):
        self.schema = schema
        self.field_map = {f.name: f for f in schema.fields}
    
    def validate_record(self, record: Dict[str, Any]) -> ValidationResult:
        """단일 레코드 검증"""
        errors = []
        
        # 필수 필드 체크
        for field in self.schema.fields:
            if field.required and field.name not in record:
                errors.append(f"Required field missing: {field.name}")
        
        # 데이터 타입 체크
        for key, value in record.items():
            if key not in self.field_map:
                continue
            
            field = self.field_map[key]
            if not self._check_type(value, field.type):
                errors.append(f"Invalid type for {key}: expected {field.type}")
        
        return ValidationResult(
            is_valid=len(errors) == 0,
            errors=errors
        )
    
    def _check_type(self, value: Any, expected_type: str) -> bool:
        """타입 체크"""
        if value is None:
            return True
        
        type_map = {
            "integer": int,
            "string": str,
            "float": (int, float),
            "boolean": bool,
        }
        
        expected = type_map.get(expected_type)
        if expected is None:
            return True
        
        return isinstance(value, expected)
```

## Acceptance Criteria (AC)

### AC1: Pydantic 모델 정의 완료
- [ ] Dataset, DatasetCreate, DatasetSchema 모델 작성
- [ ] IngestionLog, IngestionStatus 모델 작성
- [ ] DataRecord 모델 작성
- [ ] 모든 모델에 타입 힌트 및 docstring 포함

### AC2: SQLAlchemy 모델 정의 완료
- [ ] DatasetModel 테이블 정의
- [ ] IngestionLogModel 테이블 정의
- [ ] 적절한 인덱스 설정
- [ ] 관계 및 제약조건 정의

### AC3: 데이터베이스 초기화 완료
- [ ] database.py 작성
- [ ] get_db() 함수 구현
- [ ] init_db() 함수 구현
- [ ] 연결 풀 설정

### AC4: 스키마 검증 구현
- [ ] SchemaValidator 클래스 작성
- [ ] validate_record() 메서드 구현
- [ ] 타입 체크 로직 구현
- [ ] ValidationResult 반환

### AC5: 테스트 작성
- [ ] Pydantic 모델 테스트
- [ ] SQLAlchemy 모델 테스트
- [ ] 스키마 검증 테스트
- [ ] 데이터베이스 연결 테스트

## 검증 방법

### 1. 모델 테스트 (tests/test_models.py)
```python
from src.models.dataset import Dataset, DatasetSchema, FieldSchema

def test_dataset_schema():
    schema = DatasetSchema(
        fields=[
            FieldSchema(name="id", type="integer", required=True),
            FieldSchema(name="name", type="string", required=True),
        ],
        indexes=["id"]
    )
    assert len(schema.fields) == 2
    assert schema.indexes == ["id"]

def test_dataset_create():
    dataset = DatasetCreate(
        name="users",
        description="User data",
        schema=DatasetSchema(fields=[
            FieldSchema(name="id", type="integer", required=True)
        ])
    )
    assert dataset.name == "users"
```

### 2. 데이터베이스 테스트 (tests/test_storage/test_database.py)
```python
from src.storage.database import init_db, get_db
from src.storage.models import DatasetModel

def test_init_db():
    """데이터베이스 초기화 테스트"""
    init_db()
    # 테이블 생성 확인

def test_create_dataset():
    """데이터셋 생성 테스트"""
    db = next(get_db())
    dataset = DatasetModel(
        name="test_dataset",
        schema={"fields": []}
    )
    db.add(dataset)
    db.commit()
    assert dataset.id is not None
```

### 3. 스키마 검증 테스트 (tests/test_processing/test_validator.py)
```python
from src.processing.validator import SchemaValidator
from src.models.dataset import DatasetSchema, FieldSchema

def test_valid_record():
    schema = DatasetSchema(fields=[
        FieldSchema(name="id", type="integer", required=True),
        FieldSchema(name="name", type="string", required=True),
    ])
    validator = SchemaValidator(schema)
    
    result = validator.validate_record({"id": 1, "name": "test"})
    assert result.is_valid is True

def test_missing_required_field():
    schema = DatasetSchema(fields=[
        FieldSchema(name="id", type="integer", required=True),
    ])
    validator = SchemaValidator(schema)
    
    result = validator.validate_record({})
    assert result.is_valid is False
    assert "Required field missing: id" in result.errors
```

## 예상 산출물
- 완전한 데이터 모델 정의
- 데이터베이스 스키마
- 스키마 검증 시스템
- 포괄적인 테스트 코드

## 참고사항
- roo_rules.md의 네이밍 규칙을 따르세요
- docs/ARCHITECTURE.md의 데이터 모델 섹션을 참고하세요
- docs/ADR.md의 Pydantic, SQLAlchemy 결정을 따르세요
- 모든 모델에 타입 힌트를 추가하세요

## 다음 Step
Step 2에서 데이터 수집 레이어를 구현합니다.