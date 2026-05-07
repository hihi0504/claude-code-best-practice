---
description: 최신 Claude Code 기능과 개념으로 README CONCEPTS 섹션을 업데이트합니다
argument-hint: [확인할 변경 로그 버전 수, 기본값 10]
---

# Workflow Changelog — README Concepts

저는 claude-code-best-practice 프로젝트의 코디네이터입니다. 두 개의 리서치 에이전트를 병렬로 실행하고, 결과를 기다리고, 결과를 합치고, **README CONCEPTS 섹션**(`README.md`)의 드리프트에 관한 통합 보고서를 제출하는 것이 역할입니다.

**확인할 버전:** `$ARGUMENTS` (비어 있거나 숫자가 아닌 경우 기본값: 10)

**읽기 후 보고** 워크플로우입니다. 에이전트를 실행하고, 결과를 합치고, 보고서를 작성합니다. 사용자가 승인한 경우에만 작업을 수행합니다.

---

## Phase 0: 두 에이전트 병렬 실행

Task 도구를 사용하여 **동일한 메시지에서** 두 에이전트를 **즉시** 병렬로 실행합니다:

### 에이전트 1: workflow-concepts-agent

`subagent_type: "workflow-concepts-agent"`를 사용하여 실행합니다. 이 프롬프트를 제공합니다:

> claude-code-best-practice 프로젝트의 README CONCEPTS 섹션 드리프트를 리서치하세요. 마지막 $ARGUMENTS 버전(기본값: 10)을 확인하세요.
>
> 다음 3개의 외부 소스를 가져오세요:
> 1. Claude Code 문서 인덱스: https://code.claude.com/docs/en
> 2. Claude Code 변경 로그: https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
> 3. Claude Code 기능 개요: https://code.claude.com/docs/en/overview
>
> 그런 다음 로컬 README.md (특히 CONCEPTS 테이블), CLAUDE.md, `reports/claude-global-vs-project-settings.md`를 읽으세요. 공식 문서가 Claude Code 개념/기능으로 나열하는 것과 README CONCEPTS 테이블이 문서화하는 것 사이의 차이점을 분석하세요. 누락된 개념, 변경된 개념, 폐기된 개념, URL 정확성, 설명 정확성, 뱃지 정확성을 다루는 구조화된 결과 보고서를 반환하세요.

### 에이전트 2: claude-code-guide

`subagent_type: "claude-code-guide"`를 사용하여 실행합니다. 이 프롬프트를 제공합니다:

> 최신 Claude Code 기능과 개념을 리서치하세요. 문서화되어야 할 모든 Claude Code 개념/기능의 완전한 목록을 찾아야 합니다. 각각에 대해 다음을 제공하세요:
> 1. 공식 기능 이름
> 2. 공식 문서 URL
> 3. 파일 시스템 위치 (예: `.claude/commands/`, `~/.claude/teams/`)
> 4. 간략한 설명 (한 줄)
> 5. 도입 시기 (알려진 경우 버전/날짜)
>
> 특히 다음 잠재적으로 누락된 개념을 확인하세요:
> - **워크트리** — 병렬 개발을 위한 git 워크트리 격리
> - **에이전트 팀** — 멀티 에이전트 조정
> - **태스크** — 세션 간 지속적인 작업 목록
> - **자동 메모리** — Claude의 자가 작성 프로젝트 학습
> - **키바인딩** — 사용자 정의 키보드 단축키
> - **원격 연결** — SSH, Docker, 클라우드 개발
> - **IDE 통합** — VS Code, JetBrains 확장
> - **모델 구성** — 모델 선택 및 라우팅
> - **GitHub 통합** — PR 리뷰, 이슈 분류
> - 최근 Claude Code 버전의 다른 개념
>
> 철저하게 진행하세요 — 웹을 검색하고, 문서를 가져오고, 발견한 모든 것에 대한 구체적인 버전 번호와 세부 정보를 제공하세요.

두 에이전트는 독립적으로 실행되어 결과를 반환합니다.

