# sticky.md — 追従CTAバー コンポーネント

> このファイルは `components/sticky.md` として保存し、追従要素を含むセクション制作直前に読み込む。

---

## S1：下部固定CTAバー（最もシンプルな追従）

### ① 概要
画面下部に常時固定表示されるCTAバー。スクロール位置に関わらず常に表示し、申し込みへの導線を確保する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| ボタンテキスト | CTAボタンのラベル | 「今すぐ無料で申し込む」 |
| リンク先 | ボタンのhref | `#form`（ページ内アンカー）|
| 表示タイミング | 常時表示 or スクロール後に表示 | スクロール後（FV通過後）|
| 非表示タイミング | フォームセクション到達で非表示にするか | あり |
| テキスト補足 | ボタン横のサブテキスト | 「送料無料・返金保証あり」|

### ③ テンプレート

```html
<!-- 追従CTAバー（</body>直前に配置） -->
<div class="sticky-cta" id="sticky-cta" aria-hidden="true">
  <div class="sticky-cta-inner">
    <p class="sticky-cta-sub">【SUB_TEXT】</p>
    <a href="【LINK_HREF】" class="btn btn-primary sticky-cta-btn">【BUTTON_TEXT】</a>
  </div>
</div>
```

```css
/* Sticky CTA Bar */
.sticky-cta {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.85);
  padding: 12px 20px;
  z-index: 1000;
  transform: translateY(100%);
  transition: transform 0.3s ease;
}
.sticky-cta.is-visible {
  transform: translateY(0);
}
.sticky-cta-inner {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  max-width: var(--max-width);
  margin: 0 auto;
  flex-wrap: wrap;
}
.sticky-cta-sub {
  color: #ffffff;
  font-size: 1.3rem;
}
.sticky-cta-btn {
  white-space: nowrap;
  font-size: 1.5rem;
  padding: 12px 28px;
}
```

```javascript
// Sticky CTA Bar
const stickyCta = document.getElementById('sticky-cta');
const formSection = document.getElementById('form'); // フォームセクションのID

if (stickyCta) {
  const showObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      // FVセクションが画面外に出たら表示
      if (!entry.isIntersecting) {
        stickyCta.classList.add('is-visible');
        stickyCta.setAttribute('aria-hidden', 'false');
      } else {
        stickyCta.classList.remove('is-visible');
        stickyCta.setAttribute('aria-hidden', 'true');
      }
    });
  }, { threshold: 0 });

  const heroSection = document.getElementById('hero');
  if (heroSection) showObserver.observe(heroSection);

  // フォームセクション到達で非表示
  if (formSection) {
    const hideObserver = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          stickyCta.classList.remove('is-visible');
          stickyCta.setAttribute('aria-hidden', 'true');
        } else {
          stickyCta.classList.add('is-visible');
          stickyCta.setAttribute('aria-hidden', 'false');
        }
      });
    }, { threshold: 0.1 });
    hideObserver.observe(formSection);
  }
}
```

---

## S2：上部固定ヘッダー型CTAバー

### ① 概要
画面上部に固定されるヘッダー型のCTAバー。ロゴ・商品名とボタンを横並びで表示する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| ロゴ/商品名 | 左側に表示するテキストまたは画像 | 商品ロゴ画像 or テキスト |
| ボタンテキスト | CTAボタンのラベル | 「申し込む」 |
| 背景色 | バーの背景色 | `#ffffff` or `var(--color-main)` |
| 表示タイミング | スクロール後に表示 | FV通過後 |

### ③ テンプレート

```html
<!-- 上部固定ヘッダーCTAバー（<body>直後に配置） -->
<header class="sticky-header" id="sticky-header" aria-hidden="true">
  <div class="sticky-header-inner">
    <div class="sticky-header-logo">
      <!-- ロゴ画像 or テキスト -->
      <img src="images/logo.webp" alt="【PRODUCT_NAME】" width="120" height="40" class="sticky-header-logo-img">
    </div>
    <a href="【LINK_HREF】" class="btn btn-primary sticky-header-btn">【BUTTON_TEXT】</a>
  </div>
</header>
```

```css
/* Sticky Header CTA */
.sticky-header {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  background-color: #ffffff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  padding: 10px 20px;
  z-index: 1000;
  transform: translateY(-100%);
  transition: transform 0.3s ease;
}
.sticky-header.is-visible {
  transform: translateY(0);
}
.sticky-header-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  max-width: var(--max-width);
  margin: 0 auto;
}
.sticky-header-logo-img {
  height: 40px;
  width: auto;
}
.sticky-header-btn {
  font-size: 1.4rem;
  padding: 10px 24px;
}
```

```javascript
// Sticky Header CTA（S1と同様のIntersectionObserver構造）
const stickyHeader = document.getElementById('sticky-header');

if (stickyHeader) {
  const heroSection = document.getElementById('hero');
  if (heroSection) {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (!entry.isIntersecting) {
          stickyHeader.classList.add('is-visible');
          stickyHeader.setAttribute('aria-hidden', 'false');
        } else {
          stickyHeader.classList.remove('is-visible');
          stickyHeader.setAttribute('aria-hidden', 'true');
        }
      });
    }, { threshold: 0 });
    observer.observe(heroSection);
  }
}
```

---

## 注意事項

- S1（下部）とS2（上部）を**同時に使用する場合**、`z-index` の競合に注意すること（S1: 1000 / S2: 1001 等で差をつける）
- `IntersectionObserver` のターゲットIDは案件フォルダの実際のセクションIDに合わせて変更すること
