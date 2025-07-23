# Epic: データ管理機能

## 📋 Epic概要

### Epic名
データ管理機能Epic

### 目的
記録入力機能の基盤となる最重要機能として、localStorageを使った効率的なデータ管理を実現し、ユーザーの記録データを安全かつ高速に管理する。

### 対象ユーザー
- **プライマリ**: 記録入力機能を利用するユーザー
- **セカンダリ**: データ管理・バックアップを重視するユーザー
- **特徴**: データの永続性と整合性を重視するユーザー

### ビジネス価値
- 記録入力機能の安定動作を保証
- データ損失リスクの最小化
- ユーザーの継続利用を促進
- 将来的な機能拡張の基盤提供

## 🎯 ビジネス目標
- データ管理の信頼性向上によるユーザー満足度の向上
- データ損失による離脱リスクの削減
- 効率的なデータ処理によるパフォーマンス向上
- データエクスポート機能によるユーザーエンゲージメント向上

## 📊 成功指標（定性・定量）
| 指標 | 目標値 | 測定方法 | 測定タイミング |
|------|--------|----------|----------------|
| データ保存成功率 | 99.9%以上 | 保存操作の成功/失敗率 | リリース後1週間 |
| データ読み込み速度 | 100ms以内 | 平均読み込み時間 | リリース後1週間 |
| データ整合性エラー率 | 0.1%以下 | データ不整合の発生率 | リリース後1ヶ月 |
| バックアップ利用率 | 30%以上 | エクスポート機能の利用率 | リリース後1ヶ月 |

## 🔄 大まかな体験の流れ
1. **開始**: ユーザーが記録データを入力・保存
2. **進行**: データの自動保存・整合性チェック・バックアップ
3. **完了**: 安全で高速なデータ管理の実現

## 📋 含まれるUser Story
- [localStorageデータ構造設計機能](./us-localStorageデータ構造設計機能.md)
- [記録データCRUD操作機能](./us-記録データCRUD操作機能.md)
- [データバックアップ・復旧機能](./us-データバックアップ復旧機能.md)
- [データエクスポート・インポート機能](./us-データエクスポートインポート機能.md)
- [データ整合性チェック機能](./us-データ整合性チェック機能.md)

## 🏗️ データ構造設計

### 1. localStorageキー設計
```
myguinnessbook/
├── settings/           # アプリ設定
│   ├── theme          # テーマ設定
│   ├── language       # 言語設定
│   └── preferences    # ユーザー設定
├── data/              # 記録データ
│   ├── records        # 記録一覧
│   ├── themes         # テーマ一覧
│   └── categories     # カテゴリ一覧
├── backup/            # バックアップデータ
│   ├── auto           # 自動バックアップ
│   └── manual         # 手動バックアップ
└── metadata/          # メタデータ
    ├── version        # データバージョン
    ├── lastUpdate     # 最終更新日時
    └── statistics     # 統計情報
```

### 2. 記録データ構造
```javascript
// 記録データの基本構造
{
  id: "unique_id",
  title: "記録タイトル",
  content: "記録内容",
  theme: "テーマ名",
  category: "カテゴリ",
  value: 数値,
  unit: "単位",
  date: "YYYY-MM-DD",
  timestamp: 1234567890,
  tags: ["タグ1", "タグ2"],
  attachments: [
    {
      type: "image",
      data: "base64_data",
      filename: "image.jpg"
    }
  ],
  metadata: {
    created: "2025-01-01T00:00:00Z",
    updated: "2025-01-01T00:00:00Z",
    version: "1.0"
  }
}
```

### 3. テーマデータ構造
```javascript
// テーマデータの基本構造
{
  id: "unique_id",
  name: "テーマ名",
  description: "テーマ説明",
  category: "カテゴリ",
  isDefault: false,
  isCustom: true,
  metadata: {
    created: "2025-01-01T00:00:00Z",
    updated: "2025-01-01T00:00:00Z",
    usageCount: 0
  }
}
```

## 🔧 技術的実装仕様

