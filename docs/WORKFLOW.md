# WORKFLOW — Phase 설계 + Step 실행 절차

> Bob 에게: [AGENTS.md](../AGENTS.md) §1 표가 *Phase 설계* 또는 *Step 실행* 으로 가리키면 이 파일을 읽으세요.

---

## 절차

```
1. 문서 읽기  →  2. Phase 설계  →  3. 파일 생성  →  4. Step 실행  →  5. Phase 종료
```

---

## 1. 문서 읽기

사용자가 *"phase 계획 작성해줘"* 라고 하면 다음을 읽는다:
- `docs/PRD.md`, `docs/ARCHITECTURE.md`, `docs/ADR.md`, `docs/RULES.md`

불명확한 부분이 있으면 phase 설계 전에 사용자에게 질문한다.

---

## 2. Phase 설계 원칙

### Step 분할
**DO ✅**
- 한 step = 하나의 작은 작업 (30분 ~ 2시간)
- 각 step 파일은 자기완결성 (필요한 모든 정보 포함)
- `## 읽어야 할 파일` 섹션 필수
- AC 는 실행 가능한 명령어
- 금지사항은 `"X 금지. 이유: Y"` 형식
- 네이밍은 kebab-case (`project-setup`, `api-layer`)

**DON'T ❌**
- "이전에 논의한 대로" 같은 외부 참조
- "조심해서 구현" 같은 모호한 지시
- 한 step 에 여러 작업 몰아넣기
- `Step 1`, `구현`, `API 만들기` 같은 비-kebab-case

### 의존성
- 직전 step 에만 의존 → `depends_on` 생략 가능
- 비-인접 또는 다중 의존 → `depends_on: [2, 3]` 명시

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

### 3-2. `phases/{N}-{name}/index.json`
```json
{
  "project": "MyProject",
  "phase": "0-mvp",
  "steps": [
    { "step": 0, "name": "project-setup", "status": "pending" },
    { "step": 1, "name": "core-types",    "status": "pending", "depends_on": [0] },
    { "step": 2, "name": "api-layer",     "status": "pending", "depends_on": [1] }
  ]
}
```

### 3-3. `phases/{N}-{name}/step{N}.md` 템플릿
```markdown
# Step {N}: {kebab-case-name}

## 읽어야 할 파일
- docs/ARCHITECTURE.md
- docs/RULES.md
- (의존 step 파일이 있으면)

## 작업
{무엇을 할지 한 문장 + 부연}

### 생성/수정할 파일
- `path/to/file`: 설명

### 구현 요구사항
1. 구체적 요구사항 1
2. 구체적 요구사항 2

## Acceptance Criteria
\`\`\`bash
{실행 가능한 명령어들}
\`\`\`

## 검증 절차
1. AC 명령어 실행 → 모두 통과
2. docs/RULES.md CRITICAL 위반 없음 확인
3. index.json 갱신

## 금지사항
- {구체 금지}. 이유: {왜}
```

---

## 4. Step 실행

### 4-1. 시작 표시
```json
{ "status": "in_progress", "started_at": "2026-04-28T10:30:00+09:00" }
```

### 4-2. step 파일 읽기
step 파일 + 그 안의 *읽어야 할 파일* 섹션 모두.

### 4-3. 작업 수행
파일 생성/수정 → 코드 작성 → AC 명령어로 검증.

### 4-4. 종료 상태 (셋 중 하나)

**완료**
```json
{
  "status": "completed",
  "summary": "Next.js 15 초기 설정 (package.json, tsconfig.json, src/app/layout.tsx)",
  "completed_at": "2026-04-28T11:15:00+09:00"
}
```

**에러** (자체 재시도)
```json
{
  "status": "error",
  "error_message": "TypeScript 컴파일 에러: Cannot find module '@/lib/db'",
  "failed_at": "2026-04-28T11:20:00+09:00"
}
```

**차단** (사용자 개입 필요)
```json
{
  "status": "blocked",
  "blocked_reason": "DATABASE_URL 환경 변수가 .env.local 에 없음",
  "blocked_at": "2026-04-28T11:25:00+09:00"
}
```

### 4-5. 다음 step 안내
*"Step 0 완료. 다음은 Step 1: core-types 입니다. 진행할까요?"*

---

## 5. 에러 복구

`status: error` 인 step 을 다시 요청받으면:
1. `error_message` 분석
2. 코드/설정 수정
3. status 를 `pending` → `in_progress` 로 갱신 (started_at 새로)
4. step 파일 다시 읽고 재수행

**3회 연속 실패 시 `blocked` 로 전환** + 사용자에게 사유 보고.

---

## 6. Phase 종료

phase 의 모든 step 이 `completed` 면:
1. `phases/index.json` 의 phase status 를 `completed` 로
2. 사용자에게 요약 보고 (완료 step 목록 + 주요 파일 + 다음 phase 제안)

---

## 체크리스트

### Phase 계획 시
- [ ] 필수 docs 를 모두 읽었는가?
- [ ] 각 step 이 30분 ~ 2시간 크기인가?
- [ ] 모든 step 이름이 kebab-case 인가?
- [ ] 모든 step 에 실행 가능한 AC 가 있는가?

### Step 실행 시
- [ ] step 파일과 읽어야 할 파일을 모두 읽었는가?
- [ ] 시작 시 `in_progress` + `started_at` 갱신했는가?
- [ ] AC 가 모두 통과했는가?
- [ ] CRITICAL 규칙을 지켰는가?
- [ ] 종료 상태로 갱신 + summary 가 구체적인가?
