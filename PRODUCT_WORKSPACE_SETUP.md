# プロダクトワークスペース セットアップ指示書

> このファイルを新しいプロダクトフォルダに置いてClaude Codeに読み込ませると、
> プロダクトワークスペースの構造が自動生成される。

## セットアップフロー

### Step 1: cc-company の存在確認
- `../cc-company/` が存在するか確認
- 存在しない場合 → Kenに「cc-companyフォルダのパスを教えてください」と確認

### Step 2: Vision 初期設定
- `cc-company/.company/vision/` の3ファイルを確認
- 既に内容がある → そのまま使用
- 空の場合 → Kenに選択肢を提示:
  - A: AI対話型（質問に答えていく形式で Vision を一緒に作る）
  - B: 自己入力型（Ken が直接書き込む）

### Step 3: ディレクトリ・ファイル生成
以下の構造を生成する:

```
product-xxx/
├── CLAUDE.md
├── meta.json
├── meetings/
│   ├── research/
│   ├── 01_ideation/logs/
│   ├── 02_selection/logs/
│   └── 03_requirements/logs/
└── dev/
    ├── 01_requirements/
    ├── 02_basic_design/
    ├── 03_detailed_design/
    ├── 04_implementation/src/
    ├── 04_implementation/tests/
    ├── 05_acceptance/
    └── 06_release/
```

### Step 4: meta.json 初期化
- product_name: Kenに確認
- created_at: 現在時刻（ISO8601）
- status: "meeting"
- meeting_phase.current: "01_ideation"

### Step 5: 完了報告
```
✅ プロダクトワークスペース「{product_name}」を作成しました。
📁 構造: meetings/ + dev/ の2層構造
🎯 次のステップ: 「発散会議を開いて」で壁打ちを開始できます。
```
