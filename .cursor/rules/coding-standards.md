# コーディング標準とファイル命名規則

## ファイル命名規則

### Epic関連
- Epicファイル: `epic-[Epic名].md`
- User Storyファイル: `us-[Epic名]-XX.md`
- フォルダ: `[Epic名]-epic`

### アプリケーション関連
- HTMLファイル: `[機能名].html` (例: `index.html`, `history.html`)
- CSSファイル: `[機能名].css` (例: `main.css`, `components.css`)
- JavaScriptファイル: `[機能名].js` (例: `app.js`, `chart.js`)

### アセット関連
- 画像: `[用途]-[名前].[拡張子]` (例: `icon-home.svg`, `bg-pattern.png`)
- アイコン: `icon-[名前].svg`
- ロゴ: `logo-[バリエーション].svg`

## コメント規則

### 日本語コメント
- すべてのコメントは日本語で記述
- 絵文字を適切に使用して視認性を向上
- マークダウン形式で統一

### HTMLコメント例
```html
<!-- 📝 ヘッダーセクション -->
<header class="app-header">
  <!-- 🏠 ホームボタン -->
  <button class="home-btn">🏠</button>
</header>
```

### CSSコメント例
```css
/* 🎨 メインカラーパレット */
:root {
  --primary-color: #4a90e2;    /* メインカラー */
  --secondary-color: #f39c12;  /* アクセントカラー */
  --text-color: #333;          /* テキストカラー */
}

/* 📱 レスポンシブデザイン */
@media (max-width: 768px) {
  /* モバイル用スタイル */
}
```

### JavaScriptコメント例
```javascript
// 📊 グラフデータの初期化
function initializeChartData() {
  // localStorageからデータを取得
  const savedData = localStorage.getItem('guinnessData');
  
  // データが存在しない場合は初期化
  if (!savedData) {
    localStorage.setItem('guinnessData', JSON.stringify([]));
  }
}

// 🎯 記録の追加
function addRecord(category, value, memo = '') {
  // 入力値の検証
  if (!category || value === undefined) {
    console.error('❌ 必須項目が入力されていません');
    return false;
  }
  
  // 記録オブジェクトの作成
  const record = {
    id: Date.now(),
    category: category,
    value: value,
    memo: memo,
    timestamp: new Date().toISOString()
  };
  
  // データの保存
  saveRecord(record);
}
```

## コード品質

### 基本原則
- シンプルで保守しやすいコード
- 適切なエラーハンドリング
- レスポンシブデザイン対応
- アクセシビリティを考慮

### JavaScript品質基準
```javascript
// ✅ 良い例
function calculateAverage(values) {
  if (!Array.isArray(values) || values.length === 0) {
    return 0;
  }
  
  const sum = values.reduce((acc, val) => acc + val, 0);
  return Math.round((sum / values.length) * 100) / 100;
}

// ❌ 悪い例
function calcAvg(vals) {
  return vals.reduce((a,b) => a+b, 0) / vals.length;
}
```

### CSS品質基準
```css
/* ✅ 良い例 - BEM記法を使用 */
.record-form {
  padding: 1rem;
  border-radius: 8px;
  background-color: var(--bg-color);
}

.record-form__input {
  width: 100%;
  padding: 0.5rem;
  border: 1px solid var(--border-color);
}

.record-form__input--error {
  border-color: var(--error-color);
}

/* ❌ 悪い例 - 非構造的なクラス名 */
.form {
  padding: 1rem;
}

.input {
  width: 100%;
}
```

## エラーハンドリング

### JavaScriptエラーハンドリング
```javascript
// 🛡️ 安全なデータ取得
function getStoredData(key) {
  try {
    const data = localStorage.getItem(key);
    return data ? JSON.parse(data) : null;
  } catch (error) {
    console.error('❌ データの取得に失敗しました:', error);
    return null;
  }
}

// 🛡️ 安全なデータ保存
function saveData(key, data) {
  try {
    localStorage.setItem(key, JSON.stringify(data));
    return true;
  } catch (error) {
    console.error('❌ データの保存に失敗しました:', error);
    return false;
  }
}
```

## レスポンシブデザイン

### ブレークポイント
```css
/* 📱 モバイルファースト */
.container {
  width: 100%;
  padding: 1rem;
}

/* 📱 タブレット */
@media (min-width: 768px) {
  .container {
    max-width: 768px;
    margin: 0 auto;
  }
}

/* 💻 デスクトップ */
@media (min-width: 1024px) {
  .container {
    max-width: 1024px;
  }
}
```

## アクセシビリティ

### HTMLアクセシビリティ
```html
<!-- ✅ 良い例 -->
<button 
  class="record-btn" 
  aria-label="記録を追加"
  aria-describedby="record-help">
  記録する
</button>
<div id="record-help" class="sr-only">
  記録を追加するボタンです
</div>

<!-- ❌ 悪い例 -->
<div class="record-btn" onclick="addRecord()">
  記録する
</div>
```

### スクリーンリーダー対応
```css
/* 🦯 スクリーンリーダー専用クラス */
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
``` 