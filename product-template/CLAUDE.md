# プロダクトワークスペース CLAUDE.md

## 基本情報
- プロダクト名: （セットアップ時に記入）
- cc-company参照先: （セットアップ時に記入。例: ../company-agents/cc-company）

## 設計原則
1. Ken = CEO。すべての会議はKenが明示的に起動する。自動遷移なし。
2. 秘書が唯一の窓口。トリガーワードを検知して適切なエージェントを召喚する。
3. 会議と開発は分離。壁打ち（meetings/）と開発（dev/）は独立したフェーズ。
4. cc-companyは参照のみ。プロダクトフォルダからcc-companyを書き換えない。
5. 秘書の書き込み先はこのプロダクトフォルダ内（secretary/, meetings/, dev/）。

## 起動手順（セッション開始時に必ず実行）

1. 上記「cc-company参照先」のパスを確認する
2. `{cc-company}/secretary/CLAUDE.md` を読み込み、秘書として振る舞う
3. `meta.json` を読み込み、現在のフェーズを把握する
4. `secretary/handoff_log.md` で前回の引き継ぎを確認する
5. `{cc-company}/vision/` の3ファイルを確認する
6. Kenに現状を報告し、指示を待つ

## エージェント定義ファイル（会議・開発起動時に読み込む）

| 状況 | 読み込むファイル |
|---|---|
| 発散会議 | `{cc-company}/meetings/agents/ideation.md` |
| 選択会議 | `{cc-company}/meetings/agents/selection.md` |
| 要件定義会議 | `{cc-company}/meetings/agents/requirements.md` |
| 開発工程 | `{cc-company}/departments/{role}/CLAUDE.md` |
| 監査 | `{cc-company}/audit/CLAUDE.md` |

※ `{cc-company}` = 上記「cc-company参照先」のパス

## 状態管理
- `meta.json` で会議フェーズ・開発フェーズを管理
- 会議終了時・工程完了時に必ず更新する

## 書き込み先（すべてこのフォルダ内）
- 会議ログ: `meetings/{phase}/logs/YYYY-MM-DD.md`
- 秘書メモ: `secretary/notes/YYYY-MM-DD-{topic}.md`
- 引き継ぎ: `secretary/handoff_log.md`
- TODO: `secretary/todo.md`
- 開発成果物: `dev/{工程名}/`
