# AGENTS.md — Bob 진입점 (라우터)

> Bob/Roo-Cline 에게: 이 파일은 항상 로드되는 최소 컨텍스트입니다.
> 상세 가이드는 `docs/*.md` 에 있고, **작업이 시작될 때만** 필요한 파일을 읽습니다.

---

## 0. 이 프로젝트는 무엇을 하는가

Bob 과 함께 SDLC (계획 → 설계 → 구현 → 테스트 → 배포) 를 단계별로 진행합니다.

- **입력**: `docs/*.md` (사람이 작성)
- **출력**: `phases/{N}-{name}/` (Bob 이 생성·갱신)

작업 단위는 **Phase → Step**:
- Phase = 큰 기능 묶음 (`0-mvp`, `1-auth`)
- Step = 30분 ~ 2시간짜리 단일 작업 (`project-setup`, `core-types`)

---

## 1. 작업별 참조 (lazy load)

| 작업 | 먼저 읽을 파일 |
|------|----------------|
| Phase / Step **설계** | `docs/WORKFLOW.md`, `docs/PRD.md`, `docs/ARCHITECTURE.md`, `docs/ADR.md`, `docs/RULES.md` |
| Step **실행** | `phases/{N}-{name}/step{N}.md`, `docs/RULES.md` |
| **테스트** 작성 | `docs/TESTING.md` |
| **UI** 작업 (있을 때) | `docs/UI_GUIDE.md` |
| **배포 / 릴리즈** | `docs/DEPLOY.md` |

표에 없으면 읽지 않는다.

---

## 2. 항상 지킬 4가지

1. **`docs/RULES.md` 의 CRITICAL 규칙** — 위반 시 step 완료 불가
2. **자기완결적 step 파일** — "이전에 논의한 대로" 같은 외부 참조 금지
3. **AC 는 실행 가능한 명령어** — `npm test`, `pytest`, `curl ...`
4. **`index.json` 상태 갱신** — 시작/종료 모두

---

## 3. 상태 머신 (index.json)

```
pending → in_progress → completed
                     ↘ error → (재시도) → in_progress
                     ↘ blocked
```

| status | 추가 필드 |
|--------|-----------|
| `pending` | (없음) |
| `in_progress` | `started_at` |
| `completed` | `summary`, `completed_at` |
| `error` | `error_message`, `failed_at` |
| `blocked` | `blocked_reason`, `blocked_at` |

- 모든 timestamp 는 ISO 8601 + 타임존
- `error` 가 3회 이상 반복되면 `blocked` 로 전환 후 사용자에게 보고

---

## 4. 사용자 요청 → 1차 행동

| 사용자 요청 | Bob 의 첫 행동 |
|-------------|----------------|
| "phase 계획 작성해줘" | §1 의 *Phase 설계* 행을 따라 파일 읽기 → 질문 정리 → phase 생성 |
| "step{N} 진행해줘" | 해당 step 파일 + 그 안의 *읽어야 할 파일* 모두 읽고 시작 |
| "PRD 변경됐어" | `docs/PRD.md` 다시 읽고 미완료 step 영향 분석 |
| "step{N} 가 실패했어" | `error_message` 분석 → 수정 → 재시도 |

자세한 절차는 [docs/WORKFLOW.md](docs/WORKFLOW.md).
