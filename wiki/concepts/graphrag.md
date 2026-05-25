---
title: "GraphRAG"
type: concept
aliases: ["Graph Retrieval-Augmented Generation"]
tags: [rag, knowledge-graph, retrieval]
last_updated: 2026-05-25
status: active
---

# GraphRAG

## 概要

GraphRAG は、文書から構築した知識グラフを検索・回答生成に利用する RAG の設計群
として、本 wiki では扱う。グラフによる RAG 拡張には、知識グラフではなく
レビュー行動の二部グラフを探索する [[concepts/bipartite-graph-retrieval|BGR]] も
あり、同一視せず比較する。

## 事実

- 対象論文は、GraphRAG が知識グラフを活用し、情報の構造化とエンティティ間関係の把握を期待される手法であると述べる。概要。
- 対象論文は、[[concepts/chunk-based-graphrag|文書チャンク方式]] と [[concepts/community-based-graphrag|コミュニティ分類方式]] を比較する。3.1 節。
- 両 GraphRAG 方式では、ノード抽出または知識グラフ構築に LLM を使用するため、情報が欠損した不完全なグラフが構築される可能性があると報告されている。3.6 節、p.4。
- Arakaki・Fushimi (2026) は、KG を用いる RAG 推薦研究との違いとして、レビュー文と二部グラフ検索に特化する点を述べる。2.3-2.5 節、pp.5-6。

## 解釈

- GraphRAG は単一の固定手法というより、どの単位をノード化し、どの関係や要約を検索可能にするかという設計空間として整理すると研究テーマを見つけやすい。
- 性能評価と同時に、生成された知識グラフが原文のどの証拠を保持・喪失したかを評価対象にする価値がある。
- [[concepts/bipartite-graph-retrieval|BGR]] の行動関係証拠と GraphRAG の内容証拠は、推薦理由生成において補完的に評価できる可能性がある。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 関連ページ

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]]
- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]]
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 矛盾

- BGR はレビュー行動の二部グラフを探索する方式であり、文書から知識グラフを構築する GraphRAG の同義語ではない。比較時にはグラフの対象を明記する。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 概要、3.1 節、3.6 節。
- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - 2.3-2.5 節、グラフ型推薦との差異。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
