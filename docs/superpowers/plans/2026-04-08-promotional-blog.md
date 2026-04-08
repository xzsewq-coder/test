# 홍보용 개인 블로그 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 강의 및 홈페이지 제작 의뢰 유치를 위한 개인 홍보 블로그를 정적 HTML/CSS/JS로 구현하고 GitHub Pages에 배포한다.

**Architecture:** 단일 페이지 랜딩(index.html) + 블로그 섹션(posts/)으로 구성. 모든 스타일은 style.css에 집중 관리, 인터랙션은 main.js에 분리. 빌드 도구 없이 순수 HTML/CSS/JS만 사용.

**Tech Stack:** HTML5, CSS3 (CSS Variables, Flexbox, Grid), Vanilla JS, Pretendard 폰트 (CDN), GitHub Pages

> **커밋 방법:** 모든 커밋/푸시는 소스트리(SourceTree)로 직접 처리. 각 태스크 완료 후 소스트리에서 스테이징 → 커밋 → 푸시.

---

## 파일 목록

| 파일 | 역할 |
|------|------|
| `index.html` | 메인 랜딩 페이지 (전체 섹션) |
| `style.css` | 전체 스타일 (CSS 변수, 레이아웃, 컴포넌트, 반응형) |
| `main.js` | 인터랙션 (스티키 헤더, 스무스 스크롤, 햄버거 메뉴) |
| `posts/index.html` | 블로그 글 목록 페이지 |
| `posts/2026-04-08-첫번째-글.html` | 블로그 개별 글 템플릿 |
| `assets/images/` | 포트폴리오 스크린샷, 프로필 사진 |

---

## Task 1: 프로젝트 기반 세팅 (CSS 변수 + 폰트)

**Files:**
- Modify: `index.html`
- Create: `style.css`

- [ ] **Step 1: index.html 기본 구조 작성**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>홍길동 | 강의 & 홈페이지 제작</title>
  <meta name="description" content="웹 개발 강의와 홈페이지 제작 전문가 홍길동입니다." />
  <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <!-- 섹션들이 여기 들어갑니다 -->
  <script src="main.js"></script>
</body>
</html>
```

- [ ] **Step 2: style.css CSS 변수 및 기본 리셋 작성**

```css
/* ===== CSS 변수 ===== */
:root {
  --bg-primary: #0d1117;
  --bg-secondary: #161b22;
  --bg-card: #1c2128;
  --accent: #1d6ae5;
  --accent-hover: #2979f5;
  --text-primary: #e6edf3;
  --text-secondary: #8b949e;
  --border: #30363d;
  --radius: 8px;
  --max-width: 1100px;
  --font: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
}

/* ===== 리셋 ===== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; }

body {
  background-color: var(--bg-primary);
  color: var(--text-primary);
  font-family: var(--font);
  font-size: 16px;
  line-height: 1.6;
}

a { color: inherit; text-decoration: none; }

img { max-width: 100%; display: block; }

/* ===== 공통 레이아웃 ===== */
.container {
  max-width: var(--max-width);
  margin: 0 auto;
  padding: 0 24px;
}

section {
  padding: 80px 0;
}

.section-title {
  font-size: 2rem;
  font-weight: 700;
  margin-bottom: 48px;
  text-align: center;
}

.section-title span {
  color: var(--accent);
}

/* ===== 버튼 ===== */
.btn {
  display: inline-block;
  padding: 12px 28px;
  border-radius: var(--radius);
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s, transform 0.1s;
}

.btn-primary {
  background: var(--accent);
  color: #fff;
}

.btn-primary:hover {
  background: var(--accent-hover);
  transform: translateY(-1px);
}

.btn-outline {
  background: transparent;
  color: var(--text-primary);
  border: 2px solid var(--border);
}

.btn-outline:hover {
  border-color: var(--accent);
  color: var(--accent);
  transform: translateY(-1px);
}
```

- [ ] **Step 3: 브라우저에서 index.html 열어 배경색 #0d1117이 적용됐는지 확인**

브라우저에서 파일을 직접 열거나 VS Code Live Server로 열기. 배경이 거의 검정색이면 정상.

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: 프로젝트 기반 세팅 - CSS 변수, 리셋, 공통 스타일`

---

## Task 2: 네비게이션 헤더

**Files:**
- Modify: `index.html` (header 추가)
- Modify: `style.css` (nav 스타일 추가)
- Create: `main.js` (스티키 + 햄버거 JS)

- [ ] **Step 1: index.html에 header 추가 (`<body>` 안 최상단)**

