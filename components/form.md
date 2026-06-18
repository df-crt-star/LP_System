# form.md — フォーム・アコーディオンCTA コンポーネント

> このファイルは `components/form.md` として保存し、フォームを含むセクション制作直前に読み込む。

---

## F1：シンプル入力フォーム

### ① 概要
名前・電話番号・メールアドレス等の基本項目を縦並びで入力するシンプルなフォーム。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| 入力項目 | 何を入力させるか | 名前・電話番号・メール |
| 必須/任意 | 各項目の必須・任意区分 | 名前：必須、メール：任意 |
| ボタンテキスト | 送信ボタンのラベル | 「無料で申し込む」 |
| 送信先 | action属性のURL | `https://example.com/send` |
| プライバシーポリシー | リンクの有無・URL | あり / `https://example.com/privacy` |

### ③ テンプレート

```html
<section id="form" class="section-form">
  <div class="inner-container">
    <h2 class="section-title">【TITLE】</h2>
    <form class="form-block" action="【ACTION_URL】" method="post">
      <div class="form-row">
        <label class="form-label" for="name">お名前 <span class="form-required">必須</span></label>
        <input class="form-input" type="text" id="name" name="name" placeholder="山田 太郎" required>
      </div>
      <div class="form-row">
        <label class="form-label" for="tel">電話番号 <span class="form-required">必須</span></label>
        <input class="form-input" type="tel" id="tel" name="tel" placeholder="090-0000-0000" required>
      </div>
      <div class="form-row">
        <label class="form-label" for="email">メールアドレス <span class="form-optional">任意</span></label>
        <input class="form-input" type="email" id="email" name="email" placeholder="example@mail.com">
      </div>
      <p class="form-privacy">
        <a href="【PRIVACY_URL】" target="_blank" rel="noopener">プライバシーポリシー</a>に同意の上、送信してください。
      </p>
      <button class="btn btn-primary btn-form-submit" type="submit">【BUTTON_TEXT】</button>
    </form>
  </div>
</section>
```

```css
/* Form Block */
.section-form { padding: 60px 0; background-color: var(--color-bg); }
.form-block { max-width: 600px; margin: 40px auto 0; }
.form-row { display: flex; flex-direction: column; gap: 8px; margin-bottom: 24px; }
.form-label { font-size: 1.4rem; font-weight: 700; }
.form-required { display: inline-block; background-color: var(--color-accent); color: #fff; font-size: 1.1rem; padding: 2px 6px; border-radius: 3px; margin-left: 6px; }
.form-optional { display: inline-block; background-color: #999; color: #fff; font-size: 1.1rem; padding: 2px 6px; border-radius: 3px; margin-left: 6px; }
.form-input { width: 100%; padding: 12px 16px; border: 1px solid #ccc; border-radius: 4px; font-size: 1.6rem; transition: border-color 0.2s ease; }
.form-input:focus { outline: none; border-color: var(--color-accent); }
.form-privacy { font-size: 1.2rem; color: #666; text-align: center; margin-bottom: 24px; }
.form-privacy a { color: var(--color-accent); text-decoration: underline; }
.btn-form-submit { display: block; width: 100%; padding: 18px; font-size: 1.8rem; border: none; }
```

---

## F2：アコーディオンCTA（折りたたみ式フォーム）

### ① 概要
「今すぐ申し込む」ボタンをタップすると、その場でフォームが展開されるアコーディオン型CTA。スクロールなしで申し込みを完結させたい場合に使用する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| トリガーボタンテキスト | 折りたたみを開くボタンのラベル | 「今すぐ無料で申し込む」 |
| 展開後のフォーム内容 | F1と同様の入力項目確認 | 名前・電話番号 |
| 初期状態 | 開いた状態 or 閉じた状態 | 閉じた状態（デフォルト） |

### ③ テンプレート

```html
<section id="cta" class="section-cta">
  <div class="inner-container">
    <div class="accordion-cta">
      <!-- トリガーボタン -->
      <button class="accordion-cta-trigger btn btn-primary" type="button" aria-expanded="false" aria-controls="accordion-form">
        【TRIGGER_TEXT】
      </button>
      <!-- 展開されるフォームエリア -->
      <div class="accordion-cta-body" id="accordion-form" aria-hidden="true">
        <form class="form-block" action="【ACTION_URL】" method="post">
          <div class="form-row">
            <label class="form-label" for="acc-name">お名前 <span class="form-required">必須</span></label>
            <input class="form-input" type="text" id="acc-name" name="name" placeholder="山田 太郎" required>
          </div>
          <div class="form-row">
            <label class="form-label" for="acc-tel">電話番号 <span class="form-required">必須</span></label>
            <input class="form-input" type="tel" id="acc-tel" name="tel" placeholder="090-0000-0000" required>
          </div>
          <p class="form-privacy">
            <a href="【PRIVACY_URL】" target="_blank" rel="noopener">プライバシーポリシー</a>に同意の上、送信してください。
          </p>
          <button class="btn btn-primary btn-form-submit" type="submit">【SUBMIT_TEXT】</button>
        </form>
      </div>
    </div>
  </div>
</section>
```

