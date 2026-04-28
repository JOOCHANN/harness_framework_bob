# ⚡ 5분 빠른 시작

Bob을 처음 사용하시나요? 이 가이드를 따라하세요!

## 1단계: 프로젝트 정보 입력 (2분)

### roo_rules.md 수정

```markdown
# 프로젝트: 내 프로젝트 이름

## 기술 스택
- Next.js 15
- TypeScript strict mode
- Tailwind CSS

## 아키텍처 규칙
- CRITICAL: 모든 API 로직은 app/api/에서만 처리
- CRITICAL: 클라이언트에서 직접 외부 API 호출 금지
```

### docs/PRD.md 수정

```markdown
# PRD: 내 프로젝트

## 목표
간단한 블로그 플랫폼 만들기

## 사용자
개인 블로거

## 핵심 기능
1. 글 작성/수정/삭제
2. 마크다운 지원
3. 태그 기능
```

### docs/ARCHITECTURE.md & ADR.md

기본 템플릿 그대로 사용하거나 간단히 수정

---

## 2단계: Bob에게 요청 (1분)

VSCode에서 Bob(Roo-Cline)을 열고 **복사 & 붙여넣기**:

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

---

## 3단계: Bob과 개발 시작 (2분)

Bob이 phase 파일들을 생성하면:

```
Bob, phases/0-mvp/step0.md를 읽고 작업을 시작해줘.
완료되면 phases/0-mvp/index.json의 해당 step status를 업데이트해줘.
```

각 step이 완료되면:

```
좋아! 다음 step을 진행해줘.
```

---

## 완료! 🎉

이제 Bob과 함께 체계적으로 개발할 수 있습니다.

### 다음 단계

- [README.md](README.md) - 전체 가이드
- [.roo/harness_workflow.md](.roo/harness_workflow.md) - 상세 워크플로우
- [roo_rules.md](roo_rules.md) - 프로젝트 규칙 예제

### 문제 해결

**Q: Bob이 파일을 못 찾아요**
```
Bob, 현재 디렉토리의 파일 목록을 보여줘.
```

**Q: Step이 실패했어요**
```
Bob, phases/0-mvp/index.json을 확인하고 
error_message를 보여줘. 그리고 수정해줘.
```

**Q: 처음부터 다시 시작하고 싶어요**
```
Bob, phases/ 폴더를 삭제하고 
다시 phase 계획을 작성해줘.