```html
<header class="navbar" id="navbar">
  <div class="container navbar__inner">
    <a href="#" class="navbar__logo">홍길동</a>
    <nav class="navbar__menu" id="navMenu">
      <a href="#about">소개</a>
      <a href="#services">서비스</a>
      <a href="#portfolio">포트폴리오</a>
      <a href="#blog">블로그</a>
      <a href="#contact" class="btn btn-primary">문의하기</a>
    </nav>
    <button class="navbar__toggle" id="navToggle" aria-label="메뉴 열기">
      <span></span><span></span><span></span>
    </button>
  </div>
</header>
```

- [ ] **Step 2: style.css에 navbar 스타일 추가**

```css
/* ===== 네비게이션 ===== */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 16px 0;
  transition: background 0.3s, box-shadow 0.3s;
}

.navbar.scrolled {
  background: rgba(13, 17, 23, 0.95);
  backdrop-filter: blur(8px);
  box-shadow: 0 1px 0 var(--border);
}

.navbar__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.navbar__logo {
  font-size: 1.25rem;
  font-weight: 700;
  color: var(--text-primary);
}

.navbar__menu {
  display: flex;
  align-items: center;
  gap: 32px;
}

.navbar__menu a {
  color: var(--text-secondary);
  font-size: 0.95rem;
  transition: color 0.2s;
}

.navbar__menu a:hover {
  color: var(--text-primary);
}

.navbar__toggle {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 4px;
}

.navbar__toggle span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--text-primary);
  transition: transform 0.3s, opacity 0.3s;
}

/* 햄버거 열림 상태 */
.navbar__toggle.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.navbar__toggle.open span:nth-child(2) { opacity: 0; }
.navbar__toggle.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

/* ===== 반응형: 모바일 메뉴 ===== */
@media (max-width: 768px) {
  .navbar__toggle { display: flex; }

  .navbar__menu {
    display: none;
    position: absolute;
    top: 100%;
    left: 0;
    right: 0;
    flex-direction: column;
    background: var(--bg-secondary);
    padding: 24px;
    gap: 20px;
    border-top: 1px solid var(--border);
  }

  .navbar__menu.open { display: flex; }
}
```

- [ ] **Step 3: main.js 생성 — 스티키 헤더 + 햄버거 메뉴**

```js
// ===== 스티키 헤더 =====
const navbar = document.getElementById('navbar');
window.addEventListener('scroll', () => {
  if (window.scrollY > 20) {
    navbar.classList.add('scrolled');
  } else {
    navbar.classList.remove('scrolled');
  }
});

// ===== 햄버거 메뉴 =====
const navToggle = document.getElementById('navToggle');
const navMenu = document.getElementById('navMenu');

navToggle.addEventListener('click', () => {
  navToggle.classList.toggle('open');
  navMenu.classList.toggle('open');
});

// 메뉴 링크 클릭 시 모바일 메뉴 닫기
navMenu.querySelectorAll('a').forEach(link => {
  link.addEventListener('click', () => {
    navToggle.classList.remove('open');
    navMenu.classList.remove('open');
  });
});
```

- [ ] **Step 4: 브라우저에서 확인**

- 데스크탑: 가로 메뉴가 보이는지 확인
- 브라우저 창을 768px 이하로 줄이면 햄버거 버튼이 나타나는지 확인
- 햄버거 클릭 시 메뉴가 열리고 닫히는지 확인
- 스크롤하면 헤더 배경이 불투명해지는지 확인

- [ ] **Step 5: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`, `main.js`  
커밋 메시지: `feat: 스티키 네비게이션 헤더, 햄버거 메뉴`

---

## Task 3: Hero 섹션

**Files:**
- Modify: `index.html` (hero 섹션 추가)
- Modify: `style.css` (hero 스타일 추가)

- [ ] **Step 1: index.html header 아래에 hero 섹션 추가**

```html
<main>
  <!-- ===== HERO ===== -->
  <section class="hero" id="home">
    <div class="container hero__inner">
      <p class="hero__badge">웹 개발 강의 · 홈페이지 제작</p>
      <h1 class="hero__title">안녕하세요,<br /><span>홍길동</span>입니다</h1>
      <p class="hero__desc">
        실무 중심 웹 개발 강의와 맞춤형 홈페이지 제작으로<br />
        당신의 온라인 성장을 함께 만들어갑니다.
      </p>
      <div class="hero__cta">
        <a href="https://open.kakao.com/YOUR_LINK" class="btn btn-primary" target="_blank" rel="noopener">강의 신청하기</a>
        <a href="mailto:your@email.com" class="btn btn-outline">제작 문의하기</a>
      </div>
    </div>
  </section>
```

> **주의:** `https://open.kakao.com/YOUR_LINK`와 `your@email.com`은 실제 링크로 교체 필요.

- [ ] **Step 2: style.css에 hero 스타일 추가**

