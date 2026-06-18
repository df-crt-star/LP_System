# section-templates.md — セクション・テンプレートモジュール

> **【AIへの指示】**
> 本ファイルは、LPの各セクションを構築する際のベースとなるレイアウトフォーマット（A/B/C案）を定義したものである。
> STEP 3「コーディング」の際、AIは制作対象のセクション内容に応じて、以下のフォーマットから適切なものを選択して実装すること。
> インラインスタイル（`style=""`）の使用は絶対禁止。すべてのスタイルはCSSクラスで定義すること。

---

## 1. ヒーローセクション（ファーストビュー）のフォーマット

### フォーマットA: スタンダード（テキスト＋画像 分割型）

左側にキャッチコピーとCTAボタン、右側に商品画像を配置する王道レイアウト。

**HTML構造:**
```html
<section id="hero" class="section-hero">
  <div class="inner-container hero-grid">
    <div class="hero-content">
      <h1 class="hero-title">キャッチコピーがここに入ります</h1>
      <p class="hero-desc">サブコピーや簡単な説明文。</p>
      <a href="#form" class="btn btn-primary">今すぐ申し込む</a>
    </div>
    <div class="hero-image-wrapper">
      <img src="images/hero-img.webp" alt="商品画像" width="600" height="600" class="hero-image">
    </div>
  </div>
</section>
```

**CSS要件:**
- PC版（`md`以上）では CSS Grid または Flexbox で2カラムにする
- SP版では画像を上、テキストを下（またはその逆）の1カラムにする

```css
.section-hero { padding: 60px 0; }
.hero-grid {
  display: flex;
  flex-direction: column;
  gap: 32px;
  align-items: center;
}
@media screen and (min-width: 768px) {
  .hero-grid {
    flex-direction: row;
    justify-content: space-between;
  }
  .hero-content { flex: 1; }
  .hero-image-wrapper { flex: 1; }
}
.hero-title { font-size: 3.2rem; font-weight: 700; line-height: 1.4; margin-bottom: 16px; }
.hero-desc { font-size: 1.6rem; margin-bottom: 24px; }
.hero-image { width: 100%; height: auto; }
```

---

### フォーマットB: フル背景画像（テキスト中央配置）

画面いっぱいの背景画像の上に、中央揃えでテキストとボタンを配置するインパクト重視のレイアウト。

**【重要】** `background-image` のインラインスタイル（`style="background-image: url(...)"`) は**絶対禁止**。
背景画像は必ず `<img>` タグで実装し、テキストは `position: absolute` で重ねること（`rules-core.md` §8参照）。

**HTML構造:**
```html
<section id="hero" class="section-hero">
  <div class="hero-full-bg-wrapper">
    <!-- 背景画像（<img>タグで実装・background-image禁止） -->
    <img
      src="images/hero-bg.webp"
      alt="ヒーローセクション背景"
      width="1440"
      height="800"
      class="hero-full-bg-img"
    >
    <!-- テキスト・CTAを絶対配置で重ねる -->
    <div class="hero-full-bg-overlay" aria-hidden="true"></div>
    <div class="hero-full-bg-content inner-container">
      <h1 class="hero-title">キャッチコピーがここに入ります</h1>
      <a href="#form" class="btn btn-primary">今すぐ申し込む</a>
    </div>
  </div>
</section>
```

**CSS要件:**
- `.hero-full-bg-wrapper` を `position: relative` にし、子要素を絶対配置で重ねる
- オーバーレイは `::before` 疑似要素ではなく、独立した `.hero-full-bg-overlay` 要素で実装する

```css
.section-hero { width: 100%; }
.hero-full-bg-wrapper {
  position: relative;
  width: 100%;
  overflow: hidden;
}
.hero-full-bg-img {
  display: block;
  width: 100%;
  height: auto;
  min-height: 400px;
  object-fit: cover;
}
/* 暗いオーバーレイ（テキスト可読性確保） */
.hero-full-bg-overlay {
  position: absolute;
  inset: 0;
  background-color: rgba(0, 0, 0, 0.4);
}
/* テキスト・CTAコンテンツ */
.hero-full-bg-content {
  position: absolute;
  inset: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  gap: 24px;
  padding: 40px 20px;
}
.hero-full-bg-content .hero-title {
  font-size: 3.2rem;
  font-weight: 700;
  color: #ffffff;
  line-height: 1.4;
  text-shadow: 0 2px 8px rgba(0, 0, 0, 0.5);
}
```

---

### フォーマットC: 絶対配置ベース（リッチLP・画像重ね合わせ型）

ベースとなる背景画像の上に、複数の要素（人物、商品、バッジ等）を `position: absolute;` で重ねて配置するレイアウト。

**HTML構造:**
```html
<section id="hero" class="section-hero">
  <div class="inner-container">
    <div class="relative-wrapper">
      <!-- ベース背景 -->
      <img src="images/hero-base.webp" alt="背景" width="1000" height="600" class="base-bg">
      <!-- 重ねる要素（必要に応じて animation-float を付与） -->
      <img src="images/hero-item-1.webp" alt="商品" width="300" height="400" class="absolute-item item-product">
      <img src="images/hero-badge.webp" alt="No.1バッジ" width="150" height="150" class="absolute-item item-badge animation-float">
    </div>
  </div>
</section>
```

**CSS要件:**
- 親要素 `.relative-wrapper` に `position: relative;` を設定
- 子要素 `.absolute-item` は `position: absolute;` と `top/left/transform` で位置を調整

```css
.relative-wrapper { position: relative; width: 100%; max-width: var(--max-width); margin: 0 auto; }
.base-bg { display: block; width: 100%; height: auto; }
.absolute-item { position: absolute; height: auto; }
/* 位置は案件に合わせて個別に指定する */
.item-product { top: 10%; right: 5%; width: 30%; }
.item-badge { top: 5%; left: 5%; width: 15%; }
```

