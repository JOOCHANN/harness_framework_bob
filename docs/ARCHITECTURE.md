# 아키텍처

## 디렉토리 구조
```
src/
├── app/              # 페이지 + API 라우트
├── components/       # UI 컴포넌트
├── types/            # 도메인 타입
├── lib/              # 유틸 / 헬퍼
└── services/         # 외부 API 래퍼
```

## 패턴
{사용하는 디자인 패턴 — 예: Server Components 기본, 인터랙션이 필요한 곳만 Client Component}

## 데이터 흐름
```
{데이터 흐름 — 예:
사용자 입력 → Client Component → API Route → 외부 API → 응답 → UI
}
```

## 상태 관리
{서버 상태 vs 클라이언트 상태 — 어디서 어떻게 관리하는지}

## 에러 / 로깅
- 사용자 향: {예: toast 알림 + 재시도}
- 시스템 향: {예: console.error + 외부 수집기}
