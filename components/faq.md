# faq.md — アコーディオンFAQ コンポーネント

> このファイルは `components/faq.md` として保存し、FAQセクション制作直前に読み込む。

---

## F1：シンプルアコーディオンFAQ

### ① 概要
質問をクリック/タップすると回答が展開されるアコーディオン型FAQ。1件ずつ開閉する。

### ② 確認事項

| 項目 | 確認内容 | 例 |
|------|---------|-----|
| 件数 | FAQ件数 | 5〜8件 |
| 初期状態 | 全件閉じた状態 or 1件目を開いた状態 | 全件閉じた状態（デフォルト）|
| 排他制御 | 1件開いたら他を閉じるか | あり（デフォルト）|
| Q/Aラベルデザイン | テキストのみ or アイコン付き | アイコン付き（Q/A）|
| セクションタイトル | 見出しテキスト | 「よくある質問」|

### ③ テンプレート

```html
<section id="faq" class="section-faq">
  <div class="inner-container">
    <h2 class="section-title">【SECTION_TITLE】</h2>
    <dl class="faq-list">

      <!-- FAQ項目1 -->
      <div class="faq-item">
        <dt class="faq-question" role="button" tabindex="0" aria-expanded="false" aria-controls="faq-answer-1">
          <span class="faq-q-label">Q</span>
          <span class="faq-q-text">【QUESTION_1】</span>
          <span class="faq-icon" aria-hidden="true"></span>
        </dt>
        <dd class="faq-answer" id="faq-answer-1" aria-hidden="true">
          <span class="faq-a-label">A</span>
          <span class="faq-a-text">【ANSWER_1】</span>
        </dd>
      </div>

      <!-- FAQ項目2（同じ構造で繰り返す） -->
      <div class="faq-item">
        <dt class="faq-question" role="button" tabindex="0" aria-expanded="false" aria-controls="faq-answer-2">
          <span class="faq-q-label">Q</span>
          <span class="faq-q-text">【QUESTION_2】</span>
          <span class="faq-icon" aria-hidden="true"></span>
        </dt>
        <dd class="faq-answer" id="faq-answer-2" aria-hidden="true">
          <span class="faq-a-label">A</span>
          <span class="faq-a-text">【ANSWER_2】</span>
        </dd>
      </div>

      <!-- 以降、同じ構造で繰り返す -->

    </dl>
  </div>
</section>
```

```css
/* FAQ Section */
.section-faq { padding: 60px 0; }
.faq-list { margin-top: 40px; display: flex; flex-direction: column; gap: 12px; }

.faq-item {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
}

/* 質問行 */
.faq-question {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 18px 20px;
  cursor: pointer;
  background-color: #ffffff;
  transition: background-color 0.2s ease;
  list-style: none;
}
.faq-question:hover { background-color: #f5f5f5; }
.faq-question[aria-expanded="true"] { background-color: #f5f5f5; }

.faq-q-label {
  flex-shrink: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: var(--color-accent);
  color: #ffffff;
  font-size: 1.5rem;
  font-weight: 700;
  border-radius: 50%;
}
.faq-q-text { flex: 1; font-size: 1.5rem; font-weight: 700; line-height: 1.6; }

/* 開閉アイコン（+/-） */
.faq-icon {
  flex-shrink: 0;
  width: 20px;
  height: 20px;
  position: relative;
}
.faq-icon::before,
.faq-icon::after {
  content: '';
  position: absolute;
  background-color: var(--color-main);
  border-radius: 2px;
  transition: transform 0.3s ease, opacity 0.3s ease;
}
.faq-icon::before { width: 2px; height: 100%; top: 0; left: 50%; transform: translateX(-50%); }
.faq-icon::after  { width: 100%; height: 2px; top: 50%; left: 0; transform: translateY(-50%); }
.faq-question[aria-expanded="true"] .faq-icon::before { transform: translateX(-50%) rotate(90deg); opacity: 0; }

/* 回答行 */
.faq-answer {
  display: flex;
  align-items: flex-start;
  gap: 12px;
  padding: 0 20px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.4s ease, padding 0.4s ease;
  background-color: #fafafa;
}
.faq-answer.is-open {
  max-height: 400px; /* 回答の最大高さ。長い場合は増やす */
  padding: 18px 20px;
}
.faq-a-label {
  flex-shrink: 0;
  width: 32px;
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #555;
  color: #ffffff;
  font-size: 1.5rem;
  font-weight: 700;
  border-radius: 50%;
}
.faq-a-text { flex: 1; font-size: 1.4rem; line-height: 1.8; color: #555; }
```

```javascript
// Accordion FAQ
const faqQuestions = document.querySelectorAll('.faq-question');

faqQuestions.forEach((question) => {
  question.addEventListener('click', () => {
    const answerId = question.getAttribute('aria-controls');
    const answer = document.getElementById(answerId);
    const isOpen = question.getAttribute('aria-expanded') === 'true';

    // 排他制御：他のFAQをすべて閉じる
    faqQuestions.forEach((q) => {
      const aId = q.getAttribute('aria-controls');
      const a = document.getElementById(aId);
      q.setAttribute('aria-expanded', 'false');
      if (a) { a.classList.remove('is-open'); a.setAttribute('aria-hidden', 'true'); }
    });

    // クリックされたFAQを開く（すでに開いていた場合は閉じる）
    if (!isOpen) {
      question.setAttribute('aria-expanded', 'true');
      if (answer) { answer.classList.add('is-open'); answer.setAttribute('aria-hidden', 'false'); }
    }
  });

  // キーボード操作対応（Enter / Space）
  question.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      question.click();
    }
  });
});
```

---

## 注意事項

- `max-height` の値は回答テキストの長さに応じて調整すること（長い回答は `600px` 以上に設定）
- 排他制御が不要な場合（複数同時展開を許可する場合）は、JSの「排他制御」ブロックを削除すること
- `aria-expanded` / `aria-hidden` / `aria-controls` はアクセシビリティのために必ず維持すること
