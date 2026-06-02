# plan.md — human-to-code-translation 랜딩 페이지 (FROZEN)

Mode: GREENFIELD + UI/UX overlay. Topology: single-driver (deep-and-narrow, 한 페이지 look-and-feel).
Objective: 레포 랜딩 페이지를 만들어 GitHub Pages(main `/docs`)에 호스팅하고 repo description/homepage에 링크를 추가.

---

## Architecture

### Stack decision
**단일 정적 HTML 파일 `docs/index.html`, inline CSS, 빌드 단계 없음, JS 프레임워크 없음, Jekyll 없음(`.nojekyll` 포함).**

근거:
- 콘텐츠가 텍스트/표/코드 블록 중심이라 동적 로직이 필요 없다 → 정적 단일 파일이 가장 단순하고 되돌리기 쉽다(brief Non-goals, Priority Rule 7).
- AC7(외부 빌드 의존성 0, 파일을 그대로 브라우저로 열어도 렌더)을 만족하려면 inline CSS·무빌드가 필수.
- Jekyll은 GitHub Pages 기본 처리기인데, `_`로 시작하는 폴더/파일을 무시하고 markdown을 변환하려 든다. 우리는 순수 정적 서빙만 원하므로 `docs/.nojekyll`로 Jekyll 파이프라인을 끈다(brief Non-goals).
- JS 미사용: 탭 UI 대신 CSS-only 3-컬럼/스택 레이아웃으로 Python/Java/C 코드를 동시 노출 → JS 0, prefers-reduced-motion·키보드 a11y 부담 최소(AC5, AC9).
- 외부 폰트/스크립트 미링크: system font stack + 코드용 `ui-monospace` → 즉시 로드(Priority Rule 9), 추적/쿠키 0(brief Non-goals).

taste §2 적용: 이 브리프는 "공식 디자인 시스템"이 아니라 "미학(개발자 도구 / dark-tech 변형의 라이트 버전)"에 해당 → §2.B에 따라 native CSS로 직접 구현, 공식 패키지 미사용. 단일 파일·무프레임워크라 §3.A의 React/Tailwind 기본 스택은 의도적으로 적용하지 않음(브리프가 빌드리스를 명시).

### Deployment
- GitHub Pages source: **main 브랜치 `/docs` 폴더** (Settings → Pages → Branch: main, Folder: /docs).
- Public URL: `https://cskwork.github.io/human-to-code-translation-skill/`
- AC11: repo homepageUrl(description 옆 링크)을 위 URL로 설정.

### File list (tiny)
| 파일 | 목적 |
|---|---|
| `docs/index.html` | 랜딩 페이지 전체(inline `<style>`, 콘텐츠, 단일 진입점) |
| `docs/.nojekyll` | Jekyll 비활성화(빈 파일) |

기존 `docs/changelog/`는 그대로 둔다(공존 검증은 Plan grounding Q2 참조).

---

## Design Read + dials (UI/UX overlay, taste §0-§2)

### Design Read (one line)
Reading this as: **개발자 도구(Claude Code / Codex 스킬) 마케팅 랜딩 for 알고리즘 학습자·교사·스킬 설치 개발자, with a calm developer-doc language (등폭 코드 우선·콘텐츠가 곧 디자인), leaning toward native CSS + system fonts + Claude clay-coral 단일 액센트, motion 최소.**

### The three dials
- **DESIGN_VARIANCE: 5** — 콘텐츠(매핑표·6단계·3언어 코드)가 주인공이고 신뢰·가독이 목표라 과한 비대칭/아트성은 역효과. taste §1.A "minimalist/clean → 5-6", §1.B "Developer portfolio/devtool ≈ 6"에서 한 단계 낮춰 5(섹션-배경 리듬으로 구분, 레이아웃은 정직한 그리드).
- **MOTION_INTENSITY: 2** — AC9 prefers-reduced-motion 필수 + 텍스트 중심 즉시 로드(Rule 9) + AI-slop 글로우 금지(Rule 10). 진입 fade/hover 색 전환 정도만, 그마저 reduced-motion에서 전부 제거. taste §1.A "accessibility-critical → 2-3"의 하단.
- **VISUAL_DENSITY: 5** — 매핑표 14행·3언어 코드·6단계가 들어가는 정보 밀도 높은 페이지. "cockpit"은 아니되 art-gallery 여백은 사치. taste §1.B Developer 4 + 표 밀도 고려 5.

### System vs aesthetic
taste §2.B의 **aesthetic 트랙** — 공식 디자인 시스템 없음. native CSS로 "developer-doc / restrained dark-tech의 라이트 버전"을 정직하게 구현. 코드 주석에 "borrowed inspiration"임을 명시할 필요 없음(원작 토큰 import 0).

