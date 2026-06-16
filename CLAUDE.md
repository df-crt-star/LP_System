# CLAUDE.md — LP_System プロジェクト指示書

このファイルはClaude Codeがこのリポジトリで作業する際の指示書です。

---

## Coding Guidelines（HTML/CSS/JS 絶対遵守ルール）

**本プロジェクトでHTML・CSS・JavaScriptを生成・編集する際は、必ず `/our-standard-rules.md`（制作部共通コーディングルール・ガイドライン）をルールブックとして厳格に遵守すること。一項目たりとも省略・無視することを禁ずる。**

主要な遵守事項（詳細は `our-standard-rules.md` を参照）:

- **HTML構造**: `lang="ja"`, `charset="UTF-8"`, セマンティックタグ（`<section>` / `<main>` / `<header>` / `<footer>`）、`<h1>` はページ内1回のみ、見出し階層を正しく維持すること
- **ファイル構成**: `index.html` / `css/style.css` / `js/main.js` / `images/` / `videos/` の標準構成を厳守すること
- **CSSリセット**: すべてのCSSファイル冒頭に `our-standard-rules.md` §3 のリセット・ベーススタイルブロックを必ず記述すること
- **レスポンシブ**: モバイルファースト・`min-width` メディアクエリのみ使用（`max-width` のみは禁止）。ブレークポイントは sm(〜767px) / md(768px〜) / lg(1200px〜)
- **画像**: WebP形式、`<img>` タグに `width` / `height` / `alt`（空文字禁止）必須。ファーストビュー外は `loading="lazy"`
- **動画**: MP4は `autoplay muted playsinline loop` の4属性すべて必須
- **クラス命名**: ケバブケース（kebab-case）のみ。キャメル・スネーク・パスカルは禁止
- **JavaScript**: Vanilla JSのみ（jQuery等の外部ライブラリ禁止）、`var` 禁止、スクロールアニメーションは `IntersectionObserver` を使用
- **絶対禁止**: `!important` の多用、インラインスタイル直書き、`<table>` レイアウト、`background-image` による非装飾画像の埋め込み、`scroll` イベントへの直接バインド

コード生成前に `our-standard-rules.md` の該当セクションを参照し、すべての要件を満たしていることを確認してからコードを出力すること。
