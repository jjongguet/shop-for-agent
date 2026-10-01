# shop-for-agent

에이전트·터미널에서 한국 커머스 상품을 검색하고 구매 링크를 받는 무료 스킬.

- **설치물 0** — API 키도, 가입도, 바이너리도 없다. 스킬 파일 하나가 전부다.
- 중앙 서버(shopforagent.shop)가 큐레이션된 제휴 링크를 반환한다.

## 설치 (Claude Code)

```sh
mkdir -p ~/.claude/skills/shop-for-agent
curl -fsSL -o ~/.claude/skills/shop-for-agent/SKILL.md \
  https://raw.githubusercontent.com/jjongguet/shop-for-agent/main/SKILL.md
```

다른 에이전트(GJC, Cursor 등)도 동일 — `SKILL.md`를 해당 스킬 디렉터리에 복사하면 끝.

설치 후 이렇게 써본다:

> 냉동 딸기 싼 데 찾아줘

## API

풀 검색(큐레이션된 제휴 링크)과 쿠팡 실검색(운영자 파트너스 키로 즉석 생성):

```
GET https://shopforagent.shop/api/pool/search?keywords=딸기
GET https://shopforagent.shop/api/coupang/search?keywords=무선청소기
```

```json
{
  "results": [
    {
      "platform": "toss",
      "product_name": "뉴뜨레 냉동 딸기, 국내산, 1kg, 3개",
      "affiliate_url": "https://toss.im/_m/7x8SVGn8",
      "added_at": "2026-09-16T00:09:39+09:00"
    }
  ]
}
```

- `keywords` 필수(쉼표 구분), `platform`·`limit` 선택
- 쿠팡 엔드포인트는 키 등록 전 `503 coupang_not_configured`로 대기 응답 — 응답에 `disclosure`(파트너스 수수료 고지) 포함
- 결과 없으면 `"results": []` — 위조 링크 없음
- `GET /healthz` 생존 확인
- 레이트 리밋: IP당 시간당 60요청

## 지원 플랫폼

- **toss** (토스쇼핑 쉐어링크 · 수수료 10%) — 라이브, 2026-09-16 시드
- **coupang** (쿠팡 파트너스 실검색) — 서버 키 등록 대기
- **oliveyoung** (쇼핑 큐레이터 · 최대 7%) — 가입 심사 대기
- **linkprice 다몰** (이마트몰·G마켓·롯데온·하이마트·오늘의집·11번가·알리익스프레스) — 광고주 승인 대기

풀은 운영자가 큐레이션하며 계속 확장된다.

## 워크스페이스 표준 (v1.0.1)

- **프로필**: `standard.json` — profiles `[skill]`, roots `.`. 검증: `node ../workspace-standard/validator/cli.mjs validate --root .` → green.
- **대시보드** (`dashboard/`): 로컬 운영 뷰 — 레포 태그·dashboard package.json 버전을 빌드 시점에 읽어 정적으로 표시한다(스킬 자체는 파일 1개라 서버 상태 없음). **Supabase 미연결(보류 — Addendum 1)** — 봉투 열람 영역은 미연결 표시, push-envelope 없음.
- **검증기**: 로컬 전용 — `dashboard/`에서 `npm run validate`로 실행한다. 호스팅 빌드 서버엔 정본이 없어 `vercel-build`에는 넣지 않는다; 호스팅 빌드는 `vercel-build`(`next build`)가 담당한다.
- **steps.json 수동 절**: ①버전 기록(mac — 릴리스 태그 시점) ②검증(server — validator green) ③판정(human). 타이머 자동 실행 없음.
- 스킬 노출 API는 풀 검색(`/api/pool/search`)과 쿠팡 실검색(`/api/coupang/search`) 두 가지 — 구 핫딜 카테고리(`/api/hotdeal/top`)는 f29로 제거되어 스킬이 더 이상 호출하지 않는다.

## 공시

이 스킬이 반환하는 링크에는 **운영자 제휴 코드가 포함되어 있습니다**.
사용자 추가 비용은 없으며, 링크를 통해 구매가 일어나면 운영자에게 제휴 수수료가 지급됩니다.

## License

MIT
