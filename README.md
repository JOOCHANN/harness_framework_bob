# Bob 하네스 프레임워크

IBM Code Assistant **Bob (Roo-Cline)** 과 함께 SDLC (계획 → 설계 → 구현 → 테스트 → 배포) 를 단계별로 진행하기 위한 초기 사용자용 자산입니다.

> **Bob 에게**: 먼저 [AGENTS.md](AGENTS.md) 만 읽으세요. 다른 파일은 작업이 시작될 때 그때 읽습니다.

---

## 동작 원리

```
[항상 로드]    AGENTS.md          ← 작업 → 파일 라우팅 표
                  │
[필요할 때만]  ├─ "Phase 설계"  → docs/WORKFLOW.md, PRD, ARCHITECTURE, ADR
               ├─ "step 진행"   → step{N}.md, RULES.md
               ├─ "테스트"      → docs/TESTING.md
               ├─ "배포"        → docs/DEPLOY.md
               └─ "UI"          → docs/UI_GUIDE.md
```

AGENTS.md 한 파일에 모든 컨텍스트를 넣으면 토큰을 낭비합니다. 작업 종류에 따라 필요한 docs/ 만 lazy-load 하는 구조입니다.

---

## 핵심 컨셉

프로젝트를 **Phase**(큰 기능 단위) → **Step**(30분~2시간 작업) 으로 나누고, Bob 이 step 을 순서대로 실행하며 `phases/{N}-{name}/index.json` 에 상태를 갱신합니다.

---

## 폴더 구조

```
.
├── AGENTS.md                # Bob 진입점 (라우터)
├── docs/
│   ├── PRD.md               # 무엇을 만들지
│   ├── ARCHITECTURE.md      # 어떻게 만들지
│   ├── ADR.md               # 어떤 기술/설계 결정
│   ├── RULES.md             # CRITICAL 규칙
│   ├── WORKFLOW.md          # Phase/Step 절차
│   ├── TESTING.md           # 테스트 전략
│   ├── DEPLOY.md            # 배포 / 릴리즈
│   └── UI_GUIDE.md          # (선택) UI 프로젝트만
└── phases/                  # Bob 이 생성 (gitignore 가능)
    └── {N}-{name}/
        ├── index.json
        └── step{N}.md
```

---

## 5줄 시작 가이드

```
1. docs/PRD.md, ARCHITECTURE.md, ADR.md, RULES.md 를 프로젝트에 맞게 작성
2. (해당 시) TESTING.md, DEPLOY.md 도 채움
3. Bob 에게: "AGENTS.md 읽고 phases/0-mvp/ 계획 작성해줘"
4. "phases/0-mvp/step0.md 진행해줘" → 완료되면 "다음 step" 반복
5. 에러 시: "phases/0-mvp/index.json 의 error_message 보고 수정해줘"
```
