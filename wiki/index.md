---
title: "Research LLM Wiki Index"
type: index
last_updated: 2026-05-25
---

# Research LLM Wiki Index

論文・概念・問いを横断して研究テーマ候補を探索するための入口です。ページを追加
または大きく更新したときは、この索引と [../logs/log.md](../logs/log.md) を更新します。

## 案内

| 分類 | 内容 |
| --- | --- |
| 論文 | 読解済み論文の要点、限界、接続 |
| 概念 | 手法、指標、問題設定、背景概念 |
| 研究テーマ候補 | 検討中の研究テーマ候補 |
| 未解決課題 | 未解決課題、要検証の問い |
| 比較 | 比較軸を揃えた表と差分 |

## 論文

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - 学術論文 QA で2種の GraphRAG と RAG を比較。
- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]] - レビュー二部グラフで化粧品推薦理由の説得力を高める RAG を評価。

## 概念

- [[concepts/graphrag|GraphRAG]] - 知識グラフを用いた RAG の設計空間。
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]] - 5W1H と談話関係を保持する方式。
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]] - コミュニティ要約を用いる方式。
- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - レビューアーとアイテムの関係を辿り推薦理由の証拠を増やす方式。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - 推薦理由のわかりやすさ・納得感・参考度を扱う評価観点。

## 研究テーマ候補

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 行動二部グラフと知識グラフで説得力と忠実性を両立するテーマ候補。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 求める証拠粒度に応じた検索ルーティングを問う。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - KG 構築時の根拠喪失を測定・抑制する問い。
- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]] - 説得力改善と根拠品質・偏りの同時評価を問う。
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]] - 経験証拠と内容証拠の検索統合を問う。

## 比較

- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - 構造と QASPER の性能差。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - QASPER QA と化粧品推薦におけるグラフ追加の役割を対照。

## テンプレート

- [論文テンプレート](templates/paper.md)
- [概念テンプレート](templates/concept.md)
- [研究テーマ候補テンプレート](templates/theme.md)
- [未解決課題テンプレート](templates/question.md)
- [比較テンプレート](templates/comparison.md)

## 作業メモ

### 事実

- `raw/` は原資料の保存場所であり、知識ページとは分離されている。
- 知識ページの初期カテゴリは Papers、Concepts、Themes、Questions、Comparisons
  である。
- Anaguchi et al. (2025) の GraphRAG 比較論文が最初の論文資料として取り込まれた。
- Arakaki・Fushimi (2026) は、レビュー二部グラフを追加した RAG 推薦を化粧品レビューと317人のアンケートで評価した。

### 解釈

- 研究テーマ候補を問いと比較表から明示的に接続すると、文献要約だけで終わらず、
  次の調査または実験へ進みやすくなる。
- 現在の資料からは、質問特性に応じた検索方式選択と知識グラフの証拠保持が
  有望な探索方向として浮かび上がる。
- レビュー行動グラフによる説得力と、知識グラフによる内容証拠の保持を組み合わせる
  研究テーマ候補が新たに生じる。

### 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]
- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- [../README.md](../README.md) - この wiki の目的と初期構成。
- `../raw/papers/graphrag-comparison.pdf` - 最初に取り込んだ論文原資料。参照しやすい英数字ファイル名に変更済み。
- `../raw/papers/bipartite-graph-rag-cosmetics.pdf` - 二部グラフ検索による RAG 推薦の原資料。
- [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) - `raw/` と `wiki/`
  を分離する設計の参照元。