```css
/* ===== HERO ===== */
.hero {
  min-height: 100vh;
  display: flex;
  align-items: center;
  padding-top: 80px; /* navbar 높이 보정 */
  background: radial-gradient(ellipse at 60% 40%, rgba(29, 106, 229, 0.12) 0%, transparent 60%);
}

.hero__inner {
  max-width: 720px;
}

.hero__badge {
  display: inline-block;
  padding: 6px 14px;
  border: 1px solid var(--accent);
  border-radius: 20px;
  color: var(--accent);
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 20px;
  letter-spacing: 0.05em;
}

.hero__title {
  font-size: clamp(2.4rem, 6vw, 4rem);
  font-weight: 800;
  line-height: 1.2;
  margin-bottom: 20px;
}

.hero__title span {
  color: var(--accent);
}

.hero__desc {
  font-size: 1.15rem;
  color: var(--text-secondary);
  margin-bottom: 36px;
  line-height: 1.8;
}

.hero__cta {
  display: flex;
  gap: 16px;
  flex-wrap: wrap;
}
```

- [ ] **Step 3: 브라우저에서 확인**

- Hero가 화면 전체 높이를 차지하는지 확인
- 이름에 파란색 포인트가 적용됐는지 확인
- 버튼 2개가 나란히 보이는지 확인
- 모바일에서 텍스트 크기가 적절한지 확인

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: Hero 섹션`

---

## Task 4: About 섹션

**Files:**
- Modify: `index.html`
- Modify: `style.css`

- [ ] **Step 1: index.html에 about 섹션 추가 (hero 섹션 아래)**

```html
  <!-- ===== ABOUT ===== -->
  <section id="about">
    <div class="container">
      <h2 class="section-title">소개<span>.</span></h2>
      <div class="about__inner">
        <div class="about__photo">
          <img src="assets/images/profile.jpg" alt="홍길동 프로필 사진" />
        </div>
        <div class="about__text">
          <h3>웹을 통해 가치를 만드는 사람</h3>
          <p>
            5년간 다양한 기업의 홈페이지와 웹 서비스를 제작해왔습니다.
            기술적인 완성도뿐 아니라, 클라이언트의 목표에 맞는 결과물을
            만드는 것을 가장 중요하게 생각합니다.
          </p>
          <p>
            강의에서는 실무에서 바로 쓸 수 있는 내용을 쉽게 전달하는 것을
            목표로 합니다. 비전공자도 이해할 수 있는 강의를 지향합니다.
          </p>
          <ul class="about__tags">
            <li>HTML / CSS / JS</li>
            <li>반응형 웹</li>
            <li>SEO 최적화</li>
            <li>랜딩 페이지</li>
          </ul>
        </div>
      </div>
    </div>
  </section>
```

> **주의:** 소개 문구와 태그는 실제 본인의 내용으로 교체 필요. `assets/images/profile.jpg`에 프로필 사진 추가 필요.

- [ ] **Step 2: style.css에 about 스타일 추가**

```css
/* ===== ABOUT ===== */
.about__inner {
  display: grid;
  grid-template-columns: 280px 1fr;
  gap: 60px;
  align-items: start;
}

.about__photo img {
  width: 100%;
  border-radius: 12px;
  border: 1px solid var(--border);
}

.about__text h3 {
  font-size: 1.5rem;
  font-weight: 700;
  margin-bottom: 16px;
}

.about__text p {
  color: var(--text-secondary);
  margin-bottom: 16px;
  line-height: 1.8;
}

.about__tags {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  list-style: none;
  margin-top: 24px;
}

.about__tags li {
  padding: 6px 14px;
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 20px;
  font-size: 0.85rem;
  color: var(--text-secondary);
}

@media (max-width: 768px) {
  .about__inner {
    grid-template-columns: 1fr;
  }

  .about__photo {
    max-width: 200px;
    margin: 0 auto;
  }
}
```

- [ ] **Step 3: 브라우저에서 확인**

- 사진과 텍스트가 2열로 나타나는지 확인
- 모바일에서 1열로 쌓이는지 확인
- 태그 칩들이 보이는지 확인

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: About 섹션`

---

## Task 5: Services 섹션

**Files:**
- Modify: `index.html`
- Modify: `style.css`

- [ ] **Step 1: index.html에 services 섹션 추가**

```html
  <!-- ===== SERVICES ===== -->
  <section id="services" style="background: var(--bg-secondary);">
    <div class="container">
      <h2 class="section-title">서비스<span>.</span></h2>
      <div class="services__grid">
        <div class="service-card">
          <div class="service-card__icon">📚</div>
          <h3>웹 개발 강의</h3>
          <p>HTML, CSS, JavaScript를 처음부터 실무 수준까지 가르칩니다. 비전공자도 따라올 수 있는 커리큘럼.</p>
          <ul class="service-card__list">
            <li>1:1 맞춤 강의</li>
            <li>그룹 온라인 강의</li>
            <li>기업 출강 교육</li>
          </ul>
          <a href="https://open.kakao.com/YOUR_LINK" class="btn btn-primary" target="_blank" rel="noopener">강의 신청하기</a>
        </div>
        <div class="service-card">
          <div class="service-card__icon">💻</div>
          <h3>홈페이지 제작</h3>
          <p>브랜드에 맞는 홈페이지를 디자인부터 개발까지 원스톱으로 제작합니다. 반응형 · SEO 기본 적용.</p>
          <ul class="service-card__list">
            <li>기업/개인 홈페이지</li>
            <li>랜딩 페이지</li>
            <li>포트폴리오 사이트</li>
          </ul>
          <a href="mailto:your@email.com" class="btn btn-outline">제작 문의하기</a>
        </div>
      </div>
    </div>
  </section>
```

