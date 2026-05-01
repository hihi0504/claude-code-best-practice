---
name: agent-browser
description: AI 에이전트를 위한 브라우저 자동화 CLI. 사용자가 웹사이트 탐색, 양식 작성, 버튼 클릭, 스크린샷 촬영, 데이터 추출, 웹 앱 테스트, 또는 기타 브라우저 자동화 작업이 필요할 때 사용하세요. "웹사이트 열기", "양식 작성", "버튼 클릭", "스크린샷 촬영", "페이지에서 데이터 스크래핑", "웹 앱 테스트", "사이트 로그인", "브라우저 동작 자동화" 등의 요청이나 프로그래밍 방식의 웹 상호작용이 필요한 모든 작업에 사용됩니다.
allowed-tools: Bash(agent-browser:*)
---

# agent-browser를 이용한 브라우저 자동화

## 핵심 워크플로우

모든 브라우저 자동화는 이 패턴을 따릅니다:

1. **탐색**: `agent-browser open <url>`
2. **스냅샷**: `agent-browser snapshot -i` (요소 참조 `@e1`, `@e2` 등 가져오기)
3. **상호작용**: 참조를 사용하여 클릭, 입력, 선택
4. **재스냅샷**: 네비게이션이나 DOM 변경 후 새로운 참조 가져오기

```bash
agent-browser open https://example.com/form
agent-browser snapshot -i
# 출력: @e1 [input type="email"], @e2 [input type="password"], @e3 [button] "Submit"

agent-browser fill @e1 "user@example.com"
agent-browser fill @e2 "password123"
agent-browser click @e3
agent-browser wait --load networkidle
agent-browser snapshot -i  # 결과 확인
```

## 필수 명령어

```bash
# 탐색
agent-browser open <url>              # 탐색 (별칭: goto, navigate)
agent-browser close                   # 브라우저 닫기

# 스냅샷
agent-browser snapshot -i             # 참조가 있는 인터랙티브 요소 (권장)
agent-browser snapshot -i -C          # 커서 인터랙티브 요소 포함 (onclick이 있는 div, cursor:pointer)
agent-browser snapshot -s "#selector" # CSS 선택자로 범위 지정

# 상호작용 (스냅샷의 @refs 사용)
agent-browser click @e1               # 요소 클릭
agent-browser fill @e2 "text"         # 초기화 후 텍스트 입력
agent-browser type @e2 "text"         # 초기화 없이 텍스트 입력
agent-browser select @e1 "option"     # 드롭다운 옵션 선택
agent-browser check @e1               # 체크박스 체크
agent-browser press Enter             # 키 누르기
agent-browser scroll down 500         # 페이지 스크롤

# 정보 가져오기
agent-browser get text @e1            # 요소 텍스트 가져오기
agent-browser get url                 # 현재 URL 가져오기
agent-browser get title               # 페이지 제목 가져오기

# 대기
agent-browser wait @e1                # 요소 대기
agent-browser wait --load networkidle # 네트워크 유휴 대기
agent-browser wait --url "**/page"    # URL 패턴 대기
agent-browser wait 2000               # 밀리초 대기

# 캡처
agent-browser screenshot              # 임시 디렉토리에 스크린샷
agent-browser screenshot --full       # 전체 페이지 스크린샷
agent-browser pdf output.pdf          # PDF로 저장
```

## 일반적인 패턴

### 양식 제출

```bash
agent-browser open https://example.com/signup
agent-browser snapshot -i
agent-browser fill @e1 "Jane Doe"
agent-browser fill @e2 "jane@example.com"
agent-browser select @e3 "California"
agent-browser check @e4
agent-browser click @e5
agent-browser wait --load networkidle
```

### 상태 유지를 통한 인증

```bash
# 한 번 로그인하고 상태 저장
agent-browser open https://app.example.com/login
agent-browser snapshot -i
agent-browser fill @e1 "$USERNAME"
agent-browser fill @e2 "$PASSWORD"
agent-browser click @e3
agent-browser wait --url "**/dashboard"
agent-browser state save auth.json

# 이후 세션에서 재사용
agent-browser state load auth.json
agent-browser open https://app.example.com/dashboard
```

### 데이터 추출

```bash
agent-browser open https://example.com/products
agent-browser snapshot -i
agent-browser get text @e5           # 특정 요소 텍스트 가져오기
agent-browser get text body > page.txt  # 모든 페이지 텍스트 가져오기

# 파싱을 위한 JSON 출력
agent-browser snapshot -i --json
agent-browser get text @e1 --json
```

