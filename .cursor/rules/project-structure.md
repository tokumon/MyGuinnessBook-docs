# プロジェクト構造とディレクトリ構成

## 📁 推奨ディレクトリ構造

```
MyGuinnessBook-docs/
├── 📄 README.md                    # プロジェクト概要
├── 📄 .cursorrules                 # Cursorルール（メイン）
├── 📁 .cursor/                     # Cursor設定フォルダ
│   └── 📁 rules/                   # 個別ルールファイル
│       ├── 📄 epic-creation-workflow.md
│       ├── 📄 coding-standards.md
│       └── 📄 project-structure.md
├── 📁 prd/                         # プロダクト要件定義
│   ├── 📄 README.md
│   └── 📁 prd-template/
├── 📁 epics/                       # Epicドキュメント
│   ├── 📁 [epic名]-epic/
│   │   ├── 📄 epic-[epic名].md
│   │   ├── 📁 user-stories/
│   │   └── 📁 assets/
│   └── ...
├── 📁 docs/                        # ドキュメント
│   ├── 📄 api.md
│   ├── 📄 deployment.md
│   └── 📄 troubleshooting.md
└── 📁 templates/                   # テンプレートファイル
    ├── 📄 epic-template.md
    ├── 📄 user-story-template.md
    └── 📄 prd-template.md
```

## 🎯 ディレクトリの役割

### 📁 `.cursor/`
Cursorエディタの設定とルールファイルを格納
- `rules/`: 個別のCursorルールファイル
- 各ルールは独立したMarkdownファイルとして管理

### 📁 `prd/`
プロダクト要件定義（Product Requirements Document）
- 企画書、要件定義書
- テンプレートファイル
- プロジェクト計画書

### 📁 `epics/`
EpicとUser Storyの管理
- 各Epicは独立したフォルダで管理
- User Storyは各Epicフォルダ内に配置
- 関連アセットも含む

### 📁 `docs/`
プロジェクトドキュメント
- API仕様書
- デプロイメント手順
- トラブルシューティング
- アーキテクチャ設計書

### 📁 `templates/`
テンプレートファイル
- Epic作成用テンプレート
- User Story作成用テンプレート
- PRD作成用テンプレート

## 📋 ファイル命名規則

### Epic関連
```
epics/
├── user-authentication-epic/
│   ├── epic-user-authentication.md
│   ├── user-stories/
│   │   ├── us-user-authentication-01.md
│   │   └── us-user-authentication-02.md
│   └── assets/
│       └── auth-flow-diagram.png
```

### テンプレートファイル
```
templates/
├── 📄 epic-template.md           # Epic作成用テンプレート
├── 📄 user-story-template.md     # User Story作成用テンプレート
├── 📄 prd-template.md            # PRD作成用テンプレート
└── 📄 api-doc-template.md        # API仕様書テンプレート
```

## 🔧 設定ファイル

### ルートレベルの設定ファイル
```
MyGuinnessBook-docs/
├── 📄 .gitignore           # Git除外設定
├── 📄 .cursorrules         # Cursorルール（メイン）
├── 📄 README.md            # プロジェクト概要
└── 📄 CONTRIBUTING.md      # コントリビューションガイド
```

## 📋 テンプレート管理

### テンプレートファイル構成
```
templates/
├── 📄 epic-template.md           # Epic作成用テンプレート
├── 📄 user-story-template.md     # User Story作成用テンプレート
├── 📄 prd-template.md            # PRD作成用テンプレート
├── 📄 api-doc-template.md        # API仕様書テンプレート
└── 📄 meeting-template.md        # 会議議事録テンプレート
```

## 📊 ドキュメント構造

### Epicドキュメント構造
```markdown
epics/
├── [epic名]-epic/
│   ├── epic-[epic名].md          # Epic概要
│   ├── user-stories/             # User Story一覧
│   │   ├── us-[epic名]-01.md
│   │   ├── us-[epic名]-02.md
│   │   └── ...
│   └── assets/                   # 関連アセット
│       ├── diagrams/             # 図表
│       ├── mockups/              # モックアップ
│       └── references/           # 参考資料
```

## 🚀 ドキュメント公開

### GitHub Pages用
```
MyGuinnessBook-docs/
├── 📄 README.md            # プロジェクト概要
├── 📁 prd/                 # プロダクト要件定義
├── 📁 epics/               # Epicドキュメント
├── 📁 docs/                # 技術ドキュメント
└── 📁 templates/           # テンプレートファイル
```

## 📝 ドキュメント構造

### ドキュメント階層
```
docs/
├── 📄 getting-started.md   # はじめに
├── 📄 architecture.md      # アーキテクチャ設計
├── 📄 api-specification.md # API仕様書
├── 📄 deployment-guide.md  # デプロイメントガイド
├── 📄 troubleshooting.md   # トラブルシューティング
└── 📁 guides/              # ガイド
    ├── 📄 epic-creation.md
    ├── 📄 user-story-writing.md
    └── 📄 documentation-standards.md
``` 