# CC Company × Product Workspace System

Ken を CEO とする仮想会社組織（cc-company）に、プロダクトごとの壁打ち会議と開発フローを統合したシステム。

## 構造

```
cc-company/          ← 会社共通リソース（参照のみ）
product-template/    ← プロダクトテンプレート（コピーして使う）
```

## 基本原則

- **1プロダクト = 1フォルダ**（完全自己完結）
- **Ken = CEO**（全会議はKen起動・自動遷移なし）
- **秘書が唯一の窓口**（トリガーワード検知 → エージェント召喚）
- **会議と開発は分離**（橋渡しは要件定義の出力のみ）

---

## 全体アーキテクチャ

```mermaid
flowchart TD
    subgraph legend["凡例"]
        direction LR
        L1["🟥 Ken起動"]
        L2["🟨 自動起動"]
        L3["🟩 既存構造"]
        L4["⬜ 新規追加"]
    end

    Ken["👤 Ken (CEO)<br/>発言でフェーズ決定"]
    Secretary["📋 秘書<br/>トリガーワード検知 → 会議召喚 or CEO転送"]
    CEO_Agent["🏛 CEO Agent (振り分け)<br/>Vision整合 → 担当ルーティング"]

    Ken --> Secretary
    Secretary --> CEO_Agent

    subgraph meetings[".company/meetings/ [新規追加]"]
        direction TB
        M1["🟥 発散会議<br/>「発散会議を開いて」→ Ken起動<br/>ビジョン番人・アナロジー職人・ユーザー代弁者<br/>→ secretary/notes/ に保存"]
        M2["🟥 選択会議<br/>「選択会議を開いて」→ Ken起動<br/>破壊チーム・5-Whys・数字ハンター・シンセサイザー<br/>→ ceo/pending_decisions.md に保存"]
        M3["⚡ 要件定義会議（ブリッジ）<br/>「要件定義会議を開いて」or 選択確定後 自動<br/>破壊チーム・PM Agent・技術Agent・シンセサイザー<br/>→ projects/PRJ-xxx/01_requirements/ に直接出力"]
        M1 --> M2 --> M3
    end

    subgraph vision_meeting["Vision整合会議（自動）"]
        VM["CEO が Vision抵触を検知した時点で自動召喚<br/>参加: CEO + 秘書 + 発案者ロール<br/>→ ceo/pending_decisions.md に記録"]
    end

    Secretary --> meetings
    CEO_Agent -.-> vision_meeting

    subgraph dev[".company/dev/projects/PRJ-xxx/"]
        direction TB
        D1["01 要件定義 (PM)<br/>requirements.md"]
        D2["02 基本設計 (Architect)<br/>basic_design.md"]
        D3["03 詳細設計 (Sr.Eng)<br/>detailed_design.md"]
        D4["04 実装 (Engineer)<br/>src/ + self_review.md"]
        D5["05 受入テスト (QA)<br/>test_result.md"]
        D6["06 リリース (DevOps)<br/>release_record.md"]

        D1 --> A1["🟨 監査<br/>自動起動"]
        D2 --> A2["🟨 監査<br/>自動起動"]
        D3 --> A3["🟨 監査<br/>自動起動"]
        D4 --> A4["🟨 監査<br/>自動起動"]
        D5 --> A5["🟨 監査<br/>自動起動"]
        D6 --> A6["🟨 監査<br/>自動起動"]

        D1 --> D2 --> D3 --> D4 --> D5 --> D6
    end

    subgraph remand["差し戻し会議"]
        R["監査FAIL時 自動起動<br/>担当Agent + 監査Agent + 5-Whys<br/>→ 該当工程に戻る"]
    end

    M3 -->|"ブリッジ出力"| D1
    A1 & A2 & A3 & A4 & A5 & A6 -->|"FAIL"| remand
    remand -->|"差し戻し"| dev
```

---

## 会議の機能フロー — 3つの会議と内部エージェント

