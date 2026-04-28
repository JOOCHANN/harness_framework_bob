# Bob 워크플로우

> **Bob에게**: 먼저 [../AGENTS.md](../AGENTS.md)를 읽고, 이 워크플로우를 따라주세요.

## 🎯 워크플로우

```
1. 문서 읽기 → 2. Phase 설계 → 3. 파일 생성 → 4. Step 실행
```

---

## 1. 문서 읽기

사용자가 phase 계획을 요청하면 다음 파일들을 읽으세요:

```
- docs/RULES.md
- docs/PRD.md
- docs/ARCHITECTURE.md
- docs/ADR.md
```

불명확한 사항이 있으면 질문하세요.

---

## 2. Phase 설계

Phase를 여러 step으로 나누세요. 각 step은 **하나의 작은 작업**만 수행합니다.

### 설계 원칙

**1. Scope 최소화**
- ❌ "API + UI + 테스트 구현"
- ✅ "API 구현" → "UI 구현" → "테스트 작성" (3개 step)

**2. 자기완결성**
- 각 step 파일에 필요한 모든 정보 포함
- "이전에 논의한 대로" 같은 외부 참조 금지

**3. 실행 가능한 AC**
- ❌ "API가 잘 동작해야 한다"
- ✅ `npm run build && npm test`

**4. 구체적인 금지사항**
- ❌ "조심해서 구현하라"
- ✅ "클라이언트에서 직접 DB 접근 금지. 이유: 보안"

### Step 네이밍

kebab-case 사용:
- ✅ `project-setup`, `api-layer`, `auth-flow`
- ❌ `Step 1`, `구현`, `API 만들기`

---

## 3. 파일 생성

### 3-1. `phases/index.json`

```json
{
  "phases": [
    { "dir": "0-mvp", "status": "pending" }
  ]
}
```

### 3-2. `phases/0-mvp/index.json`

```json
{
  "project": "MyProject",
  "phase": "0-mvp",
  "steps": [
    { "step": 0, "name": "project-setup", "status": "pending" },
    { "step": 1, "name": "core-types", "status": "pending" },
    { "step": 2, "name": "api-layer", "status": "pending" }
  ]
}
```

### 3-3. `phases/0-mvp/step{N}.md`

각 step마다 파일 생성:

```markdown
# Step 0: project-setup

## 읽어야 할 파일

먼저 아래 파일들을 읽고 프로젝트를 이해하라:
- `docs/ARCHITECTURE.md`
- `docs/ADR.md`
- `docs/RULES.md`

## 작업

Next.js 15 프로젝트를 초기 설정합니다.

### 생성할 파일
- `package.json`: 의존성 정의
- `tsconfig.json`: TypeScript 설정
- `next.config.js`: Next.js 설정

### 구현 요구사항
1. Next.js 15 + TypeScript strict mode
2. Tailwind CSS 설정
3. 기본 디렉토리 구조:
   ```
   src/
   ├── app/
   ├── components/
   └── types/
   ```

## Acceptance Criteria

```bash
npm install
npm run build
npm run lint
```

## 검증 절차

1. AC 명령어 실행
2. 체크리스트 확인:
   - docs/ARCHITECTURE.md 구조를 따르는가?
   - docs/ADR.md 기술 스택을 벗어나지 않았는가?
   - docs/RULES.md CRITICAL 규칙을 위반하지 않았는가?
3. index.json 업데이트:
   - 성공 → `"status": "completed"`, `"summary": "Next.js 15 프로젝트 초기 설정 완료"`
   - 실패 → `"status": "error"`, `"error_message": "구체적 에러 내용"`

## 금지사항

- 추가 라이브러리 설치 금지. 이유: ADR.md에 명시된 것만 사용
- 디렉토리 구조 임의 변경 금지. 이유: ARCHITECTURE.md 준수
```

---

## 4. Step 실행

사용자가 각 step을 요청하면:

### 4-1. Step 파일 읽기
```
phases/0-mvp/step0.md를 읽습니다
```

### 4-2. 작업 수행
- 파일 생성/수정
- 코드 작성
- 테스트 실행

### 4-3. index.json 업데이트

**완료 시**:
```json
{
  "step": 0,
  "name": "project-setup",
  "status": "completed",
  "summary": "Next.js 15 프로젝트 초기 설정 완료",
  "completed_at": "2024-01-15T10:30:00+09:00"
}
```

**에러 시**:
```json
{
  "step": 0,
  "name": "project-setup",
  "status": "error",
  "error_message": "TypeScript 컴파일 에러: ...",
  "failed_at": "2024-01-15T10:30:00+09:00"
}
```

**차단 시** (사용자 개입 필요):
```json
{
  "step": 0,
  "name": "project-setup",
  "status": "blocked",
  "blocked_reason": "DATABASE_URL 환경 변수 필요",
  "blocked_at": "2024-01-15T10:30:00+09:00"
}
```

### 4-4. 다음 step 안내
```
Step 0이 완료되었습니다.
다음은 Step 1: core-types입니다.
계속 진행할까요?
```

---

## 에러 복구

에러 발생 시:

1. **에러 분석**: error_message 확인
2. **수정**: 코드 수정 또는 설정 변경
3. **상태 초기화**: status를 "pending"으로 변경
4. **재시도**: step 파일을 다시 읽고 작업 수행

---

## 체크리스트

매 step 완료 시 확인:

- [ ] AC 명령어가 모두 통과하는가?
- [ ] docs/ARCHITECTURE.md 구조를 따르는가?
- [ ] docs/ADR.md 기술 스택을 벗어나지 않았는가?
- [ ] docs/RULES.md CRITICAL 규칙을 위반하지 않았는가?
- [ ] index.json이 업데이트되었는가?
- [ ] summary가 명확하게 작성되었는가?

---

## 💡 팁

### 좋은 Summary 작성

✅ 좋은 예:
```
"User, Post, Comment 타입 정의 완료 (src/types/models.ts)"
```

❌ 나쁜 예:
```
"타입 정의 완료"
```

### Step 크기 조절

- Step이 너무 크면: 2개로 나누기
- Step이 너무 작으면: 합치기
- 목표: 30분~2시간 내 완료 가능한 크기