> **주의:** 링크와 이메일을 실제 정보로 교체 필요.

- [ ] **Step 2: style.css에 services 스타일 추가**

```css
/* ===== SERVICES ===== */
.services__grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}

.service-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 36px;
  display: flex;
  flex-direction: column;
  gap: 16px;
  transition: border-color 0.2s, transform 0.2s;
}

.service-card:hover {
  border-color: var(--accent);
  transform: translateY(-4px);
}

.service-card__icon {
  font-size: 2.5rem;
}

.service-card h3 {
  font-size: 1.4rem;
  font-weight: 700;
}

.service-card p {
  color: var(--text-secondary);
  line-height: 1.7;
  flex: 1;
}

.service-card__list {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.service-card__list li {
  color: var(--text-secondary);
  font-size: 0.9rem;
  padding-left: 16px;
  position: relative;
}

.service-card__list li::before {
  content: '✓';
  position: absolute;
  left: 0;
  color: var(--accent);
  font-weight: 700;
}

@media (max-width: 768px) {
  .services__grid { grid-template-columns: 1fr; }
}
```

- [ ] **Step 3: 브라우저에서 확인**

- 카드 2개가 나란히 보이는지 확인
- 호버 시 카드가 살짝 올라오고 테두리가 파란색으로 바뀌는지 확인
- 각 카드 하단에 CTA 버튼이 있는지 확인

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: Services 섹션`

---

## Task 6: Portfolio 섹션

**Files:**
- Modify: `index.html`
- Modify: `style.css`
- Create: `assets/images/` 폴더에 포트폴리오 이미지 추가

- [ ] **Step 1: assets/images 폴더 생성 후 포트폴리오 이미지 준비**

탐색기에서 `assets/images/` 폴더를 만들고 포트폴리오 스크린샷 파일을 `portfolio-1.jpg`, `portfolio-2.jpg`, `portfolio-3.jpg` 형식으로 저장. (이미지가 없으면 아래 HTML의 `src`를 임시 placeholder URL로 대체 가능)

- [ ] **Step 2: index.html에 portfolio 섹션 추가**

```html
  <!-- ===== PORTFOLIO ===== -->
  <section id="portfolio">
    <div class="container">
      <h2 class="section-title">포트폴리오<span>.</span></h2>
      <div class="portfolio__grid">
        <a href="https://example.com" class="portfolio-item" target="_blank" rel="noopener">
          <img src="assets/images/portfolio-1.jpg" alt="프로젝트명 1" />
          <div class="portfolio-item__overlay">
            <span class="portfolio-item__title">프로젝트명 1</span>
            <span class="portfolio-item__link">보기 →</span>
          </div>
        </a>
        <a href="https://example.com" class="portfolio-item" target="_blank" rel="noopener">
          <img src="assets/images/portfolio-2.jpg" alt="프로젝트명 2" />
          <div class="portfolio-item__overlay">
            <span class="portfolio-item__title">프로젝트명 2</span>
            <span class="portfolio-item__link">보기 →</span>
          </div>
        </a>
        <a href="https://example.com" class="portfolio-item" target="_blank" rel="noopener">
          <img src="assets/images/portfolio-3.jpg" alt="프로젝트명 3" />
          <div class="portfolio-item__overlay">
            <span class="portfolio-item__title">프로젝트명 3</span>
            <span class="portfolio-item__link">보기 →</span>
          </div>
        </a>
      </div>
    </div>
  </section>
```

> **주의:** `href`의 URL과 `alt`, `portfolio-item__title` 텍스트를 실제 프로젝트 정보로 교체 필요.

- [ ] **Step 3: style.css에 portfolio 스타일 추가**

```css
/* ===== PORTFOLIO ===== */
.portfolio__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.portfolio-item {
  position: relative;
  border-radius: 10px;
  overflow: hidden;
  aspect-ratio: 16 / 10;
  background: var(--bg-card);
  border: 1px solid var(--border);
}

.portfolio-item img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s;
}

.portfolio-item__overlay {
  position: absolute;
  inset: 0;
  background: rgba(13, 17, 23, 0.85);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  opacity: 0;
  transition: opacity 0.3s;
}

