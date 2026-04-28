# 프로젝트: {프로젝트명}

## 기술 스택
- {프레임워크 (예: Next.js 15)}
- {언어 (예: TypeScript strict mode)}
- {스타일링 (예: Tailwind CSS)}

## 아키텍처 규칙
- CRITICAL: {절대 지켜야 할 규칙 1 (예: 모든 API 로직은 app/api/ 라우트 핸들러에서만 처리)}
- CRITICAL: {절대 지켜야 할 규칙 2 (예: 클라이언트 컴포넌트에서 직접 외부 API를 호출하지 말 것)}
- {일반 규칙 (예: 컴포넌트는 components/ 폴더에, 타입은 types/ 폴더에 분리)}

## 개발 프로세스
- CRITICAL: 새 기능 구현 시 반드시 테스트를 먼저 작성하고, 테스트가 통과하는 구현을 작성할 것 (TDD)
- 커밋 메시지는 conventional commits 형식을 따를 것 (feat:, fix:, docs:, refactor:)

## Bob 작업 지침
- 모든 작업은 `/docs/` 하위 문서(PRD, ARCHITECTURE, ADR 등)를 먼저 읽고 시작할 것
- 변경 전 반드시 관련 파일들을 읽고 기존 코드 스타일과 패턴을 파악할 것
- 각 step은 독립적으로 실행 가능해야 하며, 이전 대화 컨텍스트에 의존하지 말 것
- AC(Acceptance Criteria)는 실행 가능한 명령어로 작성할 것

## 명령어
npm run dev      # 개발 서버
npm run build    # 프로덕션 빌드
npm run lint     # ESLint
npm run test     # 테스트

---

## 📝 작성 가이드 (Bob이 읽지 않음 - 사용자용)

### 기술 스택 예제
```
- Next.js 15 (App Router)
- TypeScript strict mode
- Tailwind CSS
- Prisma + PostgreSQL
- Jest + Playwright
```

### 아키텍처 규칙 예제
```
- CRITICAL: 모든 API 로직은 app/api/ 라우트 핸들러에서만 처리
- CRITICAL: 클라이언트 컴포넌트에서 직접 외부 API 호출 금지
- CRITICAL: 환경 변수는 .env.local에만 저장 (절대 커밋하지 않음)
- 컴포넌트는 components/ 폴더에, 타입은 types/ 폴더에 분리
- Server Components를 기본으로, 인터랙션 필요 시에만 Client Component 사용
```

### 개발 프로세스 예제
```
- CRITICAL: TDD - 테스트 먼저 작성 후 구현
- 커밋 메시지: feat:, fix:, docs:, refactor:, test:, chore:
- PR 전 반드시 npm run build && npm test 통과 확인
- 코드 리뷰 필수 (최소 1명)
```

### 명령어 예제
```
npm run dev          # 개발 서버 (localhost:3000)
npm run build        # 프로덕션 빌드
npm run lint         # ESLint 검사
npm run test         # Jest 단위 테스트
npm run test:e2e     # Playwright E2E 테스트
npm run db:migrate   # Prisma 마이그레이션