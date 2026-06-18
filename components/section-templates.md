# セクション・テンプレートモジュール（section-templates.md）

> **【AIへの指示】**
> 本ファイルは、LPの各セクションを構築する際のベースとなるレイアウトフォーマット（A/B/C案）を定義したものである。
> STEP 3「3案提示」の際、AIは制作対象のセクション内容に応じて、以下のフォーマットから適切なものを選択して人間に提案すること。

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
- PC版（`md`以上）では CSS Grid または Flexbox で2カラムにする。
- SP版では画像を上、テキストを下（またはその逆）の1カラムにする。

### フォーマットB: フル背景画像（テキスト中央配置）
画面いっぱいの背景画像の上に、中央揃えでテキストとボタンを配置するインパクト重視のレイアウト。

**HTML構造:**
```html
<section id="hero" class="section-hero hero-full-bg" style="background-image: url('images/hero-bg.webp');">
  <div class="inner-container hero-center-content">
    <h1 class="hero-title">キャッチコピーがここに入ります</h1>
    <a href="#form" class="btn btn-primary">今すぐ申し込む</a>
  </div>
</section>
```

**CSS要件:**
- `background-size: cover; background-position: center;` を使用。
- テキストが読めるよう、背景に暗いオーバーレイ（`::before` 疑似要素と `rgba`）を重ねる。

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
- 親要素 `.relative-wrapper` に `position: relative;` を設定。
- 子要素 `.absolute-item` は `position: absolute;` と `top/left/transform` で位置を調整。

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
        <img src="images/icon-1.webp" alt="理由1" width="100" height="100" class="feature-icon">
        <h3 class="feature-card-title">理由その1</h3>
        <p class="feature-card-desc">説明文がここに入ります。</p>
      </div>
      <!-- カード2, 3... -->
    </div>
  </div>
</section>
```

**CSS要件:**
- PC版は CSS Grid (`grid-template-columns: repeat(3, 1fr);`) を使用。
- SP版は1カラム (`grid-template-columns: 1fr;`)。

### フォーマットB: 左右交互配置型（ジグザグレイアウト）
画像とテキストのブロックを、行ごとに左右交互に配置するレイアウト。

**HTML構造:**
```html
<section id="details" class="section-details">
  <div class="inner-container">
    
    <!-- ブロック1（画像左・テキスト右） -->
    <div class="detail-row fade-in">
      <div class="detail-image-wrapper">
        <img src="images/detail-1.webp" alt="詳細1" width="500" height="400">
      </div>
      <div class="detail-content">
        <h3>見出し1</h3>
        <p>説明文1</p>
      </div>
    </div>

    <!-- ブロック2（テキスト左・画像右） -->
    <div class="detail-row detail-row-reverse fade-in">
      <div class="detail-content">
        <h3>見出し2</h3>
        <p>説明文2</p>
      </div>
      <div class="detail-image-wrapper">
        <img src="images/detail-2.webp" alt="詳細2" width="500" height="400">
      </div>
    </div>

  </div>
</section>
```

**CSS要件:**
- `.detail-row` は Flexbox で横並びにする。
- `.detail-row-reverse` は `flex-direction: row-reverse;` を指定して左右を入れ替える。
- SP版ではすべて「画像が上、テキストが下」になるよう `flex-direction: column;` に統一する。

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
          <h3>お申し込み</h3>
          <p>フォームからお申し込みください。</p>
        </div>
      </div>
      <!-- STEP 02, 03... -->

    </div>
  </div>
</section>
```

**CSS要件:**
- ステップ間の矢印は、CSSの `::after` 疑似要素（三角形やアイコンフォント）を使用して実装する。
- 要素同士が線で繋がっているようなデザイン（タイムライン型）にする場合は、親要素に `position: relative;` を設定し、絶対配置で直線を引く。
