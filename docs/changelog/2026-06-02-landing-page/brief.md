# Brief — human-to-code-translation 랜딩 페이지

## Goal
이 스킬의 가치(사람의 풀이 → 코드 번역, 6단계 파이프라인 + 매핑표)를 한눈에 보여주는 랜딩 페이지를 만들어 GitHub Pages에 배포하고, repo description/homepage에 그 링크를 노출한다.

## Audience
- 알고리즘/코딩테스트를 가르치는 사람
- "사람으로는 풀겠는데 코드로 못 옮기겠다"는 학습자
- Claude Code / Codex 스킬을 찾는 개발자 (설치 대상)

## Acceptance criteria (machine-checkable)
- AC1: `docs/index.html` 가 존재하고 유효한 HTML5 문서다 (`<!doctype html>`, `<title>` 포함).
- AC2: 히어로 영역에 제품명 + 한 줄 가치제안 + 주요 CTA(GitHub repo 링크) 포함.
- AC3: 6단계 번역 파이프라인이 순서대로 노출된다 (말→손풀이→규칙→변수/흐름→코드→트레이싱).
- AC4: 핵심 매핑표(사람의 머릿속 행동 → 기계 구성요소) 최소 8행 노출.
- AC5: 워크드 예제 코드 블록 노출 — 이진 탐색을 Python/Java/C 3개 언어로 나란히.
- AC6: 설치 명령(`git clone ... ~/.claude/skills/human-to-code-translation`) 노출.
- AC7: 외부 빌드 의존성 0 — inline CSS, 단일 HTML, 파일을 그대로 브라우저로 열어도 렌더된다.
- AC8: 반응형 — 360px(모바일) ~ 1280px(데스크톱)에서 레이아웃이 깨지지 않는다.
- AC9: 접근성 — 본문 텍스트 대비 AAA(>=7:1), 키보드 포커스 가시, `prefers-reduced-motion` 대응.
- AC10: GitHub Pages 활성화 후 https://cskwork.github.io/human-to-code-translation-skill/ 가 200으로 응답하고 랜딩 페이지를 반환한다.
- AC11: repo의 homepageUrl(description 링크)이 위 Pages URL로 설정된다.

## Non-goals
- 백엔드/서버/빌드 파이프라인 없음 (정적 페이지 only).
- 다국어 토글 없음 (콘텐츠는 한국어 원문 유지, 코드/식별자는 원형).
- 블로그/문서 사이트 생성기(Jekyll/Docusaurus 등) 도입 없음 — `.nojekyll` 로 순수 정적 서빙.
- 애널리틱스/쿠키/외부 추적 없음.

## Validation
GREENFIELD 수요 검증:
- 대상 레포는 실존하며(원격 origin 확인), 콘텐츠(README/SKILL/worked-examples)가 충분히 풍부해 보여줄 가치가 있다.
- 현재 homepageUrl 이 비어 있어 방문자가 스킬을 한눈에 파악할 진입점이 없다 → 랜딩 페이지의 명확한 필요.
- ten-rules-skill(같은 저자)이 https://cskwork.github.io/ten-rules-skill/ 로 이미 랜딩을 운영 중 → 동일 패턴의 검증된 수요/관행.
- 비용/위험: 단일 정적 HTML, 무빌드, ADMIN 권한 보유 → 리스크 낮음, 되돌리기 쉬움(Pages 비활성화/파일 삭제).

Decision: GO