---

## Phase 0.5: 검증 체크리스트 읽기

**에이전트가 실행되는 동안**, 존재하는 경우 `changelog/best-practice/concepts/verification-checklist.md`를 읽습니다. 이 파일에는 누적된 검증 규칙이 포함되어 있습니다. 아직 존재하지 않으면 이 단계를 건너뜁니다 — Phase 2에서 생성됩니다.

---

## Phase 1: 이전 변경 로그 항목 읽기

**결과를 합치기 전에**, 존재하는 경우 `changelog/best-practice/concepts/changelog.md` 파일을 읽어 이전 변경 로그 항목을 가져옵니다. 각 항목은 `---`로 구분됩니다. 이전 항목에서 우선순위 작업을 파싱하여 현재 결과와 비교합니다. 이를 통해 다음을 식별할 수 있습니다:
- **반복 항목** — 이전에 나타났고 아직 해결되지 않은 문제
- **새로 해결된 항목** — 이전 실행에서 현재 수정된 문제
- **새 항목** — 이번 실행에서 처음 나타나는 문제

파일이 아직 존재하지 않으면 모든 항목은 `NEW`입니다.

---

## Phase 2: 결과 합치기 및 보고서 생성

**두 에이전트가 완료될 때까지 기다립니다.** 다음이 갖춰지면:
- **workflow-concepts-agent 결과** — 로컬 파일 읽기, 외부 문서 가져오기, 드리프트 감지가 포함된 상세 분석
- **claude-code-guide 결과** — 최신 Claude Code 기능 및 개념에 대한 독립적 리서치

두 결과를 교차 참조합니다. 전용 에이전트는 CONCEPTS별 드리프트 분석을 제공하고, claude-code-guide 에이전트는 그것이 놓친 것(예: 매우 최근의 변경 사항, 문서화되지 않은 기능, 웹 검색의 컨텍스트)을 발견할 수 있습니다. 두 에이전트 간의 모순이 있으면 사용자에게 해결을 위해 표시합니다.

**검증 체크리스트 실행 (존재하는 경우):** `changelog/best-practice/concepts/verification-checklist.md`의 모든 규칙에 대해, 확인을 수행합니다. 보고서에 **검증 로그** 섹션을 포함합니다.

**필요시 체크리스트 업데이트:** 결과가 기존 체크리스트 규칙이 다루지 않는 새로운 유형의 드리프트를 드러내는 경우, `changelog/best-practice/concepts/verification-checklist.md`에 새 규칙을 추가합니다. 파일이 존재하지 않으면 생성합니다. 규칙에는 카테고리, 확인할 내용, 깊이 수준, 비교할 소스, 추가 날짜, 출처가 포함되어야 합니다.

또한 현재 결과를 이전 변경 로그 항목(Phase 1에서)과 비교합니다. 각 우선순위 작업에 대해 다음으로 표시합니다:
- `NEW` — 이 문제가 처음 나타남
- `RECURRING` — 이전 실행에서 나타났고 아직 해결되지 않음 (처음 나타난 실행 날짜 포함)
- `RESOLVED` — 이전 실행에서 나타났지만 이제 수정됨 (해결 날짜 포함)

다음 섹션으로 구조화된 보고서를 작성합니다:

1. **누락된 개념** — 공식 문서에는 있지만 CONCEPTS 테이블에 없는 기능/개념, 포함:
   - 공식 이름과 문서 URL
   - 권장 Location 열 값
   - 권장 Description 열 값
   - 붙여넣기 준비된 정확한 마크다운 테이블 행
   - 도입 버전 (알려진 경우)
2. **변경된 개념** — 이름, URL, 위치, 설명이 변경된 개념
3. **폐기/제거된 개념** — CONCEPTS 테이블에는 있지만 공식 문서에 없는 개념
4. **URL 정확성** — 개념별 URL 확인
5. **설명 정확성** — 개념별 설명/위치 확인
6. **뱃지 정확성** — 뱃지 링크 확인 및 누락된 뱃지 권장 사항
7. **claude-code-guide 에이전트 결과** — 전용 에이전트가 캡처하지 못한 에이전트의 고유한 인사이트. 새로운 정보를 추가하는 결과만 포함합니다. 모순을 표시합니다.

