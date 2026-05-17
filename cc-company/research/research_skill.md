# リサーチSkill定義

## 判断フロー

```
1. 誰がトリガーか
   エージェント自身 → Shallow確定
   Kenの発言 → トリガーワードチェックへ

2. Kenの発言にトリガーワードが含まれるか
   含まれない → Shallow
   含まれる → Deep
```

## Shallow リサーチ
- 調べる → 結果を会議に差し込む → ドキュメント作らない
- 会議の流れを止めない

## Deep リサーチ
- 「深く調べます」と宣言
- 5+ソースを確認
- Doc生成: `meetings/research/YYYY-MM-DD_テーマ_deep.md`
- meta.json の research_log に追記
- 会議ログには1行だけ: `[リサーチ] テーマ → meetings/research/xxx.md`
- Kenに確認してから会議に戻る

## Deepトリガーワード
- 「詳しく調べて」
- 「深く調べて」
- 「たくさん調べて」
- 「しっかり調べて」
- 「網羅的に調べて」
- 「徹底的に調べて」

## エージェント別リサーチ権限

| 権限 | エージェント |
|---|---|
| なし | ビジョン番人 / 秘書 / 5-Whys / シンセサイザー / DevOps(06) |
| Shallowのみ | アナロジー職人 / ユーザー代弁者 / 破壊チーム / PM Agent / Engineer(04) / QA Lead(05) |
| Shallow/Deep可 | 数字ハンター / 技術Agent / Architect(02) / Sr.Eng(03) |
