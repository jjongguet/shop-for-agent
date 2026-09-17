# shop-for-agent — 에이전트 맵

**제품은 `SKILL.md` 파일 1개다.** 에이전트가 raw URL로 내려받는 스킬 정의가 전부.

> **포지션 (2026-09-17 판정): shopscan의 배포 채널.**
> 설치 raw URL(`raw.githubusercontent.com/jjongguet/shop-for-agent/main/SKILL.md`)의 안정성이 이 레포를 shopscan과 별도로 존재시키는 이유다. 흡수·삭제하면 기존 설치 기반이 깨진다.

- 배포 = 커밋 푸시. 설치 URL이 곧 배포 채널.
- `README.md` — 사람용 설명 (API·지원 플랫폼·공시)
- 백엔드(shopforagent.shop 서버)는 이 레포가 아니라 **shopscan 레포**가 운영한다. API 스펙이 궁금하면 `../shopscan/AGENTS.md` 참조.

## 규칙

- SKILL.md의 엔드포인트·플랫폼 목록은 shopscan 라이브 상태와 일치해야 한다 — 한쪽을 바꾸면 양쪽 확인.
- 파일 3개짜리 레포: 여기에 추가 문서를 만들지 않는다.