```css
/* Accordion CTA */
.section-cta { padding: 60px 0; }
.accordion-cta { max-width: 600px; margin: 0 auto; text-align: center; }
.accordion-cta-trigger { width: 100%; font-size: 1.8rem; padding: 20px; }
.accordion-cta-body {
  overflow: hidden;
  max-height: 0;
  transition: max-height 0.4s ease, padding 0.4s ease;
  padding: 0 20px;
}
.accordion-cta-body.is-open {
  max-height: 600px; /* フォームの高さに合わせて調整 */
  padding: 24px 20px;
}
```

```javascript
// Accordion CTA
const accordionTrigger = document.querySelector('.accordion-cta-trigger');
const accordionBody = document.querySelector('.accordion-cta-body');

if (accordionTrigger && accordionBody) {
  accordionTrigger.addEventListener('click', () => {
    const isOpen = accordionBody.classList.toggle('is-open');
    accordionTrigger.setAttribute('aria-expanded', isOpen);
    accordionBody.setAttribute('aria-hidden', !isOpen);
  });
}
```

---

## F3：ステップフォーム（2〜3ステップ）

### ① 概要
入力項目を複数ステップに分割し、「次へ」ボタンで進む形式のフォーム。離脱率を下げたい場合に使用する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| ステップ数 | 何ステップに分けるか | 2ステップ |
| 各ステップの項目 | ステップごとの入力内容 | STEP1：名前・電話 / STEP2：住所・確認 |
| プログレスバー | 進捗表示の有無 | あり |

### ③ テンプレート

```html
<section id="form" class="section-form">
  <div class="inner-container">
    <h2 class="section-title">【TITLE】</h2>
    <!-- プログレスバー -->
    <div class="step-progress">
      <div class="step-progress-item is-active">STEP 1</div>
      <div class="step-progress-item">STEP 2</div>
    </div>
    <!-- STEP 1 -->
    <div class="step-panel is-active" id="step-1">
      <form class="form-block">
        <div class="form-row">
          <label class="form-label" for="s1-name">お名前 <span class="form-required">必須</span></label>
          <input class="form-input" type="text" id="s1-name" name="name" placeholder="山田 太郎" required>
        </div>
        <div class="form-row">
          <label class="form-label" for="s1-tel">電話番号 <span class="form-required">必須</span></label>
          <input class="form-input" type="tel" id="s1-tel" name="tel" placeholder="090-0000-0000" required>
        </div>
        <button class="btn btn-primary step-next-btn" type="button">次へ進む</button>
      </form>
    </div>
    <!-- STEP 2 -->
    <div class="step-panel" id="step-2">
      <form class="form-block" action="【ACTION_URL】" method="post">
        <div class="form-row">
          <label class="form-label" for="s2-email">メールアドレス <span class="form-required">必須</span></label>
          <input class="form-input" type="email" id="s2-email" name="email" placeholder="example@mail.com" required>
        </div>
        <p class="form-privacy">
          <a href="【PRIVACY_URL】" target="_blank" rel="noopener">プライバシーポリシー</a>に同意の上、送信してください。
        </p>
        <button class="btn btn-primary btn-form-submit" type="submit">【SUBMIT_TEXT】</button>
        <button class="step-back-btn" type="button">戻る</button>
      </form>
    </div>
  </div>
</section>
```

```css
/* Step Form */
.step-progress { display: flex; justify-content: center; gap: 16px; margin-bottom: 32px; }
.step-progress-item { padding: 8px 20px; border-radius: 20px; font-size: 1.3rem; font-weight: 700; background-color: #eee; color: #999; }
.step-progress-item.is-active { background-color: var(--color-accent); color: #fff; }
.step-panel { display: none; }
.step-panel.is-active { display: block; }
.step-next-btn, .step-back-btn { display: block; width: 100%; margin-top: 16px; }
.step-back-btn { background: none; border: none; color: #999; font-size: 1.4rem; cursor: pointer; text-decoration: underline; }
```

```javascript
// Step Form
const stepPanels = document.querySelectorAll('.step-panel');
const stepProgressItems = document.querySelectorAll('.step-progress-item');
const nextBtns = document.querySelectorAll('.step-next-btn');
const backBtns = document.querySelectorAll('.step-back-btn');

function goToStep(stepIndex) {
  stepPanels.forEach((panel, i) => panel.classList.toggle('is-active', i === stepIndex));
  stepProgressItems.forEach((item, i) => item.classList.toggle('is-active', i === stepIndex));
}

nextBtns.forEach((btn, i) => btn.addEventListener('click', () => goToStep(i + 1)));
backBtns.forEach((btn, i) => btn.addEventListener('click', () => goToStep(i)));
```
