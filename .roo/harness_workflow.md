# Bob 하네스 워크플로우

> **Bob에게**: 이 문서는 하네스 프레임워크 사용법을 설명합니다. 사용자가 phase 계획 작성을 요청하면 이 워크플로우를 따라주세요.

---

## 워크플로우 개요

```
1. 탐색 → 2. 논의 → 3. Step 설계 → 4. 파일 생성 → 5. 순차 실행
```

---

## 1. 탐색 (Exploration)

사용자가 phase 계획 작성을 요청하면, 먼저 다음 파일들을 읽으세요:

```
- /roo_rules.md          # 프로젝트 규칙
- /docs/PRD.md           # 무엇을 만들 것인지
- /docs/ARCHITECTURE.md  # 어떻게 구조화할 것인지
- /docs/ADR.md           # 어떤 기술을 쓸 것인지
```

**목표**: 프로젝트의 목표, 아키텍처, 기술 스택을 완전히 이해하기

---

## 2. 논의 (Discussion)

불명확한 사항이 있으면 사용자에게 질문하세요:

```
예시:
- "API 인증 방식은 JWT를 사용할까요, 아니면 Session을 사용할까요?"
- "데이터베이스는 어떤 것을 사용할 예정인가요?"
- "UI 라이브러리는 shadcn/ui를 사용할까요?"
```

---

## 3. Step 설계 (Step Design)

Phase를 여러 step으로 나누세요. 각 step은 **하나의 작은 작업**만 수행합니다.

### 설계 원칙

1. **Scope 최소화**
   - ❌ 나쁜 예: "API + UI + 테스트 구현"
   - ✅ 좋은 예: "API 구현" → "UI 구현" → "테스트 작성" (3개 step)

2. **자기완결성**
   - 각 step 파일에 필요한 모든 정보를 포함
   - "이전에 논의한 대로" 같은 외부 참조 금지

3. **읽어야 할 파일 명시**
   ```markdown
   ## 읽어야 할 파일
   - /docs/ARCHITECTURE.md
   - /src/types/user.ts (이전 step에서 생성)
   ```

4. **실행 가능한 AC**
   - ❌ 나쁜 예: "API가 잘 동작해야 한다"
   - ✅ 좋은 예: `npm run build && npm test`

5. **구체적인 금지사항**
   - ❌ 나쁜 예: "조심해서 구현하라"
   - ✅ 좋은 예: "클라이언트에서 직접 DB 접근 금지. 이유: 보안"

### Step 네이밍

kebab-case slug 사용:
- ✅ `project-setup`, `api-layer`, `auth-flow`
- ❌ `Step 1`, `구현`, `API 만들기`

---

## 4. 파일 생성 (File Creation)

사용자가 계획을 승인하면 다음 파일들을 생성하세요:

### 4-1. `phases/index.json`

```json
{
  "phases": [
    { "dir": "0-mvp", "status": "pending" }
  ]
}
```

### 4-2. `phases/{phase-name}/index.json`

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

### 4-3. `phases/{phase-name}/step{N}.md`

각 step마다 하나의 파일을 생성하세요:

```markdown
# Step 0: project-setup

## 읽어야 할 파일
- /docs/ARCHITECTURE.md
- /docs/ADR.md
- /roo_rules.md

## 작업

Next.js 15 프로젝트를 초기 설정합니다.

### 생성할 파일
- `package.json`: 의존성 정의
- `tsconfig.json`: TypeScript 설정
- `next.config.js`: Next.js 설정
- `.env.example`: 환경 변수 템플릿

### 구현 요구사항
1. Next.js 15 + TypeScript strict mode
2. Tailwind CSS 설정
3. ESLint + Prettier 설정
4. 기본 디렉토리 구조 생성:
   ```
   src/
   ├── app/
   ├── components/
   ├── types/
   └── lib/
   ```

## Acceptance Criteria

```bash
npm install
npm run build
npm run lint
```

모든 명령어가 에러 없이 실행되어야 합니다.

## 검증 절차

1. 위 AC 명령어 실행
2. 아키텍처 체크:
   - ARCHITECTURE.md의 디렉토리 구조를 따르는가?
   - ADR.md의 기술 스택을 사용하는가?
3. `phases/0-mvp/index.json` 업데이트:
   - 성공 → `"status": "completed"`, `"summary": "Next.js 15 프로젝트 초기 설정 완료"`
   - 실패 → `"status": "error"`, `"error_message": "구체적 에러 내용"`

## 금지사항

- 추가 라이브러리 설치 금지 (ADR.md에 명시된 것만)
- 디렉토리 구조 임의 변경 금지
- 환경 변수를 .env 파일에 직접 작성 금지 (.env.example만 생성)
```

