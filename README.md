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

## 3つの会議

1. **発散会議** (01_ideation) — アイデア量産。評価禁止。
2. **選択会議** (02_selection) — やるものをKenが決断。
3. **要件定義会議** (03_requirements) — 要件に落とし込む。

## 6つの開発工程

1. 要件定義 (PM)
2. 基本設計 (Architect)
3. 詳細設計 (Senior Engineer)
4. 実装 (Engineer)
5. 受入テスト (QA Lead)
6. リリース (DevOps)

## 使い方

1. `product-template/` をコピーして `product-xxx/` を作成
2. `PRODUCT_WORKSPACE_SETUP.md` の手順に従ってセットアップ
3. 「発散会議を開いて」で壁打ち開始

## ステータス

🚧 構造確定済み・プロンプト本文は未実装
