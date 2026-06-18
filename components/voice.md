# voice.md — ユーザーボイス・お客様の声 コンポーネント

> このファイルは `components/voice.md` として保存し、ユーザーボイスセクション制作直前に読み込む。

---

## V1：カード型（グリッド並び）

### ① 概要
顔写真・名前・年齢・コメントをカード形式で並べるレイアウト。3〜4件を横並びで表示する。

### ② 確認事項

> **【AIへの指示】** `必須` は会話で確認。`任意` はデフォルト値で進め、指定があれば変更。

| 区分 | 項目 | 確認内容 | デフォルト値 |
|------|------|---------|------------|
| **必須** | カード数 | 何件表示するか | — |
| **必須** | 顔写真ファイル名 | 各カードの画像パス | — |
| 任意 | 表示項目 | 名前・年齢・職業・コメント等 | 名前・コメント |
| 任意 | 星評価 | ★評価の表示有無 | あり（5段階）|
| 任意 | 背景色 | セクション背景色 | `#f9f9f9` |

### ③ テンプレート

```html
<section id="voice" class="section-voice">
  <div class="inner-container">
    <h2 class="section-title">【SECTION_TITLE】</h2>
    <div class="voice-grid">
      <!-- カード1 -->
      <div class="voice-card fade-in">
        <div class="voice-card-header">
          <img src="images/voice-01.webp" alt="【NAME】様" width="80" height="80" loading="lazy" class="voice-card-avatar">
          <div class="voice-card-meta">
            <p class="voice-card-name">【NAME】様</p>
            <p class="voice-card-attr">【AGE】・【OCCUPATION】</p>
            <div class="voice-card-stars" aria-label="評価：5点中5点">
              <span class="star is-filled">★</span>
              <span class="star is-filled">★</span>
              <span class="star is-filled">★</span>
              <span class="star is-filled">★</span>
              <span class="star is-filled">★</span>
            </div>
          </div>
        </div>
        <p class="voice-card-comment">【COMMENT】</p>
      </div>
      <!-- カード2, 3... 同じ構造で繰り返す -->
    </div>
  </div>
</section>
```

```css
/* Voice Section */
.section-voice { padding: 60px 0; background-color: #f9f9f9; }
.voice-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 24px;
  margin-top: 40px;
}
@media screen and (min-width: 768px) {
  .voice-grid { grid-template-columns: repeat(3, 1fr); }
}
.voice-card {
  background-color: #ffffff;
  border-radius: 8px;
  padding: 24px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.06);
}
.voice-card-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 16px;
}
.voice-card-avatar {
  width: 80px;
  height: 80px;
  border-radius: 50%;
  object-fit: cover;
  flex-shrink: 0;
}
.voice-card-name { font-size: 1.5rem; font-weight: 700; }
.voice-card-attr { font-size: 1.2rem; color: #888; margin-top: 4px; }
.voice-card-stars { margin-top: 6px; }
.star { font-size: 1.6rem; color: #ddd; }
.star.is-filled { color: #f5a623; }
.voice-card-comment { font-size: 1.4rem; line-height: 1.8; color: #555; }
```

---

## V2：ビフォーアフタースライダー付き

### ① 概要
使用前・使用後の画像を横スライドで比較できるビフォーアフタースライダー。コメントと組み合わせて表示する。

### ② 確認事項

> **【AIへの指示】** `必須` は会話で確認。`任意` はデフォルト値で進め、指定があれば変更。

| 区分 | 項目 | 確認内容 | デフォルト値 |
|------|------|---------|------------|
| **必須** | 件数 | 何件表示するか | — |
| **必須** | Before/After画像 | 各件のファイル名 | — |
| 任意 | スライダー操作 | ドラッグ or ボタン | ドラッグ |
| 任意 | コメント表示 | 画像下にコメントを表示するか | あり |
| 任意 | JS実装 | Vanilla JS or jQuery twentytwenty | Vanilla JS |

### ③ テンプレート

```html
<section id="voice" class="section-voice">
  <div class="inner-container">
    <h2 class="section-title">【SECTION_TITLE】</h2>
    <div class="ba-list">
      <!-- ビフォーアフターアイテム1 -->
      <div class="ba-item fade-in">
        <div class="ba-slider" data-ba-slider>
          <div class="ba-before">
            <img src="images/voice-01-before.webp" alt="使用前" width="500" height="500" loading="lazy">
            <span class="ba-label ba-label-before">Before</span>
          </div>
          <div class="ba-after">
            <img src="images/voice-01-after.webp" alt="使用後" width="500" height="500" loading="lazy">
            <span class="ba-label ba-label-after">After</span>
          </div>
          <div class="ba-handle" aria-label="スライダーを動かしてビフォーアフターを確認"></div>
        </div>
        <div class="ba-meta">
          <p class="ba-name">【NAME】様（【AGE】）</p>
          <p class="ba-comment">【COMMENT】</p>
        </div>
      </div>
      <!-- アイテム2, 3... 同じ構造で繰り返す -->
    </div>
  </div>
</section>
```