.portfolio-item:hover img { transform: scale(1.05); }
.portfolio-item:hover .portfolio-item__overlay { opacity: 1; }

.portfolio-item__title {
  color: var(--text-primary);
  font-size: 1.1rem;
  font-weight: 700;
}

.portfolio-item__link {
  color: var(--accent);
  font-size: 0.9rem;
  font-weight: 600;
}

@media (max-width: 900px) {
  .portfolio__grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 560px) {
  .portfolio__grid { grid-template-columns: 1fr; }
}
```

- [ ] **Step 4: 브라우저에서 확인**

- 3열 그리드로 이미지가 나타나는지 확인
- 이미지 위에 마우스 올리면 오버레이가 나타나는지 확인
- 오버레이에 프로젝트명과 "보기 →"가 보이는지 확인

- [ ] **Step 5: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`, `assets/images/`  
커밋 메시지: `feat: Portfolio 섹션`

---

## Task 7: Blog Preview 섹션

**Files:**
- Modify: `index.html`
- Modify: `style.css`

- [ ] **Step 1: index.html에 blog preview 섹션 추가**

```html
  <!-- ===== BLOG PREVIEW ===== -->
  <section id="blog" style="background: var(--bg-secondary);">
    <div class="container">
      <h2 class="section-title">블로그<span>.</span></h2>
      <div class="blog__grid">
        <a href="posts/2026-04-08-첫번째-글.html" class="blog-card">
          <div class="blog-card__meta">
            <span class="blog-card__tag">강의 팁</span>
            <span class="blog-card__date">2026.04.08</span>
          </div>
          <h3 class="blog-card__title">HTML/CSS 독학 시작하는 법 — 비전공자를 위한 로드맵</h3>
          <p class="blog-card__desc">웹 개발을 독학으로 시작할 때 어디서부터 시작해야 할지 막막하셨나요? 실제 수강생 사례를 바탕으로...</p>
        </a>
        <a href="posts/2026-04-08-첫번째-글.html" class="blog-card">
          <div class="blog-card__meta">
            <span class="blog-card__tag">제작 사례</span>
            <span class="blog-card__date">2026.04.01</span>
          </div>
          <h3 class="blog-card__title">소규모 카페 홈페이지 제작 후기 — 예산 50만원으로 가능한 것들</h3>
          <p class="blog-card__desc">실제 카페 홈페이지를 제작하면서 겪은 과정을 공유합니다. 요구사항 정리부터 납품까지...</p>
        </a>
        <a href="posts/2026-04-08-첫번째-글.html" class="blog-card">
          <div class="blog-card__meta">
            <span class="blog-card__tag">개발 팁</span>
            <span class="blog-card__date">2026.03.25</span>
          </div>
          <h3 class="blog-card__title">반응형 웹의 핵심 — 미디어 쿼리 5가지 패턴</h3>
          <p class="blog-card__desc">모든 기기에서 잘 보이는 사이트를 만들기 위해 꼭 알아야 할 미디어 쿼리 패턴을 정리했습니다...</p>
        </a>
      </div>
      <div class="blog__more">
        <a href="posts/index.html" class="btn btn-outline">전체 글 보기</a>
      </div>
    </div>
  </section>
```

> **주의:** 글 제목, 날짜, 태그, 설명을 실제 내용으로 교체 필요. href 링크도 실제 파일명과 일치시켜야 함.

- [ ] **Step 2: style.css에 blog preview 스타일 추가**

```css
/* ===== BLOG PREVIEW ===== */
.blog__grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
  margin-bottom: 40px;
}

.blog-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 28px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  transition: border-color 0.2s, transform 0.2s;
}

.blog-card:hover {
  border-color: var(--accent);
  transform: translateY(-4px);
}

.blog-card__meta {
  display: flex;
  align-items: center;
  gap: 12px;
}

.blog-card__tag {
  background: rgba(29, 106, 229, 0.15);
  color: var(--accent);
  padding: 3px 10px;
  border-radius: 12px;
  font-size: 0.78rem;
  font-weight: 600;
}

.blog-card__date {
  color: var(--text-secondary);
  font-size: 0.82rem;
}

.blog-card__title {
  font-size: 1rem;
  font-weight: 700;
  line-height: 1.5;
}

.blog-card__desc {
  color: var(--text-secondary);
  font-size: 0.88rem;
  line-height: 1.6;
  flex: 1;
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.blog__more {
  text-align: center;
}

@media (max-width: 900px) {
  .blog__grid { grid-template-columns: repeat(2, 1fr); }
}

@media (max-width: 560px) {
  .blog__grid { grid-template-columns: 1fr; }
}
```

- [ ] **Step 3: 브라우저에서 확인**

