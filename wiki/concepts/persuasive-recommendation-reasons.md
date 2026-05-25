---
title: "推薦理由の説得力"
type: concept
aliases: ["Persuasiveness of Recommendation Reasons"]
tags: [recommendation, explanation, user-evaluation, rag]
last_updated: 2026-05-25
status: active
---

# 推薦理由の説得力

## 概要

推薦理由の説得力は、推薦されたアイテムをユーザーが理解し、納得し、意思決定の
参考にできるかという利用者向けの評価観点である。

## 事実

- Arakaki・Fushimi (2026) は、推薦理由のわかりやすさ、納得感、参考度を、VR と VR+BGR の比較アンケートで評価する。6 節、pp.11-14。
- 同論文では3例すべてで、VR+BGR の推薦理由が VR のみより、わかりやすさ、納得感、参考度について過半数に選ばれた。表4-6、p.14。
- 同論文では、回答者の `55.5%` が詳細な推薦理由があると安心できる旨を回答した。表7、p.15。
- 著者らは、コスメに非常に詳しい回答者では BGR を用いた推薦への評価が低い可能性を述べる。6.5 節、p.15。

## 解釈

- 説得力は、QA の正解率とは異なるユーザー評価指標であり、正しい根拠に基づくこと、レビュー偏りを増幅しないことを別途確認する必要がある。
- 高関与・高知識の利用者には、レビューの引用だけでなく成分・機能の構造化証拠が必要になる可能性がある。

## 未解決課題

- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 関連ページ

- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]]
- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]]
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]]
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - 6 節、表4-8。
- [CiNii Research record](https://cir.nii.ac.jp/crid/1390587682741687808) - 論文概要。