### 병렬 세션

```bash
agent-browser --session site1 open https://site-a.com
agent-browser --session site2 open https://site-b.com

agent-browser --session site1 snapshot -i
agent-browser --session site2 snapshot -i

agent-browser session list
```

### 시각적 브라우저 (디버깅)

```bash
agent-browser --headed open https://example.com
agent-browser highlight @e1          # 요소 강조 표시
agent-browser record start demo.webm # 세션 녹화
```

### 로컬 파일 (PDF, HTML)

```bash
# file:// URL로 로컬 파일 열기
agent-browser --allow-file-access open file:///path/to/document.pdf
agent-browser --allow-file-access open file:///path/to/page.html
agent-browser screenshot output.png
```

### iOS 시뮬레이터 (Mobile Safari)

```bash
# 사용 가능한 iOS 시뮬레이터 목록
agent-browser device list

# 특정 기기의 Safari 실행
agent-browser -p ios --device "iPhone 16 Pro" open https://example.com

# 데스크톱과 동일한 워크플로우 - 스냅샷, 상호작용, 재스냅샷
agent-browser -p ios snapshot -i
agent-browser -p ios tap @e1          # 탭 (클릭의 별칭)
agent-browser -p ios fill @e2 "text"
agent-browser -p ios swipe up         # 모바일 전용 제스처

# 스크린샷
agent-browser -p ios screenshot mobile.png

# 세션 닫기 (시뮬레이터 종료)
agent-browser -p ios close
```

**요구사항:** Xcode가 있는 macOS, Appium (`npm install -g appium && appium driver install xcuitest`)

**실제 기기:** 미리 구성된 경우 물리적 iOS 기기에서도 작동합니다. `xcrun xctrace list devices`의 UDID를 사용하여 `--device "<UDID>"`를 사용하세요.

## 참조 라이프사이클 (중요)

참조(`@e1`, `@e2` 등)는 페이지가 변경되면 무효화됩니다. 다음 작업 후에는 항상 재스냅샷하세요:

- 탐색하는 링크나 버튼 클릭
- 양식 제출
- 동적 콘텐츠 로드 (드롭다운, 모달)

```bash
agent-browser click @e5              # 새 페이지로 이동
agent-browser snapshot -i            # 반드시 재스냅샷
agent-browser click @e1              # 새로운 참조 사용
```

## 시맨틱 로케이터 (참조의 대안)

참조를 사용할 수 없거나 신뢰할 수 없는 경우 시맨틱 로케이터를 사용하세요:

```bash
agent-browser find text "Sign In" click
agent-browser find label "Email" fill "user@test.com"
agent-browser find role button click --name "Submit"
agent-browser find placeholder "Search" type "query"
agent-browser find testid "submit-btn" click
```

## 상세 문서

| 참조 | 사용 시기 |
|-----------|------------|
| [references/commands.md](references/commands.md) | 모든 옵션이 있는 전체 명령어 참조 |
| [references/snapshot-refs.md](references/snapshot-refs.md) | 참조 라이프사이클, 무효화 규칙, 문제 해결 |
| [references/session-management.md](references/session-management.md) | 병렬 세션, 상태 유지, 동시 스크래핑 |
| [references/authentication.md](references/authentication.md) | 로그인 흐름, OAuth, 2FA 처리, 상태 재사용 |
| [references/video-recording.md](references/video-recording.md) | 디버깅 및 문서화를 위한 워크플로우 녹화 |
| [references/proxy-support.md](references/proxy-support.md) | 프록시 구성, 지역 테스트, 회전 프록시 |

## 바로 사용 가능한 템플릿

| 템플릿 | 설명 |
|----------|-------------|
| [templates/form-automation.sh](templates/form-automation.sh) | 유효성 검사가 있는 양식 자동 입력 |
| [templates/authenticated-session.sh](templates/authenticated-session.sh) | 한 번 로그인, 상태 재사용 |
| [templates/capture-workflow.sh](templates/capture-workflow.sh) | 스크린샷을 포함한 콘텐츠 추출 |

```bash
./templates/form-automation.sh https://example.com/form
./templates/authenticated-session.sh https://app.example.com/login
./templates/capture-workflow.sh https://example.com ./output
```
