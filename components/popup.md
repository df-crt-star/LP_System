# popup.md — カラー/サイズ選択ポップアップ コンポーネント

> このファイルは `components/popup.md` として保存し、ポップアップを含むセクション制作直前に読み込む。
> **前提：** jQuery と slick.min.js が読み込まれていること（ポップアップ内スライダーを使用する場合）。

---

## P1：カラー/サイズ選択ポップアップ（Slickスライダー内蔵型）

### ① 概要
商品のカラー・サイズ一覧から1つをクリックすると、そのカラー/サイズの詳細画像をポップアップ内のスライダーで表示するコンポーネント。`data-images` 属性に画像パスの配列を渡すことで、トリガーごとに異なる画像セットを表示できる（VIAGE型）。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| トリガー画像 | カラー/サイズ一覧の画像ファイル名 | `images/color-bluegray.webp` |
| ポップアップ内の画像 | 各トリガーに対応する画像（複数可） | `["images/popup-01.webp","images/popup-02.webp"]` |
| ポップアップ内スライダー | あり or なし | あり（Slick使用）|
| 閉じるボタン | ×ボタン or 背景クリック or 両方 | 両方 |
| ポップアップの幅 | 最大幅 | `90vw` / `max-width: 600px` |

### ③ テンプレート

#### HTML

```html
<!-- カラー一覧（トリガー） -->
<div class="item-list">
  <img src="images/color-bluegray.webp" alt="ブルーグレー" class="js-color-popup-open"
       data-images='["images/popup_bluegray01.webp","images/popup_bluegray02.webp","images/popup_bluegray03.webp"]'>
  <img src="images/color-purple.webp" alt="パープル" class="js-color-popup-open"
       data-images='["images/popup_purple01.webp","images/popup_purple02.webp","images/popup_purple03.webp"]'>
  <!-- 繰り返す -->
</div>

<!-- ポップアップ本体（</body>直前に配置） -->
<div class="color-popup" id="color-popup" aria-hidden="true" role="dialog" aria-modal="true">
  <div class="color-popup__overlay js-popup-overlay"></div>
  <div class="color-popup__inner">
    <!-- ×ボタン -->
    <button class="color-popup__close js-popup-close" aria-label="閉じる">&times;</button>
    <!-- Slickスライダー（JSで動的に画像を挿入） -->
    <div class="color-popup__slider js-popup-slider"></div>
  </div>
</div>
```

#### CSS

```css
/* Popup */
.color-popup {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 2000;
  align-items: center;
  justify-content: center;
}
.color-popup.is-open {
  display: flex;
}
.color-popup__overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  cursor: pointer;
}
.color-popup__inner {
  position: relative;
  z-index: 1;
  background-color: #ffffff;
  border-radius: 8px;
  padding: 40px 20px 20px;
  width: 90vw;
  max-width: 600px;
  max-height: 90vh;
  overflow-y: auto;
}
.color-popup__close {
  position: absolute;
  top: 10px;
  right: 14px;
  font-size: 2.4rem;
  line-height: 1;
  background: none;
  border: none;
  cursor: pointer;
  color: #333;
  padding: 4px 8px;
}
.color-popup__close:hover { color: var(--color-accent); }

/* ポップアップ内スライダー */
.color-popup__slider .slick-slide img {
  width: 100%;
  display: block;
}
.color-popup__slider .slick-dots li button:before {
  font-size: 10px;
  color: #ccc;
  opacity: 1;
}
.color-popup__slider .slick-dots li.slick-active button:before {
  color: var(--color-accent);
  opacity: 1;
}
```

#### JavaScript（jQuery版）

