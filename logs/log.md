# Update Log

重要な wiki 更新の追記専用ログです。過去の項目を修正せず、訂正や追加説明も新しい
項目として追記します。

## 2026-05-25 | Initialize research wiki

- 研究テーマ探索向けの `raw/` と `wiki/` の構成を作成した。
- 編集規約を `AGENTS.md` に定義し、原資料の不変性、事実・解釈・未解決課題の
  分離、矛盾の保持、`[[Wikiリンク]]` の使用を定めた。
- 論文、概念、研究テーマ候補、未解決課題、比較のテンプレートを追加した。

## 2026-05-25 | Ingest GraphRAG comparison paper

- Updated: [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- Updated: [[concepts/graphrag|GraphRAG]], [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]], [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- Updated: [[comparisons/chunk-vs-community-graphrag|Chunk-based vs Community-based GraphRAG]]
- Updated: [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]], [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]
- Evidence: `raw/papers/文書のチャンクに基づく GraphRAG と コミュニティ分類に基づく GraphRAG の比較分析_Comparative Analysis of GraphRAG Based on Document Chunks and_GraphRAG Based on Community Selection of Knowledge Graph.pdf`
- Change: QASPER 上で回答型により優位な GraphRAG 方式が異なる結果と、知識グラフ構築時の情報欠損という報告課題を整理した。

## 2026-05-25 | 日本語見出しの編集規約とテンプレートを整備

- Updated: `AGENTS.md`, `wiki/templates/*.md`
- Change: Obsidian で読みやすい日本語の見出し・表示名を標準とし、ファイル名は英数字のまま維持する規約に更新した。

## 2026-05-25 | GraphRAG 原資料のファイル名変更を反映

- Updated: `wiki/index.md`, [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]], [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] および関連する概念・未解決課題ページ
- Evidence: `raw/papers/graphrag-comparison.pdf`
- Change: 原資料の内容を変えず、Obsidian と Markdown から参照しやすい英数字の短いファイル名へユーザーが変更したため、wiki の情報源パスを更新した。同時に更新対象ページの見出しを現行規約の日本語表記へ移行した。

## Entry Template

```markdown
## YYYY-MM-DD | <short update title>

- Updated: [[path/to/page|Page title]]
- Evidence: `raw/<category>/<source>`
- Change: <new knowledge, changed status, or recorded conflict>
```

## Sources

- [../README.md](../README.md) - リポジトリの目的と初期構成。
- [../AGENTS.md](../AGENTS.md) - ログ運用を含む編集規約。
