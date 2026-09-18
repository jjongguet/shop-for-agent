---
name: shop-for-agent
description: 한국 커머스 제휴링크 검색 — 상품명과 구매 링크를 반환한다. "올리브영에서 ~ 찾아줘", "어디서 사야 돼", "구매 링크 줘", "최저가 알려줘" 같은 쇼핑 질의에서 사용한다. TRIGGER when the user asks to find/buy/recommend a product or a shopping link in Korean commerce.
---

# shop-for-agent

사용자의 쇼핑 의도를 키워드로 번역해 shopforagent.shop 검색 API를 호출한다.
설치물·API 키·가입이 전혀 필요 없다.

## 호출

```sh
curl -s -G "https://shopforagent.shop/api/pool/search" \
  --data-urlencode "platform=toss" \
  --data-urlencode "keywords=딸기"
```

- `platform`: 플랫폼 필터(현재 등록: `toss` — 2026-09-16 시드). 생략하면 전체에서 검색한다.
- `keywords`: 쉼표 구분 질의 키워드(필수). 사용자 발화의 핵심 명사를 **띄어쓰기 포함 원문**으로.
- `limit`: 최대 결과 수(기본 5, 상한 50).

쿠팡 실검색(서버가 운영자의 파트너스 키로 상품 검색+제휴 링크를 즉석 생성):

```sh
curl -s -G "https://shopforagent.shop/api/coupang/search" \
  --data-urlencode "keywords=무선청소기"
```

- 응답 `results[].affiliate_url`이 제휴 링크다. 상품 원문 URL은 노출되지 않는다.
- `disclosure` 필드가 있으면 링크 안내에 그 문구를 함께 전달한다(파트너스 수수료 고지).
- `503 coupang_not_configured`면 "쿠팡 연동 대기 중"이라고 답한다 — 다른 링크를 지어내지 않는다.

급상승 핫딜 조회(커뮤니티 반응 지표 기반 — 운영자 수집 DB):

```sh
curl -s -G "https://shopforagent.shop/api/hotdeal/top" \
  --data-urlencode "hours=2" --data-urlencode "limit=20"
```

- 최근 N시간(기본 2, 상한 24)에 처음 관측되어 댓글·조회수가 오르는 딜 목록이다.
- "요즘 뜨는 게 뭐야"류 질문에 `deals[].title`·`url`(커뮤니티 게시글 원문)로 답한다.
- 이 링크는 제휴 링크가 아닌 커뮤니티 원문 링크다 — 원문 그대로 전달한다.

응답 예(실측, 2026-09-16):

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

## 규칙

1. **의도 → 키워드 번역**: "젤 네일 리무버 같은 거 없어?" → `keywords=젤 네일 리무버,네일 리무버`.
   핵심 명사와 축약형을 함께 넣으면 매칭률이 오른다.
2. **정직한 미스**: `"results": []`면 반드시 "제휴 링크 없음"이라고 답한다.
   일반 쇼핑몰 URL을 지어내 대체하지 않는다.
3. `affiliate_url`은 원문 그대로 전달한다(변형·축약 금지).
4. API가 응답하지 않으면(타임아웃·5xx) 그 사실을 알린다. 실패를 숨기지 않는다.
5. 쿠팡 응답의 `disclosure` 문구는 링크 안내에 함께 노출한다 — 생략하지 않는다.

## 공시

이 스킬이 반환하는 링크에는 운영자 제휴 코드가 포함되어 있다(토스쇼핑 쉐어링크 · 쿠팡 파트너스 · 올리브영 쇼핑 큐레이터 · 링크프라이스). 링크를 통한 구매 시 운영자에게 수수료가 지급되며 사용자 추가 비용은 없다.
