# rules-core.md — 制作部共通コーディング絶対ルール

**バージョン**: 2.0.0 / **適用範囲**: 制作部が制作するすべてのLPコーディング成果物

---

## 1. テックスタック（絶対遵守）

| 技術 | 仕様 |
|------|------|
| HTML | HTML5・セマンティックマークアップ必須 |
| CSS | CSS3・モバイルファースト・`min-width` メディアクエリ |
| JavaScript | ES6+ Vanilla JS のみ（外部ライブラリ禁止） |
| 画像 | WebP（`.webp`）必須 |
| 動画 | MP4（`autoplay muted playsinline loop` 4属性すべて必須） |

---

## 2. CSS変数（案件ごとに値を設定する）

```css
:root {
  --max-width: 1000px;          /* PC版最大横幅 */
  --color-main: #333333;        /* メインカラー */
  --color-accent: #e74c3c;      /* アクセントカラー */
  --color-bg: #ffffff;          /* 背景色 */
  --font-family-base: "Noto Sans JP", "Hiragino Kaku Gothic ProN", sans-serif;
  --font-size-base: 1.6rem;
  --line-height-base: 1.8;
}
```

---

## 3. CSSリセット・ベーススタイル（必ず冒頭に記述）

```css
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
html { font-size: 62.5%; scroll-behavior: smooth; }
body {
  font-family: var(--font-family-base);
  font-size: var(--font-size-base);
  line-height: var(--line-height-base);
  color: var(--color-main);
  background-color: var(--color-bg);
  -webkit-font-smoothing: antialiased;
}
img, video { max-width: 100%; height: auto; display: block; }
a { color: inherit; text-decoration: none; }
ul, ol { list-style: none; }
.inner-container { width: 90%; max-width: var(--max-width); margin: 0 auto; }
```

---

## 4. ブレークポイント

| 名前 | 幅 | 用途 |
|------|----|------|
| `sm` | 〜767px | スマートフォン（デフォルト） |
| `md` | 768px〜 | タブレット・PC |
| `lg` | 1200px〜 | 大画面PC |

メディアクエリは `min-width` のみ使用。`max-width` 単独は禁止。

---

## 5. 命名規則

- クラス名：**ケバブケース**のみ（例: `section-title`）
- キャメルケース・スネークケース・パスカルケース：**禁止**
- 構造：簡易BEM（Block / Block-Element / Block--Modifier）

---

## 6. 画像・動画の必須実装ルール

**画像（`<img>`）**
- `width`, `height`, `alt` 属性を必ず明記
- `alt` は空文字禁止（内容を簡潔に説明する）
- ファーストビュー外は `loading="lazy"` を付与
- 背景画像は `background-image` 禁止 → 必ず `<img>` タグで実装

**動画（`<video>`）**
- `autoplay muted playsinline loop` の4属性すべて必須

---

## 7. JavaScript基本ルール

- `var` 禁止 → `const` 優先、再代入時のみ `let`
- スクロールアニメーション：`scroll` イベント直接バインド禁止 → `IntersectionObserver` を使用
- すべてのJSは `js/main.js` に記述、`</body>` 直前で読み込む

---

## 8. 絶対禁止事項

| 禁止事項 | 理由 |
|---------|------|
| `!important` の多用 | 保守性破綻 |
| インラインスタイル（`style=""`） | プラットフォーム変換時に二重適用が発生 |
| `<table>` によるレイアウト | レスポンシブ対応不可 |
| jQuery等の外部ライブラリ | 表示速度低下・外部依存リスク |
| `var` による変数宣言 | スコープバグの原因 |
| `scroll` イベントへの直接バインド | パフォーマンス低下 |
| `background-image` による画像埋め込み（装飾目的以外） | alt属性付与不可・SEO低下 |
| `<img>` の `alt` 省略または空文字 | WCAG違反 |
| `<img>` の `width`・`height` 省略 | レイアウトシフト（CLS）発生 |
| MP4の `muted` 省略 | 自動再生ブロック |
| MP4の `playsinline` 省略 | iOSで全画面強制 |
| キャメルケース・スネークケースのクラス名 | 命名規則違反 |

---

*コード例（HTML/CSS/JSの実装テンプレート）は `core/rules-reference.md` を参照すること。*