---

## 5. 순차 실행 (Sequential Execution)

사용자가 각 step을 요청하면:

1. **해당 step 파일 읽기**
   ```
   phases/0-mvp/step0.md를 읽습니다
   ```

2. **작업 수행**
   - 파일 생성/수정
   - 코드 작성
   - 테스트 실행

3. **index.json 업데이트**
   ```json
   {
     "step": 0,
     "name": "project-setup",
     "status": "completed",
     "summary": "Next.js 15 프로젝트 초기 설정 완료",
     "completed_at": "2024-01-15T10:30:00+09:00"
   }
   ```

4. **다음 step 안내**
   ```
   Step 0이 완료되었습니다.
   다음은 Step 1: core-types입니다.
   계속 진행할까요?
   ```

---

## 상태 관리

각 step은 다음 상태 중 하나를 가집니다:

| 상태 | 의미 | 기록 필드 |
|------|------|----------|
| `pending` | 대기 중 | - |
| `completed` | 완료 | `completed_at`, `summary` |
| `error` | 실패 | `failed_at`, `error_message` |
| `blocked` | 차단됨 | `blocked_at`, `blocked_reason` |

### 상태 전이 예제

**완료 시**:
```json
{
  "step": 1,
  "name": "core-types",
  "status": "completed",
  "summary": "User, Post 타입 정의 완료 (src/types/)",
  "completed_at": "2024-01-15T11:00:00+09:00"
}
```

**에러 시**:
```json
{
  "step": 2,
  "name": "api-layer",
  "status": "error",
  "error_message": "TypeScript 컴파일 에러: User 타입을 찾을 수 없음",
  "failed_at": "2024-01-15T11:30:00+09:00"
}
```

**차단 시** (사용자 개입 필요):
```json
{
  "step": 3,
  "name": "database-setup",
  "status": "blocked",
  "blocked_reason": "DATABASE_URL 환경 변수가 설정되지 않음. .env.local 파일에 추가 필요",
  "blocked_at": "2024-01-15T12:00:00+09:00"
}
```

---

## 에러 복구

에러가 발생하면:

1. **에러 분석**
   ```
   error_message를 읽고 원인 파악
   ```

2. **수정**
   ```
   코드 수정 또는 설정 변경
   ```

3. **상태 초기화**
   ```json
   {
     "step": 2,
     "name": "api-layer",
     "status": "pending"
   }
   ```
   (`error_message`와 `failed_at` 필드 삭제)

4. **재시도**
   ```
   step 파일을 다시 읽고 작업 수행
   ```

---

## 실전 팁

### 좋은 Step 작성

✅ **DO**:
- 한 step에서 한 가지만 하기
- 실행 가능한 AC 작성
- 구체적인 금지사항 명시
- 이전 step에서 생성된 파일 경로 명시

❌ **DON'T**:
- 여러 모듈을 한 step에서 수정
- 추상적인 AC ("잘 동작해야 한다")
- 모호한 지시 ("적절히 구현하라")
- 외부 컨텍스트 참조 ("이전에 말한 대로")

### Summary 작성 요령

다음 step에 유용한 정보를 담으세요:

✅ 좋은 예:
```
"User, Post, Comment 타입 정의 완료 (src/types/models.ts)"
```

❌ 나쁜 예:
```
"타입 정의 완료"
```

---

## 체크리스트

매 step 완료 시 확인:

- [ ] AC 명령어가 모두 통과하는가?
- [ ] ARCHITECTURE.md 구조를 따르는가?
- [ ] ADR.md 기술 스택을 벗어나지 않았는가?
- [ ] roo_rules.md CRITICAL 규칙을 위반하지 않았는가?
- [ ] index.json이 업데이트되었는가?
- [ ] summary가 명확하게 작성되었는가?