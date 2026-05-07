---
paths:
  - "**/*.md"
---

# Markdown 문서

## 문서 표준

- 파일을 집중적이고 간결하게 유지 — 파일당 하나의 주제
- 문서 간 링크는 상대 링크 사용 (예: `../best-practice/claude-memory.md`), 절대 GitHub URL 사용 금지
- best-practice 및 report 문서 상단에 뒤로 가기 링크 포함 (기존 파일의 패턴 참조)
- 새 개념이나 보고서를 추가할 때 README.md의 해당 테이블(CONCEPTS 또는 REPORTS) 업데이트

## 구조 관례

- 모범 사례 문서는 `best-practice/`에 위치
- 구현 문서는 `implementation/`에 위치
- 보고서는 `reports/`에 위치
- 팁은 `tips/`에 위치
- 변경 로그 추적은 `changelog/<category>/`에 위치

## 포맷

- 구조적 비교를 위해 테이블 사용 (참조: README CONCEPTS 테이블)
- 모범 사례나 구현 문서를 링크할 때 `!/tags/`의 배지 이미지 사용
- 제목은 계층적으로 유지 — 레벨 건너뛰지 않기 (예: `##`에서 `####`로 바로 가지 않기)
