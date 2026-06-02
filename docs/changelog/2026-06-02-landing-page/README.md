# Run: human-to-code-translation 랜딩 페이지

/ supergoal run record. Mode: **GREENFIELD** + UI/UX overlay. Topology: **single-driver** (한 페이지 look-and-feel = deep-and-narrow).

Objective: 이 레포의 랜딩 페이지를 만들어 GitHub Pages에 호스팅하고, repo description/homepage에 그 링크를 추가한다.

## Priority Rules (ten-rules → web-design 도메인, advisory)

1. 첫 화면에서 "무엇을·누구에게·왜"가 5초 안에 읽혀야 한다 (명확한 히어로 + 한 줄 가치제안).
2. 하나의 주요 행동(설치/GitHub)으로 시선을 모은다 — CTA 우선순위 단일화.
3. 콘텐츠가 곧 디자인: 실제 매핑표·코드 예제를 보여주고 추상 마케팅 문구는 줄인다.
4. 단일 고정 액센트 컬러 1개 (브랜드 정렬: Claude clay coral `#d97757` 계열) — 그라데이션 텍스트/버튼 금지.
5. 접근성 필수: 본문 대비 AAA(>=7:1), 기타 텍스트 AA(>=4.5:1), 키보드 포커스, prefers-reduced-motion 대응.
6. 모바일 우선 반응형 — 단일 컬럼에서 깨지지 않게.
7. 빌드리스 단일 HTML(inline CSS) — GitHub Pages에 무빌드 배포, 의존성 0.
8. 코드 블록은 등폭 + 가독 하이라이트, 가로 스크롤 허용(잘림 금지).
9. 성능: 외부 폰트/스크립트 최소화, 이미지 없는 텍스트 중심 → 즉시 로드.
10. em-dash 금지, AI-slop(과한 글로우/이모지 남발) 금지.

## Decisions / Audit log

- 2026-06-02: Mode GREENFIELD 확정. 산출물은 정적 단일 페이지 → 배포 단순성 위해 단일 HTML 채택.
- 2026-06-02: 배포 방식 = **main 브랜치 `/docs` 폴더** (GitHub Pages source). 근거: 단일 브랜치 유지, repo root는 README/SKILL 보존, vault changelog와 같은 docs/ 아래 그룹핑. URL → https://cskwork.github.io/human-to-code-translation-skill/
- 2026-06-02: **SKIP 로그** — Intake와 Validate를 orchestrator가 직접 brief.md로 통합 작성(저위험 기획 단계). 핵심 분리(builder != verifier)는 유지. Plan/Build/Verify/QA/Committee는 분리된 fresh 서브에이전트로 디스패치.
- 2026-06-02: 콘텐츠 소스 = 레포의 README.md / SKILL.md / worked-examples.md (실측).

## Escalations

- (없음 — 추가 시 기록)

- 2026-06-02: RE-PLAN: 사용자 추가 요청 "기본 영어 + 한국어 전환". plan.md non-goal(다국어 토글 없음)을 사용자 지시로 오버라이드. 영어 기본 + nav 토글(EN/한국어) + 인라인 JS(외부 의존성 0 유지) + localStorage + noscript 영어 폴백. AC10/AC11은 1차 배포(commit 63551fd)로 검증 완료(라이브 200).
