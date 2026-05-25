---
title: "根拠付き推薦のためのハイブリッド Graph-RAG"
type: theme
tags: [rag, recommendation, bipartite-graph, knowledge-graph, explainability]
last_updated: 2026-05-25
status: exploring
---

# 根拠付き推薦のためのハイブリッド Graph-RAG

## 概要

レビューアーと商品の行動関係を辿る二部グラフ検索（BGR）と、商品内容の根拠を
保持する知識グラフ型検索を組み合わせ、説得力がありながら検証可能な推薦理由を
生成する研究テーマ候補である。

## 中心となる研究課題

ベクトル検索（VR）に BGR と知識グラフ型検索を組み合わせることで、推薦理由の
納得感を保ちながら、根拠への忠実性と専門的な内容の網羅性を改善できるか。

## 事実

- Arakaki・Fushimi (2026) は、化粧品推薦において VR+BGR が VR のみより、3つの推薦例すべてで推薦理由のわかりやすさ、納得感、参考度について高い選択率を得たと報告する。表4-6、p.14。
- 同論文は、コスメに非常に詳しい回答者では提案方式への評価が相対的に低い可能性を述べ、成分や機能等を扱う Knowledge-Graph Retrieval の追加を今後の課題として示す。6.5 節、7 節、pp.15, 17。
- Anaguchi et al. (2025) は、知識グラフ型 GraphRAG で質問型によって優位な構造が異なること、および知識グラフ構築時に情報欠損が起こりうることを報告する。表2-4、3.6 節、pp.3-4。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] は、BGR がレビュー行動の二部グラフを用い、既存 GraphRAG が文書由来の意味・知識構造を用いるという違いを整理している。

## 解釈

- 仮説: BGR による経験的なレビュー証拠と、知識グラフ型検索による内容証拠を併用すれば、推薦理由の説得力と検証可能性を単独方式より高められる。
- 貢献候補: 行動グラフと知識グラフの役割分担、および利用者の知識水準や問いの性質に応じた検索経路選択の評価枠組みを示すこと。
- 研究上の意味: 既存2論文は異なる用途と評価指標を扱うため、結果の数値を直接比較するのではなく、相互に不足する証拠種別を補完する設計として検証する価値がある。

## 未解決課題

- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]
- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]
- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]

## 研究方法

| 項目 | 計画 |
| --- | --- |
| 対象設定 | 化粧品など、レビュー証拠と商品属性・成分等の内容証拠を収集できる推薦領域 |
| 提案手法 | VR+BGR で候補商品と経験的レビュー証拠を取得し、知識グラフ型検索で成分・機能・注意点等の内容証拠を補強する |
| 比較対象 | VR、VR+BGR、VR+知識グラフ検索、VR+BGR+知識グラフ検索 |
| 評価指標 | わかりやすさ、納得感、参考度、根拠忠実性、内容網羅性、誤主張率、露出偏り、検索コスト |
| 必要なデータ | レビュー二部グラフ、商品属性・成分・機能に関する根拠文、推薦出力へのユーザー評価 |
| 反証条件 | 説得力が改善しない、または根拠忠実性や公平性が単独方式より悪化する |
| 主なリスク | レビュー偏り、知識グラフの情報欠損、利用者属性の扱いに伴う公平性・プライバシー問題 |

## 関連ページ

- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]] - BGR と利用者評価の根拠論文。
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - 知識グラフ型 GraphRAG と QA 評価の根拠論文。
- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - 行動関係からレビュー証拠を取得する方式。
- [[concepts/graphrag|GraphRAG]] - 内容証拠の構造化と検索に関する概念。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - 利用者向け評価の概念。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - 2論文を横断する比較。

## 矛盾

- BGR と既存 GraphRAG は、用いるグラフの対象と評価タスクが異なる。BGR の利用者評価結果と GraphRAG の QA 評価結果を数値で直接比較して、方式の優劣を断定しない。
- BGR と知識グラフ型検索を統合した方式の有効性は、参照した論文ではまだ実証されていない。このページの統合方針は研究仮説として扱う。

## 情報源

- `raw/papers/bipartite-graph-rag-cosmetics.pdf` - BGR の構成、アンケート評価結果、今後の課題。表4-8、6.5 節、7 節。
- `raw/papers/graphrag-comparison.pdf` - GraphRAG の構成差、質問型別結果、知識グラフの情報欠損に関する指摘。表2-4、3.6 節。
- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]] - 原資料から整理済みの論文ページ。
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - 原資料から整理済みの論文ページ。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - 両論文の比較整理。
