---
title: "二部グラフ検索（BGR）"
type: concept
aliases: ["Bipartite Graph Retrieval", "BGR"]
tags: [rag, bipartite-graph, recommendation, reviews]
last_updated: 2026-05-25
status: active
---

# 二部グラフ検索（BGR）

## 概要

BGR は、レビューアーとアイテムを二部グラフとして扱い、ベクトル検索で得た
レビューから隣接するレビューやアイテムを探索して、推薦理由に追加の観点を与える
検索方式である。

## 事実

- Arakaki・Fushimi (2026) は、レビューアー集合 `U`、アイテム集合 `I`、レビューによる関係 `R` から二部グラフ `B = (U, I, R)` を定義する。3 節、pp.7-8。
- 同論文の BGR は、VR で取得したレビューの対象アイテムに対する他レビューと、そのレビューアーが投稿した他アイテムのレビューを辿る。英文概要、3 節。
- 同論文は、BGR によって得たレビューを LLM に渡し、最終的に推薦アイテムと推薦理由を生成する。3 節、pp.7-8。
- 同論文では、VR+BGR の推薦理由が VR のみよりアンケート上のわかりやすさ、納得感、参考度で多く選択された。表4-6、p.14。

## 解釈

- BGR は関係を LLM が抽出する知識グラフではなく、レビュー履歴として観測済みのリンクを探索するため、構築時の関係幻覚とは異なるリスクを持つ。
- ただし、レビュー数や接続性の高い利用者・商品が選ばれやすくなるなら、説得力と人気偏重のトレードオフが生じうる。

## 未解決課題

- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 関連ページ

- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]]
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]]
- [[concepts/graphrag|GraphRAG]]
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]]
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 矛盾

- BGR は本論文中で知識グラフ型推薦とは区別されているため、知識グラフを構築する GraphRAG の同義語として扱わない。

## 情報源

- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - 英文概要、3 節、表4-6。
- [CiNii Research record](https://cir.nii.ac.jp/crid/1390587682741687808) - 英文概要と書誌情報。