### Brand alignment + locked palette
브랜드: cskwork의 Claude Code / Codex 개발자 스킬. **단일 고정 액센트 = Claude clay-coral `#d97757` 계열**(brief Rule 4). 팔레트를 새로 발명하지 않고 이 한 색을 액센트 시스템으로 잠근다.

| 토큰 | hex | 용도 | 검증 |
|---|---|---|---|
| `--bg` (background) | `#faf9f7` | 페이지 바탕(warm off-white, clay 계열과 동일 온도) | — |
| `--surface` | `#ffffff` | 카드/표/섹션 패널 | bg 대비 충분 |
| `--text` (body) | `#1a1a1a` | 본문 텍스트 | **bg 대비 16.54:1 → AAA(>=7:1) 통과** ← 의도된 본문 페어 |
| `--muted` (muted text) | `#57534e` | 캡션·보조 텍스트 | bg 대비 7.25:1 (AA 여유, AAA 경계 통과) |
| `--accent` | `#d97757` | 보더·룰·아이콘·강조 마커(텍스트/버튼 채움 아님) | 장식 전용(2.97:1, 본문 금지) |
| `--accent-ink` | `#b85c3e` | 링크/액센트 *텍스트* (clay 딥 변형) | bg 대비 4.30:1 (AA 통과) |
| `--accent-fill` | `#a84f33` | 흰 글자를 얹는 CTA 버튼 배경(채움 필요 시) | 흰색 대비 5.48:1 (AA 통과) |
| `--code-bg` (code-block bg) | `#2b2622` | 코드 블록 배경(warm near-black) | code text 대비 12.02:1 → AAA |
| `--code-fg` | `#e8e6e3` | 코드 블록 텍스트 | 위와 동일 페어 |

**의도된 AAA 본문 페어: `--text #1a1a1a` on `--bg #faf9f7` = 16.54:1** (AC9, Rule 5 충족, 계산 검증 완료).

핵심 규칙: `#d97757`(--accent)는 대비 2.97:1로 텍스트에 못 쓴다. 텍스트성 강조는 반드시 `--accent-ink #b85c3e`, 흰 글자 얹는 채움은 `--accent-fill #a84f33`로 분리. 이것이 "단일 액센트 시스템"을 a11y와 양립시키는 방식(같은 clay 계열의 명도 변형이므로 색은 1개로 유지).

### Hard visual bans (honor)
- 단일 고정 액센트 1개(clay-coral) — 다른 hue 추가 금지.
- gradient text 금지 / gradient-fill 버튼 금지(버튼은 solid `--accent-fill` 또는 보더+`--accent-ink` 텍스트).
- colored glow shadow 금지(그림자는 중성 저알파만, 예: `rgba(0,0,0,.06)`).
- em-dash(—) 금지 → 본문/마크업에서 하이픈·괄호·줄바꿈으로 대체(콘텐츠 원문 인용은 원문 유지하되 신규 작성 카피엔 미사용).
- 섹션-배경 리듬: `--bg`와 `--surface`를 교대로 깔아 섹션 경계를 색이 아닌 톤으로 구분(VARIANCE 5의 구조 장치).
- decoration보다 real content: 매핑표·실제 코드·설치 명령을 1순위로, 장식 도형/스톡 일러 0(Rule 3).
- 이모지 남발 금지(taste §3.D), 과한 글로우/AI-slop 금지(Rule 10).

---

## Content sections (각 섹션 → AC 매핑)

순서대로. 콘텐츠 언어 = 한국어(레포 원문), 코드/식별자 = 원형 verbatim.

