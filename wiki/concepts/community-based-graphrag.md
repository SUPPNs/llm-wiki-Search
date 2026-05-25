---
title: "コミュニティ分類に基づく GraphRAG"
type: concept
aliases: ["Community-based GraphRAG"]
tags: [graphrag, community, summarization, knowledge-graph]
last_updated: 2026-05-25
status: active
---

# コミュニティ分類に基づく GraphRAG

## 概要

文書から抽出したノードとエッジをコミュニティに分類し、その要約情報を介して
回答文脈を組み立てる GraphRAG 方式である。

## 事実

- 対象論文の説明では、文書を分割してノードとエッジを特定し、ノード要約を付与し、知識グラフをコミュニティに分類してコミュニティ要約を生成する。2.2 節、pp.1-2。
- QASPER 比較実験で、抽出型 QA の F 値は `0.31`、要約型 QA の F 値は `0.52`、Yes/No 型の完全一致率は `0.49` であった。表2-4、p.3。
- 同実験において抽出型と要約型の評価値は比較三手法中で最も高かった。3.4.1 節、p.3。
- 著者らは、この方式では文書全体のトピックを考慮し、関連概念の集合として情報を整理するため、広範な知識を踏まえた回答生成が可能であったと考察する。3.5.2 節、p.4。
- 著者らは、具体的な手法名や定量的結果の正確な取得が難しい場合と、一部情報が省略されるケースを報告している。3.6 節、p.4。

## 解釈

- コミュニティ要約は複数箇所をまとめる問いに向く可能性があるが、数値や固有表現の証拠保存には別の索引が必要になりうる。
- 要約に基づく検索結果から原文証拠への逆引きが可能であれば、包括性と忠実性の両立を評価しやすくなる。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 関連ページ

- [[concepts/graphrag|GraphRAG]]
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]]
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 2.2 節、表2-4、3.5-3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
