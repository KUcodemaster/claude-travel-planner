# ✈️ Travel Planner Skill

여행지와 일정을 입력하면 **브라우저에서 바로 열리는 인터랙티브 여행 계획 웹페이지**를 만들어 주는 [Claude Code](https://claude.com/claude-code) / Claude Agent Skill입니다.

A Claude Agent Skill that turns a **destination + dates** into a polished, self-contained **interactive itinerary web page** (single HTML file — no server, no external assets).

---

## 무엇을 하나요? / What it does

사용자가 "도쿄로 5일 여행 계획 짜줘"처럼 여행지와 기간을 말하면, 스킬이:

Give it a destination and length of stay, and the skill will:

- 🔎 **실제 정보를 웹 검색으로 검증** — 요일·휴무일·일몰·서머타임·이동시간·시즌 이벤트·예약 필요 항목
  *Verifies real facts via web search — opening days, sunset & DST, travel times, seasonal events, bookings*
- 🗓️ **시간표 기반 일정** — 각 활동에 실제 시각·소요시간, 장소 간 이동 구간 분리
  *Time-blocked plan with real times, durations, and separate transit segments*
- 🔀 **J(계획형)/P(탐색형) 두 모드** — Day별 시간표 vs 지역별 둘러보기
  *Two views: J (day-by-day schedule) and P (browse by area)*
- 🎨 **테마 SVG 일러스트** — 외부 이미지 없이 목적지 정체성을 벡터로 (CSP·저작권 안전)
  *Inline SVG illustrations per theme — no external images*
- 🍽️ **맛집·카페 + 🎟️ 예매 링크** — 구글맵·클룩·트립닷컴·공식 사이트 하이브리드
  *Nearby eats + booking links (Google Maps / Klook / Trip.com / official)*
- ✅ **체크리스트·진행률**, 다크/라이트, **모바일 반응형**, 진입 애니메이션
  *Checklist progress, dark/light, mobile-responsive, intro animation*

---

## 설치 / Install

### Claude Code
1. 이 저장소를 스킬 디렉토리에 두세요 / Place this repo under your skills directory:
   ```
   ~/.claude/skills/travel-planner/     (개인 / personal)
   .claude/skills/travel-planner/       (프로젝트 / per-project)
   ```
   즉, `SKILL.md`가 `.../skills/travel-planner/SKILL.md`에 오도록 폴더째 복사합니다.
2. Claude Code를 재시작하면 스킬이 자동 인식됩니다.

### 사용 / Use
그냥 자연어로 요청하세요. 스킬이 자동으로 트리거됩니다:

Just ask in natural language — the skill triggers automatically:

```
오사카로 4일 여행 계획 웹페이지 만들어줘
Plan a 6-day trip to Lisbon in May and build the web page
```

또는 명시적으로 / Or invoke explicitly: `/travel-planner`

---

## 구조 / Structure

```
travel-planner/
├── SKILL.md                    # 스킬 지침 (Claude가 읽는 본체)
├── assets/
│   └── template.html           # 완성형 참고 구현 (시드니 7일 예시)
├── references/
│   └── design-notes.md         # 팔레트·일러스트·기능 설계 메모
├── README.md
└── LICENSE
```

- **`SKILL.md`** — Claude가 따르는 절차: 정보 검증 → 데이터 설계 → HTML 생성 → 검증 → 배포 → 타당성 검토
- **`assets/template.html`** — 시드니 7일 예시가 담긴 완성형 참고. 새 여행지는 이 뼈대를 목적지에 맞게 재구성
- **`references/design-notes.md`** — 색·타이포·SVG·기능·배포 주의사항

---

## 결과 예시 / Example output

`assets/template.html`을 브라우저로 열어보세요 — 2026년 10월 시드니 7일 여행 계획입니다.
Open `assets/template.html` in a browser to see a sample: a 7-day Sydney itinerary (Oct 2026).

---

## 동작 방식 / How it works

이 스킬은 코드를 실행하는 프로그램이 아니라 **Claude에게 주는 지침서**입니다. Claude가 `SKILL.md`를 읽고, 웹 검색으로 사실을 확인한 뒤, 참고 템플릿을 바탕으로 목적지에 맞는 HTML을 **생성**합니다.

This skill is not a script — it is a **set of instructions for Claude**. Claude reads `SKILL.md`, verifies facts via web search, and **generates** a destination-specific HTML page using the reference template.

필요 도구 / Tools used: 웹 검색(`tavily-search` 또는 `WebSearch`/`WebFetch`), 파일 쓰기, (선택) Artifact 게시, (선택) 정적 호스팅 배포.

---

## 주의 / Notes

- 생성된 정보는 **참고용**입니다. 운영시간·요금·이벤트는 여행 전 공식 사이트에서 재확인하세요.
  *Generated info is for reference — reconfirm hours/prices/events before travel.*
- 스킬은 검색으로 확인된 사실만 사용하도록 설계되어 있으나, LLM 특성상 오류가 있을 수 있습니다.
  *The skill is designed to use only verified facts, but LLM errors are possible.*

## 라이선스 / License

MIT — 자유롭게 사용·수정·재배포하세요. See [LICENSE](./LICENSE).