- 블로그 카드 3개가 3열로 보이는지 확인
- 호버 시 카드가 올라오고 테두리가 파란색으로 변하는지 확인
- "전체 글 보기" 버튼이 중앙에 있는지 확인

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: Blog Preview 섹션`

---

## Task 8: Contact/CTA 섹션 + Footer

**Files:**
- Modify: `index.html`
- Modify: `style.css`

- [ ] **Step 1: index.html에 contact 섹션과 footer 추가 (blog 섹션 아래)**

```html
  <!-- ===== CONTACT ===== -->
  <section id="contact">
    <div class="container">
      <h2 class="section-title">문의하기<span>.</span></h2>
      <p class="contact__desc">강의 또는 홈페이지 제작에 관심 있으신가요? 편하게 연락 주세요.</p>
      <div class="contact__grid">
        <div class="contact-card">
          <div class="contact-card__icon">📚</div>
          <h3>강의 신청</h3>
          <p>커리큘럼 및 수강료 문의, 강의 일정 확인</p>
          <a href="https://open.kakao.com/YOUR_LINK" class="btn btn-primary" target="_blank" rel="noopener">카카오톡 오픈채팅</a>
        </div>
        <div class="contact-card">
          <div class="contact-card__icon">💻</div>
          <h3>홈페이지 제작 의뢰</h3>
          <p>견적 문의, 포트폴리오 상담, 제작 기간 확인</p>
          <a href="mailto:your@email.com" class="btn btn-outline">이메일 보내기</a>
        </div>
      </div>
    </div>
  </section>

</main>

<!-- ===== FOOTER ===== -->
<footer class="footer">
  <div class="container footer__inner">
    <p class="footer__name">홍길동</p>
    <p class="footer__copy">© 2026 홍길동. All rights reserved.</p>
    <nav class="footer__nav">
      <a href="#about">소개</a>
      <a href="#services">서비스</a>
      <a href="#portfolio">포트폴리오</a>
      <a href="#blog">블로그</a>
    </nav>
  </div>
</footer>
```

> **주의:** 카카오톡 링크와 이메일을 실제 정보로 교체 필요.

- [ ] **Step 2: style.css에 contact + footer 스타일 추가**

```css
/* ===== CONTACT ===== */
.contact__desc {
  text-align: center;
  color: var(--text-secondary);
  margin-bottom: 40px;
  font-size: 1.05rem;
}

.contact__grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
  max-width: 700px;
  margin: 0 auto;
}

.contact-card {
  background: var(--bg-card);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 36px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  gap: 14px;
  transition: border-color 0.2s;
}

.contact-card:hover { border-color: var(--accent); }

.contact-card__icon { font-size: 2.5rem; }

.contact-card h3 {
  font-size: 1.2rem;
  font-weight: 700;
}

.contact-card p {
  color: var(--text-secondary);
  font-size: 0.9rem;
  line-height: 1.6;
  flex: 1;
}

@media (max-width: 600px) {
  .contact__grid { grid-template-columns: 1fr; }
}

/* ===== FOOTER ===== */
.footer {
  border-top: 1px solid var(--border);
  padding: 32px 0;
  background: var(--bg-secondary);
}

.footer__inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-wrap: wrap;
  gap: 16px;
}

.footer__name {
  font-weight: 700;
  font-size: 1rem;
}

.footer__copy {
  color: var(--text-secondary);
  font-size: 0.85rem;
}

.footer__nav {
  display: flex;
  gap: 24px;
}

.footer__nav a {
  color: var(--text-secondary);
  font-size: 0.85rem;
  transition: color 0.2s;
}

.footer__nav a:hover { color: var(--text-primary); }