```mermaid
flowchart TD
    subgraph ideation["🎯 発散会議 (01_ideation)"]
        direction TB
        I_pre["📋 前提確認<br/>vision/ を参照<br/>mission・values・principles を確認"]
        I_pre --> I1["🏛 ビジョン番人<br/>ビジョンから外れたら即指摘<br/>それ以外は黙る<br/>リサーチ: なし"]
        I_pre --> I2["🖼 アナロジー職人<br/>他業界類似事例を提示<br/>最低3つ出す<br/>リサーチ: Shallowのみ"]
        I_pre --> I3["👤 ユーザー代弁者<br/>行動ベースで具体化<br/>否定しない<br/>リサーチ: Shallowのみ"]
        I_pre --> I4["📋 秘書（進行・記録）<br/>番号付きリストで随時整理<br/>評価しない<br/>リサーチ: なし"]
        I1 & I2 & I3 & I4 --> I_end["Ken:「以上」で終了"]
        I_end --> I_out["📄 出力<br/>meetings/01_ideation/logs/<br/>YYYY-MM-DD.md<br/>・アイデアリスト（番号付き）<br/>・ビジョン整合メモ"]
    end

    I_out -->|"Kenが考える時間（自動遷移しない）"| S_start

    subgraph selection["🎯 選択会議 (02_selection)"]
        direction TB
        S_start["📂 前提確認<br/>01_ideation/logs/ 最新を読む<br/>なければKenに確認して止まる"]
        S_start -->|"固定順で進む"| S1["① 💀 破壊チーム<br/>「1年後の失敗理由」を1つ<br/>肯定一切なし<br/>リサーチ: Shallowのみ"]
        S1 --> S2["② 📊 数字ハンター<br/>市場規模・競合・収益の概算<br/>数字のない主張を通さない<br/>リサーチ: Shallow/Deep可"]
        S2 --> S3["③ 🔮 5-Whys<br/>Kenの「やりたい」を5回掘る<br/>Kenが答えるまで次に進まない<br/>リサーチ: なし"]
        S3 -->|"Ken 判断"| S4["④ 📦 シンセサイザー（最後に一度）<br/>やる / やらない / 保留<br/>理由3行以内<br/>捨てたアイデアと理由<br/>リサーチ: なし"]
        S4 --> S_out["📄 出力<br/>meetings/02_selection/logs/<br/>YYYY-MM-DD.md<br/>ceo/pending_decisions.md<br/>audit/decisions.log"]
    end

    S_out -->|"Kenが寝かせる時間（自動遷移しない）"| R_start

    subgraph requirements["⚡ 要件定義会議 (03_requirements)"]
        direction TB
        R_start["📂 前提確認<br/>02_selection/logs/ を読む<br/>PRJ-ID採番・meta.json初期化"]

        subgraph phaseA["PHASE A — 検証（Kenが「進む」と言うまで留まる）"]
            RA1["💀 破壊チーム<br/>「3ヶ月後の失敗理由」を先出し<br/>Kenが納得するまでBに入らない"]
            RA2["👤 ユーザー代弁者<br/>「本当に使うか」を問い続ける"]
        end

        R_start --> phaseA
        phaseA -->|"Ken:「フェーズBに進む」"| phaseB

        subgraph phaseB["PHASE B — 要件定義"]
            RB1["📋 PM Agent<br/>ユーザーストーリー<br/>受け入れ条件<br/>スコープ3列(IN/v2/やらない)<br/>リサーチ: Shallowのみ"]
            RB2["⚙ 技術 Agent<br/>PMの要件に即リアルタイム反応<br/>工数フラグ・スコープ調整<br/>リサーチ: Shallow/Deep可"]
            RB3["🔮 5-Whys<br/>「とりあえず」が出たら掘る"]
            RB4["📦 シンセサイザー<br/>requirements.md 出力<br/>未決事項リスト必須"]
        end

        phaseB --> R_bridge["⚡ ブリッジ出力<br/>dev/01_requirements/<br/>requirements.md に直接書き込む<br/>→ 開発フローの正式インプット"]
        R_bridge --> R_log["📄 ログ出力<br/>03_requirements/logs/<br/>YYYY-MM-DD.md<br/>meta.json: dev_phase pending維持"]
    end

    R_log -->|"「開発を始めて」まで待機"| DevStart["開発フェーズへ"]
```

---

## リサーチフロー — Shallow / Deep 判断と実行