| # | 섹션 | 내용 / 소스 | 충족 AC |
|---|---|---|---|
| 1 | **Hero** (`#top`) | 제품명 `human-to-code-translation` + 한 줄 가치제안("사람의 풀이 → 기계 코드, 그 중간 다리를 단계적으로 놓아주는 Claude Code / Codex 스킬", README L3) + 주요 CTA: GitHub repo 링크(primary) + "설치" 앵커(secondary). 파이프라인 한 줄(`말 → 손풀이 → 규칙 → 변수/흐름 → 코드 → 트레이싱`, README L12). | AC2, (AC1 doctype/title) |
| 2 | **무엇인가 / 핵심 원리** (`#what`) | "코드는 갑자기 튀어나오지 않는다 … 의미는 언어 무관, 달라지는 건 문법뿐"(README L9-15, SKILL L10-12). 의미=언어무관 메시지로 3언어 병렬의 정당성 제시. | AC2 보강 |
| 3 | **6단계 번역 파이프라인** (`#pipeline`) | 6단계를 순서대로 카드/리스트: 말→손풀이→암묵지 명시화→상태/변수→흐름·종료→코드+트레이싱 (SKILL L23-30 = 정본, README L19-24). 각 단계 1-2줄 설명. | **AC3** |
| 4 | **핵심 매핑표** (`#mapping`) | "사람의 표현/머릿속 행동 → 기계 구성요소" 표. SKILL L36-51의 14행 전부 노출(brief 최소 8행 초과 달성). `for`/`while`/set/dict/스택/큐/투 포인터/DP 등 식별자 원형. | **AC4** |
| 5 | **워크드 예제: 이진 탐색 3언어** (`#example`) | SKILL L67-105의 Python/Java/C 코드 블록을 나란히(데스크톱 3-컬럼 grid, 모바일 1-컬럼 스택, CSS-only, JS 탭 없음). 위에 1줄 문제 요약 + "다리는 동일, 문법만 다름" 교훈(SKILL L107). 코드 verbatim. | **AC5**, AC8 |
| 6 | **설치** (`#install`) | `git clone https://github.com/cskwork/human-to-code-translation-skill.git ~/.claude/skills/human-to-code-translation` (README L46-47). 코드 블록으로 복사 가능하게. 트리거 자동 로드 한 줄(README L50). | **AC6** |
| 7 | **누구에게** (`#who`) | 가르치는 사람 / "사람으로는 풀겠는데 코드로 못 옮기겠다" 학습자 / 경계·종료 조건에 막히는 사람(README L54-56, SKILL L16-20). | AC2 보강 |
| 8 | **Footer** (`#footer`) | GitHub repo 링크, Pages URL, MIT 라이선스(README L60), "더 많은 예제: worked-examples.md"(스택/해시/DP) 안내. | AC2 보강 |

전 섹션 공통: 반응형(360px~1280px, CSS Grid + `min-h`/`max-width` 컨테이너, flex-% 계산 금지 taste §3.E) → **AC8**; 키보드 포커스 가시 outline + `prefers-reduced-motion` 미디어쿼리 → **AC9**; inline `<style>`·외부 의존 0 → **AC7**; `<!doctype html>` + `<title>` + 시맨틱 `<main>/<section>/<nav>` → **AC1**.

AC10(Pages 200), AC11(homepageUrl 설정)은 Build/Deploy 단계의 운영 작업(Contracts에 정확 문자열 고정).

---

## Task table (slices)

이 산출물은 정적 단일 파일이라 1 빌드 슬라이스 + 1 배포 슬라이스로 분리. 각 슬라이스 <=5 files / <=~500 lines.

| Slice | 범위 | 파일(수) | 예상 라인 | Acceptance check | 재사용 |
|---|---|---|---|---|---|
| **S1 — 페이지 빌드** | `docs/index.html`(전체 8섹션 + inline CSS) + `docs/.nojekyll` | 2 | ~350-450 | `docs/index.html`을 브라우저로 직접 열어 **AC1-AC9 시각 확인**: doctype/title 존재(AC1), 히어로 제품명+가치제안+GitHub CTA(AC2), 6단계 순서대로(AC3), 매핑표 >=8행(실제 14, AC4), Python/Java/C 코드 나란히(AC5), 설치 명령 정확(AC6), 외부요청 0·file:// 렌더(AC7, DevTools Network 빈 상태로 확인), 360/768/1280px 무파손(AC8), 본문 대비 16.54:1·Tab 포커스 보임·reduced-motion 시 애니메이션 없음(AC9). | 콘텐츠는 README/SKILL/worked-examples에서 verbatim 인용(재작성 아님). 팔레트 hex는 본 plan에서 고정. |
| **S2 — Pages 배포 + 링크** | main `/docs` Pages 활성화, homepageUrl 설정 | 0 신규(설정 + git) | — | **AC10**: `curl -sI https://cskwork.github.io/human-to-code-translation-skill/` 가 `200` 반환하고 본문에 랜딩 콘텐츠 포함. **AC11**: `gh repo view --json homepageUrl` 결과가 정확히 Pages URL. | ten-rules-skill이 동일 main `/docs` Pages 패턴으로 이미 운영 중(brief Validation) → 검증된 절차 재사용. |

S2는 Pages 빌드 전파 지연(수십 초~수 분) 가능 → 200 확인은 재시도 허용.

---

## Contracts

