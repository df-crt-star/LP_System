# animation.md — アニメーション実装モジュール

> **【AIへの指示】**
> 本ファイルは STEP 0 で選択されたアニメーションのみ参照すること。
> 各アニメーションは「① 概要 / ② 確認事項 / ③ テンプレート」の3ブロックで構成されている。
> ③ テンプレートの `【 】` 内の値を会話で確認してから埋めること。

---

## 設計思想

- **Layer 1**：定義済みアニメーション8種（A1〜A8）。型はテンプレートで固定、値は会話で確認して埋める。
- **Layer 2**：カスタムアニメーション。参考URL・言葉で指定 → AIが解析・言語化 → 承認後に生成 → Layer 1への登録提案。

### JS統合ルール（重要）

A1・A6・A7・A8 はすべて `IntersectionObserver` で `.is-visible` クラスを付与する方式。
**これら4種のJSは `main.js` 内で1本に統合すること。重複記述は禁止。**

```javascript
// ===== スクロール連動アニメーション（A1/A6/A7/A8 共通）=====
// .fade-in / .slide-in-left / .slide-in-right / .zoom-in を一括監視
const scrollAnimElements = document.querySelectorAll(
  '.fade-in, .slide-in-left, .slide-in-right, .zoom-in'
);
if (scrollAnimElements.length > 0) {
  const scrollObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        entry.target.classList.add('is-visible');
        scrollObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.1 });
  scrollAnimElements.forEach((el) => scrollObserver.observe(el));
}
```

---

## Layer 1：定義済みアニメーション8種

---

### A1：フェードイン（スクロール連動）

#### ① 概要
スクロールで要素が画面内に入ったとき、透明な状態から下からフワッと現れるアニメーション。最も汎用的な演出。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 移動距離 | 下から何px上に移動するか | `20px` |
| 速さ | アニメーション時間 | `0.6s` |
| イージング | 動きのカーブ | `ease` |
| 発火タイミング | 要素の何%が見えたら発火するか | `0.1`（10%）|

#### ③ テンプレート

```css
/* A1: フェードイン */
.fade-in {
  opacity: 0;
  transform: translateY(【移動距離: 20px】);
  transition: opacity 【速さ: 0.6s】 【イージング: ease】,
              transform 【速さ: 0.6s】 【イージング: ease】;
}
.fade-in.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

```html
<!-- 使用例: フェードインさせたい要素に .fade-in を付与 -->
<div class="card-item fade-in">コンテンツ</div>
```

---

### A2：ふわふわ浮遊（Float）

#### ① 概要
要素が上下にゆっくりループし続けるアニメーション。商品画像・バッジ・アイコン等に使用する。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 浮き上がり距離 | 上方向への移動量 | `15px` |
| 速さ | 1サイクルの時間 | `3s` |
| イージング | 動きのカーブ | `ease-in-out` |

#### ③ テンプレート

```css
/* A2: ふわふわ浮遊 */
@keyframes float-up-down {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-【浮き上がり距離: 15px】); }
  100% { transform: translateY(0); }
}
.animation-float {
  animation: float-up-down 【速さ: 3s】 【イージング: ease-in-out】 infinite;
}
```

```html
<!-- 使用例: 絶対配置要素と組み合わせる -->
<img src="images/badge.webp" alt="バッジ" width="150" height="150"
     class="absolute-item animation-float">
