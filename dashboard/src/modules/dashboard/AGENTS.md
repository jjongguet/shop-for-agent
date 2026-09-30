# dashboard 모듈 — 4봉투 열람(Supabase 연결 전까지 휴면)

레포 자기 Supabase 공통 봉투 4종(status·ledger·workflow_runs·workflow_steps)을 읽어
대시보드 서버 렌더에 제공한다(f5 — 자기 Supabase만). **현재는 Supabase 미연결
(보류 — Addendum 1)** — page는 이 리더를 호출하지 않고 로컬 메타를 표시한다.
연결 시 factory 본보기처럼 `readAll()` 서버 렌더로 전환한다.

- 읽기 구현은 `@/adapters/envelopes`(PostgREST fetch, server 전용, N14 — postgres 라이브러리 불요) 경유.
- fail-loud: 자격 미설정 시 조용한 빈값 대신 예외 — 대시보드에 envelope error로 표시.
- 밖으로 노출하는 계약은 `public.ts`뿐이다.
