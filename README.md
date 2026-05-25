# Research LLM Wiki

論文研究のテーマ探索のために、原資料と LLM による整理結果を分離して蓄積する
Markdown 知識ベースです。Obsidian でそのまま閲覧できるよう、知識ページ間の
関係は `[[Wikiリンク]]` で結びます。

このリポジトリは Karpathy の LLM Wiki の運用思想と
[Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) の構造を参考に、
研究調査向けの四つの中核層で運用します。

- `raw/`: immutable source storage。論文、ブログ、ドキュメント、GitHub 由来資料、
  Web ページ、動画メモなどの原資料を変更せず保持する不変層
- `wiki/`: LLM が原資料を根拠に生成・更新する知識層
- `schema/`: 命名規則、テンプレート、ingest・query・lint 手順、エージェント規約
- `logs/`: ingest、query、lint の実行履歴を追記する記録層

## Directory Structure

```text
research-llm-wiki/
├── raw/
│   ├── papers/
│   ├── blogs/
│   ├── docs/
│   ├── github/
│   ├── web/
│   ├── youtube/
│   └── notes/
├── wiki/
│   ├── concepts/
│   ├── papers/
│   ├── architectures/
│   ├── themes/
│   ├── questions/
│   ├── comparisons/
│   └── index.md
├── schema/
│   ├── AGENTS.md
│   ├── naming.md
│   ├── ingest.md
│   ├── query.md
│   ├── lint.md
│   └── templates/
├── logs/
│   ├── ingest.md
│   ├── query.md
│   └── lint.md
├── scripts/
└── README.md
```

## What Goes Where

| 場所 | 内容 | 編集方針 |
| --- | --- | --- |
| `raw/` | PDF、ブログ、仕様、Web・動画・GitHub 由来資料、一次メモ | 原資料として変更しない |
| `wiki/papers/` | 論文・資料単位の読解と位置付け | LLM が更新可 |
| `wiki/concepts/` | 手法、概念、評価軸、データセット | LLM が更新可 |
| `wiki/architectures/` | システム構成、機能分解、運用フロー | LLM が更新可 |
| `wiki/themes/` | 研究テーマ候補と検証計画 | LLM が更新可 |
| `wiki/questions/` | 未解決課題、検証すべき問い | LLM が更新可 |
| `wiki/comparisons/` | 手法・論文・課題設定の比較表 | LLM が更新可 |
| `schema/` | 運用規則とページテンプレート | 運用設計の変更時に更新 |
| `logs/ingest.md` | 原資料の取り込み履歴 | 追記専用 |
| `logs/query.md` | Query 成果を wiki に保存した履歴 | 追記専用 |
| `logs/lint.md` | Wiki 点検の履歴 | 追記専用 |

## Getting Started

1. 原資料を適切な `raw/` サブフォルダに保存します。保存後は上書き、移動、
   改名、削除を行いません。
2. LLM はまず [schema/AGENTS.md](schema/AGENTS.md)、
   [schema/ingest.md](schema/ingest.md)、[wiki/index.md](wiki/index.md) を読みます。
3. 原資料を読み、[schema/templates/](schema/templates/) の雛形に従って、
   既存ページがあれば更新し、なければ `wiki/` にページを作成します。
4. ページでは事実、解釈、未解決課題を分離し、`情報源` から原資料を追跡可能にします。
5. 重要な取り込みは [logs/ingest.md](logs/ingest.md) に追記し、
   [schema/lint.md](schema/lint.md) に従って点検します。

## 運用サイクル

- **Ingest**: [schema/ingest.md](schema/ingest.md) に従って原資料を wiki 化し、
  [logs/ingest.md](logs/ingest.md) に記録します。
- **Query**: [schema/query.md](schema/query.md) に従い、有用な比較、接続、研究テーマ候補、
  未解決課題を会話だけに残さず wiki に保存し、[logs/query.md](logs/query.md) に記録します。
- **Lint**: [schema/lint.md](schema/lint.md) に従い、索引漏れ、リンク切れ、孤立ページ、
  セクション不足、情報源や矛盾の記録不足を点検し、[logs/lint.md](logs/lint.md) に記録します。

## Page Templates

- [論文ページ](schema/templates/paper.md)
- [概念ページ](schema/templates/concept.md)
- [アーキテクチャページ](schema/templates/architecture.md)
- [研究テーマ候補ページ](schema/templates/theme.md)
- [未解決課題ページ](schema/templates/question.md)
- [比較ページ](schema/templates/comparison.md)

## Conventions

- ファイル名とリンク規則は [schema/naming.md](schema/naming.md) に定めます。
- 内部参照は `[[concepts/retrieval-augmented-generation|Retrieval-Augmented Generation]]`
  のような Obsidian 形式を使います。
- 出典同士が食い違う場合は結論を消さず、対象ページの `矛盾` に双方を記録します。
- 詳細な編集規約は [schema/AGENTS.md](schema/AGENTS.md) を優先します。

## References

- [Karpathy, LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
  - Query の成果を蓄積する wiki と定期 lint の運用思想
- [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) - raw、wiki、agent schema
  を分離し、Wiki リンクで知識を結ぶ実装の参照元
- [Pratiyush/llm-wiki Architecture](https://github.com/Pratiyush/llm-wiki/blob/master/docs/architecture.md)
  - レイヤー構造と LLM 管理の wiki の説明
