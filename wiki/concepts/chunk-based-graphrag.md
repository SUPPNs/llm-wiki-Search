---
title: "文書チャンクに基づく GraphRAG"
type: concept
aliases: ["Chunk-based GraphRAG"]
tags: [graphrag, chunk, discourse-relation, 5w1h]
last_updated: 2026-05-25
status: active
---

# 文書チャンクに基づく GraphRAG

## 概要

文書チャンクの 5W1H 情報とチャンク間の談話関係を知識グラフに保持し、ベクトル
検索で得たチャンクから関連チャンクを辿る GraphRAG 方式である。

## 事実

- 知識グラフは各チャンクの 5W1H 情報とチャンク間の談話関係を含む。2.1 節、p.1。
- 検索では、質問埋め込みによる類似検索で得たチャンクからグラフを検索し、談話関係を辿って関連チャンクを取得する。また、質問から抽出した 5W1H によるグラフ検索も行う。2.1 節、p.1。
- QASPER 比較実験で、抽出型 QA の F 値は `0.25`、要約型 QA の F 値は `0.49`、Yes/No 型の完全一致率は `0.54` であった。表2-4、p.3。
- 同実験において Yes/No 型の完全一致率は比較三手法中で最も高かった。表4、p.3。
- 著者らは、広い文脈を要する質問で不完全回答が生成される傾向と、談話関係抽出が不十分な場合を報告している。3.6 節、p.4。

## 解釈

- 根拠となる局所記述を辿る設計は、回答根拠が具体的な Yes/No 判定や詳細抽出に適合しやすい可能性がある。
- 談話関係の欠落が長距離の文脈統合を阻害するなら、関係抽出の検証精度がこの方式の上限を決める。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 関連ページ

- [[concepts/graphrag|GraphRAG]]
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 2.1 節、表2-4、3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
