# Ingest Log

原資料の取り込みと、それに伴う重要な wiki・schema 更新の追記専用ログです。
このファイルは旧 `logs/log.md` の履歴を継承しています。

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

## 2026-05-25 | 二部グラフ検索によるコスメ推薦論文を取り込み

- Updated: [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]]
- Updated: [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]], [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]], [[concepts/graphrag|GraphRAG]]
- Updated: [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]], [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]
- Updated: [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]], [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]
- Updated: [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]
- Evidence: `raw/papers/bipartite-graph-rag-cosmetics.pdf`, `raw/papers/graphrag-comparison.pdf`
- Change: BGR による推薦理由の評価結果を取り込み、既存の知識グラフ型 GraphRAG とグラフ対象・目的・評価指標の違いを比較した。両方式の数値は直接比較せず、行動証拠と内容証拠を統合する研究テーマ候補として整理した。

## 2026-05-25 | ハイブリッド Graph-RAG テーマページを UTF-8 日本語で修復

- Updated: [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]
- Evidence: [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|BGR 論文]], [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較論文]], [[comparisons/graph-augmented-rag-objectives|横断比較]]
- Change: Obsidian で読める UTF-8 の日本語 Markdown としてテーマページを再構成し、指定された `研究方法` セクションを含む構造へ揃えた。

## 2026-05-26 | index を研究領域別 synthesis entry point に強化

- Updated: `wiki/index.md`
- Evidence: [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較分析]], [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|BGR 論文]], [[comparisons/graph-augmented-rag-objectives|横断比較]], [[themes/hybrid-graph-rag-for-grounded-recommendations|ハイブリッド研究テーマ]]
- Change: 学術文書 QA、説明可能な推薦、行動証拠と内容証拠の統合の各研究領域で `concepts`・`comparisons`・`questions`・`themes` を束ね、全ページカタログと現時点の統合的な読みを同一 index に集約した。新しいレイヤーは追加していない。

## 2026-05-26 | Qiita の GraphRAG 実装解説を取り込み

- Updated: [[concepts/graphrag|GraphRAG]], [[comparisons/rag_vs_graphrag|従来 RAG と GraphRAG の比較]]
- Updated: [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]], [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]]
- Updated: `wiki/index.md`
- Evidence: `raw/blogs/graphrag_qiita_ksonoda_2024.md`, `raw/papers/graphrag-comparison.pdf`
- Change: GraphRAG の定義、従来 RAG との差、グラフ検索、Neo4j、LangChain の実装要素を整理した。解説記事のデモは論文の定量評価と区別し、実装と根拠忠実性の比較課題および研究テーマ候補に接続した。
- Structure: 指定された open problems の内容は、現行規約で定めた未解決課題の保存先 `wiki/questions/` に保存し、新しい `research_questions/` レイヤーは追加していない。

## 2026-05-26 | HITL ML システムアーキテクチャ論文を取り込み

- Updated: [[papers/shimbo-2024-hitl-ml-system-architecture|HITL ML の運用のためのシステムアーキテクチャの定義と実践]]
- Updated: [[concepts/human_in_the_loop_ml|Human in the Loop Machine Learning]], [[architectures/hitl_ml_architecture|HITL ML 運用・開発連携アーキテクチャ]]
- Updated: [[questions/hitl-ml-open-problems|HITL ML の運用データと再学習をどう評価するか]], [[themes/hitl-industrial-inspection-operation-evaluation|HITL 産業検査における運用データと再学習の評価]]
- Updated: `wiki/index.md`, `wiki/templates/architecture.md`, `README.md`, `AGENTS.md`
- Evidence: `raw/papers/human_in_the_loop_ml_system_architecture_2024.pdf`
- Change: 腐食診断を事例とする HITL の概念、運用・開発連携アーキテクチャ、人間の役割、報告された導入結果と再学習・ラベル・画像品質の課題を整理した。資料本文が `MLOps` を扱っていないことを明示し、MLOps との関係は未評価として残した。
- Structure: ユーザー指定の `wiki/architectures/` は新しいアーキテクチャページ種別として登録した。未解決課題は編集規約に従って `wiki/questions/hitl-ml-open-problems.md` に保存し、新しい `wiki/research_questions/` は追加していない。

## 2026-05-26 | Karpathy 型の中核構成へ運用層を整理

- Updated: `schema/AGENTS.md`, `schema/naming.md`, `schema/ingest.md`, `schema/query.md`, `schema/lint.md`, `schema/templates/*.md`
- Updated: `logs/ingest.md`, `logs/query.md`, `logs/lint.md`, `README.md`, `wiki/index.md`
- Change: `raw / wiki / schema / logs` を中核構成とし、旧 `AGENTS.md` と `wiki/templates/` を `schema/` へ、旧 `logs/log.md` を `logs/ingest.md` へ移動した。今後の query 成果と lint 結果の記録先を分離した。

## 2026-05-26 | HITL ML ingest 結果の根拠対応を点検

- Updated: [[papers/shimbo-2024-hitl-ml-system-architecture|HITL ML の運用のためのシステムアーキテクチャの定義と実践]], [[concepts/human_in_the_loop_ml|Human in the Loop Machine Learning]], `wiki/index.md`
- Evidence: `raw/papers/human_in_the_loop_ml_system_architecture_2024.pdf`
- Change: 4.2 節の個別指標（運転員の画像入力時間 `67%` 削減、保守員の作業量 `30%` 減少）と、結論の総括値（人間の作業負荷 `50%` 削減）は、本文で集計関係が示されていないことを `矛盾` と synthesis に明記した。既存の HITL 概念、アーキテクチャ、問い、テーマの構成と主要引用は原資料に沿うことを確認した。

## Entry Template

```markdown
## YYYY-MM-DD | <short update title>

- Updated: [[path/to/page|Page title]]
- Evidence: `raw/<category>/<source>`
- Change: <new knowledge, changed status, or recorded conflict>
```

## Sources

- [../README.md](../README.md) - リポジトリの目的と初期構成。
- [../schema/AGENTS.md](../schema/AGENTS.md) - ログ運用を含む編集規約。
