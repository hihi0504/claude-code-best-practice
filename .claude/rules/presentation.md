---
paths:
  - "presentation/**"
---

# 프레젠테이션 위임

## 위임 규칙

프레젠테이션을 업데이트, 수정 또는 수정하는 모든 요청은 해당 프레젠테이션 에이전트가 처리해야 합니다. **프레젠테이션 HTML을 직접 편집하지 마세요.** 사용자가 언급한 프레젠테이션에 따라 라우팅합니다:

| 프레젠테이션 | 경로 | 에이전트 |
|---|---|---|
| Vibe Coding → Agentic Engineering | `presentation/vibe-coding-to-agentic-engineering/index.html` | `presentation-vibe-coding` |
| Claude Code & Gemini CLI (GDG Kolachi 이벤트 덱) | `presentation/2026-04-25-gdg-kolachi-cli-claude-code-gemini/index.html` | `presentation-claude-gemini` |
| Claude Code Best Practice (표준 재사용 가능 덱) | `presentation/claude-code-best-practice/index.html` | `presentation-claude-code` |

Agent 도구를 통해 호출합니다:

```
Agent(subagent_type="presentation-vibe-coding", description="...", prompt="...")
Agent(subagent_type="presentation-claude-gemini", description="...", prompt="...")
Agent(subagent_type="presentation-claude-code", description="...", prompt="...")
```

사용자가 어떤 것인지 지정하지 않고 "프레젠테이션"이라고만 말하면, 위임하기 전에 어떤 것인지 먼저 물어보세요. 참고로 "메인 프레젠테이션" 또는 "best-practice 덱"은 일반적으로 `presentation-claude-code` — 표준 재사용 가능 덱 —을 의미하지만 모호한 경우 확인하세요.

## 이유

각 프레젠테이션은 고유한 슬라이드 번호, 레벨 시스템, 대상 청중을 가집니다. 프레젠테이션별 에이전트를 통해 각 프레젠테이션은 서로를 오염시키지 않고 집중된 지식 베이스를 유지하고 발전할 수 있습니다. vibe-coding 에이전트는 해당 덱에 특화된 프레임워크/구조/스타일링 스킬을 사전 로드합니다. claude-gemini 에이전트는 비기술적 GDG 이벤트 청중을 대상으로 하며 6단계 레벨의 journey-bar를 사용합니다. claude-code 에이전트는 표준 재사용 가능 모범 사례 덱(2026-04-30에 GDG 덱에서 포크됨)을 소유합니다 — 동일한 청중 목소리, 더 단순한 구조(level-badge만, journey bar 없음), 이벤트 중립적 정체성.
