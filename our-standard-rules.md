# 制作部共通コーディングルール・ガイドライン（The Law Book）

> **【AIへの指示】**
> 本ドキュメントは、LPコーディングにおいて全セクションに適用される「グローバルなコーディング法典」である。
> セクション固有の実装（アニメーションやフォーム等）については、`components/` 以下の該当ファイルを参照すること。

---

## 1. 基本テックスタックと構造ルール

### 1.1 使用技術スタック
- **HTML5**: セマンティックマークアップ必須
- **CSS3**: モダンCSS・レスポンシブ対応必須
- **JavaScript**: ES6+ (Vanilla JS)。外部ライブラリへの依存を禁ずる

### 1.2 HTMLドキュメントの基本構造
`lang="ja"`、`charset="UTF-8"` を必須とする。

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
  <header class="site-header"></header>
  <main class="site-main">
    <!-- セクションをここに追加 -->
  </main>
  <footer class="site-footer"></footer>
  <script src="js/main.js"></script>
</body>
</html>
```

### 1.3 HTMLセマンティックマークアップ
- セクションの区切りには必ず `<section>` タグを使用すること（`<div>` は禁止）。
- 見出しタグは文書構造に従い、上から順に `<h2>`, `<h3>`, `<h4>` と階層を正しく使用すること。
- `<h1>` はページ全体で1回のみ使用する。

---

## 2. ファイル・ディレクトリ構成

ファイル名はすべて小文字のケバブケースで統一すること。

```
project-root/
├── index.html
├── css/style.css
├── js/main.js
├── images/
└── videos/
```

---

## 3. CSSベーススタイルとリセット

すべてのCSSファイルの冒頭に、以下のリセット・ベーススタイルを必ず記述すること。

```css
/* ==========================================================================
   Reset & Base Styles
   ========================================================================== */

*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  font-size: 62.5%; /* 1rem = 10px */
  scroll-behavior: smooth;
}

body {
  font-family: var(--font-family-base, "Noto Sans JP", sans-serif);
  font-size: 1.6rem;
  line-height: 1.8;
  color: var(--color-text, #333333);
  background-color: var(--color-base, #ffffff);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

img, video {
  max-width: 100%;
  height: auto;
  display: block;
}

a {
  color: inherit;
  text-decoration: none;
}

ul, ol {
  list-style: none;
}

/* ==========================================================================
   Layout Utilities
   ========================================================================== */

.inner-container {
  width: 90%;
  max-width: var(--max-width-content, 1000px);
  margin: 0 auto;
}
```

---

## 4. レスポンシブ対応とブレークポイント

レスポンシブ対応は**モバイルファースト**を基本とし、`min-width` を使用すること。

| ブレークポイント名 | 幅 | 対象デバイス |
|-------------------|----|-------------|
| `sm` | 〜 767px | スマートフォン（デフォルトのCSSとして記述） |
| `md` | 768px 〜 | タブレット・PC |
| `lg` | 1200px 〜 | 大画面PC |

---

## 5. 命名規則と設計思想

クラス名はすべて**ケバブケース（kebab-case）**を使用すること。キャメルケース、スネークケース、パスカルケースはすべて禁止する。
簡易的なBEM（Block Element Modifier）の概念を採用し、親要素と子要素の関係性を明確にすること（例: `.card-item`, `.card-item-title`, `.btn-primary`）。

---

## 6. プラットフォームコンバートへの配慮

制作したHTMLは、EC Force、Shopify、WordPress等への変換を前提とする。
- インラインCSS化ツールを通した際にレイアウトが崩れないよう、極端に複雑なセレクタ（例: `div > ul li:nth-child(2n+1) span::before`）の使用を避けること。
- 動的になり得る要素（商品名、価格など）は独立した要素としてマークアップし、他の要素と密結合させないこと。

---

## 7. 絶対禁止事項（Global Prohibitions）

以下の実装は、いかなる理由があっても行ってはならない。

| 禁止事項 | 理由 |
|---------|------|
| `!important` の多用 | スタイルの優先度管理が破綻し、保守性が著しく低下するため |
| インラインスタイル（`style=""`）の直接記述 | プラットフォーム変換時に二重適用が発生するため |
| `<table>` タグによるレイアウト | セマンティクスに反し、レスポンシブ対応が困難になるため |
| jQuery等の外部ライブラリの読み込み | 初期ヒアリングで許可された場合を除き、外部依存リスクを避けるため |
| `var` による変数宣言 | スコープの問題によるバグの原因となるため（`const` / `let` を使用） |
| `scroll` イベントへの直接バインド | パフォーマンスの低下を招くため（`IntersectionObserver` を使用） |
| `background-image` による画像の埋め込み | 装飾目的以外では `alt` 属性が付与できず、アクセシビリティが低下するため |
| `<img>` タグの `alt`, `width`, `height` 属性の省略 | CLS（レイアウトシフト）の発生とアクセシビリティ違反を防ぐため |
| MP4動画の `autoplay muted playsinline loop` 属性の省略 | iOS等で自動再生がブロックされるのを防ぐため（4つすべて必須） |
