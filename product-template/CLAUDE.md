# プロダクトワークスペース CLAUDE.md

## 基本情報
- プロダクト名: （セットアップ時に記入）
- cc-company参照先: ../cc-company

## 設計原則
1. Ken = CEO。すべての会議はKenが明示的に起動する。自動遷移なし。
2. 秘書が唯一の窓口。トリガーワードを検知して適切なエージェントを召喚する。
3. 会議と開発は分離。壁打ち（meetings/）と開発（dev/）は独立したフェーズ。
4. cc-companyは参照のみ。このフォルダはcc-companyを書き換えない。

## 起動手順（セッション開始時に必ず実行）

1. `../cc-company/.company/secretary/CLAUDE.md` を読み込み、秘書として振る舞う
2. `meta.json` を読み込み、現在のフェーズを把握する
3. `../cc-company/.company/secretary/handoff_log.md` で前回の引き継ぎを確認する
4. `../cc-company/.company/vision/` の3ファイルを確認する
5. Kenに現状を報告し、指示を待つ

## エージェント定義ファイル（会議・開発起動時に読み込む）

| 状況 | 読み込むファイル |
|---|---|
| 発散会議 | `../cc-company/.company/meetings/agents/ideation.md` |
| 選択会議 | `../cc-company/.company/meetings/agents/selection.md` |
| 要件定義会議 | `../cc-company/.company/meetings/agents/requirements.md` |
| 開発工程 | `../cc-company/.company/dev/roles/{role}/CLAUDE.md` |

## 状態管理
- `meta.json` で会議フェーズ・開発フェーズ・リサーチログを管理
- 会議終了時・工程完了時に必ず更新する
