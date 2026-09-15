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

## 공시

이 스킬이 반환하는 링크에는 **운영자 제휴 코드가 포함되어 있습니다**.
사용자 추가 비용은 없으며, 링크를 통해 구매가 일어나면 운영자에게 제휴 수수료가 지급됩니다.

## License

MIT
