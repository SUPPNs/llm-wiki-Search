---
title: "GraphRAG"
type: concept
aliases: ["Graph Retrieval-Augmented Generation"]
tags: [rag, knowledge-graph, retrieval, neo4j, langchain]
last_updated: 2026-05-26
status: active
---

# GraphRAG

## 概要

GraphRAG は、テキストから抽出したエンティティと関係をグラフとして検索可能にし、
通常のテキスト検索で得るコンテキストを補う RAG の設計群である。本 wiki では、
学術論文による評価結果と、Neo4j・LangChain を用いた実装解説を区別して整理する。

## 事実

- ksonoda (2024) は、従来 RAG を、質問に類似するテキストをベクトル検索し、取得したコンテキストを LLM に与える仕組みとして説明する。
- 同記事は、GraphRAG を、文章中の関係をグラフデータモデルで表現し、グラフ検索をベクトル検索・全文検索に加えてコンテキスト取得へ利用する実装として説明する。
- 同記事のサンプルでは、Neo4j がノード・関係・元文書を格納し、グラフ検索、全文インデックス、`Neo4jVector` によるハイブリッド検索の基盤として用いられる。
- 同記事は、LangChain の `LLMGraphTransformer.convert_to_graph_documents()` でテキストからノードと関係を抽出し、`Neo4jGraph.add_graph_documents()` で Neo4j にロードするコードを示す。
- 同記事では、質問からエンティティを抽出し、Cypher を用いた `structured_retriever` と `vector_index.similarity_search()` の結果を結合して LLM に渡すパイプラインを実装している。
- 同記事の執筆時点では、`LLMGraphTransformer` は `langchain_experimental` の機能であり、グラフ検索用 retriever はサンプル内で定義していると説明される。
- Anaguchi et al. (2025) は QASPER で通常の RAG と 2 種の GraphRAG を比較し、抽出型・要約型ではコミュニティ分類方式、Yes/No 型ではチャンク方式が最良であると報告する。
- Anaguchi et al. (2025) は、LLM を用いた知識グラフ構築に情報欠損が生じうることを報告する。

## グラフ検索の役割

| 観点 | 整理 |
| --- | --- |
| 対象 | テキストから抽出したエンティティをノード、関係をエッジとして扱う |
| 追加する検索能力 | ベクトル類似度だけでは取得しにくい、ノード間関係や複数チャンクにまたがる文脈を辿る |
| サンプルでの処理 | 質問から抽出したエンティティを起点に、Cypher で近傍の関係を取得する |
| 統合方法 | グラフ検索結果を、全文検索・ベクトル検索のコンテキストとともに LLM 入力へ加える |

## Neo4j と LangChain の実装要素

| 要素 | 記事中の役割 |
| --- | --- |
| `ChatOpenAI` | グラフ抽出および最終回答生成に使用する LLM |
| `LLMGraphTransformer` | チャンクテキストからノードと関係を含む `GraphDocument` を生成 |
| `Neo4jGraph` | 生成したグラフ文書の Neo4j への保存と Cypher クエリ実行 |
| `Neo4jVector.from_existing_graph()` | Neo4j 上の文書ノードに埋め込みを持たせ、キーワード検索とベクトル検索を併用 |
| 全文インデックス | エンティティ候補を多少の表記差を許容して検索するために作成 |
| `structured_retriever` | 抽出エンティティの近傍関係を Cypher で取得する記事内の独自関数 |
| `RunnableParallel` | 検索結果と質問をプロンプトへ組み込み、回答チェーンを構成 |

## 解釈

- GraphRAG は単一の固定方式ではなく、グラフ抽出、検索経路、結果統合、根拠保持の選択から成る設計空間として扱うのが適切である。
- Neo4j を用いたサンプルは実装の入口を示すが、GraphRAG が従来 RAG より優れる条件を実証する比較評価ではない。
- 記事が示すグラフ・全文・ベクトル検索の統合と、論文が示す質問型別の性能差を接続すると、検索経路の選択と証拠忠実性を同時に評価する研究が考えられる。
- [[concepts/bipartite-graph-retrieval|BGR]] はレビュー行動の二部グラフを探索するため、文書由来の知識関係を抽出する本ページの GraphRAG とは区別して比較する必要がある。

## 未解決課題

- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - Neo4j・LangChain 型実装を公平に評価するための問い。
- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 関連ページ

- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]]
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]
- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]]
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 矛盾

- ksonoda (2024) は GraphRAG の効果を主として実装上の期待とデモで説明する一方、Anaguchi et al. (2025) は GraphRAG の優位性が質問型により変わり、知識グラフ構築の欠損も生じうると報告する。効果を一般化せず、実験で確認する必要がある。
- BGR はレビュー行動の二部グラフを探索する方式であり、文書から知識グラフを構築する GraphRAG の同義語ではない。

## 情報源

- `raw/blogs/graphrag_qiita_ksonoda_2024.md` - RAG と GraphRAG の説明、Neo4j・LangChain サンプル、記事記載の実装上の留保。
- `raw/papers/graphrag-comparison.pdf` - GraphRAG の比較評価と知識グラフ欠損に関する報告。
- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - BGR との概念境界の確認。
- [Qiita: GraphRAGをわかりやすく解説](https://qiita.com/ksonoda/items/98a6607f31d0bbb237ef) - 保存済みブログ原資料の公開元。
