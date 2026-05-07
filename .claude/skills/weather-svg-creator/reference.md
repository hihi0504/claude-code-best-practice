# Weather SVG Creator — 참조 자료

## SVG 템플릿

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 300 160" width="300" height="160">
  <rect width="300" height="160" rx="12" fill="#1a1a2e"/>
  <text x="150" y="45" text-anchor="middle" fill="#8892b0" font-family="system-ui" font-size="14">Unit: [Celsius/Fahrenheit]</text>
  <text x="150" y="100" text-anchor="middle" fill="#ccd6f6" font-family="system-ui" font-size="42" font-weight="bold">[value]°[C/F]</text>
  <text x="150" y="140" text-anchor="middle" fill="#64ffda" font-family="system-ui" font-size="16">Dubai, UAE</text>
</svg>
```

### 플레이스홀더

| 플레이스홀더 | 교체 값 | 예시 |
|-------------|-------------|---------|
| `[Celsius/Fahrenheit]` | 입력의 전체 단위명 | `Celsius` |
| `[value]` | 입력의 숫자 온도 | `26.2` |
| `[C/F]` | 단위 약어 | `C` 또는 `F` |

### 디자인 사양

| 속성 | 값 |
|----------|-------|
| 크기 | 300 x 160 px |
| 모서리 반경 | 12 px |
| 배경 | `#1a1a2e` (짙은 남색) |
| 단위 레이블 | `#8892b0` (부드러운 파랑), 14px |
| 온도 | `#ccd6f6` (연한 파랑), 42px 굵게 |
| 위치 | `#64ffda` (청록색 강조), 16px |
| 폰트 | `system-ui` |
| 모든 텍스트 | 가운데 정렬 (`text-anchor="middle"` at x=150) |

---

## 출력 마크다운 템플릿

```markdown
# Weather Result

## Temperature
[value]°[C/F]

## Location
Dubai, UAE

## Unit
[Celsius/Fahrenheit]

## SVG Card
![Weather Card](weather.svg)
```

---

## 출력 경로

| 파일 | 경로 |
|------|------|
| SVG 카드 | `orchestration-workflow/weather.svg` |
| 마크다운 요약 | `orchestration-workflow/output.md` |
