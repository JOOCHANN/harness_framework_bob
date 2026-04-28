# Step 0: 프로젝트 초기 설정

## 목표
데이터 플랫폼 프로젝트의 기본 구조와 개발 환경을 설정합니다.

## 작업 내용

### 1. 프로젝트 구조 생성
다음 디렉토리 구조를 생성하세요:
```
data-platform/
├── src/
│   ├── __init__.py
│   ├── ingestion/
│   │   └── __init__.py
│   ├── processing/
│   │   └── __init__.py
│   ├── storage/
│   │   └── __init__.py
│   └── api/
│       └── __init__.py
├── tests/
│   ├── __init__.py
│   ├── test_ingestion/
│   ├── test_processing/
│   ├── test_storage/
│   └── test_api/
├── config/
│   ├── __init__.py
│   └── settings.py
├── requirements.txt
├── requirements-dev.txt
├── .env.example
├── .gitignore
├── docker-compose.yml
├── Dockerfile
└── README.md
```

### 2. requirements.txt 작성
다음 패키지들을 포함하세요:
```
fastapi==0.104.1
uvicorn[standard]==0.24.0
pandas==2.1.3
sqlalchemy==2.0.23
psycopg2-binary==2.9.9
pydantic==2.5.0
pydantic-settings==2.1.0
python-dotenv==1.0.0
redis==5.0.1
```

### 3. requirements-dev.txt 작성
```
pytest==7.4.3
pytest-asyncio==0.21.1
pytest-cov==4.1.0
black==23.11.0
pylint==3.0.2
mypy==1.7.1
httpx==0.25.2
```

### 4. .env.example 작성
```
# Database
DATABASE_URL=postgresql://admin:secret@localhost:5432/dataplatform

# Redis
REDIS_URL=redis://localhost:6379/0

# API
API_HOST=0.0.0.0
API_PORT=8000
API_RELOAD=true

# Logging
LOG_LEVEL=INFO
```

### 5. docker-compose.yml 작성
PostgreSQL, Redis, API 서비스를 포함한 Docker Compose 파일을 작성하세요.

### 6. Dockerfile 작성
Python 3.11 기반의 멀티 스테이지 빌드 Dockerfile을 작성하세요.

### 7. config/settings.py 작성
Pydantic Settings를 사용한 설정 관리 클래스를 작성하세요:
```python
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    database_url: str
    redis_url: str
    api_host: str = "0.0.0.0"
    api_port: int = 8000
    log_level: str = "INFO"
    
    class Config:
        env_file = ".env"
```

### 8. .gitignore 업데이트
Python 프로젝트에 필요한 .gitignore 항목들을 추가하세요.

## Acceptance Criteria (AC)

### AC1: 디렉토리 구조 생성 완료
- [ ] src/ 하위 모든 모듈 디렉토리 생성
- [ ] tests/ 하위 모든 테스트 디렉토리 생성
- [ ] config/ 디렉토리 생성
- [ ] 모든 디렉토리에 __init__.py 파일 존재

### AC2: 의존성 파일 작성 완료
- [ ] requirements.txt 작성 (프로덕션 패키지)
- [ ] requirements-dev.txt 작성 (개발 패키지)
- [ ] 모든 패키지 버전 고정

### AC3: 환경 설정 파일 작성 완료
- [ ] .env.example 작성
- [ ] config/settings.py 작성 및 동작 확인
- [ ] 환경 변수 로딩 테스트

### AC4: Docker 설정 완료
- [ ] docker-compose.yml 작성
- [ ] Dockerfile 작성
- [ ] `docker-compose up` 실행 성공

### AC5: 기본 문서 작성
- [ ] README.md에 프로젝트 설명 추가
- [ ] 설치 방법 문서화
- [ ] 실행 방법 문서화

## 검증 방법

### 1. 구조 검증
```bash
# 디렉토리 구조 확인
tree src/ tests/ config/
```

### 2. 의존성 설치 검증
```bash
# 가상환경 생성 및 패키지 설치
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

### 3. Docker 검증
```bash
# Docker Compose 실행
docker-compose up -d

# 서비스 상태 확인
docker-compose ps

# PostgreSQL 연결 테스트
docker-compose exec postgres psql -U admin -d dataplatform -c "SELECT 1;"

# Redis 연결 테스트
docker-compose exec redis redis-cli ping

# 정리
docker-compose down
```

### 4. 설정 로딩 검증
```python
# test_settings.py
from config.settings import Settings

def test_settings_load():
    settings = Settings()
    assert settings.database_url is not None
    assert settings.redis_url is not None
    assert settings.api_port == 8000
```

## 예상 산출물
- 완전한 프로젝트 디렉토리 구조
- 설치 가능한 의존성 파일들
- 실행 가능한 Docker 환경
- 기본 설정 관리 시스템

## 참고사항
- roo_rules.md의 파일 구조 규칙을 따르세요
- docs/ARCHITECTURE.md의 레이어 구조를 반영하세요
- docs/ADR.md의 기술 스택을 사용하세요
- 모든 파일에 적절한 주석을 추가하세요

## 다음 Step
Step 1에서 데이터 모델 및 스키마를 정의합니다.