# Bob 하네스 프레임워크

Bob(Roo-Cline)과 함께 프로젝트를 체계적으로 개발하는 프레임워크입니다.

> **Bob에게**: 먼저 [AGENTS.md](AGENTS.md)를 읽어주세요.

## 🎯 핵심 아이디어

프로젝트를 **Phase**(큰 기능 단위) → **Step**(작은 작업)으로 나누고, Bob이 각 step을 순차적으로 실행합니다.

## ⚡ 1분 시작

```
1. docs/ 파일들을 프로젝트에 맞게 작성
2. Bob에게 요청: "docs/ 읽고 phases/0-mvp/ 계획 작성해줘"
3. Bob에게 요청: "phases/0-mvp/step0.md 읽고 작업해줘"
```

끝! 이후 step1, step2... 순서대로 진행하면 됩니다.

## 📁 구조

```
.
├── docs/
│   ├── PRD.md              # 무엇을 만들지
│   ├── ARCHITECTURE.md     # 어떻게 만들지
│   ├── ADR.md              # 어떤 기술 쓸지
│   ├── UI_GUIDE.md         # 디자인 가이드
│   ├── RULES.md            # 프로젝트 규칙
│   └── WORKFLOW.md         # Bob 워크플로우 가이드
├── phases/                 # Bob이 생성
│   └── 0-mvp/
│       ├── index.json      # step 목록 + 상태
│       ├── step0.md        # 각 step 작업 내용
│       ├── step1.md
│       └── ...
└── AGENTS.md               # Bob용 핵심 규칙
```

## 💬 Bob과 대화하기

### Phase 계획 만들기
```
Bob, 다음 파일들을 읽어줘:
- AGENTS.md
- docs/RULES.md
- docs/PRD.md
- docs/ARCHITECTURE.md
- docs/ADR.md
- docs/WORKFLOW.md

그리고 MVP를 위한 phase 계획을 phases/0-mvp/에 작성해줘.
```

### Step 실행하기
```
Bob, phases/0-mvp/step0.md를 읽고 작업해줘.
완료되면 phases/0-mvp/index.json의 status를 업데이트해줘.
```

### 에러 발생 시
```
Bob, step 2가 실패했어. 
phases/0-mvp/index.json 확인하고 error_message 보고 수정해줘.
```

## 🎓 핵심 규칙

1. **한 번에 하나씩**: Step을 순서대로 진행
2. **상태 업데이트**: 각 step 완료 시 index.json 업데이트 필수
3. **문서 우선**: docs/ 파일들을 먼저 작성하고 시작
4. **Bob에게 맡기기**: Phase 나누기, Step 설계는 Bob이 더 잘함

## 📚 더 알아보기

- [Bob 워크플로우 가이드](docs/WORKFLOW.md) - Bob이 읽을 상세 가이드
- [빠른 시작 가이드](QUICKSTART.md) - 더 자세한 튜토리얼

## ❓ FAQ

**Q: Phase는 몇 개로 나눠야 하나요?**  
A: Bob에게 물어보세요. PRD 읽고 적절히 나눠줍니다.

**Q: Step이 너무 크면?**  
A: Bob에게 "이 step을 2개로 나눠줘"라고 요청하세요.

**Q: 중간에 요구사항이 바뀌면?**
A: docs/PRD.md 수정하고 Bob에게 "docs/PRD.md 변경됐어, phase 계획 다시 검토해줘"

**Q: 여러 Phase 동시 진행?**  
A: 가능하지만 비추천. 한 번에 하나씩이 명확합니다.