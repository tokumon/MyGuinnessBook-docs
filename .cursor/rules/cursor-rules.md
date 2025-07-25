# Cursorルール設定

## 概要
このファイルは、Cursorでの開発作業を効率化するためのルール設定です。

## 自動化ワークフロー

### 1. Epic作成ワークフロー
**トリガー**: "epic作成"
**実行内容**: 
- ユーザーにEpicの詳細をヒアリング
- prd-templateフォルダを複製して新しいEpic用フォルダを作成
- ヒアリング内容を基にEpicとUser Storyの草案を作成

### 2. 業務終了ワークフロー
**トリガー**: "業務終了"
**実行内容**:
- 今日の日付を取得
- flow/[yyyy]/フォルダ構造を確認・作成
- 今日の活動内容を整理
- worklogファイルを自動作成

## ファイル命名ルール

### Epic
- **ファイル名**: `epic-[日本語でEpic名].md`
- **フォルダ名**: `[日本語でEpic名]-epic`
- **例**: `epic-記録入力機能.md`, `記録入力機能-epic/`

### User Story
- **ファイル名**: `us-[日本語でユーザーストーリータイトル].md`
- **例**: `us-ギネステーマ自作機能.md`

### Worklog
- **ファイル名**: `[yyyymmdd]-worklog.md`
- **場所**: `flow/[yyyy]/[yyyymmdd]-worklog.md`
- **例**: `flow/2025/20250723-worklog.md`

## フォルダ構造ルール

### PRD関連
```
prd/
├── README.md
├── prd-template/
│   ├── epic-template.md
│   └── userstory-template.md
└── [Epic名]-epic/
    ├── epic-[Epic名].md
    └── us-[User Story名].md
```

### Worklog関連
```
flow/
└── [yyyy]/
    └── [yyyymmdd]-worklog.md
```

## 品質基準

### Epic作成時
- 目的の明確化
- スコープの定義
- User Storyの洗い出し
- 優先度の設定

### User Story作成時
- ユーザー視点での記述
- 具体的な受け入れ基準
- UI/UX要件の明記
- 技術的考慮事項の記載

### Worklog作成時
- 活動内容の正確な記録
- 成果物の詳細な整理
- 次回タスクの明確化
- 振り返りのポエム

## エラーハンドリング

### 共通エラー処理
- ファイル作成エラー時の再試行
- フォルダ作成エラー時の代替手段
- 入力値検証エラー時の明確な指示

### Epic作成エラー
- ヒアリング内容不十分時の追加質問
- テンプレートファイル不足時の手動作成
- 重複Epic名の検出と警告

### Worklog作成エラー
- 日付取得エラー時の手動入力
- 既存ファイル上書き時の確認
- 活動内容抽出失敗時の手動補完

## 使用例

### Epic作成
```
ユーザー: "epic作成"
→ ヒアリング開始
→ EpicとUser Storyの自動作成
→ 完了確認
```

### 業務終了
```
ユーザー: "業務終了"
→ 日付取得
→ 活動内容分析
→ Worklog自動作成
→ 完了通知
```

## カスタマイズガイド

### テンプレートの変更
- 各テンプレートファイルを編集
- 新しいセクションの追加
- 既存セクションの修正

### ワークフローの拡張
- 新しいトリガーワードの追加
- 追加の自動化処理
- 外部ツールとの連携

### 品質基準の調整
- 受け入れ基準の変更
- エラーハンドリングの強化
- バリデーションルールの追加 