@media (max-width: 600px) {
  .footer__inner { flex-direction: column; text-align: center; }
  .footer__nav { justify-content: center; }
}
```

- [ ] **Step 3: 브라우저에서 전체 페이지 확인**

- 모든 섹션이 순서대로 나타나는지 확인
- 네비게이션 링크를 클릭하면 해당 섹션으로 스무스 스크롤되는지 확인
- 페이지 하단에 Footer가 있는지 확인
- 모바일(768px 이하)에서 전체 레이아웃이 깨지지 않는지 확인

- [ ] **Step 4: 소스트리에서 커밋**

스테이징: `index.html`, `style.css`  
커밋 메시지: `feat: Contact 섹션, Footer`

---

## Task 9: 블로그 목록 페이지 + 글 템플릿

**Files:**
- Create: `posts/index.html`
- Create: `posts/2026-04-08-첫번째-글.html`

- [ ] **Step 1: posts/index.html 생성**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>블로그 | 홍길동</title>
  <meta name="description" content="웹 개발 강의 팁, 제작 사례를 공유하는 블로그입니다." />
  <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
  <link rel="stylesheet" href="../style.css" />
  <link rel="stylesheet" href="posts.css" />
</head>
<body>
  <header class="navbar scrolled" id="navbar">
    <div class="container navbar__inner">
      <a href="../index.html" class="navbar__logo">홍길동</a>
      <nav class="navbar__menu" id="navMenu">
        <a href="../index.html#about">소개</a>
        <a href="../index.html#services">서비스</a>
        <a href="../index.html#portfolio">포트폴리오</a>
        <a href="index.html" style="color: var(--accent);">블로그</a>
        <a href="../index.html#contact" class="btn btn-primary">문의하기</a>
      </nav>
      <button class="navbar__toggle" id="navToggle" aria-label="메뉴 열기">
        <span></span><span></span><span></span>
      </button>
    </div>
  </header>

  <main class="posts-main">
    <div class="container">
      <h1 class="posts-title">블로그</h1>
      <div class="blog__grid">
        <a href="2026-04-08-첫번째-글.html" class="blog-card">
          <div class="blog-card__meta">
            <span class="blog-card__tag">강의 팁</span>
            <span class="blog-card__date">2026.04.08</span>
          </div>
          <h2 class="blog-card__title">HTML/CSS 독학 시작하는 법 — 비전공자를 위한 로드맵</h2>
          <p class="blog-card__desc">웹 개발을 독학으로 시작할 때 어디서부터 시작해야 할지 막막하셨나요? 실제 수강생 사례를 바탕으로 정리했습니다.</p>
        </a>
        <!-- 새 글 추가 시 위 blog-card 블록을 복사하여 여기 위에 붙여넣기 -->
      </div>
    </div>
  </main>

  <footer class="footer">
    <div class="container footer__inner">
      <p class="footer__name">홍길동</p>
      <p class="footer__copy">© 2026 홍길동. All rights reserved.</p>
    </div>
  </footer>

  <script src="../main.js"></script>
</body>
</html>
```

- [ ] **Step 2: posts/posts.css 생성 (블로그 페이지 전용 추가 스타일)**

```css
.posts-main {
  padding-top: 120px;
  min-height: 80vh;
}

.posts-title {
  font-size: 2.5rem;
  font-weight: 800;
  margin-bottom: 48px;
}

.post-content {
  max-width: 720px;
  margin: 0 auto;
}

.post-content h1 {
  font-size: 2rem;
  font-weight: 800;
  margin-bottom: 8px;
}

.post-content .post-meta {
  color: var(--text-secondary);
  font-size: 0.9rem;
  margin-bottom: 40px;
  display: flex;
  gap: 16px;
}

.post-content .post-body {
  color: var(--text-secondary);
  line-height: 1.9;
}

.post-content .post-body h2 {
  color: var(--text-primary);
  font-size: 1.4rem;
  font-weight: 700;
  margin: 40px 0 16px;
}

.post-content .post-body p { margin-bottom: 20px; }

.post-content .post-body code {
  background: var(--bg-card);
  border: 1px solid var(--border);
  padding: 2px 6px;
  border-radius: 4px;
  font-size: 0.9em;
  color: var(--text-primary);
}

.post-back {
  display: inline-block;
  margin-bottom: 32px;
  color: var(--text-secondary);
  font-size: 0.9rem;
  transition: color 0.2s;
}

.post-back:hover { color: var(--accent); }
```

- [ ] **Step 3: posts/2026-04-08-첫번째-글.html 생성 (개별 글 템플릿)**

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>HTML/CSS 독학 시작하는 법 | 홍길동</title>
  <meta name="description" content="웹 개발을 독학으로 시작할 때 어디서부터 시작해야 할지 막막하셨나요?" />
  <link rel="stylesheet" as="style" crossorigin href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard.min.css" />
  <link rel="stylesheet" href="../style.css" />
  <link rel="stylesheet" href="posts.css" />