```

---

### A3：揺れ（左右シェイク）

#### ① 概要
要素が左右に小刻みに揺れるアニメーション。注目を集めたいボタン・アイコンに使用する。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 揺れ幅 | 左右への移動量 | `6px` |
| 速さ | 1サイクルの時間 | `0.5s` |
| 繰り返し | ループ or 一定回数 | `infinite` |

#### ③ テンプレート

```css
/* A3: 揺れ（左右シェイク） */
@keyframes shake-lr {
  0%, 100% { transform: translateX(0); }
  20%       { transform: translateX(-【揺れ幅: 6px】); }
  40%       { transform: translateX(【揺れ幅: 6px】); }
  60%       { transform: translateX(-【揺れ幅: 6px】); }
  80%       { transform: translateX(【揺れ幅: 6px】); }
}
.animation-shake {
  animation: shake-lr 【速さ: 0.5s】 ease-in-out 【繰り返し: infinite】;
}
```

```html
<!-- 使用例 -->
<img src="images/icon-arrow.webp" alt="矢印" width="40" height="40" class="animation-shake">
```

---

### A4：脈打ち（拡縮パルス）

#### ① 概要
要素が心拍のように拡大・縮小を繰り返すアニメーション。CTAボタン・価格表示・バッジ等に使用する。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 拡大率 | 最大サイズ（1.0 = 等倍） | `1.08` |
| 速さ | 1サイクルの時間 | `1.2s` |
| イージング | 動きのカーブ | `ease-in-out` |

#### ③ テンプレート

```css
/* A4: 脈打ち（拡縮パルス） */
@keyframes pulse-scale {
  0%, 100% { transform: scale(1); }
  50%       { transform: scale(【拡大率: 1.08】); }
}
.animation-pulse {
  animation: pulse-scale 【速さ: 1.2s】 【イージング: ease-in-out】 infinite;
}
```

```html
<!-- 使用例 -->
<a href="#form" class="btn btn-primary animation-pulse">今すぐ申し込む</a>
```

---

### A5：カウントアップ

#### ① 概要
数値が0から目標値まで増加するアニメーション。実績数・満足度・販売数等の数字表示に使用する。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 目標値 | カウントアップする最終数値 | `【要確認】` |
| 速さ | カウントアップにかける時間（ms） | `2000`（2秒）|
| 単位 | 数値の後に付ける文字 | `「件」「%」「万」等` |
| 発火タイミング | スクロールで画面内に入ったとき | IntersectionObserver使用 |

#### ③ テンプレート

```html
<!-- data属性に目標値と単位を設定 -->
<span class="count-up" data-target="【目標値】" data-suffix="【単位】">0</span>
```

```javascript
// A5: カウントアップ
function countUp(el) {
  const target = parseInt(el.dataset.target, 10);
  const suffix = el.dataset.suffix || '';
  const duration = 【速さ: 2000】; // ms
  const startTime = performance.now();

  function update(currentTime) {
    const elapsed = currentTime - startTime;
    const progress = Math.min(elapsed / duration, 1);
    // イージング（ease-out）
    const eased = 1 - Math.pow(1 - progress, 3);
    el.textContent = Math.floor(eased * target) + suffix;
    if (progress < 1) requestAnimationFrame(update);
  }
  requestAnimationFrame(update);
}

const countUpElements = document.querySelectorAll('.count-up');
if (countUpElements.length > 0) {
  const countObserver = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
      if (entry.isIntersecting) {
        countUp(entry.target);
        countObserver.unobserve(entry.target);
      }
    });
  }, { threshold: 0.5 });
  countUpElements.forEach((el) => countObserver.observe(el));
}
```

---

### A6：スライドイン（左から）

#### ① 概要
スクロールで要素が画面内に入ったとき、左から右へスライドしながら現れるアニメーション。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 移動距離 | 左から何px移動するか | `40px` |
| 速さ | アニメーション時間 | `0.6s` |
| イージング | 動きのカーブ | `ease` |

#### ③ テンプレート

```css
/* A6: スライドイン（左から） */
.slide-in-left {
  opacity: 0;
  transform: translateX(-【移動距離: 40px】);
  transition: opacity 【速さ: 0.6s】 【イージング: ease】,
              transform 【速さ: 0.6s】 【イージング: ease】;
}
.slide-in-left.is-visible {
  opacity: 1;
  transform: translateX(0);
}
```

```html
<!-- 使用例 -->
<div class="detail-content slide-in-left">テキストコンテンツ</div>
```

---

### A7：スライドイン（右から）

#### ① 概要
スクロールで要素が画面内に入ったとき、右から左へスライドしながら現れるアニメーション。A6と組み合わせてジグザグレイアウトに使用する。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 移動距離 | 右から何px移動するか | `40px` |
| 速さ | アニメーション時間 | `0.6s` |
| イージング | 動きのカーブ | `ease` |

#### ③ テンプレート

```css
/* A7: スライドイン（右から） */
.slide-in-right {
  opacity: 0;
  transform: translateX(【移動距離: 40px】);
  transition: opacity 【速さ: 0.6s】 【イージング: ease】,
              transform 【速さ: 0.6s】 【イージング: ease】;
}
.slide-in-right.is-visible {
  opacity: 1;
  transform: translateX(0);
}
```

```html
<!-- 使用例: A6と組み合わせてジグザグ演出 -->
<div class="detail-image-wrapper slide-in-right">
  <img src="images/detail.webp" alt="詳細画像" width="500" height="400" loading="lazy">
