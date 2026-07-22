# 디자인·기능 설계 메모 (Design Notes)

`assets/template.html`(시드니 7일 예시)의 설계 근거입니다. 새 여행지를 만들 때 이 원칙을 따르되, 색·일러스트·문구는 목적지에 맞게 다시 정하세요.

## 팔레트 (Palette)

여행지의 정체성을 색으로 인코딩합니다. 시드니 예시:
- `--harbour:#0a5a6b` (딥 하버 틸 — 주 강조)
- `--jacaranda:#6b5bd0` (자카란다 보라 — 계절 시그니처)
- `--coral:#e8895f` (석양 산호빛 — 포인트)
- `--sand:#f4efe4` / `--paper:#fbf8f1` (웜 뉴트럴)
- `--ink:#15232b` (틸 편향 잉크)

새 목적지는 그곳의 바다/사막/숲/도시 등에서 색을 끌어오세요. 뉴트럴은 순수 회색 대신 강조색 쪽으로 살짝 편향.

다크/라이트 둘 다 토큰으로 정의:
- `:root`에 라이트값 → `@media (prefers-color-scheme: dark)`에서 토큰만 재정의
- `:root[data-theme="dark"]` / `:root[data-theme="light"]`로 뷰어 토글도 대응

## 타이포 (Type)

- 폰트 CDN은 Artifact CSP에 막힘 → **시스템 폰트 스택** 사용
- 디스플레이(헤드라인)는 Georgia 등 세리프, 본문은 시스템 산세리프
- 개성은 타입스케일·레터스페이싱·색으로. 숫자 정렬엔 `tabular-nums`

## SVG 일러스트 (Illustrations)

- 외부 이미지 금지(CSP·저작권) → **인라인 SVG 벡터**로 각 테마 표현
- 시드니 테마키: `opera`(오페라하우스+브리지), `coast`(해안 절벽), `zoo`(코알라), `mountain`(세 자매봉), `wine`(포도밭+잔), `market`(시장 차양), `ferry`(페리)
- 반응형: `viewBox="0 0 800 190"` + CSS `aspect-ratio:800/190` + `height:auto` (고정 height 금지)
- `preserveAspectRatio="xMidYMid meet"` (slice는 모바일에서 잘림). 배경 rect가 viewBox를 꽉 채우면 여백 없음
- `svg{max-width:100%}` 안전장치

## 핵심 기능 (Features)

1. **J/P 모드**: J(계획형)=Day별 시간표, P(탐색형)=지역별 나열. `localStorage`에 선택 저장
2. **시간표**: 각 블록에 `time`+`dur`, 이동은 `move:1`로 분리(점선·화살표 아이콘)
3. **체크리스트**: 각 활동 체크 → 진행률 바, 완료 Day엔 탭에 ✓. `localStorage` 저장
4. **구글맵 링크**: `https://www.google.com/maps/search/?api=1&query=` + 검색어
5. **예매 하이브리드**: 공식 사이트 + 플랫폼(클룩/트립닷컴 등) + 검색 폴백 링크. 개별 딥링크는 만료 위험이 있어 검색 링크를 함께
6. **맛집·카페**: 검증된 실명 1~2곳 + 구글맵 링크 + "근처 더 보기"
7. **인트로 애니메이션(선택)**: 진입 시 테마 애니메이션, `sessionStorage`로 1회만, `prefers-reduced-motion` 존중, 스킵 버튼
8. **반응형**: 태블릿(768)·모바일(560)·소형(380) 브레이크포인트

## 배포 시 주의 (Deploy gotchas)

- **viewport 메타 필수**: 정적 HTML로 배포할 때 `<meta name="viewport" content="width=device-width, initial-scale=1">`이 없으면 모바일이 데스크톱 폭으로 렌더링됨(모바일 뷰 안 됨)
- Artifact는 head를 자동 래핑하므로 `<title>+<style>+body`만 두면 됨
- Vercel 등은 완전한 HTML 문서로 감싸기

## 검증 (QA)

- `<script>` 추출 → `node --check`로 문법
- 데이터: 모든 블록에 시각, 링크 https, 정답/맵 검색어 유효
- 배포 후 실제 URL 200 확인, viewport·핵심 요소 반영 확인
- 서브에이전트 타당성 검토(요일·이동·계절·예약·동선) 권장
