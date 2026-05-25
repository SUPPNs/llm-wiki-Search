---
title: 文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析
type: paper
authors:
  - ANAGUCHI Fumikatsu
  - CHAKRABORTY Sudesna
  - MORITA Takeshi
  - EGAMI Shusaku
  - UGAI Takanori
  - FUKUDA Ken
year: 2025
venue: The 39th Annual Conference of the Japanese Society for Artificial Intelligence
doi: 10.11517/pjsai.jsai2025.0_3p1os46a02
tags:
  - graphrag
  - rag
  - knowledge-graph
  - qasper
  - question-answering
last_updated: 2026-05-25
status: ingested
---

# 文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析

## 概要

学術論文 QA において、通常の RAG、文書チャンクの 5W1H と談話関係を使う
GraphRAG、知識グラフのコミュニティ要約を使う GraphRAG を比較した論文である。
抽出型と要約型ではコミュニティ分類方式が最良であった一方、Yes/No 型では
チャンク方式が最良であり、検索構造と質問型の対応が研究課題として現れる。

## 研究上の位置付け

- 問題設定: 学術論文本文からの情報探索型 QA で、GraphRAG の知識グラフ構造と性能差を比較する。
- 主張される貢献: 文書チャンク方式とコミュニティ分類方式を、知識表現・文脈理解および QA 性能から分析する。
- 関連する比較: [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]]

## 事実

- 本論文は JSAI2025 発表論文であり、DOI は `10.11517/pjsai.jsai2025.0_3p1os46a02` である。CiNii 書誌情報。
- 実験は QASPER を用いる。論文は QASPER を 1,585 本の学術論文と 5,049 件の質問回答事例を持つ情報探索型 QA データセットとして説明する。3.2 節、p.2。
- 評価対象は、回答不能を除く質問型からランダム抽出した各 100 事例である。3.2 節、p.2。
- RAG 実装には `text-embedding-3-small` と `gpt-4o-mini-2024-07-18` が用いられた。3.1 節、p.2。
- 抽出型 QA の F 値は、RAG が `0.20`、文書チャンク方式が `0.25`、コミュニティ分類方式が `0.31` であり、コミュニティ分類方式が最高である。表2、p.3。
- 要約型 QA の F 値は、RAG が `0.51`、文書チャンク方式が `0.49`、コミュニティ分類方式が `0.52` であり、コミュニティ分類方式が最高である。表3、p.3。
- Yes/No 型 QA の完全一致率は、RAG が `0.51`、文書チャンク方式が `0.54`、コミュニティ分類方式が `0.49` であり、文書チャンク方式が最高である。表4、p.3。
- 著者らは、チャンク方式では広い文脈を要する質問への不完全回答、コミュニティ分類方式では具体的手法名や定量結果の正確な取得の難しさを報告している。3.6 節、p.4。
- 著者らは、両 GraphRAG 方式で LLM によるノード抽出・知識グラフ構築に起因する情報欠損の可能性を指摘している。3.6 節、p.4。

## 解釈

- 単一の GraphRAG 構造を全質問に固定するよりも、質問型または必要な証拠粒度に応じて検索方針を切り替える設計が有望である。
- コミュニティ要約は広い関連文脈の提供に寄与しうるが、要約過程で消える固有名・数値を別経路で保持する必要がある。
- 本実験は回答型別の性能逆転を示しており、GraphRAG の評価では総合スコアだけでなく質問特性別の分析が重要である。

## 手法と評価

| 項目 | 内容 |
| --- | --- |
| 手法 | RAG、文書チャンクの 5W1H・談話関係 KG に基づく GraphRAG、コミュニティ要約 KG に基づく GraphRAG の比較 |
| データセット / 環境 | QASPER。回答不能を除く質問型から各 100 事例を評価 |
| 使用モデル | `text-embedding-3-small`; `gpt-4o-mini-2024-07-18` |
| 評価指標 | 抽出型: 完全一致率、適合率、再現率、F 値。要約型: BLEU、BERTScore 指標値。Yes/No 型: 完全一致率 |
| 主な結果 | 抽出型・要約型はコミュニティ分類方式、Yes/No 型はチャンク方式が最高性能 |
| 報告された限界 | KG の情報欠損、チャンク方式の文脈把握不足、コミュニティ方式の具体情報取得不足、曖昧質問への意図ずれ |

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 回答型によって優位な方式が変わる。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - 両方式に KG の欠損可能性が報告されている。

## 関連ページ

- [[concepts/graphrag|GraphRAG]] - 知識グラフを介した RAG の比較対象。
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]] - 詳細情報と Yes/No QA に強みを示した方式。
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]] - 抽出型・要約型 QA に強みを示した方式。
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - 本論文の比較結果を研究探索向けに整理した表。

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 取り込まれた原資料。読みやすい英数字ファイル名へ改名済み。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一 DOI の公開本文。本文節・表・ページ確認に使用。
- [CiNii Research record](https://cir.nii.ac.jp/crid/1390023229740664576) - タイトル、著者、年、DOI、掲載誌の書誌確認に使用。
