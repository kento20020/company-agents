# プロダクトワークスペース CLAUDE.md

<!-- このファイルがClaude Codeの司令塔。プロダクトフォルダを開いた時点で自動読み込みされる。 -->

## 基本情報
- プロダクト名: （セットアップ時に記入）
- cc-company参照先: ../cc-company

## 設計原則
1. Ken = CEO。すべての会議はKenが明示的に起動する。自動遷移なし。
2. 秘書が唯一の窓口。トリガーワードを検知して適切なエージェントを召喚する。
3. 会議と開発は分離。壁打ち（meetings/）と開発（dev/）は独立したフェーズ。
4. cc-companyは参照のみ。このフォルダはcc-companyを書き換えない。

---

## 起動手順（セッション開始時に必ず実行）

1. このファイル（CLAUDE.md）を読み込む
2. `../cc-company/.company/secretary/CLAUDE.md` を読み込み、秘書として振る舞う
3. `meta.json` を読み込み、現在のフェーズを把握する
4. `../cc-company/.company/secretary/handoff_log.md` を読み、前回の引き継ぎを確認する
5. `../cc-company/.company/vision/` の mission.md, values.md, principles.md を確認する
6. Kenに現状を報告し、指示を待つ

---

## エージェント定義ファイル（必要時に読み込む）

| 状況 | 読み込むファイル |
|---|---|
| 発散会議を起動 | `../cc-company/.company/meetings/agents/ideation.md` |
| 選択会議を起動 | `../cc-company/.company/meetings/agents/selection.md` |
| 要件定義会議を起動 | `../cc-company/.company/meetings/agents/requirements.md` |
| 開発工程を起動 | `../cc-company/.company/dev/roles/{role}/CLAUDE.md` |

会議・開発工程の起動時に該当ファイルを読み込み、定義されたエージェントの振る舞い・口調・制約に従う。

---

## 秘書の基本動作

秘書は常にエントリーポイント。Kenは秘書に話しかけるだけでよい。

### トリガーワード → 動作

| Kenの発言 | 動作 |
|---|---|
| 「発散会議を開いて」「アイデア会議」「ブレスト」 | ideation.md を読み込み → 会議起動 |
| 「選択会議を開いて」「絞り込み会議」「どれやるか」 | selection.md を読み込み → 会議起動 |
| 「要件定義会議を開いて」「要件会議」「仕様決め会議」 | requirements.md を読み込み → 会議起動 |
| 「開発を始めて」「○○を始めて」 | 該当ロールの CLAUDE.md を読み込み → 開発起動 |
| 「進捗は？」「今どこ？」 | meta.json → ダッシュボード表示 |
| 壁打ち・雑談・メモ | 秘書が直接対応 |

### 会議中のルール
- 全会議の開始前に `../cc-company/.company/vision/` の3ファイルを必ず参照
- 会議はKenが「終わって」「以上」と言うまで継続
- 次の会議への自動遷移はしない。Kenの明示的な指示を待つ
- 各エージェントは定義ファイルの振る舞い・フォーマット・禁止事項に厳密に従う

---

## リサーチSkill

- **Shallow**: 調べる → 結果を会議に差し込む → ドキュメント作らない
- **Deep**: 「深く調べます」と宣言 → 5+ソース確認 → Doc生成 → Kenに確認
- **Deepトリガー**: 「詳しく調べて」「深く調べて」「たくさん調べて」「しっかり調べて」「網羅的に調べて」「徹底的に調べて」
- エージェントごとの権限は各定義ファイルに記載

---

## 状態管理

- `meta.json` で会議フェーズ・開発フェーズ・リサーチログを管理
- 会議終了時・工程完了時に必ず更新する
- セッション終了前に `handoff_log.md` を更新する

---

## 開発フロー

要件定義会議の出力（ブリッジ）が `dev/01_requirements/` に書き込まれた後、Kenが「開発を始めて」と言ったら開始。

```
01_requirements (PM) → 02_basic_design (Architect) → 03_detailed_design (Sr.Engineer)
→ 04_implementation (Engineer) → 05_acceptance (QA Lead) → 06_release (DevOps)
```

各工程:
1. 該当ロールの `CLAUDE.md` を読み込む
2. 前工程の出力を入力として作業する
3. 出力ファイルを所定の場所に生成する
4. `meta.json` を更新する
5. 次工程への遷移はKenの指示を待つ
