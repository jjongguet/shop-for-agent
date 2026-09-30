# shop-for-agent — 에이전트 맵

**제품은 `SKILL.md` 파일 1개다.** 에이전트가 raw URL로 내려받는 스킬 정의가 전부.

> **포지션 (2026-09-17 판정): shopscan의 배포 채널.**
> 설치 raw URL(`raw.githubusercontent.com/jjongguet/shop-for-agent/main/SKILL.md`)의 안정성이 이 레포를 shopscan과 별도로 존재시키는 이유다. 흡수·삭제하면 기존 설치 기반이 깨진다.

- 배포 = 커밋 푸시. 설치 URL이 곧 배포 채널.
- `README.md` — 사람용 설명 (API·지원 플랫폼·공시)
- 백엔드(shopforagent.shop 서버)는 이 레포가 아니라 **shopscan 레포**가 운영한다. API 스펙이 궁금하면 `../shopscan/AGENTS.md` 참조.

## 규칙

- SKILL.md의 엔드포인트·플랫폼 목록은 shopscan 라이브 상태와 일치해야 한다 — 한쪽을 바꾸면 양쪽 확인.
- 제품 문서는 SKILL.md·README.md뿐 — 추가 산문 문서를 만들지 않는다. 표준 운영 파일(standard.json·steps.json·dashboard/)은 워크스페이스 표준 소속이므로 예외.

## 워크스페이스 표준 (v1.0.1)

- **프로필**: `standard.json` — profiles `[skill]`, roots `.`. 검증: `node ../workspace-standard/validator/cli.mjs validate --root .` → green 필수.
- **대시보드** (`dashboard/`): 로컬 운영 뷰 — 레포 태그·dashboard package.json 버전을 빌드 시점에 읽어 정적 표시. **Supabase 미연결(보류 — Addendum 1)** — 봉투 열람 영역은 미연결 표시, push-envelope 없음.
- **검증기 훅**: `dashboard/package.json` `vercel-build`가 `next build` 전에 검증기(`validate --root ..`)를 선결합 — green 아니면 빌드 실패.
- **steps.json 수동 절**: ①버전 기록(mac — 릴리스 태그 시점) ②검증(server — validator green) ③판정(human). 타이머 자동 실행 없음.
- 스킬 노출 API는 풀 검색·쿠팡 실검색 두 가지 — `/api/hotdeal/top`(급상승 핫딜) 절은 f29로 SKILL.md에서 제거됐다. 서버 측 엔드포인트 운영은 shopscan 몫.
