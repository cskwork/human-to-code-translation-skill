# Changelog 2026-06-02 — human-to-code-translation 스킬 신설

## 목적
알고리즘 문제를 받았을 때 "사람이 머릿속으로 푸는 방식" → "기계(C/Java/Python)가 실행하는 코드"로 바꾸는 그 중간 다리(번역 과정)를, 단계적·친절하게 보여주는 스킬.

## 결정과 근거 (why)

- **이름 `human-to-code-translation`**: writing-skills의 CSO 원칙(active, 무엇을 하는지). 사용자 원안 "machine-language-translate"는 'machine language=어셈블리' 오해 소지가 있어 배제(사용자 선택으로 확정).
- **예제: Python 깊게 + C/Java/Python 3종 대조**: 이 스킬의 핵심 가치가 "여러 기계 언어로의 번역"이고, "다리(의미)는 같고 문법만 다르다"를 직접 보여주는 게 교육적 핵심이라 판단(사용자 선택으로 확정). writing-skills의 "one excellent example"는 *동일 문제*를 깊게 한 번 다루는 것으로 충족 — 언어만 표면 대조.

## RED (baseline, 스킬 없이)
- general-purpose 에이전트에게 이진 탐색 문제를 "사람→코드로 가르쳐줘"라고 시킴.
- 결과: Opus급은 *이미 아는 교과서 문제* + *명시적 지시* 조합에선 이미 잘함(말→규칙→코드→트레이싱을 즉흥 생성).
- 갭 식별: ① 일관성(시키지 않으면/낯선 문제엔 안 함) ② 재사용 가능한 "사람 행동→기계 구성요소" 매핑표 부재 ③ 언어 독립성(3종 대조) 미제시 ④ 낯선 문제에서 잘 건너뛰는 단계(암묵지 명시화, 상태→변수 매핑).

## GREEN (스킬 설계)
- 위 4개 갭을 직접 겨냥: 6단계 파이프라인(항상 이 순서) + 핵심 매핑표 + 3종 언어 대조 + "시키지 않아도 적용" 트리거.
- 워크드 예제: SKILL.md에 이진 탐색(투 포인터), worked-examples.md에 스택(괄호)/해시(two-sum)/누적변수(Kadane)로 일반화.
- 코드 정확성: Python 스니펫 4종(search/is_valid/two_sum/max_subarray) 경계 케이스 assert로 검증 → ALL OK.
- GREEN 검증: 스킬 적용 에이전트에게 *낯선 문제*(주식 사고팔기 최대이익) 투입해 파이프라인 일관 적용 확인.

## 파일
- `SKILL.md` — 파이프라인 + 매핑표 + 이진 탐색 워크드 예제(3종 언어) + 실수/레드플래그
- `worked-examples.md` — 스택/해시/DP 추가 예제(전부 6단계 적용)
- `README.md` — repo 소개/설치
- `log/changelog-2026-06-02.md` — 본 문서

## 배포
- private GitHub repo: cskwork/human-to-code-translation-skill (스킬 1개=repo 1개 기존 패턴 따름)
