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
として、本資料では扱われている。取り込んだ論文は、グラフの組織化の違いが QA の
強みと弱みに結びつくことを比較している。

## 事実

- 対象論文は、GraphRAG が知識グラフを活用し、情報の構造化とエンティティ間関係の把握を期待される手法であると述べる。概要。
- 対象論文は、[[concepts/chunk-based-graphrag|文書チャンク方式]] と [[concepts/community-based-graphrag|コミュニティ分類方式]] を比較する。3.1 節。
- 両 GraphRAG 方式では、ノード抽出または知識グラフ構築に LLM を使用するため、情報が欠損した不完全なグラフが構築される可能性があると報告されている。3.6 節、p.4。

## 解釈

- GraphRAG は単一の固定手法というより、どの単位をノード化し、どの関係や要約を検索可能にするかという設計空間として整理すると研究テーマを見つけやすい。
- 性能評価と同時に、生成された知識グラフが原文のどの証拠を保持・喪失したかを評価対象にする価値がある。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 関連ページ

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 概要、3.1 節、3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
