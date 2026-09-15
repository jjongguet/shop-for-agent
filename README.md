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

> 올리브영에서 젤 네일 리무버 찾아줘

## API

단일 공개 엔드포인트:

```
GET https://shopforagent.shop/api/pool/search?platform=oliveyoung&keywords=젤 네일 리무버,네일 리무버
```

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

- `keywords` 필수(쉼표 구분), `platform`·`limit` 선택
- 결과 없으면 `"results": []` — 위조 링크 없음
- `GET /healthz` 생존 확인
- 레이트 리밋: IP당 시간당 60요청

## 지원 플랫폼

풀에 등록된 플랫폼(2026-09 출발: 올리브영). 풀은 운영자가 큐레이션하며 계속 확장된다.

## 공시

이 스킬이 반환하는 링크에는 **운영자 제휴 코드가 포함되어 있습니다**.
사용자 추가 비용은 없으며, 링크를 통해 구매가 일어나면 운영자에게 제휴 수수료가 지급됩니다.

## License

MIT
