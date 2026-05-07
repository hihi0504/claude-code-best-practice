---
description: 두바이 날씨를 가져오고 SVG 날씨 카드를 생성합니다
model: haiku
allowed-tools:
  - AskUserQuestion
  - Agent
  - Skill
---

# Weather Orchestrator 명령어

두바이, UAE의 현재 기온을 가져와 시각적 SVG 날씨 카드를 생성합니다.

## 실행 계약 (비협상적)

이 명령어는 반드시 `weather-agent` 서브에이전트에 위임하여 완료해야 합니다. 다음은 금지됩니다:

- Bash, WebFetch 또는 기타 도구를 직접 사용하여 날씨 데이터 가져오기
- Step 1 건너뛰기 (사용자의 단위 선호도는 필수 입력입니다)
- 에이전트가 온도를 반환하기 전에 `weather-svg-creator` 호출

Agent 도구를 호출할 수 없는 경우 중단하고 사용자에게 오류를 보고하세요. 즉흥적으로 대처하지 마세요.

## 워크플로우

### Step 1: 사용자 선호도 묻기

AskUserQuestion 도구를 사용하여 사용자에게 섭씨 또는 화씨 중 어떤 온도를 원하는지 물어봅니다. 진행하기 전에 선택한 단위를 캡처합니다.

### Step 2: 에이전트를 통해 날씨 데이터 가져오기

Agent 도구를 사용하여 weather 에이전트를 호출합니다:

- subagent_type: weather-agent
- description: 두바이 날씨 데이터 가져오기
- prompt: 사용자가 요청한 [단위]로 두바이, UAE의 현재 기온을 가져오세요. 숫자 온도 값과 단위를 반환하세요. 에이전트는 상세 지침을 제공하는 weather-fetcher 스킬이 사전 로드되어 있습니다.
- model: haiku

에이전트가 완료될 때까지 기다리고 반환된 온도 값과 단위를 캡처합니다.

**Fail-closed 가드레일**: 에이전트가 숫자 온도와 단위를 반환하지 않으면 Step 3으로 진행하지 마세요. 사용자에게 실패를 보고하고 중단하세요.

### Step 3: SVG 날씨 카드 생성

Skill 도구를 사용하여 weather-svg-creator 스킬을 호출합니다:

- skill: weather-svg-creator

스킬은 Step 2의 온도 값과 단위(현재 컨텍스트에서 사용 가능)를 사용하여 SVG 카드를 생성하고 출력 파일을 작성합니다.

## 출력 요약

사용자에게 다음을 보여주는 명확한 요약을 제공합니다:

- 요청한 온도 단위
- 두바이에서 가져온 온도
- `orchestration-workflow/weather.svg`에 생성된 SVG 카드
- `orchestration-workflow/output.md`에 작성된 요약
