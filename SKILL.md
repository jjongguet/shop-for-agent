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
  --data-urlencode "platform=oliveyoung" \
  --data-urlencode "keywords=젤 네일 리무버,네일 리무버"
```

- `platform`: 플랫폼 필터(현재 등록: `oliveyoung`). 생략하면 전체에서 검색한다.
- `keywords`: 쉼표 구분 질의 키워드(필수). 사용자 발화의 핵심 명사를 **띄어쓰기 포함 원문**으로.
- `limit`: 최대 결과 수(기본 5, 상한 50).

응답 예:

```json
{
  "results": [
    {
      "platform": "oliveyoung",
      "product_name": "젤 네일 리무버 500ml",
      "affiliate_url": "https://...",
      "added_at": "2026-09-15T10:00:00Z"
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

## 공시

이 스킬이 반환하는 링크에는 운영자 제휴 코드가 포함되어 있다.
