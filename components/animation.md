# アニメーション実装モジュール（animation.md）

> **【AIへの指示】**
> 本ファイルは、LPにおける「アニメーション・動的演出」を実装する際のルールとテンプレートである。
> セクション内に以下の演出が含まれる場合、必ずこのテンプレートを使用して実装すること。

---

## 1. スクロールフェードイン（IntersectionObserver）

スクロールに連動したフェードインアニメーションは、`IntersectionObserver` を使用して実装する。`scroll` イベントへの直接バインドはパフォーマンス低下を招くため禁止する。

### 1.1 HTML実装
フェードインさせたい要素に `.fade-in` クラスを付与する。

```html
<div class="card-item fade-in">
  <!-- コンテンツ -->
</div>
```

### 1.2 CSS実装
初期状態（透明・下にズレた状態）と、表示状態（`.is-visible`）を定義する。

```css
/* ==========================================================================
   Animation: Fade In
   ========================================================================== */

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

### 1.3 JavaScript実装
`main.js` に以下のコードを記述する。一度発火したら監視を解除し、何度もアニメーションが実行されるのを防ぐ。

```javascript
// スクロールフェードインアニメーション
const fadeInElements = document.querySelectorAll('.fade-in');

if (fadeInElements.length > 0) {
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
}
```

---

## 2. ふわふわループ可動（Float Animation）

背景画像の上などに配置した要素を「上下にふわふわとループ可動させる」演出の実装テンプレート。

### 2.1 CSS実装
キーフレームを定義し、汎用クラス `.animation-float` を作成する。

```css
/* ==========================================================================
   Animation: Float (上下ふわふわループ)
   ========================================================================== */

@keyframes float-up-down {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-15px); /* 浮き上がる距離 */
  }
  100% {
    transform: translateY(0);
  }
}

.animation-float {
  animation: float-up-down 3s ease-in-out infinite;
}
```

### 2.2 HTML実装
絶対配置（`position: absolute;`）と組み合わせて使用することが多い。

```html
<div class="relative-wrapper">
  <img src="images/bg.webp" alt="背景" width="1000" height="600" class="base-bg">
  <!-- animation-floatクラスを追加するだけで適用される -->
  <img
    src="images/item.webp"
    alt="浮遊するアイコン"
    width="200"
    height="200"
    class="absolute-item animation-float"
  >
</div>
```

---

## 3. アニメーション実装の禁止事項

1. **jQueryの `animate()` や `fadeIn()` メソッドの使用禁止**
   - 外部ライブラリへの依存を防ぐため、必ずCSS Transition/Animation または Vanilla JS を使用すること。
2. **`scroll` イベントリスナーの直接バインド禁止**
   - `window.addEventListener('scroll', ...)` は使用せず、必ず `IntersectionObserver` を使用すること。
3. **カクつきの原因となるプロパティのアニメーション禁止**
   - `margin`, `padding`, `width`, `height`, `top`, `left` などのレイアウトプロパティをアニメーションさせないこと。
   - アニメーションには必ず `transform`（`translate`, `scale`, `rotate`）と `opacity` を使用すること。
