# 🛡️ AI 에이전트 규칙

> **Bob/Roo-Cline에게**: 이 프레임워크로 작업할 때 반드시 따라야 할 규칙입니다.

## 핵심 원칙

### 1. 파일 역할 이해하기

```
docs/
├── PRD.md              → 무엇을 만들지
├── ARCHITECTURE.md     → 어떻게 만들지
├── ADR.md              → 어떤 기술 쓸지
├── UI_GUIDE.md         → 디자인 가이드
├── RULES.md            → 프로젝트 규칙 (CRITICAL)
└── WORKFLOW.md         → Bob 워크플로우 가이드

phases/                 → Bob이 생성 (Phase 계획 + Step 파일)
```

### 2. 워크플로우

```
Phase 계획 요청 받음
  ↓
docs/ 읽기 (PRD, ARCHITECTURE, ADR, RULES)
  ↓
phases/{N}-{name}/ 생성
  ↓
index.json + step{N}.md 파일들 생성
  ↓
사용자가 각 step 실행 요청
  ↓
step 파일 읽고 작업 수행
  ↓
index.json 상태 업데이트
```

### 3. Step 설계 규칙

**DO ✅**
- 한 step은 하나의 작은 작업만 (30분~2시간)
- 각 step 파일에 모든 정보 포함 (자기완결성)
- **"읽어야 할 파일" 섹션 필수** (docs/ + 이전 step 파일)
- AC는 실행 가능한 명령어로 작성
- 금지사항은 "X 금지. 이유: Y" 형식
- kebab-case 네이밍 (`project-setup`, `api-layer`)

**DON'T ❌**
- "이전에 논의한 대로" 같은 외부 참조
- "조심해서 구현" 같은 모호한 지시
- 여러 작업을 한 step에 몰아넣기

### 4. 상태 관리

**필수 필드**:
```json
{
  "step": 0,
  "name": "project-setup",
  "status": "pending"  // pending, completed, error, blocked
}
```

**완료 시 추가**:
```json
{
  "status": "completed",
  "summary": "구체적인 완료 내용",
  "completed_at": "2024-01-15T10:30:00+09:00"
}
```

**에러 시 추가**:
```json
{
  "status": "error",
  "error_message": "구체적인 에러 내용",
  "failed_at": "2024-01-15T10:30:00+09:00"
}
```

### 5. Step 파일 구조

```markdown
# Step {N}: {name}

## 작업
{무엇을 할지 명확하게}

### 생성할 파일
- `path/to/file`: 설명

### 구현 요구사항
1. 구체적 요구사항 1
2. 구체적 요구사항 2

## Acceptance Criteria
```bash
npm run build
npm test
```

## 검증 절차
1. AC 명령어 실행
2. index.json 업데이트

## 금지사항
- 구체적 금지사항 (이유 포함)
```

## 자주 하는 실수

### ❌ 나쁜 예
```markdown
# Step 1: API 구현

## 작업
이전에 논의한 대로 API를 구현합니다.

## AC
API가 잘 동작해야 합니다.
```

### ✅ 좋은 예
```markdown
# Step 1: user-api

## 작업
User CRUD API를 구현합니다.

### 생성할 파일
- `src/app/api/users/route.ts`: GET, POST 엔드포인트
- `src/types/user.ts`: User 타입 정의

### 구현 요구사항
1. GET /api/users: 모든 사용자 조회
2. POST /api/users: 새 사용자 생성
3. 입력 검증: email 형식, name 필수

## Acceptance Criteria
```bash
npm run build
curl http://localhost:3000/api/users
```

## 금지사항
- 클라이언트에서 직접 DB 접근 금지 (보안 이슈)
```

## 체크리스트

### Phase 계획 시
- [ ] docs/ 모든 파일을 읽었는가?
- [ ] 각 step이 30분~2시간 크기인가?
- [ ] step 이름이 kebab-case인가?
- [ ] 모든 step에 AC가 있는가?

### Step 실행 시
- [ ] step 파일을 읽었는가?
- [ ] AC 명령어가 모두 통과했는가?
- [ ] docs/RULES.md의 CRITICAL 규칙을 지켰는가?
- [ ] index.json을 업데이트했는가?
- [ ] summary가 구체적으로 작성되었는가?

---

더 자세한 내용은 [docs/WORKFLOW.md](docs/WORKFLOW.md)를 참고하세요.