# 테스트 전략

> 테스트 관련 step 의 AC 를 작성하거나 새 모듈에 테스트를 추가할 때 참조.

## 도구
- 단위 / 통합: {예: Vitest + Testing Library}
- E2E: {예: Playwright}

## 무엇을 테스트하는가
- **반드시**: 비즈니스 로직, API 입출력, 버그 픽스(회귀 방지)
- **굳이 안 해도**: 단순 UI 마크업, 외부 라이브러리 동작 자체

## 커버리지 목표
| 영역 | 목표 |
|------|------|
| `src/lib/` (순수 함수) | 100% |
| `src/app/api/` | 90% |
| 전체 | ≥ 80% |

## 명령어
```bash
npm test              # 단위 + 통합
npm run test:e2e      # E2E
npm run test:coverage # 커버리지 리포트
```

## Bob 에게 주는 가이드
- step 의 AC 에는 **실행 가능한 테스트 명령어** 포함 (`npm test -- src/foo.test.ts`)
- 새 모듈을 만들 때 같은 step 에서 테스트 파일도 함께 생성
- 기존 모듈 수정 시 기존 테스트가 깨지지 않는지 검증
