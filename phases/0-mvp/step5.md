# Step 5: REST API 레이어 구현

## 목표
FastAPI를 사용하여 데이터 조회 및 관리를 위한 REST API를 구현합니다.

## 작업 내용

### 1. FastAPI 애플리케이션 설정 (src/api/main.py)
- FastAPI 앱 초기화
- CORS 설정
- 미들웨어 설정
- 라우터 등록

### 2. 데이터 조회 API (src/api/routers/data.py)
- GET /api/v1/data/{dataset}: 데이터 조회
- 필터링 (where 조건)
- 정렬 (order_by)
- 페이지네이션 (limit/offset)

### 3. 데이터셋 관리 API (src/api/routers/datasets.py)
- GET /api/v1/datasets: 데이터셋 목록
- POST /api/v1/datasets: 데이터셋 생성
- GET /api/v1/datasets/{id}: 데이터셋 상세
- DELETE /api/v1/datasets/{id}: 데이터셋 삭제

### 4. 메타데이터 API (src/api/routers/metadata.py)
- GET /api/v1/metadata/datasets: 데이터셋 메타데이터
- GET /api/v1/metadata/datasets/{dataset}/schema: 스키마 정보
- GET /api/v1/metadata/datasets/{dataset}/stats: 통계 정보

## Acceptance Criteria (AC)

### AC1: FastAPI 설정
- [ ] main.py 작성
- [ ] 라우터 등록
- [ ] CORS 설정
- [ ] 에러 핸들러

### AC2: 데이터 조회 API
- [ ] 조회 엔드포인트 구현
- [ ] 필터링 기능
- [ ] 페이지네이션
- [ ] 응답 모델 정의

### AC3: 데이터셋 관리 API
- [ ] CRUD 엔드포인트 구현
- [ ] 요청 검증
- [ ] 에러 응답
- [ ] OpenAPI 문서

### AC4: 메타데이터 API
- [ ] 메타데이터 엔드포인트
- [ ] 스키마 조회
- [ ] 통계 정보
- [ ] 캐싱 적용

### AC5: 테스트 작성
- [ ] API 통합 테스트
- [ ] 엔드포인트별 테스트
- [ ] 에러 케이스 테스트

## 검증 방법
```bash
# 서버 실행
uvicorn src.api.main:app --reload

# API 테스트
curl http://localhost:8000/api/v1/datasets
curl http://localhost:8000/api/v1/data/users?limit=10

# OpenAPI 문서 확인
open http://localhost:8000/docs
```

## 참고사항
- docs/ARCHITECTURE.md의 API Layer 설계 참고
- docs/PRD.md의 데이터 API 요구사항 확인
- docs/ADR.md의 FastAPI 결정 참고
- roo_rules.md의 API 규칙 준수