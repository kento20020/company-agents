# Company Agents

Claude Code で動く仮想会社テンプレート。フォルダをコピーして `claude` を起動するだけ。

## クイックスタート

```bash
# テンプレートをコピー
cp -r template/ my-product/
cd my-product/

# Claude Code を起動
claude
```

秘書が立ち上がります。「発散会議を開いて」で開始。

## 仕組み

- **1フォルダ = 1プロダクト**（完全自己完結、外部参照なし）
- **秘書が唯一の窓口**（話しかけるだけでOK）
- **会議**: 複数エージェントがアイデア出し・検証・要件定義
- **開発**: 秘書が1工程ずつロールを演じて実装・監査

詳細は [template/README.md](template/README.md) を参照。

## ライセンス

MIT
