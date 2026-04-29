# 프로젝트 규칙: {프로젝트명}

> `CRITICAL` 라벨이 붙은 규칙은 Bob 이 step 검증 시 반드시 확인합니다.

## 기술 스택
- {프레임워크}
- {언어}
- {스타일링}

## 아키텍처 규칙
- **CRITICAL**: {절대 지켜야 할 규칙 1 — 예: 모든 API 로직은 app/api/ 에서만}
- **CRITICAL**: {절대 지켜야 할 규칙 2 — 예: 클라이언트에서 직접 외부 API 호출 금지}
- **CRITICAL**: 환경 변수는 `.env.local` 에만 (커밋 금지)
- {일반 규칙}

## 개발 프로세스
- **CRITICAL**: 새 기능은 테스트 먼저 작성 (TDD)
- 커밋 메시지: conventional commits (`feat:`, `fix:`, `docs:`, `refactor:`, `test:`)
- PR 전 빌드 + 테스트 통과 필수

## 명령어
```bash
npm run dev      # 개발 서버
npm run build    # 빌드
npm test         # 테스트
npm run lint     # 린트
```