</div>
```

---

### A8：ズームイン

#### ① 概要
スクロールで要素が画面内に入ったとき、小さい状態から等倍にズームしながら現れるアニメーション。

#### ② 確認事項

| パラメータ | 確認内容 | デフォルト値 |
|-----------|---------|------------|
| 初期スケール | 開始時のサイズ（1.0 = 等倍） | `0.85` |
| 速さ | アニメーション時間 | `0.6s` |
| イージング | 動きのカーブ | `ease` |

#### ③ テンプレート

```css
/* A8: ズームイン */
.zoom-in {
  opacity: 0;
  transform: scale(【初期スケール: 0.85】);
  transition: opacity 【速さ: 0.6s】 【イージング: ease】,
              transform 【速さ: 0.6s】 【イージング: ease】;
}
.zoom-in.is-visible {
  opacity: 1;
  transform: scale(1);
}
```

```html
<!-- 使用例 -->
<div class="feature-card zoom-in">カードコンテンツ</div>
```

---

## Layer 2：カスタムアニメーション

### フロー

定義済み8種（A1〜A8）に該当しない動きを実装したい場合に使用する。

```
STEP 1：言葉 or 参考URLで動きを伝える
         例）「右上から斜めに落ちてくる感じ」
             「このURLのXXXと同じ動き → https://...」

STEP 2：AIが動きを言語化して確認する
         例）「translateX(30px) + translateY(-30px) から
              translateX(0) + translateY(0) へ 0.8s で移動する動き
              でよろしいでしょうか？」

STEP 3：承認 → コード生成

STEP 4：Layer 1への登録提案
         クラス名候補を .animation-[英単語] 形式でAIが提案する
         例）.animation-drop-in-diagonal
         → 承認されたら本ファイルのLayer 1に追記する
```

### カスタムアニメーション テンプレート

```css
/* Layer 2 カスタム: 【アニメーション名】 */
@keyframes 【keyframe名: animation-custom-xxx】 {
  0%   { transform: 【開始状態】; opacity: 【開始opacity】; }
  100% { transform: 【終了状態】; opacity: 【終了opacity】; }
}
.animation-【英単語】 {
  animation: 【keyframe名】 【速さ】 【イージング】 【繰り返し: 1 or infinite】;
}
```

---

## GIF実装パターン（画像重ね層型）

### 概要
GIFアニメーション画像を**背景画像に重ねて配置**するパターン。画像に焼き込みできない動きのある要素（矢印・バッジ・アイコン等）をGIFで実装する際に使用する（減肥茨型）。

### 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| GIFファイル名 | GIF画像のパス | `images/animation-01.gif` |
| 配置位置 | 背景画像に対する相対位置 | カンプ参照（`top`, `left` 等）|
| サイズ | GIFの表示サイズ | `width: 80px` 等 |
| 重ねる数 | 何枚のGIFを重ねるか | 1枚〜複数 |

### テンプレート

```html
<!-- GIF：背景画像に重ねる場合（減肥茨型）-->
<div class="gif-wrapper">
  <img src="images/section-bg.webp" alt="">
  <!-- GIF 1枚目 -->
  <div class="gif-overlay gif-overlay-1">
    <img src="images/animation-01.gif" alt="">
  </div>
  <!-- GIF 2枚目（複数ある場合）-->
  <div class="gif-overlay gif-overlay-2">
    <img src="images/animation-02.gif" alt="">
  </div>
</div>
```

```css
/* GIF重ね層 */
.gif-wrapper {
  position: relative; /* 親要素に position:relative が必須 */
}
.gif-wrapper > img {
  width: 100%;
  display: block;
}
.gif-overlay {
  position: absolute;
  /* 座標はカンプを参照して設定する */
}
.gif-overlay img {
  width: 100%; /* 幅は親要素の幅に対する割合で指定 */
  display: block;
}

/* 例：カンプに合わせた座標設定 */
.gif-overlay-1 {
  top: 10%;
  left: 5%;
  width: 20%;
}
.gif-overlay-2 {
  top: 30%;
  right: 5%;
  width: 15%;
}
```

> **注意：** GIFの配置座標（`top`/`left`/`right`/`bottom`/`width`）は必ずカンプ（`design/`フォルダの画像）を参照して設定すること。

---

## 禁止事項

| 禁止事項 | 理由 |
|---------|------|
| jQuery の `animate()` / `fadeIn()` 等の使用 | 外部ライブラリ依存禁止 |
| `scroll` イベントへの直接バインド | パフォーマンス低下（`IntersectionObserver` を使用すること）|
| `margin` / `padding` / `width` / `height` のアニメーション | レイアウトシフトによるカクつきの原因 |
| A1/A6/A7/A8 のJS重複記述 | `main.js` 内で1本に統合すること |
