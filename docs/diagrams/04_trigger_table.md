# 秘書が検知するトリガーワード一覧

| Ken発言 | 起動タイプ | 参加エージェント → 出力先 |
|---|---|---|
| 「発散会議を開いて」 | Ken起動 | ビジョン番人・アナロジー・ユーザー代弁者 → secretary/notes/ |
| 「選択会議を開いて」 | Ken起動 | 破壊・5-Whys・数字ハンター・まとめ役 → ceo/pending_decisions.md |
| 「要件定義会議を開いて」 | Ken起動 or 選択後自動 | 破壊・PM Agent・技術Agent・まとめ役 → 01_requirements/ |
| 監査FAIL検知（自動） | システム自動 | 担当Agent + 監査 + 5-Whys → 該当工程に差し戻し |
| Vision抵触検知（自動） | システム自動 | CEO + 秘書 + 発案者ロール → ceo/pending_decisions.md |
| 「議題Xで会議を開いて」 | Ken起動（カスタム） | 秘書がアジェンダ解析 → 最適エージェント自動選定 |

> ★ 要件定義会議の出力が dev/projects/ の 01_requirements/ に直接書き込まれることで、既存ウォーターフォールフローに自然に接続される
