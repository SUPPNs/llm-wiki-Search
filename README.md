# Research LLM Wiki

論文研究のテーマ探索のために、原資料と LLM による整理結果を分離して蓄積する
Markdown 知識ベースです。Obsidian でそのまま閲覧できるよう、知識ページ間の
関係は `[[Wikiリンク]]` で結びます。

このリポジトリは [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki)
の考え方を研究調査向けに適用しています。中心となる設計は次の二層と編集規約です。

- `raw/`: immutable source storage。論文、ブログ、公開ドキュメント、GitHub 由来資料、
  Web ページ、動画メモ、手元の観察記録などの原資料を変更せず保持する不変層
- `wiki/`: LLM が原資料を根拠に整理し、関連付け、問いを育てる知識層
- `AGENTS.md`: LLM が一貫した方法で知識層を更新するための規約

## Directory Structure

```text
research-llm-wiki/
├── AGENTS.md
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
│   ├── themes/
│   ├── questions/
│   ├── comparisons/
│   ├── templates/
│   └── index.md
├── logs/
│   └── log.md
├── scripts/
└── README.md
```

## What Goes Where

| 場所 | 内容 | 編集方針 |
| --- | --- | --- |
| `raw/papers/` | PDF、書誌情報、論文本文の保存物 | 原資料として変更しない |
| `raw/blogs/` | ブログ記事や解説記事の保存物 | 原資料として変更しない |
| `raw/docs/` | 公式ドキュメント、技術仕様、公開資料の保存物 | 原資料として変更しない |
| `raw/github/` | README、issue、discussion、コード参照など GitHub 由来の保存物 | 原資料として変更しない |
| `raw/web/` | Web 記事や公開ドキュメントの保存物 | 原資料として変更しない |
| `raw/youtube/` | 動画の URL、字幕、視聴メモの保存物 | 原資料として変更しない |
| `raw/notes/` | 人間が取得した一次メモ、観察記録 | 原資料として変更しない |
| `wiki/papers/` | 論文単位の読解と位置付け | LLM が更新可 |
| `wiki/concepts/` | 手法、概念、評価軸、データセット | LLM が更新可 |
| `wiki/themes/` | 研究テーマ候補と検証計画 | LLM が更新可 |
| `wiki/questions/` | 未解決課題、検証すべき問い | LLM が更新可 |
| `wiki/comparisons/` | 手法・論文・課題設定の比較表 | LLM が更新可 |

## Getting Started

1. 収集した論文、ブログ、ドキュメント、GitHub 由来資料などの原資料を適切な
   `raw/` サブフォルダに保存します。`raw/` は immutable source storage とし、
   保存後は上書きせず、訂正が必要な場合は別資料として追加します。
2. LLM はまず [wiki/index.md](wiki/index.md) と `AGENTS.md` を読みます。
3. 原資料を読んで、該当する雛形を `wiki/templates/` から選び、既存ページが
   あれば更新し、なければページを作成します。
4. ページには、根拠のある事実、解釈、未解決課題を混在させずに記述し、
   `情報源` セクションから原資料を追跡可能にします。
5. テーマや問いを `[[Wikiリンク]]` で接続し、重要な更新を
   [logs/log.md](logs/log.md) に追記します。

## 運用サイクル

- 取り込み: 新しい原資料を読み、関連する知識ページと `wiki/index.md` を更新します。
- 問い合わせ: 有用な比較、分析、接続、研究テーマ候補、未解決課題が得られたら
  会話だけに残さず `wiki/` に保存します。
- 定期点検: ときどき wiki-lint を行い、索引漏れ、リンク切れ、孤立ページ、
  矛盾、情報源やページ化すべき概念の不足を確認します。

## Page Templates

- [論文ページ](wiki/templates/paper.md)
- [概念ページ](wiki/templates/concept.md)
- [研究テーマ候補ページ](wiki/templates/theme.md)
- [未解決課題ページ](wiki/templates/question.md)
- [比較ページ](wiki/templates/comparison.md)

## Conventions

- ページ名は内容が安定するまでは短く明確にし、ファイル名は `kebab-case.md` を
  基本とします。
- 内部参照は `[[concepts/retrieval-augmented-generation|Retrieval-Augmented Generation]]`
  のような Obsidian 形式を使います。
- 出典同士が食い違う場合は結論を消さず、対象ページの `矛盾` に双方を
  記録します。
- 詳細な編集規約は [AGENTS.md](AGENTS.md) を優先します。

## References

- [Karpathy, LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
  - Query の成果を蓄積する wiki と定期 lint の運用思想
- [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) - raw、wiki、agent schema
  を分離し、Wiki リンクで知識を結ぶ実装の参照元
- [Pratiyush/llm-wiki Architecture](https://github.com/Pratiyush/llm-wiki/blob/master/docs/architecture.md)
  - 三層構造と LLM 管理の wiki レイヤーの説明
