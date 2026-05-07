---
name: presentation-styling
description: 프레젠테이션의 CSS 클래스, 컴포넌트 패턴, 구문 강조에 대한 지식
---

# Presentation Styling 스킬

`presentation/index.html`에서 사용되는 CSS 클래스와 HTML 패턴입니다.

## CSS 컴포넌트 클래스

### 레이아웃

- `.two-col` — 24px 간격의 2열 그리드 레이아웃
- `.info-grid` — 정보 카드를 위한 2열 그리드
- `.col-card` — 열 내부의 카드 (`.good`을 추가하면 녹색 테두리, `.bad`를 추가하면 빨간색 테두리)
- `.info-card` — 정보 그리드 내의 카드

### 콘텐츠 블록

- `.trigger-box` — 짙은 좌측 테두리가 있는 회색 박스 (핵심 개념, 전제조건용)
- `.how-to-trigger` — 녹색 테두리가 있는 녹색 박스 ("직접 해보기" 액션용)
- `.warning-box` — 경고 테두리가 있는 주황색 박스 (중요 경고용)
- `.code-block` — 모노스페이스 폰트의 짙은 코드 표시 블록

### 목록

- `.use-cases` — 아이콘+텍스트 목록 항목 컨테이너
- `.use-case-item` — 아이콘과 텍스트가 있는 개별 항목
- `.feature-list` — 간단한 테두리 목록

### 태그 & 배지

- `.matcher-tag` — 회색 인라인 pill 태그
- `.weight-badge` — 녹색 pill 배지 (가중치가 있는 슬라이드에 JS가 자동 삽입)

## 코드 블록 구문 강조

`.code-block` 내부에서 구문 색상 지정을 위해 다음 span을 사용합니다:

```html
<div class="code-block">
<span class="comment"># 이것은 주석입니다</span>
<span class="key">field_name</span>: <span class="string">value</span>
<span class="cmd">&gt;</span> 실행할 명령
</div>
```

- `.comment` — 주석에 녹색 (#6a9955)
- `.key` — 속성명/키에 파랑 (#9cdcfe)
- `.string` — 문자열 값에 주황 (#ce9178)
- `.cmd` — 명령/프롬프트에 노랑 (#dcdcaa)

## 슬라이드 유형 패턴

### 2열 콘텐츠 슬라이드 (좋은 예 vs 나쁜 예)
```html
<div class="slide" data-slide="N" data-weight="5">
    <h1>제목</h1>
    <div class="two-col">
        <div class="col-card bad">
            <h4>이전 (Vibe Coding)</h4>
            <!-- 나쁜 예시 -->
        </div>
        <div class="col-card good">
            <h4>이후 (Agentic)</h4>
            <!-- 좋은 예시 -->
        </div>
    </div>
</div>
```

슬라이드 HTML에 `<span class="weight-badge">`를 하드코딩하지 마세요. 프레젠테이션 JavaScript가 가중치 배지를 자동으로 삽입하고 제거합니다.

### 코드 예시가 있는 콘텐츠 슬라이드
```html
<div class="slide" data-slide="N">
    <h1>제목</h1>
    <div class="trigger-box">
        <h4>핵심 개념</h4>
        <p>설명</p>
    </div>
    <div class="code-block"><span class="comment"># 예시</span>
<span class="key">field</span>: <span class="string">value</span></div>
</div>
```

### 아이콘 목록 패턴
```html
<div class="use-cases">
    <div class="use-case-item">
        <span class="use-case-icon">이모지</span>
        <div class="use-case-text">
            <strong>제목</strong>
            <span>설명 텍스트</span>
        </div>
    </div>
</div>
```

## Journey Bar 관련

- `.journey-bar` — 진행 바 아래에 고정된 bar
- `.journey-bar.hidden` — 타이틀 슬라이드에서 숨김
- Journey bar 색상은 HSL 보간을 통해 빨간색(0%)에서 녹색(100%)으로 전환
- 가중치 배지는 JS가 가중치가 있는 슬라이드의 `h1` 요소에 자동 삽입