### Section anchor ids (verbatim)
`#top`, `#what`, `#pipeline`, `#mapping`, `#example`, `#install`, `#who`, `#footer`
(히어로/내비 스킵 링크는 `#top`를 메인 콘텐츠 시작으로 사용; 키보드 a11y용 "본문 바로가기" 링크 1개 권장.)

### Install command (verbatim, 1자도 변경 금지)
```
git clone https://github.com/cskwork/human-to-code-translation-skill.git ~/.claude/skills/human-to-code-translation
```
(README의 줄바꿈+백슬래시 형태도 그대로 보여도 됨; 복사 시 한 줄로 동작.)

### GitHub repo URL (CTA target)
```
https://github.com/cskwork/human-to-code-translation-skill
```

### GitHub Pages URL (AC10/AC11 target)
```
https://cskwork.github.io/human-to-code-translation-skill/
```

### 제목 / `<title>` (AC1)
`human-to-code-translation — 사람의 풀이를 코드로 번역하는 Claude Code / Codex 스킬`
(주: 이 title 내 구분자는 가독용이며 em-dash 금지 규칙은 본문 카피 대상 → title은 하이픈성 구분 기호 사용. 안전하게 콜론/중점으로 대체 가능: `human-to-code-translation · 사람의 풀이를 코드로 번역하는 Claude Code / Codex 스킬`. Build는 중점 버전 채택.)

---

## Plan grounding (self-run, answered)

**Q1. 왜 정적-사이트 생성기(Jekyll/Docusaurus)가 아니라 단일 파일인가?**
A. brief Non-goals가 SSG 도입을 명시적으로 금지하고 AC7이 "외부 빌드 의존성 0, file://로도 렌더"를 요구한다. 콘텐츠는 정적 텍스트/표/코드뿐이라 SSG의 템플릿/빌드 이점이 0. 단일 파일이 최소 비용·최대 되돌리기 용이성(brief Validation 리스크 평가)과 정합. 결론: 단일 `docs/index.html`.

**Q2. main `/docs`를 Pages 소스로 쓰면 `docs/changelog/` vault와 충돌하나?**
A. 충돌 없음. Pages는 `/docs`를 사이트 루트로 서빙하지만 `.nojekyll`로 Jekyll 변환을 끄므로 `docs/changelog/*.md`는 변환 대상이 아니라 단순 정적 파일로 남는다(아무도 링크 안 하면 노출 0). 루트 진입점은 `docs/index.html`. vault는 git에는 있되 사이트 내비에서 참조하지 않음 → 방문자 경험과 분리. 추가로 `docs/changelog/`의 `2026-06-02-...` 디렉터리는 `_` 시작이 아니므로 Jekyll 무시 규칙과도 무관(어차피 .nojekyll). 결론: 그대로 공존.

**Q3. clay-coral 액센트로 AAA 본문 대비가 가능한가?**
A. 액센트 색 자체를 본문에 쓰지 않으면 가능. 본문은 `#1a1a1a` on `#faf9f7` = 16.54:1로 AAA를 크게 상회(검증 완료). `#d97757`는 2.97:1이라 텍스트 금지 → 보더/룰/마커 장식 전용. 텍스트성 강조는 같은 clay 계열 딥 변형 `#b85c3e`(4.30:1 AA), 흰 글자 버튼은 `#a84f33`(5.48:1 AA). "단일 액센트"는 hue 1개를 의미하며 명도 변형 3종은 같은 색 시스템 → 규칙 위반 아님. 결론: AAA 달성 + 단일 액센트 양립.

**Q4. reduced-motion + 키보드 a11y 계획은?**
A. 모션은 MOTION_INTENSITY 2 — 진입 fade-in과 hover 색 전환만. `@media (prefers-reduced-motion: reduce){ * { animation:none; transition:none; } }`로 전부 제거(AC9). 키보드: 시맨틱 랜드마크(`<nav>/<main>/<section>`), 모든 인터랙티브(링크/CTA)에 가시 `:focus-visible` outline(`outline: 2px solid var(--accent-ink); outline-offset: 2px;` — 대비 충분), 상단에 "본문 바로가기" 스킵 링크. JS 0이라 포커스 트랩/위젯 a11y 위험 없음. 결론: 기본 HTML 시맨틱 + CSS 포커스만으로 AC9 충족.

**Q5. 코드 3언어를 탭(JS) 없이 어떻게 "나란히" 보이나?**
A. CSS Grid 3-컬럼(데스크톱 `lg`+) ↔ 1-컬럼 스택(모바일). 각 컬럼에 언어 라벨 + `<pre><code>` 블록. 코드가 길면 컬럼 내부 가로 스크롤 허용(Rule 8: 잘림 금지). JS 탭은 reduced-motion/키보드/file:// 모두에서 추가 위험만 만들고 AC5("나란히")는 동시 노출이 더 직접적 충족. 결론: CSS-only 병렬.

