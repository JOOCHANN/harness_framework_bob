# Step 4: 데이터 저장 레이어 구현

## 목표
처리된 데이터를 PostgreSQL에 효율적으로 저장하는 Storage Layer를 구현합니다.

## 작업 내용

### 1. PostgreSQL Storage 구현 (src/storage/postgres_storage.py)
- SQLAlchemy를 사용한 데이터 저장
- 배치 삽입 (bulk insert)
- 트랜잭션 관리
- 에러 핸들링 및 롤백

### 2. Repository 패턴 구현 (src/storage/repositories/)
- DatasetRepository: 데이터셋 CRUD
- IngestionLogRepository: 수집 로그 CRUD
- 동적 테이블 생성 (데이터셋별)

### 3. 데이터 마이그레이션 (alembic/)
- Alembic 설정
- 초기 마이그레이션 스크립트
- 마이그레이션 실행 스크립트

## Acceptance Criteria (AC)

### AC1: PostgreSQL Storage 구현
- [ ] PostgreSQLStorage 클래스 작성
- [ ] save() 메서드 구현 (배치 삽입)
- [ ] query() 메서드 구현
- [ ] 트랜잭션 관리

### AC2: Repository 구현
- [ ] DatasetRepository 작성
- [ ] IngestionLogRepository 작성
- [ ] CRUD 메서드 구현
- [ ] 동적 테이블 생성

### AC3: 마이그레이션 설정
- [ ] Alembic 초기화
- [ ] 마이그레이션 스크립트 작성
- [ ] 업그레이드/다운그레이드 테스트

### AC4: 테스트 작성
- [ ] Storage 단위 테스트
- [ ] Repository 테스트
- [ ] 트랜잭션 테스트

## 검증 방법
```python
# 데이터 저장 테스트
storage = PostgreSQLStorage(engine)
storage.save(df, "users")

# 데이터 조회 테스트
result = storage.query("SELECT * FROM users LIMIT 10")
assert len(result) == 10
```

## 참고사항
- docs/ARCHITECTURE.md의 Storage Layer 설계 참고
- docs/PRD.md의 데이터 저장 요구사항 확인
- docs/ADR.md의 PostgreSQL, SQLAlchemy 결정 참고