---

## 2. コンテンツセクション（特徴・理由・選ばれる理由など）のフォーマット

### フォーマットA: グリッドカード型（3列/4列並び）

アイコン＋タイトル＋テキストのカードを均等に並べるレイアウト。

**HTML構造:**
```html
<section id="features" class="section-features">
  <div class="inner-container">
    <h2 class="section-title">選ばれる3つの理由</h2>
    <div class="features-grid">
      <div class="feature-card fade-in">
        <img src="images/icon-1.webp" alt="理由1" width="100" height="100" loading="lazy" class="feature-icon">
        <h3 class="feature-card-title">理由その1</h3>
        <p class="feature-card-desc">説明文がここに入ります。</p>
      </div>
      <!-- カード2, 3... -->
    </div>
  </div>
</section>
```

**CSS要件:**
- PC版は CSS Grid (`grid-template-columns: repeat(3, 1fr);`) を使用
- SP版は1カラム (`grid-template-columns: 1fr;`)

```css
.section-features { padding: 60px 0; }
.features-grid { display: grid; grid-template-columns: 1fr; gap: 24px; margin-top: 40px; }
@media screen and (min-width: 768px) {
  .features-grid { grid-template-columns: repeat(3, 1fr); }
}
.feature-card { text-align: center; padding: 24px; background-color: #fff; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.06); }
.feature-icon { width: 80px; height: auto; margin: 0 auto 16px; }
.feature-card-title { font-size: 1.8rem; font-weight: 700; margin-bottom: 12px; }
.feature-card-desc { font-size: 1.4rem; color: #555; line-height: 1.8; }
```

---

### フォーマットB: 左右交互配置型（ジグザグレイアウト）

画像とテキストのブロックを、行ごとに左右交互に配置するレイアウト。

**HTML構造:**
```html
<section id="details" class="section-details">
  <div class="inner-container">
    <!-- ブロック1（画像左・テキスト右） -->
    <div class="detail-row fade-in">
      <div class="detail-image-wrapper">
        <img src="images/detail-1.webp" alt="詳細1" width="500" height="400" loading="lazy" class="detail-image">
      </div>
      <div class="detail-content">
        <h3 class="detail-title">見出し1</h3>
        <p class="detail-desc">説明文1</p>
      </div>
    </div>
    <!-- ブロック2（テキスト左・画像右） -->
    <div class="detail-row detail-row-reverse fade-in">
      <div class="detail-content">
        <h3 class="detail-title">見出し2</h3>
        <p class="detail-desc">説明文2</p>
      </div>
      <div class="detail-image-wrapper">
        <img src="images/detail-2.webp" alt="詳細2" width="500" height="400" loading="lazy" class="detail-image">
      </div>
    </div>
  </div>
</section>
```

**CSS要件:**
- `.detail-row` は Flexbox で横並びにする
- `.detail-row-reverse` は `flex-direction: row-reverse;` を指定して左右を入れ替える
- SP版ではすべて「画像が上、テキストが下」になるよう `flex-direction: column;` に統一する

```css
.section-details { padding: 60px 0; }
.detail-row {
  display: flex;
  flex-direction: column;
  gap: 32px;
  align-items: center;
  margin-bottom: 60px;
}
@media screen and (min-width: 768px) {
  .detail-row { flex-direction: row; }
  .detail-row-reverse { flex-direction: row-reverse; }
  .detail-image-wrapper, .detail-content { flex: 1; }
}
.detail-image { width: 100%; height: auto; border-radius: 8px; }
.detail-title { font-size: 2.4rem; font-weight: 700; margin-bottom: 16px; }
.detail-desc { font-size: 1.5rem; line-height: 1.9; color: #555; }
```

---

### フォーマットC: ステップ・フロー型

手順や流れを、矢印や数字アイコンとともに縦または横に並べるレイアウト。

**HTML構造:**
```html
<section id="flow" class="section-flow">
  <div class="inner-container">
    <h2 class="section-title">ご利用の流れ</h2>
    <div class="flow-list">
      <div class="flow-step fade-in">
        <div class="flow-step-number">STEP 01</div>
        <div class="flow-step-body">
          <h3 class="flow-step-title">お申し込み</h3>
          <p class="flow-step-desc">フォームからお申し込みください。</p>
        </div>
      </div>
      <!-- STEP 02, 03... -->
    </div>
  </div>
</section>
```

**CSS要件:**
- ステップ間の矢印は、CSSの `::after` 疑似要素（三角形）を使用して実装する
- タイムライン型にする場合は、親要素に `position: relative;` を設定し、絶対配置で直線を引く

```css
.section-flow { padding: 60px 0; }
.flow-list { display: flex; flex-direction: column; gap: 0; margin-top: 40px; }
.flow-step {
  display: flex;
  align-items: flex-start;
  gap: 20px;
  padding: 24px 0;
  position: relative;
}
/* ステップ間の矢印（下向き三角） */
.flow-step:not(:last-child)::after {
  content: '';
  position: absolute;
  bottom: -12px;
  left: 40px;
  transform: translateX(-50%);
  width: 0;
  height: 0;
  border-left: 12px solid transparent;
  border-right: 12px solid transparent;
  border-top: 12px solid var(--color-accent);
  z-index: 1;
}
.flow-step-number {
  flex-shrink: 0;
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--color-accent);
  color: #fff;
  font-size: 1.3rem;
  font-weight: 700;
  border-radius: 50%;
}
.flow-step-title { font-size: 1.8rem; font-weight: 700; margin-bottom: 8px; }
.flow-step-desc { font-size: 1.4rem; color: #555; line-height: 1.8; }
```
