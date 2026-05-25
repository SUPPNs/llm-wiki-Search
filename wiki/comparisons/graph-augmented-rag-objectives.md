---
title: "グラフ拡張 RAG の目的と評価の比較"
type: comparison
tags: [rag, graphrag, bipartite-graph, comparison, evaluation]
last_updated: 2026-05-25
status: active
---

# グラフ拡張 RAG の目的と評価の比較

## 概要

Anaguchi et al. (2025) の知識グラフ型 GraphRAG 比較と、Arakaki・Fushimi
(2026) の二部グラフ検索による推薦理由生成を対照し、RAG にグラフ探索を加える
目的、保持する証拠、評価方法の違いを整理する。

## 比較範囲

- 比較対象: [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較分析]], [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索によるコスメ推薦理由]]
- 関連する概念: [[concepts/graphrag|GraphRAG]], [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]], [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]]
- 関連する研究テーマ候補: [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 事実

| 比較軸 | 知識グラフ型 GraphRAG 比較 | 二部グラフ検索による RAG 推薦 | 根拠 |
| --- | --- | --- | --- |
| 対象課題 | 学術論文に対する情報探索型 QA | 化粧品の対話型推薦と推薦理由生成 | 各論文の概要・実験節 |
| グラフの対象 | 文書チャンクの 5W1H・談話関係、または知識グラフのコミュニティ要約 | レビューアーとアイテムを結ぶレビュー関係の二部グラフ | Anaguchi 2.1-2.2 節; Arakaki 3 節 |
| ベクトル検索との関係 | 通常の RAG と2種の GraphRAG を比較 | VR に BGR を追加し、VR のみと比較 | Anaguchi 表2-4; Arakaki 5-6 節 |
| データ | QASPER の質問型別各100事例 | @cosme のレビュー群と317人アンケート | Anaguchi 3.2 節; Arakaki 4, 6 節 |
| 評価目的 | 抽出・要約・Yes/No 回答性能 | 推薦理由のわかりやすさ、納得感、参考度、満足度 | Anaguchi 表2-4; Arakaki 表3-8 |
| 報告された結果 | 抽出・要約ではコミュニティ方式、Yes/No ではチャンク方式が最良 | VR+BGR が VR のみより3例すべての説明評価で多く選択された | Anaguchi 表2-4; Arakaki 表4-6 |
| 報告された課題 | KG 構築時の情報欠損、具体値取得や広域文脈の不足 | 専門知識の高い層での評価低下の可能性、KG 検索による網羅性向上の必要 | Anaguchi 3.6 節; Arakaki 6.5, 7 節 |

## 解釈

- 両論文はベクトル検索のみでは不足する文脈をグラフから補うが、前者は文書意味構造、後者はレビュー行動関係を利用しており、補う証拠の種類が異なる。
- BGR で利用者・アイテム周辺の経験的証拠を集め、知識グラフ型検索で成分・機能・数値などの内容証拠を回収する統合設計は、推薦理由の納得感と根拠忠実性の両方を検証できる研究方向になる。
- 評価対象と指標が異なるため、両論文の数値を並べて方式の優劣を判断することはできない。

## 未解決課題

- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 関連ページ

- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]]
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]]

## 矛盾

- 「GraphRAG」の対象は Anaguchi et al. では文書由来の知識グラフであり、Arakaki・Fushimi の BGR はレビュー行動の二部グラフである。名称上近くても同一のグラフ構築法として比較しない。
- 評価結果に直接の矛盾は確認できない。QA 品質と推薦理由の説得力という異なる目的の結果であるためである。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - Anaguchi et al. (2025) の方式・評価・結果。
- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - Arakaki・Fushimi (2026) の方式・評価・結果。
- [CiNii Research record for BGR paper](https://cir.nii.ac.jp/crid/1390587682741687808) - BGR 論文の書誌と概要。
