# Epic作成ワークフロー

## 概要
「epic作成」と入力された場合、以下のワークフローを自動実行します：

1. ユーザーにEpicの詳細をヒアリング
2. prd-templateフォルダを複製して新しいEpic用フォルダを作成
3. ヒアリング内容を基にEpicとUser Storyの草案を作成

## ヒアリング項目

### 基本情報
- **Epic名**: どのような機能・機能群を作成しますか？
- **目的**: このEpicで解決したい問題や目指す価値は何ですか？

### 詳細情報
- **主要機能**: このEpicに含まれる主要な機能は何ですか？
- **対象ユーザー**: 主な対象ユーザーは誰ですか？
- **ビジネス価値**: どのようなビジネス価値を提供しますか？

### User Story関連
- **想定されるUser Story**: このEpicに含まれるUser Storyの候補はありますか？

## 実行手順

### 1. ヒアリング実行
- 上記のヒアリング項目を順次質問
- ユーザーの回答を記録
- 必要に応じて追加質問

### 2. Epic用フォルダの作成
- `prd-template`フォルダを複製
- 新しいフォルダ名: `[Epic名]-epic`
- 例: `写真管理機能-epic`

### 3. Epicドキュメントの作成
- `epic-template.md`を基にEpicドキュメントを作成
- ファイル名: `epic-[Epic名].md`
- ヒアリング内容を反映
- 元の`epic-template.md`は削除

### 4. User Storyの草案作成
- ヒアリングで特定されたUser Storyの候補を基に草案を作成
- `userstory-template.md`を基に各User Storyを作成
- ファイル名: `us-[User Story名].md`
- 元の`userstory-template.md`は削除

## 作成されるファイル構造
```
prd/[Epic名]-epic/
├── epic-[Epic名].md
├── us-[User Story1名].md
├── us-[User Story2名].md
└── ...
```

## 使用するテンプレート
- Epicテンプレート: [prd/prd-template/epic-template.md](mdc:prd/prd-template/epic-template.md)
- User Storyテンプレート: [prd/prd-template/userstory-template.md](mdc:prd/prd-template/userstory-template.md)

## エラーハンドリング
- ヒアリング内容が不十分な場合は追加質問
- フォルダ作成エラー時はエラーメッセージを表示
- テンプレートファイルが見つからない場合は手動作成

## 注意事項
- ヒアリング内容を基に適切なEpicとUser Storyを提案
- ファイル命名ルールに従ってファイル名を設定
- 既存のEpicとの重複を避ける
- 作成後はユーザーに確認を求める

## 使用例
ユーザー入力: "epic作成"
→ ヒアリング開始
→ 回答を基にEpicとUser Storyの草案を作成
description:
globs:
alwaysApply: false
---