우선순위 **액션 아이템** 요약 테이블로 끝냅니다:

```
Priority Actions:
#  | Type                | Action                                     | Status
1  | Missing Concept     | Add <concept> row to CONCEPTS table         | NEW
2  | Changed URL         | Update <concept> docs link                  | NEW
3  | Changed Description | Update <concept> description                | RECURRING (처음 발견: <날짜>)
4  | Deprecated Concept  | Remove <concept> row from CONCEPTS table    | NEW
5  | Broken Badge        | Fix badge link for <concept>                | NEW
```

이전 실행에서 더 이상 문제가 없는 항목을 나열하는 **마지막 실행 이후 해결된 항목** 섹션도 포함합니다.

---

## Phase 2.5: 변경 로그에 요약 추가

**이 단계는 필수 — 사용자에게 보고서를 제출하기 전에 항상 실행합니다.**

기존 `changelog/best-practice/concepts/changelog.md` 파일을 읽은 다음 끝에 새 항목을 **추가**(덮어쓰지 말 것)합니다. 파일이 존재하지 않으면 Status 범례 테이블과 첫 번째 항목으로 만듭니다. 항목 형식은 정확히 다음과 같아야 합니다:

```markdown
---

## [<YYYY-MM-DD HH:MM AM/PM PKT>] Claude Code v<VERSION>

| # | Priority | Type | Action | Status |
|---|----------|------|--------|--------|
| 1 | HIGH/MED/LOW | <type> | <action description> | <status> |
| ... | ... | ... | ... | ... |
```

**Status 형식 — 다음 세 가지 형식 중 하나를 사용해야 합니다:**
- `COMPLETE (이유)` — 작업이 수행되어 성공적으로 해결됨
- `INVALID (이유)` — 결과가 잘못되었거나 적용 불가능하거나 의도적임
- `ON HOLD (이유)` — 외부 의존성 또는 사용자 결정 대기로 연기됨

`(이유)`는 필수이며 무엇이 수행되었는지 또는 이유를 간략히 설명해야 합니다.

**추가 규칙:**
- 항상 추가 — 이전 항목을 덮어쓰거나 교체하지 마세요
- 날짜와 시간은 파키스탄 표준시(PKT, UTC+5) 기준으로 명령이 실행될 때; `TZ=Asia/Karachi date "+%Y-%m-%d %I:%M %p PKT"`를 실행하여 가져오세요. 버전은 에이전트 결과에서 가져옵니다
- 각 항목은 `---`로 구분됩니다
- **HIGH, MEDIUM, LOW 우선순위 항목만 포함** — NONE 우선순위 항목은 제외

---

## Phase 2.6: 마지막 업데이트 뱃지 업데이트

**이 단계는 필수 — Phase 2.5 직후, 보고서 제출 전에 항상 실행합니다.**

`README.md` 상단(3번째 줄)의 "Last Updated" 뱃지를 업데이트합니다. `TZ=Asia/Karachi date "+%b %d, %Y %-I:%M %p PKT"`를 실행하여 시간을 가져오고, URL 인코딩(공백은 `%20`, 쉼표는 `%2C`)하고, 뱃지의 날짜 부분을 교체합니다.

**뱃지 업데이트를 변경 로그나 보고서의 액션 아이템으로 기록하지 마세요.**

---

## Phase 2.7: 모든 CONCEPTS URL 검증

**이 단계는 필수 — Phase 2.6 이후, 보고서 제출 전에 항상 실행합니다.**

CONCEPTS 테이블의 각 개념에 대해:

