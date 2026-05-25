---
title: "二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上 - 対話型コスメ推薦システム"
type: paper
authors:
  - ARAKAKI Ryoma
  - FUSHIMI Takayasu
year: 2026
venue: "情報知識学会誌 Vol. 36, No. 1"
doi: "10.2964/jsik_2025_037"
tags: [rag, bipartite-graph, recommendation, cosmetics, persuasiveness]
last_updated: 2026-05-25
status: ingested
---

# 二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上 - 対話型コスメ推薦システム

## 概要

化粧品レビューを用いる対話型推薦において、ベクトル検索（VR）にレビューアーと
アイテムの二部グラフ検索（BGR）を加え、推薦理由の説得力を高める手法を評価した
論文である。アンケートでは、VR のみより VR+BGR の推薦理由がわかりやすさ、
納得感、参考度で選ばれる割合が高かった。

## 研究上の位置付け

- 問題設定: ユーザー属性とレビューを用いて、納得しやすい化粧品推薦理由を生成する。
- 主張される貢献: ベクトル類似性だけでなく、関連アイテムとレビューアーのレビュー履歴を二部グラフから追加取得する RAG 推薦を提案・評価する。
- 関連する比較: [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]]

## 事実

- 論文の英題は "Improving the Persuasiveness of RAG-based Recommendation Reasons using Bipartite Graph Exploration - Interactive cosmetics Recommendation System -" であり、著者は ARAKAKI Ryoma と FUSHIMI Takayasu、DOI は `10.2964/jsik_2025_037` である。CiNii 書誌情報、p.3。
- 提案方式は、ベクトル検索で得たレビューに加えて、その対象アイテムに対する他のレビュー、および取得レビューの投稿者が他アイテムに書いたレビューを取得する二部グラフ検索（BGR）を用いる。英文概要、pp.3-4。
- システムは LLM インターフェースに `gemma-2-27b-it.f16.Q8_0.gguf`、レビューの埋め込みに `intfloat/multilingual-e5-large` を用いる。3 節、pp.6-7。
- データは @cosme 由来で、スキンケア等の3カテゴリに含まれる 6,904 アイテム、478,064 レビューアー、2,243,612 レビューを対象とする。4 節、p.8。
- 定性評価では、RAG なし、RAG（VR のみ）、RAG（VR+BGR）の出力を比較し、`k = 1`、`h = 10`、`f = 10`、`g = 1` と設定した。5 節、p.8。
- 定量評価では、システム A（VR）とシステム B（VR+BGR）を3つの推薦例で比較し、18歳から60歳までの男女317人の回答を得た。6 節、pp.11-13。
- 推薦理由のわかりやすさでシステム B が選ばれた割合は、3アイテムについて `52.7%`、`67.5%`、`67.5%` であった。表4、p.14。
- 推薦理由の納得感でシステム B が選ばれた割合は、`51.1%`、`60.9%`、`66.6%` であった。表5、p.14。
- 推薦理由を参考にする度合いでシステム B が選ばれた割合は、`52.4%`、`62.8%`、`64.4%` であった。表6、p.14。
- 推薦理由について「詳細な理由があると安心できる」と回答した人は `55.5%` であった。表7、pp.13, 15。
- 著者らは、コスメに非常に詳しい回答者ではシステム B の評価が相対的に低い可能性を述べ、成分や機能等を扱う Knowledge-Graph Retrieval の追加を今後の課題とする。6.5 節、7 節、pp.15, 17。

## 解釈

- BGR は、文書内の概念関係を構築する既存の GraphRAG とは異なり、ユーザーとアイテムの観測済みレビュー関係を辿って説明材料を増やす設計である。
- 既存の QASPER 上の GraphRAG 比較が「回答の正確さ・包括性」を評価したのに対し、本論文は「推薦理由が納得されるか」を評価しており、グラフ検索の価値は用途別の評価軸で検証する必要がある。
- レビュー数の多い利用者やアイテムを用いた説明は信頼感を高める可能性がある一方、人気偏重やレビュー偏りを生じさせる可能性も検討対象になる。

## 手法と評価

| 項目 | 内容 |
| --- | --- |
| 手法 | VR の取得レビューからアイテム・レビューアーを辿る二部グラフ検索を追加した RAG 推薦 |
| データセット / 環境 | @cosme: 6,904 アイテム、478,064 レビューアー、2,243,612 レビュー |
| 使用モデル | `gemma-2-27b-it.f16.Q8_0.gguf`; `intfloat/multilingual-e5-large` |
| ベースライン | RAG なし、RAG（VR のみ） |
| 評価指標 | アンケートによる満足度、推薦理由のわかりやすさ、納得感、参考度 |
| 主な結果 | VR+BGR が VR より、3例すべてでわかりやすさ・納得感・参考度の選択率が高い |
| 報告された限界 | コスメに詳しい層での評価低下の可能性、成分・機能等を含む網羅的説明の不足 |

## 未解決課題

- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]] - 説得力向上が正確性や偏りと両立するかは未検証である。
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]] - 著者らは Knowledge-Graph Retrieval の追加を今後の課題とする。

## 関連ページ

- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - 提案システムの追加検索機構。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - 本論文が評価する目的。
- [[concepts/graphrag|GraphRAG]] - 知識グラフ型 RAG との比較対象。
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - グラフ拡張 RAG の別用途の比較論文。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - 両論文の対照。
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 比較から生じる研究テーマ候補。

## 矛盾

- 本論文の BGR はレビューアー・アイテム間の二部グラフ探索であり、[[concepts/graphrag|GraphRAG]] が扱う知識グラフの構築・探索とは対象グラフが異なる。両者を同一手法として性能比較しない。

## 情報源

- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - 取り込まれた原資料。英文概要、3-7 節、表3-9 を参照。
- [CiNii Research record](https://cir.nii.ac.jp/crid/1390587682741687808) - タイトル、著者、DOI、掲載誌、英文概要の確認。