</head>
<body>
  <header class="navbar scrolled" id="navbar">
    <div class="container navbar__inner">
      <a href="../index.html" class="navbar__logo">홍길동</a>
      <nav class="navbar__menu" id="navMenu">
        <a href="../index.html#about">소개</a>
        <a href="../index.html#services">서비스</a>
        <a href="../index.html#portfolio">포트폴리오</a>
        <a href="index.html" style="color: var(--accent);">블로그</a>
        <a href="../index.html#contact" class="btn btn-primary">문의하기</a>
      </nav>
      <button class="navbar__toggle" id="navToggle" aria-label="메뉴 열기">
        <span></span><span></span><span></span>
      </button>
    </div>
  </header>

  <main class="posts-main">
    <div class="container">
      <a href="index.html" class="post-back">← 블로그 목록으로</a>
      <article class="post-content">
        <h1>HTML/CSS 독학 시작하는 법 — 비전공자를 위한 로드맵</h1>
        <div class="post-meta">
          <span>2026.04.08</span>
          <span>강의 팁</span>
        </div>
        <div class="post-body">
          <p>웹 개발을 독학으로 시작할 때 어디서부터 시작해야 할지 막막하셨나요? 이 글에서는 실제 수강생 사례를 바탕으로 효율적인 학습 순서를 정리했습니다.</p>

          <h2>1단계: HTML 구조 이해하기</h2>
          <p>HTML은 웹 페이지의 뼈대입니다. 태그의 역할과 중첩 구조를 이해하는 것이 첫 번째 목표입니다.</p>

          <h2>2단계: CSS로 꾸미기</h2>
          <p>스타일링은 처음엔 막막하지만, Flexbox와 Grid 두 가지만 제대로 익히면 대부분의 레이아웃을 구현할 수 있습니다.</p>

          <p>강의에 대해 더 궁금하신 점이 있으시면 편하게 문의해주세요.</p>
        </div>
      </article>
    </div>
  </main>

  <footer class="footer">
    <div class="container footer__inner">
      <p class="footer__name">홍길동</p>
      <p class="footer__copy">© 2026 홍길동. All rights reserved.</p>
    </div>
  </footer>

  <script src="../main.js"></script>
</body>
</html>
```

- [ ] **Step 4: 브라우저에서 확인**

- `posts/index.html` 열어서 블로그 카드 목록이 보이는지 확인
- 카드 클릭 시 개별 글 페이지로 이동하는지 확인
- 개별 글 페이지에서 "← 블로그 목록으로" 링크가 동작하는지 확인
- `index.html`의 블로그 섹션에서 "전체 글 보기" 클릭 시 `posts/index.html`로 이동하는지 확인

- [ ] **Step 5: 소스트리에서 커밋**

스테이징: `posts/index.html`, `posts/posts.css`, `posts/2026-04-08-첫번째-글.html`  
커밋 메시지: `feat: 블로그 목록 페이지, 개별 글 템플릿`

---

## Task 10: GitHub Pages 배포 설정

**Files:**
- Modify: `README.md`

- [ ] **Step 1: GitHub에 새 저장소 생성**

GitHub(github.com)에서 새 저장소 생성:
- Repository name: `username.github.io` (username은 본인 GitHub 아이디) 또는 임의의 이름
- Public으로 설정
- "Add a README file" 체크 해제 (이미 로컬에 있음)

- [ ] **Step 2: 소스트리에서 원격 저장소 연결 후 푸시**

소스트리 → 저장소 설정 → 원격(Remote) 추가:
- Remote name: `origin`
- URL: `https://github.com/username/저장소명.git`

이후 `main` 브랜치 푸시.

- [ ] **Step 3: GitHub Pages 활성화**

GitHub 저장소 → Settings → Pages → Source: `Deploy from a branch` → Branch: `main`, folder: `/ (root)` → Save

- [ ] **Step 4: 배포 URL 확인**

약 1~2분 후 `https://username.github.io/저장소명` 접속하여 사이트가 정상적으로 나타나는지 확인.

- [ ] **Step 5: README.md 업데이트**

```markdown
# 홍길동 개인 홍보 블로그

강의 및 홈페이지 제작 의뢰를 위한 개인 홍보 사이트.

🔗 [사이트 바로가기](https://username.github.io/저장소명)

## 기술 스택
- HTML5 / CSS3 / Vanilla JS
- Pretendard 폰트
- GitHub Pages 호스팅

## 새 블로그 글 작성 방법
1. `posts/YYYY-MM-DD-제목.html` 파일 생성 (`posts/2026-04-08-첫번째-글.html` 복사 후 수정)
2. `index.html`의 Blog Preview 섹션 최신 카드 1개 교체
3. `posts/index.html` 목록 최상단에 카드 1개 추가
4. 소스트리에서 커밋 & 푸시
```

- [ ] **Step 6: 소스트리에서 커밋 & 푸시**

스테이징: `README.md`  
커밋 메시지: `docs: README 업데이트, GitHub Pages 배포 완료`

---

## 개인화 체크리스트

구현 완료 후 실제 정보로 교체할 항목들:

- [ ] `index.html`: 이름 "홍길동" → 실제 이름
- [ ] `index.html`: Hero 한 줄 소개 문구
- [ ] `index.html`: 강의 신청 CTA URL (카카오 오픈채팅 등)
- [ ] `index.html`: 제작 문의 이메일 주소
- [ ] `index.html`: About 소개 문구, 태그
- [ ] `index.html`: Services 카드 내용
- [ ] `index.html`: Portfolio 프로젝트명, URL, 이미지
- [ ] `index.html`: Blog Preview 글 3개 실제 내용
- [ ] `assets/images/profile.jpg`: 프로필 사진
- [ ] `assets/images/portfolio-*.jpg`: 포트폴리오 스크린샷
- [ ] `README.md`: GitHub Pages URL
