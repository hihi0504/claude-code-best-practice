---
description: 파키스탄 표준시(PKT, UTC+5) 기준으로 현재 시간을 표시합니다
---

# Time 명령어

파키스탄 표준시(PKT, UTC+5) 기준으로 현재 날짜와 시간을 표시합니다.

## 지침

1. PKT 기준으로 현재 시간을 가져오기 위해 다음 bash 명령어를 실행합니다:
   ```
   TZ='Asia/Karachi' date '+%Y-%m-%d %H:%M:%S %Z'
   ```

2. 결과를 다음 형식으로 사용자에게 표시합니다:
   ```
   Current Time in Pakistan (PKT): YYYY-MM-DD HH:MM:SS PKT
   ```

## 요구사항

- 항상 `Asia/Karachi` 타임존(UTC+5)을 사용합니다
- 24시간 형식을 사용합니다
- 시간과 함께 날짜를 포함합니다
- 출력을 간결하게 유지합니다