### 1. localStorage管理クラス
```javascript
class DataManager {
  constructor() {
    this.prefix = 'myguinnessbook/';
    this.maxSize = 5 * 1024 * 1024; // 5MB制限
  }
  
  // データ保存
  save(key, data) {
    try {
      const fullKey = this.prefix + key;
      const jsonData = JSON.stringify(data);
      
      // 容量チェック
      if (this.getCurrentSize() + jsonData.length > this.maxSize) {
        this.cleanup();
      }
      
      localStorage.setItem(fullKey, jsonData);
      return true;
    } catch (error) {
      console.error('データ保存エラー:', error);
      return false;
    }
  }
  
  // データ読み込み
  load(key) {
    try {
      const fullKey = this.prefix + key;
      const data = localStorage.getItem(fullKey);
      return data ? JSON.parse(data) : null;
    } catch (error) {
      console.error('データ読み込みエラー:', error);
      return null;
    }
  }
  
  // データ削除
  delete(key) {
    try {
      const fullKey = this.prefix + key;
      localStorage.removeItem(fullKey);
      return true;
    } catch (error) {
      console.error('データ削除エラー:', error);
      return false;
    }
  }
  
  // 容量管理
  getCurrentSize() {
    let size = 0;
    for (let key in localStorage) {
      if (key.startsWith(this.prefix)) {
        size += localStorage[key].length;
      }
    }
    return size;
  }
  
  // 古いデータの削除
  cleanup() {
    // 古い記録データを削除（30日以上前）
    const records = this.load('data/records') || [];
    const thirtyDaysAgo = Date.now() - (30 * 24 * 60 * 60 * 1000);
    const filteredRecords = records.filter(record => 
      record.timestamp > thirtyDaysAgo
    );
    this.save('data/records', filteredRecords);
  }
}
```

### 2. データ整合性チェック機能
```javascript
class DataIntegrityChecker {
  // データ構造の検証
  validateRecord(record) {
    const required = ['id', 'title', 'date', 'timestamp'];
    const missing = required.filter(field => !record[field]);
    
    if (missing.length > 0) {
      return {
        valid: false,
        errors: `必須フィールドが不足: ${missing.join(', ')}`
      };
    }
    
    // 日付形式の検証
    if (!this.isValidDate(record.date)) {
      return {
        valid: false,
        errors: '無効な日付形式'
      };
    }
    
    return { valid: true };
  }
  
  // 全データの整合性チェック
  checkAllData() {
    const records = dataManager.load('data/records') || [];
    const themes = dataManager.load('data/themes') || [];
    
    const errors = [];
    
    // 記録データの検証
    records.forEach((record, index) => {
      const validation = this.validateRecord(record);
      if (!validation.valid) {
        errors.push(`記録${index + 1}: ${validation.errors}`);
      }
    });
    
    // テーマ参照の整合性チェック
    records.forEach((record, index) => {
      const themeExists = themes.some(theme => theme.name === record.theme);
      if (!themeExists) {
        errors.push(`記録${index + 1}: 存在しないテーマ "${record.theme}"`);
      }
    });
    
    return {
      valid: errors.length === 0,
      errors: errors
    };
  }
}
```

### 3. バックアップ・復旧機能
```javascript
class BackupManager {
  // 自動バックアップ
  createAutoBackup() {
    const timestamp = new Date().toISOString();
    const backupData = {
      records: dataManager.load('data/records'),
      themes: dataManager.load('data/themes'),
      settings: dataManager.load('settings'),
      timestamp: timestamp,
      type: 'auto'
    };
    
    const backupKey = `backup/auto/${timestamp}`;
    return dataManager.save(backupKey, backupData);
  }
  
  // 手動バックアップ
  createManualBackup() {
    const timestamp = new Date().toISOString();
    const backupData = {
      records: dataManager.load('data/records'),
      themes: dataManager.load('data/themes'),
      settings: dataManager.load('settings'),
      timestamp: timestamp,
      type: 'manual'
    };
    
    const backupKey = `backup/manual/${timestamp}`;
    return dataManager.save(backupKey, backupData);
  }
  
  // バックアップからの復旧
  restoreFromBackup(backupKey) {
    const backupData = dataManager.load(backupKey);
    if (!backupData) {
      return { success: false, error: 'バックアップデータが見つかりません' };
    }
    
    try {
      // 現在のデータをバックアップ
      this.createManualBackup();
      
      // バックアップデータを復元
      dataManager.save('data/records', backupData.records);
      dataManager.save('data/themes', backupData.themes);
      dataManager.save('settings', backupData.settings);
      
      return { success: true };
    } catch (error) {
      return { success: false, error: error.message };
    }
  }
}
```

