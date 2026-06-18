# slider.md — Slickスライダー コンポーネント

> このファイルは `components/slider.md` として保存し、スライダーを含むセクション制作直前に読み込む。
> **前提：** jQuery と slick.min.js が読み込まれていること（`<head>` 内で読み込む）。

```html
<!-- head内に追加 -->
<link rel="stylesheet" href="slick/slick.css">
<link rel="stylesheet" href="slick/slick-theme.css">
<!-- body末尾に追加 -->
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<script src="slick/slick.min.js"></script>
```

---

## SL1：通常スライダー（ドット・矢印付き）

### ① 概要
画像・GIF・カードを1枚ずつスライドするスタンダードなスライダー。ドットナビゲーションと矢印ボタンを表示する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| スライド枚数 | 何枚スライドするか | 3〜5枚 |
| スライドの中身 | 画像 / GIF / カード | 画像（`images/slide-01.webp`）|
| 自動再生 | あり or なし | あり（3000ms）|
| ドット | 表示 or 非表示 | 表示 |
| 矢印 | 表示 or 非表示 | 表示 |
| ループ | 無限ループ or 折り返し | 無限ループ |

### ③ テンプレート

```html
<section id="slider" class="section-slider">
  <div class="inner-container">
    <h2 class="section-title">【SECTION_TITLE】</h2>
    <div class="slick-slider-wrap">
      <div class="js-slick-normal">
        <div class="slick-slide-item">
          <img src="images/slide-01.webp" alt="【ALT_TEXT】" loading="lazy">
        </div>
        <div class="slick-slide-item">
          <img src="images/slide-02.webp" alt="【ALT_TEXT】" loading="lazy">
        </div>
        <div class="slick-slide-item">
          <img src="images/slide-03.webp" alt="【ALT_TEXT】" loading="lazy">
        </div>
        <!-- スライドを繰り返す -->
      </div>
    </div>
  </div>
</section>
```

```css
/* SL1: 通常スライダー */
.section-slider { padding: 60px 0; }
.slick-slider-wrap { margin-top: 40px; }
.slick-slide-item img { width: 100%; display: block; }

/* ドットのカスタマイズ */
.js-slick-normal .slick-dots li button:before {
  font-size: 10px;
  color: #ccc;
  opacity: 1;
}
.js-slick-normal .slick-dots li.slick-active button:before {
  color: var(--color-accent);
  opacity: 1;
}
```

```javascript
// SL1: 通常スライダー
$(function() {
  $(".js-slick-normal").slick({
    autoplay: true,
    autoplaySpeed: 3000,
    dots: true,
    arrows: true,
    infinite: true,
    speed: 500,
    slidesToShow: 1,
    slidesToScroll: 1
  });
});
```

---

## SL2：自動無限ループ型（等速流れ）

### ① 概要
スライドが等速で自動的に流れ続けるスライダー。口コミカード・SNS投稿・ロゴ一覧の横スクロール表示に使用する（カテキン型）。

### ② 確認事項

> **【AIへの指示】** `必須` は会話で確認。`任意` はデフォルト値で進め、指定があれば変更。

| 区分 | 項目 | 確認内容 | デフォルト値 |
|------|------|---------|------------|
| **必須** | スライド画像ファイル名 | 各スライドの画像パス | — |
| 任意 | 流れる速さ | `speed` の値（ms）| `5000`（遅め）|
| 任意 | 1画面の表示枚数 | `slidesToShow` | `3` |
| 任意 | 矢印・ドット | 表示 or 非表示 | 非表示 |

### ③ テンプレート

```html
<section id="review" class="section-review">
  <div class="review-slider-outer"><!-- inner-containerを使わず全幅にする場合 -->
    <div class="js-slick-loop">
      <div class="slick-slide-item">
        <img src="images/review-01.webp" alt="" loading="lazy">
      </div>
      <div class="slick-slide-item">
        <img src="images/review-02.webp" alt="" loading="lazy">
      </div>
      <div class="slick-slide-item">
        <img src="images/review-03.webp" alt="" loading="lazy">
      </div>
      <!-- スライドを繰り返す -->
    </div>
  </div>
</section>
```

```css
/* SL2: 自動無限ループ */
.section-review { padding: 40px 0; overflow: hidden; }
.review-slider-outer { width: 100%; }
.js-slick-loop .slick-slide-item { padding: 0 8px; }
.js-slick-loop .slick-slide-item img { width: 100%; display: block; }
```

```javascript
// SL2: 自動無限ループ（等速流れ）
$(function() {
  $(".js-slick-loop").slick({
    autoplay: true,
    autoplaySpeed: 0,       // 停止なし
    speed: 5000,            // 流れる速さ（大きいほど遅い）
    cssEase: "linear",      // 等速
    dots: false,
    arrows: false,
    infinite: true,
    slidesToShow: 3,
    slidesToScroll: 1,
    pauseOnHover: false     // ホバーで止めない
  });
});
```

---

## SL3：逆方向ループ型（SL2と組み合わせ）

### ① 概要
SL2と逆方向に流れるスライダー。上下2列で逆流させることで視覚的なアクセントを作る。

### ② 確認事項

> **【AIへの指示】** `必須` は会話で確認。`任意` はデフォルト値で進め、指定があれば変更。

| 区分 | 項目 | 確認内容 | デフォルト値 |
|------|------|---------|------------|
| **必須** | スライド画像ファイル名 | 各スライドの画像パス | — |
| 任意 | SL2との組み合わせ | SL2の直下に配置するか | あり |
| 任意 | 流れる速さ | SL2と同じ or 異なる速さ | SL2と同じ（`5000`）|

### ③ テンプレート

```html
<!-- SL2の直下に追加 -->
<div class="js-slick-loop-reverse">
  <div class="slick-slide-item">
    <img src="images/review-04.webp" alt="" loading="lazy">
  </div>
  <div class="slick-slide-item">
    <img src="images/review-05.webp" alt="" loading="lazy">
  </div>
  <div class="slick-slide-item">
    <img src="images/review-06.webp" alt="" loading="lazy">
  </div>
</div>
```

```javascript
// SL3: 逆方向ループ（SL2の直後に追加）
$(function() {
  $(".js-slick-loop-reverse").slick({
    autoplay: true,
    autoplaySpeed: 0,
    speed: 5000,
    cssEase: "linear",
    dots: false,
    arrows: false,
    infinite: true,
    slidesToShow: 3,
    slidesToScroll: 1,
    pauseOnHover: false,
    rtl: true               // 逆方向
  });
});
```

---

## 注意事項

- Slickスライダーは**jQuery依存**のため、jQueryが不要な案件では使用しない
- `autoplaySpeed: 0` + `cssEase: "linear"` の組み合わせが等速流れの鍵
- 複数スライダーを同一ページで使う場合は**クラス名を別々にする**（`.js-slick-normal` / `.js-slick-loop` 等）
- `slick.css` と `slick-theme.css` の両方を読み込むこと
