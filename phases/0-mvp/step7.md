# Step 7: 문서화 및 배포 준비

## 목표
API 문서, 사용 가이드, 배포 문서를 작성하여 프로젝트를 완성합니다.

## 작업 내용

### 1. API 문서 작성
- OpenAPI (Swagger) 문서 자동 생성 확인
- 각 엔드포인트에 상세 설명 추가
- 요청/응답 예시 추가
- 에러 코드 문서화

### 2. 사용 가이드 작성 (docs/USER_GUIDE.md)
- 설치 방법
- 환경 설정
- 데이터 수집 방법
- API 사용 예시
- 트러블슈팅

### 3. 개발자 가이드 작성 (docs/DEVELOPER_GUIDE.md)
- 프로젝트 구조 설명
- 개발 환경 설정
- 코드 스타일 가이드
- 테스트 실행 방법
- 기여 가이드

### 4. 배포 문서 작성 (docs/DEPLOYMENT.md)
- Docker 배포 방법
- 환경 변수 설정
- 데이터베이스 마이그레이션
- 모니터링 설정
- 백업 및 복원

### 5. README.md 업데이트
- 프로젝트 소개
- 주요 기능
- 빠른 시작 가이드
- 문서 링크
- 라이선스

## Acceptance Criteria (AC)

### AC1: API 문서 완성
- [ ] OpenAPI 문서 확인
- [ ] 모든 엔드포인트 설명 추가
- [ ] 요청/응답 예시
- [ ] 에러 코드 정의

### AC2: 사용 가이드 작성
- [ ] USER_GUIDE.md 작성
- [ ] 설치 방법 문서화
- [ ] API 사용 예시
- [ ] 트러블슈팅 섹션

### AC3: 개발자 가이드 작성
- [ ] DEVELOPER_GUIDE.md 작성
- [ ] 프로젝트 구조 설명
- [ ] 개발 환경 설정
- [ ] 기여 가이드

### AC4: 배포 문서 작성
- [ ] DEPLOYMENT.md 작성
- [ ] Docker 배포 가이드
- [ ] 환경 설정 가이드
- [ ] 운영 가이드

### AC5: README 업데이트
- [ ] 프로젝트 소개
- [ ] 기능 목록
- [ ] 빠른 시작
- [ ] 문서 링크

## 검증 방법

### 1. API 문서 확인
```bash
# 서버 실행
uvicorn src.api.main:app --reload

# Swagger UI 확인
open http://localhost:8000/docs

# ReDoc 확인
open http://localhost:8000/redoc
```

### 2. 문서 검토
- [ ] 모든 문서가 최신 상태인지 확인
- [ ] 예시 코드가 실행 가능한지 확인
- [ ] 링크가 올바른지 확인
- [ ] 오타 및 문법 확인

### 3. 배포 테스트
```bash
# Docker 이미지 빌드
docker build -t data-platform:latest .

# Docker Compose로 배포
docker-compose up -d

# 헬스 체크
curl http://localhost:8000/health

# 정리
docker-compose down
```

## 문서 구조 예시

### USER_GUIDE.md
```markdown
# 사용자 가이드

## 설치
...

## 데이터 수집
...

## API 사용
...
```

### DEVELOPER_GUIDE.md
```markdown
# 개발자 가이드

## 프로젝트 구조
...

## 개발 환경 설정
...

## 코딩 규칙
...
```

### DEPLOYMENT.md
```markdown
# 배포 가이드

## Docker 배포
...

## 환경 변수
...

## 모니터링
...
```

## 예상 산출물
- 완전한 API 문서
- 사용자 가이드
- 개발자 가이드
- 배포 가이드
- 업데이트된 README

## 참고사항
- docs/PRD.md의 기능 설명 참고
- docs/ARCHITECTURE.md의 구조 설명 참고
- docs/ADR.md의 기술 결정 참고
- roo_rules.md의 문서화 규칙 준수

## 완료 후
모든 step이 완료되면 phases/0-mvp/index.json의 phase status를 "completed"로 업데이트하세요.