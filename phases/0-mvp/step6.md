# Step 6: 테스트 작성

## 목표
전체 시스템의 단위 테스트와 통합 테스트를 작성하여 코드 품질을 보장합니다.

## 작업 내용

### 1. 단위 테스트 작성
- tests/test_ingestion/: 데이터 수집 테스트
- tests/test_processing/: 데이터 처리 테스트
- tests/test_storage/: 데이터 저장 테스트
- tests/test_api/: API 테스트

### 2. 통합 테스트 작성
- E2E 시나리오 테스트
- 데이터 파이프라인 전체 흐름 테스트
- API 통합 테스트

### 3. 테스트 픽스처 및 Mock
- 테스트 데이터 생성
- 외부 의존성 Mock
- 데이터베이스 픽스처

### 4. 테스트 커버리지 설정
- pytest-cov 설정
- 커버리지 리포트 생성
- 80% 이상 커버리지 달성

## Acceptance Criteria (AC)

### AC1: 단위 테스트 완료
- [ ] Ingestion 테스트 (커버리지 80%+)
- [ ] Processing 테스트 (커버리지 80%+)
- [ ] Storage 테스트 (커버리지 80%+)
- [ ] API 테스트 (커버리지 80%+)

### AC2: 통합 테스트 완료
- [ ] E2E 시나리오 테스트
- [ ] 파이프라인 통합 테스트
- [ ] API 통합 테스트

### AC3: 테스트 인프라
- [ ] conftest.py 작성
- [ ] 픽스처 정의
- [ ] Mock 설정
- [ ] 테스트 데이터

### AC4: CI 설정
- [ ] pytest 실행 스크립트
- [ ] 커버리지 리포트
- [ ] 테스트 실패 시 빌드 실패

## 검증 방법
```bash
# 전체 테스트 실행
pytest

# 커버리지 포함 테스트
pytest --cov=src --cov-report=html

# 특정 모듈 테스트
pytest tests/test_api/ -v

# 커버리지 확인
open htmlcov/index.html
```

## 테스트 예시

### 단위 테스트
```python
def test_api_collector():
    collector = APICollector(config)
    data = await collector.collect(url)
    assert len(data) > 0
```

### 통합 테스트
```python
def test_full_pipeline():
    # 1. 데이터 수집
    collector = APICollector(config)
    raw_data = await collector.collect(url)
    
    # 2. 데이터 처리
    pipeline = ProcessingPipeline(schema)
    processed = pipeline.process(raw_data)
    
    # 3. 데이터 저장
    storage = PostgreSQLStorage(engine)
    storage.save(processed, "test_table")
    
    # 4. 데이터 조회
    result = storage.query("SELECT * FROM test_table")
    assert len(result) > 0
```

## 참고사항
- roo_rules.md의 테스트 규칙 준수
- docs/ADR.md의 Pytest 결정 참고
- 모든 테스트는 독립적으로 실행 가능해야 함