### 4. エクスポート・インポート機能
```javascript
class DataExporter {
  // JSON形式でエクスポート
  exportToJSON() {
    const exportData = {
      version: "1.0",
      exportDate: new Date().toISOString(),
      records: dataManager.load('data/records'),
      themes: dataManager.load('data/themes'),
      settings: dataManager.load('settings')
    };
    
    const blob = new Blob([JSON.stringify(exportData, null, 2)], {
      type: 'application/json'
    });
    
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `myguinnessbook_${new Date().toISOString().split('T')[0]}.json`;
    a.click();
    URL.revokeObjectURL(url);
  }
  
  // CSV形式でエクスポート
  exportToCSV() {
    const records = dataManager.load('data/records') || [];
    
    const csvContent = [
      ['日付', 'テーマ', 'タイトル', '内容', '数値', '単位'].join(','),
      ...records.map(record => [
        record.date,
        record.theme,
        record.title,
        record.content,
        record.value,
        record.unit
      ].join(','))
    ].join('\n');
    
    const blob = new Blob([csvContent], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `myguinnessbook_${new Date().toISOString().split('T')[0]}.csv`;
    a.click();
    URL.revokeObjectURL(url);
  }
  
  // JSONファイルからインポート
  importFromJSON(file) {
    return new Promise((resolve, reject) => {
      const reader = new FileReader();
      reader.onload = (e) => {
        try {
          const importData = JSON.parse(e.target.result);
          
          // バージョンチェック
          if (importData.version !== "1.0") {
            reject(new Error('サポートされていないバージョンです'));
            return;
          }
          
          // データの検証
          const integrityChecker = new DataIntegrityChecker();
          const validation = integrityChecker.checkAllData();
          if (!validation.valid) {
            reject(new Error('データの整合性チェックに失敗しました'));
            return;
          }
          
          // データのインポート
          dataManager.save('data/records', importData.records);
          dataManager.save('data/themes', importData.themes);
          dataManager.save('settings', importData.settings);
          
          resolve({ success: true });
        } catch (error) {
          reject(new Error('ファイルの読み込みに失敗しました'));
        }
      };
      reader.readAsText(file);
    });
  }
}
```

## 🔗 関連情報

### 依存関係
- **前提条件**: ブラウザのlocalStorage対応、HTML5/CSS3/JavaScript
- **後続機能**: 記録入力機能、統計・可視化機能、テーマ・カスタマイズ機能

### 制約事項
- **技術的制約**: localStorage容量制限（5-10MB）、ブラウザ互換性、クライアントサイドのみ
- **ビジネス制約**: データプライバシーの確保（ローカル保存のみ）
- **リソース制約**: GitHub Pagesでの静的サイト運用、サーバーサイド機能なし

### リスク
| リスク | 影響度 | 発生確率 | 対策 |
|--------|--------|----------|------|
| localStorage容量不足 | High | Medium | 自動クリーンアップ、データ圧縮 |
| ブラウザデータクリア | High | Medium | 自動バックアップ、エクスポート機能 |
| データ不整合 | Medium | Low | 整合性チェック機能、バリデーション |
| パフォーマンス低下 | Medium | Low | 効率的なデータ処理、遅延読み込み |

## 📝 備考
- localStorage容量制限を考慮した効率的なデータ管理
- データの整合性と信頼性を最優先とする
- 自動バックアップ機能でデータ損失リスクを最小化
- エクスポート・インポート機能でデータの可搬性を確保
- 将来的な機能拡張を考慮した柔軟な設計

---

**作成者**: Cursor AI Assistant  
**作成日**: 2025年7月23日  
**バージョン**: 1.0  
**ステータス**: 設計完了 