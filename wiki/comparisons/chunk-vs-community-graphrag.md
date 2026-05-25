---
title: "チャンク方式とコミュニティ方式の GraphRAG 比較"
type: comparison
tags: [graphrag, rag, qasper, retrieval, comparison]
last_updated: 2026-05-25
status: active
---

# チャンク方式とコミュニティ方式の GraphRAG 比較

## 概要

Anaguchi et al. (2025) が QASPER で比較した、通常の RAG、文書チャンクに基づく
GraphRAG、コミュニティ分類に基づく GraphRAG の構造と QA 結果を整理する。

## 比較範囲

- 根拠論文: [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]
- 比較する概念: [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]], [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]]
- ベースライン: 通常の RAG

## 事実

### 構造の比較

| Axis | 文書チャンクに基づく GraphRAG | コミュニティ分類に基づく GraphRAG | Evidence |
| --- | --- | --- | --- |
| Graph representation | 各チャンクの 5W1H とチャンク間の談話関係 | ノード・エッジと、そのコミュニティおよびコミュニティ要約 | 2.1-2.2 節、pp.1-2 |
| Retrieval behavior | ベクトル類似検索から関連チャンクを取得し、5W1H と談話関係でグラフ検索 | 類似するコミュニティまたはノードを選択し、要約を用いて回答生成 | 2.1-2.2 節、3.4.2 節 |
| Reported strength | 具体的で詳細な情報の取得 | 文書全体のトピックと関連概念を踏まえた回答 | 3.5.2 節、p.4 |
| Reported weakness | 広い文脈を要する質問への不完全回答、談話関係抽出の不足 | 手法名・定量結果の正確な取得困難、一部情報の省略 | 3.6 節、p.4 |

### QA 結果

| Task / Metric | RAG | 文書チャンクに基づく GraphRAG | コミュニティ分類に基づく GraphRAG | Best |
| --- | ---: | ---: | ---: | --- |
| Extractive / Exact Match | 0.01 | 0.01 | 0.03 | Community |
| Extractive / Precision | 0.53 | 0.62 | 0.64 | Community |
| Extractive / Recall | 0.14 | 0.18 | 0.25 | Community |
| Extractive / F1 | 0.20 | 0.25 | 0.31 | Community |
| Summarization / BLEU | 0.15 | 0.15 | 0.17 | Community |
| Summarization / Precision | 0.52 | 0.51 | 0.54 | Community |
| Summarization / Recall | 0.50 | 0.49 | 0.52 | Community |
| Summarization / F1 | 0.51 | 0.49 | 0.52 | Community |
| Yes/No / Exact Match | 0.51 | 0.54 | 0.49 | Chunk |

## 解釈

- コミュニティ分類方式は文書全体を踏まえた抽出・要約に向く可能性がある一方、局所証拠を正確に返す必要がある問いではチャンク方式の経路を残す理由がある。
- 三手法の結果は、GraphRAG の設計選択を「どのグラフが最善か」ではなく「どの質問特性にどの証拠表現が適合するか」として扱う研究方向を支持する。
- 最終回答の品質だけでなく、KG 内で数値・固有名・談話関係がどれだけ保持されたかを比較すると、性能差の原因を追跡しやすくなる。

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 関連ページ

- [[concepts/graphrag|GraphRAG]]
- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- `raw/papers/graphrag-comparison.pdf` - 2.1-2.2 節、表2-4、3.5-3.6 節。
- [J-STAGE published PDF](https://www.jstage.jst.go.jp/article/pjsai/JSAI2025/0/JSAI2025_3P1OS46a02/_pdf/-char/ja) - 同一 DOI の公開本文。
