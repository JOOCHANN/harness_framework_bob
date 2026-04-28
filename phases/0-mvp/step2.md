# Step 2: 데이터 수집 레이어 구현

## 목표
REST API와 CSV 파일에서 데이터를 수집하는 Ingestion Layer를 구현합니다.

## 작업 내용

### 1. API Collector 구현 (src/ingestion/api_collector.py)
- HTTP 클라이언트 (httpx) 사용
- 재시도 로직 (3회, exponential backoff)
- 인증 지원 (API Key, Bearer Token)
- 에러 핸들링

### 2. File Collector 구현 (src/ingestion/file_collector.py)
- CSV, JSON 파일 읽기
- 청크 단위 처리 (1000 rows)
- 파일 포맷 자동 감지
- 대용량 파일 처리

### 3. Collector Factory (src/ingestion/factory.py)
- 소스 타입에 따른 Collector 생성
- 설정 기반 Collector 초기화

## Acceptance Criteria (AC)

### AC1: API Collector 구현
- [ ] APICollector 클래스 작성
- [ ] collect() 메서드 구현
- [ ] 재시도 로직 구현
- [ ] 인증 처리

### AC2: File Collector 구현
- [ ] FileCollector 클래스 작성
- [ ] CSV 파일 읽기
- [ ] JSON 파일 읽기
- [ ] 청크 처리

### AC3: 테스트 작성
- [ ] API Collector 단위 테스트
- [ ] File Collector 단위 테스트
- [ ] Mock을 사용한 외부 API 테스트

## 검증 방법
```python
# API 수집 테스트
collector = APICollector(config)
data = await collector.collect("https://api.example.com/data")
assert len(data) > 0

# CSV 수집 테스트
collector = FileCollector("data.csv")
for chunk in collector.collect():
    assert len(chunk) <= 1000
```

## 참고사항
- docs/ARCHITECTURE.md의 Ingestion Layer 설계 참고
- docs/PRD.md의 데이터 수집 요구사항 확인
- roo_rules.md의 에러 핸들링 규칙 준수