```mermaid
flowchart TD
    Start["リサーチが必要な状況が発生"]

    Start --> Q1{"① 誰がトリガーか"}
    Q1 -->|"エージェント自身の判断"| Shallow_Confirm["🟢 Shallow 確定"]
    Q1 -->|"Kenの発言"| Q2{"② トリガーワード<br/>Kenの発言を解析"}

    Q2 -->|"含まれない"| Shallow["🟢 Shallow"]
    Q2 -->|"含まれる"| Deep["🟡 Deep"]

    subgraph trigger_words["🔴 Deep トリガーワード一覧"]
        TW1["「詳しく調べて」"]
        TW2["「深く調べて」"]
        TW3["「たくさん調べて」"]
        TW4["「しっかり調べて」"]
        TW5["「網羅的に調べて」"]
        TW6["「徹底的に調べて」"]
    end

    subgraph shallow_detail["🟢 Shallow リサーチ"]
        S1["ソース数: 1〜3件"]
        S2["ドキュメント: 作らない"]
        S3["結果の返し方: 会議の流れにそのまま差し込む"]
        S4["meta.json記録: しない"]
        S5["会議への影響: 止まらず続行"]
        S6["後から参照: できない"]
    end

    subgraph deep_detail["🟡 Deep リサーチ"]
        D1["開始前: 「深く調べます」と宣言"]
        D2["ソース数: 5件以上"]
        D3["ドキュメント: 作る"]
        D4["保存先: meetings/research/<br/>YYYY-MM-DD_テーマ_deep.md"]
        D5["meta.json記録: research_log に追記"]
        D6["完了後: 「保存しました 確認しますか？」"]
        subgraph deep_doc["Deepドキュメント構成"]
            DD1["エグゼクティブサマリ（3行以内）"]
            DD2["詳細分析（セクション分割）"]
            DD3["比較表（該当する場合）"]
            DD4["結論と示唆"]
            DD5["ソース一覧"]
        end
    end

    subgraph permissions["エージェント別リサーチ権限"]
        direction TB
        P_none["🏛 ビジョン番人: なし<br/>📋 秘書: なし<br/>🔮 5-Whys: なし<br/>📦 シンセサイザー: なし<br/>🚀 DevOps(06): なし"]
        P_shallow["🖼 アナロジー職人: Shallowのみ<br/>👤 ユーザー代弁者: Shallowのみ<br/>💀 破壊チーム: Shallowのみ<br/>📋 PM Agent: Shallowのみ<br/>💻 Engineer(04): Shallowのみ<br/>✅ QA Lead(05): Shallowのみ"]
        P_deep["📊 数字ハンター: Shallow/Deep可<br/>⚙ 技術Agent: Shallow/Deep可<br/>🏗 Architect(02): Shallow/Deep可<br/>🔧 Sr.Eng(03): Shallow/Deep可"]
    end

    Shallow --> shallow_detail
    Deep --> deep_detail
```

> ★ エージェント側から Deep を起動することはできない

---

## 秘書が検知するトリガーワード一覧

| Ken発言 | 起動タイプ | 参加エージェント → 出力先 |
|---|---|---|
| 「発散会議を開いて」 | Ken起動 | ビジョン番人・アナロジー・ユーザー代弁者 → secretary/notes/ |
| 「選択会議を開いて」 | Ken起動 | 破壊・5-Whys・数字ハンター・まとめ役 → ceo/pending_decisions.md |
| 「要件定義会議を開いて」 | Ken起動 or 選択後自動 | 破壊・PM Agent・技術Agent・まとめ役 → 01_requirements/ |
| 監査FAIL検知（自動） | システム自動 | 担当Agent + 監査 + 5-Whys → 該当工程に差し戻し |
| Vision抵触検知（自動） | システム自動 | CEO + 秘書 + 発案者ロール → ceo/pending_decisions.md |
| 「議題Xで会議を開いて」 | Ken起動（カスタム） | 秘書がアジェンダ解析 → 最適エージェント自動選定 |

> ★ 要件定義会議の出力が dev/projects/ の 01_requirements/ に直接書き込まれることで、既存ウォーターフォールフローに自然に接続される

---

## 使い方

1. `product-template/` をコピーして `product-xxx/` を作成
2. `PRODUCT_WORKSPACE_SETUP.md` の手順に従ってセットアップ
3. 「発散会議を開いて」で壁打ち開始

## ステータス

🚧 構造確定済み・プロンプト本文は未実装
