# Bob 하네스 프레임워크

Bob(Roo-Cline) 전용 SDLC 하네스 프레임워크입니다. 프로젝트를 작은 step으로 나누어 체계적으로 개발합니다.

## 🚀 빠른 시작 (5분)

### 1. 프로젝트 설정 (2분)

이 폴더를 VSCode로 열고, 다음 파일들을 프로젝트에 맞게 수정하세요:

```bash
# 1. roo_rules.md - 프로젝트 규칙 작성
# 2. docs/PRD.md - 무엇을 만들 것인지
# 3. docs/ARCHITECTURE.md - 어떻게 구조화할 것인지
# 4. docs/ADR.md - 어떤 기술을 쓸 것인지
```

### 2. Bob에게 Phase 설계 요청 (2분)

VSCode에서 Bob(Roo-Cline)을 열고 다음과 같이 요청하세요:

```
안녕 Bob! 이 프로젝트의 MVP를 구현하려고 해.

먼저 다음 파일들을 읽어줘:
- roo_rules.md
- docs/PRD.md
- docs/ARCHITECTURE.md
- docs/ADR.md

그리고 .roo/harness_workflow.md를 읽고 하네스 프레임워크 사용법을 파악해줘.

이제 MVP 구현을 위한 phase 계획을 작성해줘. 
phases/ 폴더에 필요한 파일들(index.json, step 파일들)을 생성해줘.
```

### 3. Bob과 함께 개발 시작 (1분)

Bob이 phase 파일들을 생성하면, 각 step을 순차적으로 진행하세요:

```
Bob, phases/0-mvp/step0.md를 읽고 작업을 시작해줘.
완료되면 phases/0-mvp/index.json의 해당 step status를 업데이트해줘.
```

## 📁 구조

```
.
├── .roo/
│   └── harness_workflow.md    # Bob이 읽을 워크플로우 가이드
├── docs/
│   ├── PRD.md                 # 제품 요구사항
│   ├── ARCHITECTURE.md        # 시스템 구조
│   └── ADR.md                 # 기술 결정
├── phases/                    # Bob이 생성할 phase 디렉토리
│   ├── index.json
│   └── {phase-name}/
│       ├── index.json
│       └── step{N}.md
├── roo_rules.md              # 프로젝트 규칙 (Bob이 항상 참조)
└── README.md                 # 이 파일
```

## 💡 핵심 개념

- **Phase**: 하나의 큰 기능 단위 (예: `0-mvp`, `1-auth`)
- **Step**: Phase를 구성하는 작은 작업 (예: `project-setup`, `api-layer`)
- **Bob의 역할**: 각 step을 읽고 → 작업 수행 → status 업데이트

## 🎯 실제 사용 예제

### 예제 1: 새 프로젝트 시작

```
나: Bob, Next.js 프로젝트를 시작하려고 해. 
    roo_rules.md와 docs/ 파일들을 읽고 MVP phase 계획을 작성해줘.

Bob: [파일들을 읽고 분석]
     MVP를 위해 5개 step으로 나눴습니다:
     - Step 0: project-setup
     - Step 1: core-types
     - Step 2: api-layer
     - Step 3: ui-components
     - Step 4: integration-tests
     
     phases/0-mvp/ 폴더에 파일들을 생성하겠습니다.

나: 좋아! Step 0부터 시작해줘.

Bob: [step0.md를 읽고 작업 수행]
     프로젝트 초기 설정을 완료했습니다.
     phases/0-mvp/index.json을 업데이트했습니다.
```

### 예제 2: Step 진행 중 에러 발생

```
나: Bob, step 2가 실패했어. phases/0-mvp/index.json을 확인해봐.

Bob: [index.json 확인]
     error_message를 확인했습니다. API 타입 정의가 누락되었네요.
     수정하고 다시 시도하겠습니다.

나: 좋아, status를 pending으로 바꾸고 다시 시작해줘.

Bob: [수정 후 재시도]
     완료했습니다. status를 completed로 업데이트했습니다.
```

## 🔧 Bob 작업 팁

### Bob에게 명확하게 요청하기

❌ 나쁜 예:
```
"MVP 만들어줘"
```

✅ 좋은 예:
```
"roo_rules.md와 docs/ 파일들을 먼저 읽어줘.
그리고 .roo/harness_workflow.md의 워크플로우를 따라서
phases/0-mvp/ 계획을 작성해줘."
```

### Step 진행 시

❌ 나쁜 예:
```
"다음 step 해줘"
```

✅ 좋은 예:
```
"phases/0-mvp/step1.md를 읽고 작업을 시작해줘.
완료되면 index.json의 status를 업데이트하고 summary를 작성해줘."
```

## 📚 더 알아보기

- [Bob 워크플로우 상세 가이드](.roo/harness_workflow.md)
- [프로젝트 규칙 템플릿](roo_rules.md)

## ❓ FAQ

**Q: execute.py는 언제 사용하나요?**
A: execute.py는 자동화를 위한 선택적 도구입니다. Bob과 직접 대화하는 것이 더 자연스럽고 권장됩니다.

**Q: Phase를 어떻게 나누나요?**
A: Bob에게 물어보세요! Bob이 PRD와 ARCHITECTURE를 읽고 적절히 나눠줍니다.

**Q: Step이 실패하면?**
A: Bob에게 에러를 보여주고 수정을 요청하세요. Bob이 자동으로 재시도합니다.

**Q: 여러 Phase를 동시에 진행할 수 있나요?**
A: 가능하지만 권장하지 않습니다. 한 번에 하나의 Phase에 집중하세요.