---

## Human Feedback

### Plain-language brief
이 스킬(코딩 문제를 "사람이 머리로 푸는 방식"에서 "컴퓨터 코드"로 바꿔주는 도우미)을 소개하는 **한 페이지짜리 웹사이트**를 만듭니다. 방문자가 5초 안에 "이게 뭐고, 누구에게 필요하고, 어떻게 설치하는지"를 알 수 있게 합니다. 페이지에는 6단계 번역 과정, 사람의 생각을 코드 부품으로 바꾸는 대조표, 그리고 같은 문제(이진 탐색)를 Python·Java·C 세 언어로 나란히 보여주는 예제가 들어갑니다. 사진이나 화려한 효과 없이 글과 코드 위주라 매우 빠르게 열리고, 색은 Claude 브랜드의 흙빛 코랄색 하나만 씁니다. 만든 페이지는 GitHub의 무료 호스팅(GitHub Pages)에 올리고, 저장소 설명 옆에 그 주소를 링크로 답니다. 서버도, 복잡한 빌드 과정도 없습니다. 글자와 배경의 색 대비를 최고 등급(AAA)으로 맞춰 누구나 읽기 편하게 하고, 움직임을 싫어하는 사용자 설정도 존중합니다.

### Technical brief
- **파일(2개):** `docs/index.html`(전체 페이지 + inline `<style>`), `docs/.nojekyll`(빈 파일, Jekyll 끔). 신규 의존성·빌드 0.
- **섹션(8개):** 히어로 → 핵심 원리 → 6단계 파이프라인 → 매핑표(14행) → 이진 탐색 3언어 예제(CSS Grid 병렬) → 설치 → 누구에게 → 푸터. 콘텐츠는 README/SKILL/worked-examples에서 한국어 원문·코드 verbatim 인용.
- **팔레트(고정 hex):** bg `#faf9f7`, surface `#ffffff`, body text `#1a1a1a`(대비 16.54:1 AAA), muted `#57534e`(7.25:1), 액센트 `#d97757`(장식 전용), 액센트 텍스트 `#b85c3e`(4.30:1), 버튼 채움 `#a84f33`(5.48:1), 코드 bg `#2b2622`/코드 글자 `#e8e6e3`(12.02:1 AAA). 단일 hue(clay-coral) + 명도 변형.
- **다이얼:** VARIANCE 5 / MOTION 2 / DENSITY 5.
- **테스트:** S1 = 브라우저로 `docs/index.html` 열어 AC1-AC9 육안 + DevTools Network 빈 상태(외부요청 0) + 360/768/1280 반응형 + Tab 포커스 + reduced-motion. S2 = `curl -sI` Pages 200(AC10) + `gh repo view --json homepageUrl`(AC11).
- **배포:** main `/docs` Pages, URL `https://cskwork.github.io/human-to-code-translation-skill/`.
- **리스크:** (1) Pages 전파 지연 → 200 재시도 허용. (2) 액센트 색 대비 함정 → 텍스트엔 `#b85c3e`/`#a84f33`만 사용(계산 검증 완료). (3) em-dash 혼입 → 신규 카피 검수, title은 중점(·) 사용. (4) vault 공존 → `.nojekyll`로 markdown 변환 차단(grounding Q2). 낮은 전반 리스크(되돌리기 = 파일 삭제 / Pages 비활성).

### Terms
- GitHub Pages: GitHub 저장소의 파일을 그대로 공개 웹사이트로 무료 서빙해주는 기능. 여기선 main 브랜치의 `/docs` 폴더를 사이트 루트로 사용.
- .nojekyll: 빈 파일 하나. 있으면 GitHub Pages가 Jekyll(기본 정적 사이트 변환기)을 건너뛰고 파일을 가공 없이 그대로 서빙한다.
- WCAG AAA contrast: 웹 접근성 지침의 가장 높은 글자-배경 명도 대비 등급. 본문 기준 7:1 이상이어야 함(우리 본문은 16.54:1).
- inline CSS: 별도 `.css` 파일이나 외부 링크 없이 HTML 문서 안 `<style>`에 스타일을 직접 넣는 방식. 파일 하나로 완결돼 file://로 열어도 렌더된다.
- prefers-reduced-motion: 사용자가 OS에서 "동작 줄이기"를 켰는지 알려주는 CSS 미디어 쿼리. 켜져 있으면 애니메이션/전환을 모두 끈다.

### Approval request
Approve Build, request changes, or stop.
