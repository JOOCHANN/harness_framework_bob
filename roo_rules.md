# 데이터 플랫폼 프로젝트 규칙

## 프로젝트 개요
실시간 데이터 수집, 처리, 분석을 위한 확장 가능한 데이터 플랫폼

## 코딩 규칙

### Python 코드
- Python 3.11+ 사용
- Type hints 필수 사용
- Black formatter 적용 (line length: 100)
- Pylint 점수 8.0 이상 유지
- 모든 함수에 docstring 작성 (Google style)

### 파일 구조
```
data-platform/
├── src/
│   ├── ingestion/      # 데이터 수집
│   ├── processing/     # 데이터 처리
│   ├── storage/        # 데이터 저장
│   └── api/           # REST API
├── tests/
├── config/
└── docs/
```

### 네이밍 규칙
- 파일명: `snake_case.py`
- 클래스명: `PascalCase`
- 함수/변수명: `snake_case`
- 상수: `UPPER_SNAKE_CASE`
- Private 메서드: `_leading_underscore`

## 데이터 처리 규칙

### 데이터 품질
- 모든 입력 데이터 검증 필수
- 스키마 정의 및 검증 (Pydantic 사용)
- 데이터 타입 변환 시 에러 핸들링
- 누락 데이터 처리 전략 명시

### 성능
- 배치 처리 시 chunk size: 1000
- API 응답 시간: 200ms 이하
- 데이터베이스 쿼리 최적화 (인덱스 활용)
- 메모리 사용량 모니터링

### 보안
- 환경 변수로 credential 관리 (.env)
- API 키는 절대 코드에 하드코딩 금지
- 민감 데이터 로깅 금지
- SQL injection 방지 (parameterized query)

## 테스트 규칙

### 단위 테스트
- pytest 사용
- 커버리지 80% 이상
- 각 함수마다 최소 1개 테스트
- Mock 사용하여 외부 의존성 제거

### 통합 테스트
- Docker Compose로 테스트 환경 구성
- 실제 데이터베이스 사용
- E2E 시나리오 테스트

## Git 규칙

### 커밋 메시지
```
feat(module): 기능 추가
fix(module): 버그 수정
docs: 문서 업데이트
test: 테스트 추가/수정
refactor: 코드 리팩터링
chore: 기타 작업
```

### 브랜치 전략
- `main`: 프로덕션 코드
- `develop`: 개발 브랜치
- `feat-*`: 기능 개발
- `fix-*`: 버그 수정

## 문서화 규칙

### 코드 문서
- 모든 모듈에 module docstring
- 복잡한 로직에 inline comment
- README.md에 설치 및 실행 방법

### API 문서
- OpenAPI (Swagger) 스펙 작성
- 모든 엔드포인트 예시 포함
- 에러 코드 및 메시지 정의

## 의존성 관리

### 패키지
- `requirements.txt`: 프로덕션 의존성
- `requirements-dev.txt`: 개발 의존성
- 버전 고정 (==) 사용
- 정기적인 보안 업데이트

### 주요 라이브러리
- FastAPI: REST API
- Pandas: 데이터 처리
- SQLAlchemy: ORM
- Pydantic: 데이터 검증
- Pytest: 테스트

## 배포 규칙

### Docker
- Multi-stage build 사용
- 이미지 크기 최소화
- 환경별 설정 분리 (dev/staging/prod)

### 모니터링
- 로그 레벨: INFO (prod), DEBUG (dev)
- 구조화된 로깅 (JSON format)
- 메트릭 수집 (Prometheus)

## 에러 핸들링

### 예외 처리
- 구체적인 예외 타입 사용
- 사용자 친화적 에러 메시지
- 에러 로깅 및 추적
- Graceful degradation

### 재시도 로직
- 네트워크 에러: 3회 재시도 (exponential backoff)
- 데이터베이스 에러: 2회 재시도
- 최대 재시도 시간: 30초