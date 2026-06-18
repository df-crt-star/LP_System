# rules-reference.md — コード例リファレンス

> `rules-core.md` の補足資料。コード例が必要なときのみ参照すること。

---

## 1. HTMLドキュメント基本構造

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="ページの説明文">
  <title>ページタイトル | サイト名</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <header class="site-header"><!-- ヘッダー --></header>
  <main class="site-main">
    <section id="hero" class="section-hero">
      <div class="inner-container"><!-- ヒーロー --></div>
    </section>
  </main>
  <footer class="site-footer"><!-- フッター --></footer>
  <script src="js/main.js"></script>
</body>
</html>
```

---

## 2. 背景画像上の絶対配置（頻出パターン）

```html
<div class="relative-wrapper">
  <img src="images/bg.webp" alt="セクション背景" width="1000" height="600" class="base-bg">
  <img src="images/item.webp" alt="重ねるアイテム" width="200" height="200" class="absolute-item">
</div>
```

```css
.relative-wrapper { position: relative; width: 100%; max-width: var(--max-width); margin: 0 auto; }
.base-bg { display: block; width: 100%; height: auto; }
.absolute-item { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); width: 20%; height: auto; }
```

---

## 3. Flexboxレイアウト

```css
/* 横並び（PC）→ 縦並び（SP） */
.flex-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}
@media screen and (min-width: 768px) {
  .flex-list { flex-direction: row; flex-wrap: wrap; }
  .flex-list .flex-item { flex: 1 1 calc(50% - 12px); }
}
```

---

## 4. CSS Gridレイアウト

```css
/* 1カラム（SP）→ 3カラム（PC） */
.three-col-grid { display: grid; grid-template-columns: 1fr; gap: 24px; }
@media screen and (min-width: 768px) {
  .three-col-grid { grid-template-columns: repeat(3, 1fr); }
}
```

---

## 5. スクロールアニメーション（IntersectionObserver）

```css
.fade-in { opacity: 0; transform: translateY(20px); transition: opacity 0.6s ease, transform 0.6s ease; }
.fade-in.is-visible { opacity: 1; transform: translateY(0); }
```

```javascript
const fadeInElements = document.querySelectorAll('.fade-in');
const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1 });
fadeInElements.forEach((el) => observer.observe(el));
```

---

## 6. ボタン（共通スタイル）

```css
.btn {
  display: inline-block;
  padding: 14px 30px;
  border-radius: 4px;
  font-weight: 700;
  text-align: center;
  cursor: pointer;
  transition: opacity 0.2s ease;
}
.btn:hover { opacity: 0.8; }
.btn-primary { background-color: var(--color-accent); color: #ffffff; }
.btn-secondary { background-color: #ffffff; color: var(--color-accent); border: 2px solid var(--color-accent); }
```

---

## 7. 動画・GIF実装

```html
<!-- MP4動画（4属性必須） -->
<video autoplay muted playsinline loop class="demo-video">
  <source src="videos/product-demo.mp4" type="video/mp4">
</video>

<!-- GIF（ファーストビュー外） -->
<img src="images/step.gif" alt="使用手順のアニメーション" width="600" height="400" loading="lazy" class="step-gif">
```

---

## 8. 簡易BEM命名例

```html
<div class="card-item">
  <img src="images/thumb.webp" alt="商品サムネイル" width="300" height="200" loading="lazy" class="card-item-image">
  <div class="card-item-body">
    <h3 class="card-item-title">商品タイトル</h3>
    <p class="card-item-desc">説明文</p>
  </div>
  <a href="#purchase" class="btn btn-primary">今すぐ購入する</a>
</div>
```

---

## 9. 他プラットフォームへのコンバート対応

| プラットフォーム | 変換方式 | 注意点 |
|----------------|---------|--------|
| EC Force | インラインCSS化 + 独自タグ | 複雑なセレクタは変換後に崩れる可能性あり |
| Shopify | Liquidテンプレートへの変換 | `{{ }}` タグ挿入箇所を明確に分離 |
| WordPress | PHPテンプレートへの変換 | `<?php ?>` タグ挿入箇所を明確に分離 |

動的要素（商品名・価格・カートボタン）は独立した要素としてマークアップし、他の要素と密結合させないこと。