1. **외부 문서 URL** (예: `https://code.claude.com/docs/en/skills`): WebFetch를 사용하여 각 URL을 가져오고 유효한 페이지를 반환하는지 확인합니다. 끊기거나 이동된 링크를 표시합니다.
2. **로컬 뱃지 링크** (예: `best-practice/claude-commands.md`): Read 도구를 사용하여 파일이 존재하는지 확인합니다. 끊긴 링크를 표시합니다.
3. **구현 뱃지 링크** (예: `.claude/commands/`): 경로가 존재하는지 확인합니다.

보고서에 **URL 검증 로그**를 포함합니다:

```
URL Validation Log:
#  | Concept     | URL Type  | URL                                           | Status | Notes
1  | Commands    | External  | https://code.claude.com/docs/en/skills         | OK     |
2  | Commands    | Badge     | best-practice/claude-commands.md               | OK     |
3  | Sub-Agents  | External  | https://code.claude.com/docs/en/sub-agents     | OK     |
...
```

**URL이 끊긴 경우** HIGH 우선순위 액션 아이템으로 추가합니다.

---

## Phase 3: 작업 제안

보고서 제출 후(변경 로그가 업데이트되었는지 확인), 사용자에게 묻습니다:

1. **모든 작업 실행** — 누락된 개념 추가, 변경된 개념 업데이트, 폐기된 개념 제거
2. **특정 작업 실행** — 사용자가 실행할 번호 선택
3. **보고서만 저장** — 변경 없음

실행 시:
- **누락된 개념**: 기존 형식을 따라 `README.md`의 CONCEPTS 테이블에 새 행 추가:
  ```
  | [**Name**](docs-url) | `location` | Description |
  ```
  해당 파일이 존재하는 경우에만 뱃지(베스트 프랙티스, 구현)를 추가합니다.
- **변경된 개념**: 변경된 특정 열 업데이트
- **폐기된 개념**: 행 제거 전 사용자 확인
- **끊긴 URL**: 현재 유효한 URL로 수정
- **뱃지 수정**: 뱃지 링크를 올바른 파일 경로로 업데이트
- 기존 테이블과 일관된 알파벳 또는 논리적 순서 유지
- 모든 작업 후 일관성을 위해 CONCEPTS 테이블 재확인

---

## 중요 규칙

1. **두 에이전트를 단일 메시지에서 병렬로 실행** — 순차적으로 실행하지 마세요
2. **두 에이전트가 완료될 때까지 기다리기** — 보고서를 생성하기 전에
3. **버전, URL, 날짜를 추측하지 마세요** — 에이전트의 데이터 사용
4. **누락된 개념은 최고 우선순위** — CONCEPTS 테이블은 개발자들이 처음 보는 것입니다
5. **모든 URL 확인** — 끊긴 링크는 전체 프로젝트에 대한 신뢰를 저하시킵니다
6. **자동 실행하지 마세요** — 항상 먼저 보고서 제출
7. **항상 변경 로그에 추가** — Phase 2.5는 필수입니다. 절대 건너뛰지 마세요. 이전 항목을 덮어쓰지 마세요.
8. **이전 실행과 비교** — 변경 로그에서 이전 항목을 읽고 각 액션 아이템을 NEW, RECURRING, RESOLVED로 표시합니다.
9. **존재하는 경우 검증 체크리스트 실행** — verification-checklist.md를 읽고 모든 규칙을 실행합니다. 파일이 존재하지 않고 지속적인 규칙을 보증하는 결과가 있으면 만듭니다.
10. **항상 마지막 업데이트 뱃지 업데이트** — Phase 2.6은 필수입니다.
11. **항상 모든 CONCEPTS URL 검증** — Phase 2.7은 필수입니다. 끊긴 URL은 HIGH 우선순위입니다.
12. **붙여넣기 준비된 행 제공** — 누락된 개념의 경우 실행이 복사-붙여넣기로 가능하도록 정확한 마크다운 테이블 행을 포함합니다.
13. **기존 테이블 형식 준수** — 기존 행의 열 구조, 뱃지 패턴, 링크 스타일에 맞춥니다.
