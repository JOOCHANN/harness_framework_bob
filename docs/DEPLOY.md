# 배포 / 릴리즈

> 배포 파이프라인, 환경 설정, 릴리즈 관련 step 작업 시 참조.

## 환경
| 환경 | URL | 자동 배포? |
|------|-----|------------|
| dev | {예: dev.example.com} | main 푸시 |
| prod | {예: example.com} | tag 푸시 (수동 승인) |

## 배포 플랫폼
- {예: Vercel / AWS / GCP / Kubernetes}

## CI/CD
파일 위치: {예: `.github/workflows/deploy.yml`}

단계:
1. lint + typecheck
2. test (TESTING.md 참조)
3. build
4. deploy
5. smoke test (헬스체크)

## 시크릿 / 환경 변수
- 로컬: `.env.local` (커밋 금지)
- 운영: {예: GitHub Actions Secrets}
- 시크릿 값을 코드/주석/로그/에러 메시지에 노출하지 말 것 (이 규칙을 프로젝트 차원에서 강제하려면 `docs/RULES.md` 에 CRITICAL 로 추가)

## 릴리즈 명령어
```bash
{예시}
git tag v1.0.0 -m "Release 1.0.0"
git push origin v1.0.0
```

## 롤백
1. 이전 태그 확인: `git tag --sort=-creatordate | head`
2. 롤백: {예: `kubectl rollout undo deployment/app`}
3. DB 마이그레이션이 함께였다면 down 마이그레이션

## Bob 에게 주는 가이드
- 배포 step 의 AC 에 헬스체크 명령어 포함 (`curl -f https://.../healthz`)
- 환경별 설정은 코드 분기 ❌, 환경 변수 + config 분리