```javascript
// Popup — jQuery版
$(function() {
  const popup    = $("#color-popup");
  const slider   = $(".js-popup-slider");

  // トリガークリック：ポップアップを開く
  $(document).on("click", ".js-color-popup-open", function() {
    const images = JSON.parse($(this).attr("data-images"));

    // スライダーをリセット
    if (slider.hasClass("slick-initialized")) {
      slider.slick("unslick");
    }
    slider.empty();

    // 画像を挿入
    images.forEach(function(src) {
      slider.append('<div><img src="' + src + '" alt=""></div>');
    });

    // Slick初期化
    slider.slick({
      dots: true,
      arrows: true,
      infinite: true,
      speed: 300,
      slidesToShow: 1,
      slidesToScroll: 1
    });

    // ポップアップを開く
    popup.addClass("is-open");
    popup.attr("aria-hidden", "false");
    $("body").css("overflow", "hidden"); // 背景スクロール禁止
  });

  // ×ボタン or オーバーレイクリックで閉じる
  $(document).on("click", ".js-popup-close, .js-popup-overlay", function() {
    popup.removeClass("is-open");
    popup.attr("aria-hidden", "true");
    $("body").css("overflow", "");
  });

  // ESCキーで閉じる
  $(document).on("keydown", function(e) {
    if (e.key === "Escape") {
      popup.removeClass("is-open");
      popup.attr("aria-hidden", "true");
      $("body").css("overflow", "");
    }
  });
});
```

---

## P2：シンプルポップアップ（スライダーなし）

### ① 概要
画像1枚をポップアップで拡大表示するシンプルなライトボックス型。スライダーが不要な場合に使用する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| トリガー | クリックする要素 | `<img>` or `<a>` |
| 表示する画像 | `data-popup-img` 属性で指定 | `images/popup-large.webp` |
| 閉じるボタン | ×ボタン + 背景クリック | あり |

### ③ テンプレート

```html
<!-- トリガー -->
<img src="images/thumb.webp" alt="クリックで拡大" class="js-simple-popup-open"
     data-popup-img="images/popup-large.webp">

<!-- ポップアップ本体（</body>直前に配置） -->
<div class="simple-popup" id="simple-popup" aria-hidden="true">
  <div class="simple-popup__overlay js-simple-popup-close"></div>
  <div class="simple-popup__inner">
    <button class="simple-popup__close js-simple-popup-close" aria-label="閉じる">&times;</button>
    <img src="" alt="" class="simple-popup__img" id="simple-popup-img">
  </div>
</div>
```

```css
/* Simple Popup */
.simple-popup {
  display: none;
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  z-index: 2000;
  align-items: center;
  justify-content: center;
}
.simple-popup.is-open { display: flex; }
.simple-popup__overlay {
  position: absolute;
  top: 0; left: 0;
  width: 100%; height: 100%;
  background-color: rgba(0, 0, 0, 0.8);
  cursor: pointer;
}
.simple-popup__inner {
  position: relative;
  z-index: 1;
  max-width: 90vw;
  max-height: 90vh;
}
.simple-popup__img {
  max-width: 100%;
  max-height: 90vh;
  display: block;
  border-radius: 4px;
}
.simple-popup__close {
  position: absolute;
  top: -36px;
  right: 0;
  font-size: 2.4rem;
  background: none;
  border: none;
  color: #fff;
  cursor: pointer;
}
```

```javascript
// Simple Popup — Vanilla JS版
document.querySelectorAll('.js-simple-popup-open').forEach((trigger) => {
  trigger.addEventListener('click', () => {
    const imgSrc = trigger.getAttribute('data-popup-img');
    const popup = document.getElementById('simple-popup');
    const popupImg = document.getElementById('simple-popup-img');
    popupImg.src = imgSrc;
    popup.classList.add('is-open');
    popup.setAttribute('aria-hidden', 'false');
    document.body.style.overflow = 'hidden';
  });
});

document.querySelectorAll('.js-simple-popup-close').forEach((btn) => {
  btn.addEventListener('click', () => {
    const popup = document.getElementById('simple-popup');
    popup.classList.remove('is-open');
    popup.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
  });
});

document.addEventListener('keydown', (e) => {
  if (e.key === 'Escape') {
    const popup = document.getElementById('simple-popup');
    popup.classList.remove('is-open');
    popup.setAttribute('aria-hidden', 'true');
    document.body.style.overflow = '';
  }
});
```

---

## 注意事項

- P1（カラー選択型）は**jQuery依存**。jQueryが不要な案件ではP2（Vanilla JS版）を使用すること
- ポップアップ表示中は `body` の `overflow: hidden` で背景スクロールを禁止すること
- ポップアップ本体は `</body>` 直前に配置すること（`z-index` の競合を避けるため）
- `aria-hidden` と `role="dialog"` を適切に設定してアクセシビリティに対応すること
