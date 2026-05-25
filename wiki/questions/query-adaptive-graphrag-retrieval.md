---
title: "質問特性に応じて GraphRAG の検索経路を選択できるか"
type: question
tags: [graphrag, retrieval, routing, question-answering]
last_updated: 2026-05-25
status: open
---

# 質問特性に応じて GraphRAG の検索経路を選択できるか

## 概要

質問が局所的な根拠確認を必要とするか、文書全体をまたぐ要約・関連付けを必要と
するかに応じて、チャンク検索、コミュニティ要約検索、または両者の組合せを選択
すると QA 性能を改善できるかを問う。

## 重要性

一つの方式をすべての質問に適用すると、詳細情報の保持と広い文脈の提供の間で
性能の取りこぼしが生じうる。検索経路の選択ができれば、学術文書 QA に適した
GraphRAG の研究テーマになる。

## 事実

- 対象論文の QASPER 実験では、抽出型 QA と要約型 QA でコミュニティ分類方式が最良であった。3.4.1 節、表2-3、p.3。
- 同実験では、Yes/No 型 QA で文書チャンク方式の完全一致率 `0.54` が最良であった。表4、p.3。
- 著者らは、質問の意図や性質に応じ、ベクトル検索とグラフ検索を組み合わせた検索アルゴリズムの設計が必要だと述べる。3.6 節、p.4。

## 解釈

- 質問型、必要な答えの粒度、数値・固有名の必要性を特徴量として経路をルーティングする方法は検証可能な研究方向である。
- コミュニティ要約で候補領域を絞り、チャンクまたは原文で回答証拠を回収する二段階検索は、広さと具体性を両立する候補となる。

## 解決条件

| Item                          | Description                                           |
| ----------------------------- | ----------------------------------------------------- |
| Evidence needed               | 同一 QA 集合で固定方式とルーティング方式を比較した結果                         |
| Candidate experiment / review | QASPER の質問型と証拠粒度を用いて、チャンク、コミュニティ、ハイブリッド経路を選択するアブレーション |
| Relevant metric               | 質問型別の EM/F1/BERTScore、根拠再現率、ルーティング精度、コスト              |
| Answered when                 | 固定方式に対する改善が再現され、どの質問特徴で経路が有効か説明できる                    |

## 関係する研究テーマ候補

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 利用者や必要証拠に応じた検索経路選択に接続する。

## 関連ページ

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[concepts/graphrag|GraphRAG]]
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]]
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]]
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]]

## 未解決課題

- ルーティングに使う質問特性は事前分類で十分か、それとも検索中に更新すべきか。
- コミュニティ要約から原文チャンクへ証拠を戻す経路は回答の忠実性を改善するか。

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 表2-4、3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
