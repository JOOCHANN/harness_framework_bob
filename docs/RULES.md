# 프로젝트: {프로젝트명}

## 기술 스택
- {프레임워크 (예: Next.js 15)}
- {언어 (예: TypeScript strict mode)}
- {스타일링 (예: Tailwind CSS)}

## 아키텍처 규칙
- CRITICAL: {절대 지켜야 할 규칙 1 (예: 모든 API 로직은 app/api/에서만 처리)}
- CRITICAL: {절대 지켜야 할 규칙 2 (예: 클라이언트에서 직접 외부 API 호출 금지)}
- {일반 규칙}

## 개발 프로세스
- CRITICAL: TDD 원칙 준수 (테스트 먼저 작성)
- 커밋 메시지는 conventional commits 형식 (feat:, fix:, docs:)

## 명령어
```bash
npm run dev      # 개발 서버
npm run build    # 빌드
npm run test     # 테스트
```

---

## 📝 작성 예시 (참고용)

### 기술 스택
```
- Next.js 15 (App Router)
- TypeScript strict mode
- Tailwind CSS
- Prisma + PostgreSQL
```

### 핵심 규칙
```
- CRITICAL: 모든 API 로직은 app/api/에서만 처리
- CRITICAL: 클라이언트에서 직접 외부 API 호출 금지
- CRITICAL: 환경 변수는 .env.local에만 (커밋 금지)
- Server Components 기본, 인터랙션 필요 시만 Client Component
```

### 개발 프로세스
```
- TDD: 테스트 먼저 작성
- 커밋: feat:, fix:, docs:, refactor:
- PR 전 빌드 + 테스트 통과 필수