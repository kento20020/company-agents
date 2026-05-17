# プロダクトワークスペース セットアップ

新しいプロダクトを始めるときの手順。

---

## 手順

### Step 1: テンプレートをコピー

```bash
cp -r company-agents/product-template/ product-{プロダクト名}/
cd product-{プロダクト名}/
```

### Step 2: CLAUDE.md の参照先を設定

`CLAUDE.md` を開き、「cc-company参照先」を記入する。

```markdown
- cc-company参照先: ../company-agents/cc-company
```

※ プロダクトフォルダから cc-company への相対パスを書く。
フォルダ配置例:
```
PGs/
├── company-agents/cc-company/   ← 会社本体
└── product-my-app/              ← ここから ../company-agents/cc-company
```

### Step 3: Vision を確認

`company-agents/cc-company/vision/` の3ファイルを確認する。

| ファイル | 内容 | 例 |
|---|---|---|
| mission.md | 何を成し遂げるか | 「個人開発者の生産性を10倍にする」 |
| values.md | 何を大事にするか | 「シンプルさ、速さ、楽しさ」 |
| principles.md | どう行動するか | 「まず動くものを作る」 |

- 既に記入済み → そのまま使う
- 空の場合 → 秘書と対話しながら一緒に作るか、自分で直接書く

### Step 4: Claude Code を開く

```bash
cd product-{プロダクト名}/
claude
```

秘書が自動で起動し、以下を実行する:
1. `CLAUDE.md` を読み込み
2. cc-company の `secretary/CLAUDE.md` を読み込み
3. `meta.json` で状態確認
4. 「準備できました。何をしましょうか？」と報告

### Step 5: 始める

```
Ken: 「発散会議を開いて」
```

---

## プロダクトフォルダの構成

```
product-{名前}/
├── CLAUDE.md              ← ブートローダー（cc-company参照先を設定）
├── meta.json              ← 状態管理
├── secretary/             ← 秘書の書き込み先
│   ├── handoff_log.md     　 セッション引き継ぎ
│   ├── todo.md            　 TODO
│   └── notes/             　 メモ・壁打ち記録
├── meetings/              ← 会議ログ保存先
│   ├── 01_ideation/logs/
│   ├── 02_selection/logs/
│   ├── 03_requirements/logs/
│   └── research/          　 Deepリサーチ成果物
└── dev/                   ← 開発成果物保存先
    ├── 01_requirements/
    ├── 02_basic_design/
    ├── 03_detailed_design/
    ├── 04_implementation/src/
    ├── 04_implementation/tests/
    ├── 05_acceptance/
    └── 06_release/
```

---

## できること一覧

| 言葉 | 何が起きるか |
|---|---|
| 「発散会議を開いて」 | アイデアを広げる会議（3エージェント） |
| 「選択会議を開いて」 | アイデアを検証・絞る会議（4エージェント） |
| 「要件定義会議を開いて」 | 仕様を固める会議（6エージェント） |
| 「開発を始めて」 | 工程を順に実行（設計→実装→テスト→リリース） |
| 「進捗は？」 | 現在のフェーズをダッシュボード表示 |
| 「詳しく調べて」 | Deepリサーチ（5+ソース、ドキュメント生成） |

---

## 注意事項

- 会議は自動で次に進まない。Kenが「次を開いて」と言うまで待つ
- cc-company フォルダは読み取り専用。変更しない
- 同じ日の会議ログは追記される（新規ファイル作成しない）
- 書き込みはすべてプロダクトフォルダ内で完結する
