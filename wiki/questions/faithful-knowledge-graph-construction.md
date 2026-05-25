---
title: "GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか"
type: question
tags: [graphrag, knowledge-graph, faithfulness, evaluation]
last_updated: 2026-05-25
status: open
---

# GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか

## 概要

LLM が文書からノード、関係、要約を作る過程で、回答に必要な事実が欠落または
変形する問題を、どのように測定し、検索時に回復できるよう設計するかを問う。

## 重要性

知識グラフが欠損していれば、検索アルゴリズムを改善しても必要な根拠へ到達できない。
研究文献を扱う場合、手法名、定量値、条件の欠落はテーマ評価や再現性判断を誤らせる。

## 事実

- 対象論文は、両 GraphRAG 方式がノード抽出や知識グラフ構築に LLM を使うため、情報が欠損した不完全な知識グラフを構築する可能性を指摘する。3.6 節、p.4。
- 文書チャンク方式では談話関係の抽出が不十分な場合があり、コミュニティ分類方式では一部情報が省略されるケースが確認された。3.6 節、p.4。
- コミュニティ分類方式は、具体的な手法名や定量的結果の正確な取得が難しい場合があったと報告されている。3.6 節、p.4。

## 解釈

- 知識グラフ品質は最終 QA スコアだけでなく、原文の必須事実がノード・エッジ・要約に保持されている割合として測る必要がある。
- 生成された要約や関係に、原文チャンクへの証拠リンクを必須化すれば、欠落検出と回答時の再確認を行いやすくなる。

## 解決条件

| Item                          | Description                                   |
| ----------------------------- | --------------------------------------------- |
| Evidence needed               | KG 構築後に保持された回答根拠と失われた根拠を対照できる評価データ            |
| Candidate experiment / review | QASPER の根拠箇所を用いた、ノード・関係・要約の証拠保持率と QA 性能の相関分析  |
| Relevant metric               | evidence recall、数値・固有名保持率、関係保持率、QA F1、回答根拠忠実性 |
| Answered when                 | 欠落の測定方法と、保持率または回答忠実性を改善する構築手順が実証される           |

## 関係する研究テーマ候補

- 現時点で登録された研究テーマ候補はない。

## 関連ページ

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- [[concepts/graphrag|GraphRAG]]
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]]
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]

## 未解決課題

- 原文証拠を保持するための粒度はチャンク、文、事実単位のどれが適切か。
- KG の欠損と、生成モデル自身の誤りを実験上どのように分離するか。

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一本文の参照用公開版。
