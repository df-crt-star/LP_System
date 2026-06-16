# 制作部共通コーディングルール・ガイドライン

> **本ドキュメントは、「LP自動コーディング化システム」において、AI（Claude Code等）がHTML/CSS/JavaScriptを生成する際の絶対的なルールブックである。AIはコードを生成する際、必ず本ガイドラインのすべての要件を厳格に遵守し、寸分の狂いもない正確なコードを出力すること。一項目たりとも省略・無視することを禁ずる。**

**バージョン**: 1.0.0  
**最終更新日**: 2026-06-16  
**適用範囲**: 制作部が制作するすべてのLPコーディング成果物

---

## 目次

1. [基本テックスタックと構造ルール](#1-基本テックスタックと構造ルール)
2. [ファイル・ディレクトリ構成](#2-ファイルディレクトリ構成)
3. [CSSベーススタイルとリセット](#3-cssベーススタイルとリセット)
4. [レスポンシブ対応とブレークポイント](#4-レスポンシブ対応とブレークポイント)
5. [素材の扱い（画像・動画・アニメーション）](#5-素材の扱い画像動画アニメーション)
6. [命名規則と設計思想](#6-命名規則と設計思想)
7. [レイアウト実装ルール（Flexbox / CSS Grid）](#7-レイアウト実装ルールflexbox--css-grid)
8. [特殊な演出・レイアウトの実装テンプレート](#8-特殊な演出レイアウトの実装テンプレート)
9. [Vanilla JavaScript記述ルール](#9-vanilla-javascript記述ルール)
10. [汎用性と他プラットフォームへのコンバート対応](#10-汎用性と他プラットフォームへのコンバート対応)
11. [絶対禁止事項](#11-絶対禁止事項)

---

## 1. 基本テックスタックと構造ルール

### 1.1 使用技術スタック

| 技術 | バージョン | 備考 |
|------|-----------|------|
| HTML | HTML5 | セマンティックマークアップ必須 |
| CSS | CSS3 | モダンCSS・レスポンシブ対応必須 |
| JavaScript | ES6+ (Vanilla JS) | 外部ライブラリへの依存を禁ずる |

### 1.2 HTMLドキュメントの基本構造

すべてのHTMLファイルは以下の基本構造を厳守すること。`lang` 属性は必ず `"ja"` を指定し、`charset` は `UTF-8` を使用すること。

```html
<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="ページの説明文をここに記述する">
  <title>ページタイトル | サイト名</title>
  <link rel="stylesheet" href="css/style.css">
</head>
<body>

  <header class="site-header">
    <!-- ヘッダーコンテンツ -->
  </header>

  <main class="site-main">

    <section id="hero" class="section-hero">
      <div class="inner-container">
        <!-- ヒーローセクションのコンテンツ -->
      </div>
    </section>

    <section id="features" class="section-features">
      <div class="inner-container">
        <!-- 特徴セクションのコンテンツ -->
      </div>
    </section>

    <!-- 以降、セクションを追加する -->

  </main>

  <footer class="site-footer">
    <!-- フッターコンテンツ -->
  </footer>

  <script src="js/main.js"></script>
</body>
</html>
```

### 1.3 HTMLセマンティックマークアップの厳格なルール

セクションの区切りには必ず `<section>` タグを使用すること。単なる `<div>` による区切りは禁止する。見出しタグは文書構造に従い、上から順に `<h2>`, `<h3>`, `<h4>` と階層を正しく使用すること。`<h1>` はページ全体で1回のみ使用し、通常はヒーローセクションの主見出しに割り当てる。

**【正しい構造の例】**

```html
<section id="features" class="section-features">
  <div class="inner-container">
    <h2 class="section-title">商品の特徴</h2>
    <div class="features-grid">
      <div class="feature-item">
        <h3 class="feature-item-title">独自の配合成分</h3>
        <p class="feature-item-desc">説明文がここに入ります。</p>
      </div>
    </div>
  </div>
</section>
```

**【禁止されている構造の例】**

```html
<!-- NG: セクション区切りにdivを使用している -->
<div class="features">
  <div class="inner">
    <!-- h2が飛んでh3から始まっている -->
    <h3 class="title">商品の特徴</h3>
  </div>
</div>
```

---

## 2. ファイル・ディレクトリ構成

すべてのLP制作物は以下のディレクトリ構成を標準とする。ファイル名はすべて小文字のケバブケースで統一すること。

```
project-root/
├── index.html
├── css/
│   └── style.css
├── js/
│   └── main.js
├── images/
│   ├── hero-bg.webp
│   ├── product-main.webp
│   └── icon-check.webp
└── videos/
    └── product-demo.mp4
```

| ディレクトリ/ファイル | 用途 |
|----------------------|------|
| `index.html` | メインHTMLファイル |
| `css/style.css` | すべてのスタイルを記述する単一CSSファイル |
| `js/main.js` | すべてのJavaScriptを記述する単一JSファイル |
| `images/` | WebP画像・GIFアニメーションを格納 |
| `videos/` | MP4動画ファイルを格納 |

---

## 3. CSSベーススタイルとリセット

すべてのCSSファイルの冒頭に、以下のリセット・ベーススタイルを必ず記述すること。このブロックを省略してはならない。

```css
/* ==========================================================================
   Reset & Base Styles
   ========================================================================== */

*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 62.5%; /* 1rem = 10px */
  scroll-behavior: smooth;
}

body {
  font-family: "Noto Sans JP", "Hiragino Kaku Gothic ProN", "Hiragino Sans", Meiryo, sans-serif;
  font-size: 1.6rem;
  line-height: 1.8;
  color: #333333;
  background-color: #ffffff;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img,
video {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  color: inherit;
  text-decoration: none;
}

ul,
ol {
  list-style: none;
}

/* ==========================================================================
   Layout Utilities
   ========================================================================== */

.inner-container {
  width: 90%;
  max-width: 1000px;
  margin: 0 auto;
}
```

---

## 4. レスポンシブ対応とブレークポイント

### 4.1 ブレークポイント定義

レスポンシブ対応は**モバイルファースト**を基本とする。以下のブレークポイントを標準として使用すること。

| ブレークポイント名 | 幅 | 対象デバイス |
|-------------------|----|-------------|
| `sm` | 〜 767px | スマートフォン（デフォルト） |
| `md` | 768px 〜 | タブレット・PC |
| `lg` | 1200px 〜 | 大画面PC |

### 4.2 メディアクエリの記述ルール

メディアクエリはモバイルファーストで記述し、`min-width` を使用すること。`max-width` のみによる記述は禁止する。

```css
/* モバイル（デフォルト）: 767px以下 */
.section-features {
  padding: 40px 0;
}

/* タブレット・PC: 768px以上 */
@media screen and (min-width: 768px) {
  .section-features {
    padding: 80px 0;
  }
}

/* 大画面PC: 1200px以上 */
@media screen and (min-width: 1200px) {
  .section-features {
    padding: 100px 0;
  }
}
```

---

## 5. 素材の扱い（画像・動画・アニメーション）

### 5.1 画像の取り扱い

画像形式はIllustratorから書き出した **WebP (`.webp`)** を基本とする。`<img>` タグには必ず `width`, `height`, `alt` 属性を明記し、ブラウザのレイアウトシフト（CLS: Cumulative Layout Shift）を防ぐこと。`alt` 属性は空文字（`alt=""`）にしてはならない。装飾目的のみの画像であっても、その内容を簡潔に説明するテキストを記述すること。

**【画像の実装例】**

```html
<!-- 正しい実装 -->
<img src="images/hero-bg.webp" alt="商品のメインビジュアル" width="1200" height="800" class="hero-image">

<!-- 遅延読み込みが適切な場合（ファーストビュー外の画像） -->
<img src="images/product-detail.webp" alt="商品詳細画像" width="600" height="400" loading="lazy" class="product-detail-image">
```

**【禁止されている実装例】**

```html
<!-- NG: width, height, alt属性がない -->
<img src="images/hero-bg.webp" class="hero-image">

<!-- NG: altが空文字 -->
<img src="images/hero-bg.webp" alt="" width="1200" height="800" class="hero-image">
```

### 5.2 動的要素（MP4動画）

LPに埋め込むMP4動画は、スマートフォンを含む全デバイスで確実に自動再生させるため、必ず以下の4属性をすべて付与すること。1つでも欠けた場合、iOSのSafariで自動再生が機能しない。

**必須属性（4つすべて必須）**:
- `autoplay` — 自動再生を有効にする
- `muted` — 音声をミュートにする（自動再生に必須）
- `playsinline` — iOSでインライン再生させる（全画面再生を防ぐ）
- `loop` — ループ再生させる

**【動画の実装例】**

```html
<!-- 正しい実装 -->
<video autoplay muted playsinline loop class="demo-video">
  <source src="videos/product-demo.mp4" type="video/mp4">
</video>

<!-- 幅・高さをCSSで制御する場合 -->
<div class="video-wrapper">
  <video autoplay muted playsinline loop class="demo-video">
    <source src="videos/product-demo.mp4" type="video/mp4">
  </video>
</div>
```


```css
.video-wrapper {
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

.demo-video {
  width: 100%;
  height: auto;
  display: block;
}
```

**【禁止されている実装例】**

```html
<!-- NG: playsinlineが欠けている（iOSで全画面になる） -->
<video autoplay muted loop src="videos/product-demo.mp4"></video>

<!-- NG: mutedが欠けている（ブラウザが自動再生をブロックする） -->
<video autoplay playsinline loop src="videos/product-demo.mp4"></video>
```

### 5.3 GIFアニメーション

読み込み順序やパフォーマンスを考慮し、メインコンテンツのレンダリングをブロックしないよう配慮すること。ファーストビュー外のGIFには必ず `loading="lazy"` 属性を付与して遅延読み込みを実装すること。`width` と `height` 属性も必ず明記すること。

**【GIFアニメーションの実装例】**

```html
<!-- ファーストビュー外のGIF（遅延読み込み） -->
<img
  src="images/step-animation.gif"
  alt="使用手順のアニメーション"
  width="600"
  height="400"
  loading="lazy"
  class="step-gif"
>
```

---

## 6. 命名規則と設計思想

### 6.1 CSSクラス命名規則

クラス名はすべて**ケバブケース（kebab-case）**を使用すること。

| 命名方式 | 例 | 可否 |
|---------|-----|------|
| ケバブケース | `foo-bar`, `section-title` | **必須** |
| キャメルケース | `fooBar`, `sectionTitle` | **禁止** |
| スネークケース | `foo_bar`, `section_title` | **禁止** |
| パスカルケース | `FooBar`, `SectionTitle` | **禁止** |

### 6.2 コンポーネント設計（簡易BEM）

コンポーネントの分割・再利用がしやすいよう、破綻しにくい設計を取り入れること。簡易的なBEM（Block Element Modifier）の概念を採用し、親要素（Block）と子要素（Element）の関係性を明確にする。

**命名パターン**:
- **Block**: コンポーネントの親要素。例: `.card-item`
- **Element**: Blockに属する子要素。例: `.card-item-title`, `.card-item-image`
- **Modifier**: 状態やバリエーション。例: `.btn-primary`, `.btn-secondary`

**【簡易BEMの実装例】**

```html
<!-- Block -->
<div class="card-item">
  <!-- Element -->
  <img
    src="images/thumb.webp"
    alt="商品サムネイル"
    width="300"
    height="200"
    loading="lazy"
    class="card-item-image"
  >
  <div class="card-item-body">
    <h3 class="card-item-title">商品タイトル</h3>
    <p class="card-item-desc">商品の説明文がここに入ります。</p>
  </div>
  <!-- Modifier（バリエーション） -->
  <a href="#purchase" class="btn btn-primary">今すぐ購入する</a>
</div>
```


```css
/* Block */
.card-item {
  background-color: #ffffff;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

/* Element */
.card-item-image {
  width: 100%;
  height: auto;
}

.card-item-body {
  padding: 20px;
}

.card-item-title {
  font-size: 1.8rem;
  font-weight: 700;
  margin-bottom: 10px;
}

.card-item-desc {
  font-size: 1.4rem;
  color: #666666;
}

/* Modifier */
.btn {
  display: inline-block;
  padding: 14px 30px;
  border-radius: 4px;
  font-weight: 700;
  text-align: center;
  cursor: pointer;
  transition: opacity 0.2s ease;
}

.btn:hover {
  opacity: 0.8;
}

.btn-primary {
  background-color: #e74c3c;
  color: #ffffff;
}

.btn-secondary {
  background-color: #ffffff;
  color: #e74c3c;
  border: 2px solid #e74c3c;
}
```

---

## 7. レイアウト実装ルール（Flexbox / CSS Grid）

### 7.1 Flexboxの使用ルール

1次元のレイアウト（横並び・縦並び）には**Flexbox**を使用すること。

```css
/* 横並び（デフォルト） */
.flex-row {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20px;
}

/* 縦並び */
.flex-column {
  display: flex;
  flex-direction: column;
  gap: 16px;
}

/* 中央揃え */
.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* レスポンシブ対応（モバイルで縦並び、PCで横並び） */
.features-list {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

@media screen and (min-width: 768px) {
  .features-list {
    flex-direction: row;
    flex-wrap: wrap;
  }

  .features-list .feature-item {
    flex: 1 1 calc(50% - 12px);
  }
}
```

### 7.2 CSS Gridの使用ルール

2次元のレイアウト（行と列の両方を制御する場合）には**CSS Grid**を使用すること。

```css
/* 2カラムグリッド */
.two-col-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}

@media screen and (min-width: 768px) {
  .two-col-grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* 3カラムグリッド */
.three-col-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
}

@media screen and (min-width: 768px) {
  .three-col-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
```

---

## 8. 特殊な演出・レイアウトの実装テンプレート

### 8.1 背景画像上の要素絶対配置

背景画像（例: `bg.webp`）の上に、別の要素（例: `item.webp`）を絶対配置（`position: absolute;`）で重ねる実装は頻出するため、以下のテンプレートを厳守すること。親要素には必ず `position: relative;` を指定し、絶対配置の基準点を作成すること。背景画像を `background-image` プロパティで指定することは禁止する。必ず `<img>` タグで実装すること（これにより `alt` 属性が付与でき、アクセシビリティとSEOが向上する）。

**【HTML実装テンプレート】**

```html
<div class="relative-wrapper">
  <!-- 背景となるベース画像 -->
  <img
    src="images/bg.webp"
    alt="セクション背景"
    width="1000"
    height="600"
    class="base-bg"
  >
  <!-- 絶対配置で重ねる要素 -->
  <img
    src="images/item.webp"
    alt="重ねるアイテム"
    width="200"
    height="200"
    class="absolute-item"
  >
</div>
```

**【CSS実装テンプレート】**

```css
/* 親要素: position: relative が必須 */
.relative-wrapper {
  position: relative;
  width: 100%;
  max-width: 1000px;
  margin: 0 auto;
}

/* ベース背景画像 */
.base-bg {
  display: block;
  width: 100%;
  height: auto;
}

/* 絶対配置する要素 */
.absolute-item {
  position: absolute;
  /* 位置はtop/right/bottom/leftとtransformで指定する */
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /* 中央配置の場合 */
  width: 20%; /* レスポンシブ対応のため%指定を推奨 */
  height: auto;
}
```

**【位置指定のバリエーション】**

```css
/* 右上に配置する場合 */
.absolute-item-top-right {
  position: absolute;
  top: 5%;
  right: 5%;
  width: 15%;
  height: auto;
}

/* 左下に配置する場合 */
.absolute-item-bottom-left {
  position: absolute;
  bottom: 5%;
  left: 5%;
  width: 15%;
  height: auto;
}
```

### 8.2 CSSキーフレームアニメーション（ふわふわループ）

重ねた要素などを「上下にふわふわループ動かす」汎用的なアニメーションクラスを定義する。クラス名は必ず `.animation-float` とすること。このクラスは他のクラスと組み合わせて使用する。

**【CSSアニメーション定義（必ず記述すること）】**

```css
/* ==========================================================================
   Animation: Float (上下ふわふわループ)
   ========================================================================== */

@keyframes float-up-down {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-15px); /* 浮き上がる距離。調整可能 */
  }
  100% {
    transform: translateY(0);
  }
}

.animation-float {
  animation: float-up-down 3s ease-in-out infinite;
}
```

**【絶対配置と組み合わせた使用例】**

```html
<div class="relative-wrapper">
  <img src="images/bg.webp" alt="背景" width="1000" height="600" class="base-bg">
  <!-- animation-floatクラスを追加するだけでアニメーションが適用される -->
  <img
    src="images/item.webp"
    alt="浮遊するアイコン"
    width="200"
    height="200"
    class="absolute-item animation-float"
  >
</div>
```

**【アニメーション速度・距離のカスタマイズ】**

アニメーションの速度や浮き上がり距離を変更したい場合は、`.animation-float` クラスを継承した別クラスを作成すること。元の `.animation-float` クラスの定義は変更しないこと。

```css
/* ゆっくりした動き */
.animation-float-slow {
  animation: float-up-down 5s ease-in-out infinite;
}

/* 大きく動く */
.animation-float-large {
  animation: float-up-down 3s ease-in-out infinite;
}

@keyframes float-up-down-large {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-30px); }
  100% { transform: translateY(0); }
}

.animation-float-large {
  animation: float-up-down-large 3s ease-in-out infinite;
}
```

---

## 9. Vanilla JavaScript記述ルール

### 9.1 基本ルール

- 外部ライブラリ（jQuery等）は一切使用しないこと。
- `var` の使用は禁止する。変数宣言は `const` を優先し、再代入が必要な場合のみ `let` を使用すること。
- すべてのJavaScriptは `js/main.js` に記述し、HTMLファイルの `</body>` 直前で読み込むこと。

### 9.2 DOMの取得と操作

```javascript
// 正しい実装
const targetElement = document.querySelector('.target-class');
const allItems = document.querySelectorAll('.item');

// イベントリスナーの追加
const button = document.querySelector('.btn-primary');
button.addEventListener('click', () => {
  // 処理
});

// クラスの操作
targetElement.classList.add('is-active');
targetElement.classList.remove('is-active');
targetElement.classList.toggle('is-active');
```

### 9.3 スクロールアニメーション（IntersectionObserver）

スクロールに連動したフェードインアニメーションは、`IntersectionObserver` を使用して実装すること。`scroll` イベントへの直接バインドは禁止する（パフォーマンス上の理由から）。

```javascript
// スクロールアニメーション（フェードイン）
const fadeInElements = document.querySelectorAll('.fade-in');

const observer = new IntersectionObserver((entries) => {
  entries.forEach((entry) => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
      observer.unobserve(entry.target); // 一度発火したら監視を解除
    }
  });
}, {
  threshold: 0.1, // 要素の10%が見えたら発火
});

fadeInElements.forEach((el) => observer.observe(el));
```


```css
/* フェードインアニメーション用CSS */
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

---

## 10. 汎用性と他プラットフォームへのコンバート対応

### 10.1 対応プラットフォーム一覧

本ルールに従って制作したHTMLは、以下のプラットフォームへの変換・移植を前提とする。

| プラットフォーム | 変換方式 | 注意点 |
|----------------|---------|--------|
| EC Force | インラインCSS化 + 独自タグ | 複雑なセレクタは変換後に崩れる可能性がある |
| EC-CUBE | テンプレートファイルへの組み込み | PHPタグ挿入箇所を独立した要素として確保 |
| SquadBeyond | HTMLブロックへの貼り付け | ネストが深すぎると管理が困難になる |
| Shopify | Liquidテンプレートへの変換 | `{{ }}` タグ挿入箇所を明確に分離 |
| WordPress | PHPテンプレートへの変換 | `<?php ?>` タグ挿入箇所を明確に分離 |

### 10.2 美しく標準的なHTML構造の徹底

HTML構造はハック的な書き方（不自然なネスト、意味のないラッパー要素の多用、CSSに依存しすぎたトリッキーなマークアップ）を一切禁止する。常に美しく、標準的で、誰が見ても意図が明確な構造を維持すること。

### 10.3 インラインスタイルへの変換を阻害しない記述

CSSは外部ファイル（または `<style>` ブロック）に記述するが、インラインCSS化ツールを通した際にレイアウトが崩れないよう、極端に複雑なセレクタの使用は避けること。クラスセレクタ（`.class-name`）をベースとした、シンプルで直接的なスタイリングを心がけること。

**【避けるべきセレクタの例】**

```css
/* NG: 過度に複雑なセレクタ（インラインCSS化で崩れる可能性がある） */
div > ul li:nth-child(2n+1) span::before { ... }
.parent .child + .sibling ~ .target { ... }

/* OK: シンプルなクラスセレクタ */
.list-item-odd { ... }
.sibling-target { ... }
```

### 10.4 動的要素の独立したマークアップ

各種カートシステムの独自タグ（例: ShopifyのLiquidタグ `{{ product.title }}`、WordPressのPHP関数 `<?php the_title(); ?>`）を後から挿入しやすいよう、動的になり得る要素（商品名、価格、カートボタンなど）は独立した要素としてマークアップし、他の要素と密結合させないこと。

**【正しい実装例（動的要素の独立）】**

```html
<!-- 商品情報ブロック: 各要素が独立しており、後からタグを挿入しやすい -->
<div class="product-info">
  <h2 class="product-name">商品名がここに入ります</h2>
  <p class="product-price">¥3,980（税込）</p>
  <p class="product-desc">商品の説明文がここに入ります。</p>
  <a href="#" class="btn btn-primary btn-cart">カートに入れる</a>
</div>
```

---

## 11. 絶対禁止事項

以下の実装は、いかなる理由があっても行ってはならない。

| 禁止事項 | 理由 |
|---------|------|
| `!important` の多用 | スタイルの優先度管理が破綻し、保守性が著しく低下する |
| インラインスタイル（`style=""`）の直接記述 | プラットフォーム変換時に二重適用が発生し、意図しない表示崩れを招く |
| `<table>` タグによるレイアウト | セマンティクスに反し、レスポンシブ対応が困難になる |
| jQuery等の外部ライブラリの読み込み | ページ表示速度の低下と外部依存リスクが生じる |
| `var` による変数宣言 | スコープの問題によるバグの原因となる |
| `scroll` イベントへの直接バインド | パフォーマンスの低下を招く（`IntersectionObserver` を使用すること） |
| `background-image` による画像の埋め込み（装飾目的以外） | `alt` 属性が付与できず、SEO・アクセシビリティが低下する |
| `<img>` タグの `alt` 属性の省略または空文字 | アクセシビリティ基準（WCAG）に違反する |
| `<img>` タグの `width`・`height` 属性の省略 | レイアウトシフト（CLS）が発生し、Core Web Vitalsのスコアが低下する |
| MP4動画の `muted` 属性の省略 | ブラウザの自動再生ポリシーにより自動再生がブロックされる |
| MP4動画の `playsinline` 属性の省略 | iOSのSafariで全画面再生が強制される |
| キャメルケース・スネークケースのクラス名 | 命名規則の統一性が失われ、コードの可読性が低下する |

---

*本ガイドラインは制作部の全メンバーおよびAIコーディングシステムに適用される。改訂が必要な場合は、制作部責任者の承認を経て更新すること。*