```css
/* Before/After Slider */
.ba-list { display: flex; flex-direction: column; gap: 40px; margin-top: 40px; }
@media screen and (min-width: 768px) {
  .ba-list { flex-direction: row; flex-wrap: wrap; }
  .ba-item { flex: 1 1 calc(33.333% - 16px); }
}
.ba-slider {
  position: relative;
  overflow: hidden;
  cursor: ew-resize;
  user-select: none;
  border-radius: 8px;
}
.ba-before, .ba-after { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }
.ba-before { z-index: 1; clip-path: inset(0 50% 0 0); } /* JSで動的に変更 */
.ba-after { z-index: 0; }
.ba-before img, .ba-after img { width: 100%; height: 100%; object-fit: cover; display: block; }
.ba-label {
  position: absolute;
  bottom: 8px;
  font-size: 1.2rem;
  font-weight: 700;
  color: #fff;
  background-color: rgba(0,0,0,0.5);
  padding: 2px 8px;
  border-radius: 3px;
}
.ba-label-before { left: 8px; }
.ba-label-after { right: 8px; }
.ba-handle {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 4px;
  height: 100%;
  background-color: #fff;
  z-index: 2;
  cursor: ew-resize;
}
.ba-handle::before, .ba-handle::after {
  content: '';
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 0;
  height: 0;
}
.ba-handle::before { left: -8px; border-top: 8px solid transparent; border-bottom: 8px solid transparent; border-right: 8px solid #fff; }
.ba-handle::after { right: -8px; border-top: 8px solid transparent; border-bottom: 8px solid transparent; border-left: 8px solid #fff; }
.ba-meta { padding: 16px 0; }
.ba-name { font-size: 1.4rem; font-weight: 700; margin-bottom: 8px; }
.ba-comment { font-size: 1.3rem; color: #555; line-height: 1.8; }
```

```javascript
// Before/After Slider
document.querySelectorAll('[data-ba-slider]').forEach((slider) => {
  const before = slider.querySelector('.ba-before');
  let isDragging = false;

  function updateSlider(x) {
    const rect = slider.getBoundingClientRect();
    let ratio = (x - rect.left) / rect.width;
    ratio = Math.max(0, Math.min(1, ratio));
    const percent = ratio * 100;
    before.style.clipPath = `inset(0 ${100 - percent}% 0 0)`;
    slider.querySelector('.ba-handle').style.left = `${percent}%`;
  }

  slider.addEventListener('mousedown', (e) => { isDragging = true; updateSlider(e.clientX); });
  slider.addEventListener('touchstart', (e) => { isDragging = true; updateSlider(e.touches[0].clientX); }, { passive: true });
  window.addEventListener('mousemove', (e) => { if (isDragging) updateSlider(e.clientX); });
  window.addEventListener('touchmove', (e) => { if (isDragging) updateSlider(e.touches[0].clientX); }, { passive: true });
  window.addEventListener('mouseup', () => { isDragging = false; });
    window.addEventListener('touchend', () => { isDragging = false; });
});
```

> **jQueryプロジェクトの場合：** `jquery.twentytwenty.js` + `jquery.event.move.js` を使用することも可。
> - HTML: `<div id="before-after"><img class="twentytwenty-before" src="before.jpg" alt="Before"><img class="twentytwenty-after" src="after.jpg" alt="After"></div>`
> - CSS: `twentytwenty.css` を読み込む
> - JS: `$(function(){ $("#before-after").twentytwenty(); });`
> - 注意：jQuery版はVanilla JS版と**同時に使用しない**こと

---

## V3：医師・専門家コメント型

### ① 概要
医師・機能性表示食品責任者等の専門家の言葉を引用形式で表示するレイアウト。顔写真・資格・コメントをセットにする（カテキン型）。

### ② 確認事項

> **【AIへの指示】** `必須` は会話で確認。`任意` はデフォルト値で進め、指定があれば変更。

| 区分 | 項目 | 確認内容 | デフォルト値 |
|------|------|---------|------------|
| **必須** | 件数 | 何件表示するか | — |
| **必須** | 顔写真ファイル名 | 各件の画像パス | — |
| **必須** | 資格・所属 | 資格名・所属テキスト | — |
| **必須** | コメント | 引用するテキスト | — |
| 任意 | 背景色 | セクション背景色 | `#f5f5f5` |

### ③ テンプレート

```html
<section id="doctor" class="section-doctor">
  <div class="inner-container">
    <h2 class="section-title">「ここにセクションタイトル」</h2>
    <div class="doctor-card fade-in">
      <div class="doctor-card-header">
        <img src="images/doctor-01.webp" alt="《名前》先生" width="120" height="120" loading="lazy" class="doctor-card-photo">
        <div class="doctor-card-meta">
          <p class="doctor-card-name">《名前》</p>
          <p class="doctor-card-title">《資格・所属》</p>
        </div>
      </div>
      <blockquote class="doctor-card-comment">
        <p>《コメント》</p>
      </blockquote>
    </div>
  </div>
</section>
```

```css
/* Doctor Comment */
.section-doctor { padding: 60px 0; background-color: #f5f5f5; }
.doctor-card {
  max-width: 700px;
  margin: 40px auto 0;
  background-color: #ffffff;
  border-radius: 8px;
  padding: 32px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.06);
}
.doctor-card-header {
  display: flex;
  align-items: center;
  gap: 24px;
  margin-bottom: 24px;
}
.doctor-card-photo {
  width: 120px;
  height: 120px;
  border-radius: 50%;
  object-fit: cover;
  flex-shrink: 0;
}
.doctor-card-name { font-size: 1.8rem; font-weight: 700; }
.doctor-card-title { font-size: 1.3rem; color: #888; margin-top: 6px; }
.doctor-card-comment {
  border-left: 4px solid var(--color-accent);
  padding-left: 20px;
  margin: 0;
  font-size: 1.5rem;
  line-height: 2;
  color: #444;
}
```


