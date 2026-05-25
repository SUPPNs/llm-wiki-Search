---
title: "Research LLM Wiki Index"
type: index
last_updated: 2026-05-25
---

# Research LLM Wiki Index

論文・概念・問いを横断して研究テーマ候補を探索するための入口です。ページを追加
または大きく更新したときは、この索引と [../logs/log.md](../logs/log.md) を更新します。

## 案内

| 分類 | 内容 |
| --- | --- |
| 論文 | 読解済み論文の要点、限界、接続 |
| 概念 | 手法、指標、問題設定、背景概念 |
| 研究テーマ候補 | 検討中の研究テーマ候補 |
| 未解決課題 | 未解決課題、要検証の問い |
| 比較 | 比較軸を揃えた表と差分 |

## 論文

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - 学術論文 QA で2種の GraphRAG と RAG を比較。

## 概念

- [[concepts/graphrag|GraphRAG]] - 知識グラフを用いた RAG の設計空間。
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]] - 5W1H と談話関係を保持する方式。
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]] - コミュニティ要約を用いる方式。

## 研究テーマ候補

_ページはまだありません。_

## 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 比較

- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - 構造と QASPER の性能差。

## テンプレート

- [論文テンプレート](templates/paper.md)
- [概念テンプレート](templates/concept.md)
- [研究テーマ候補テンプレート](templates/theme.md)
- [未解決課題テンプレート](templates/question.md)
- [比較テンプレート](templates/comparison.md)

## 作業メモ

### 事実

- `raw/` は原資料の保存場所であり、知識ページとは分離されている。
- 知識ページの初期カテゴリは Papers、Concepts、Themes、Questions、Comparisons
  である。
- Anaguchi et al. (2025) の GraphRAG 比較論文が最初の論文資料として取り込まれた。

### 解釈

- 研究テーマ候補を問いと比較表から明示的に接続すると、文献要約だけで終わらず、
  次の調査または実験へ進みやすくなる。
- 現在の資料からは、質問特性に応じた検索方式選択と知識グラフの証拠保持が
  有望な探索方向として浮かび上がる。

### 未解決課題

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]]
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]]

## 矛盾

- 現時点で確認された矛盾はない。

## 情報源

- [../README.md](../README.md) - この wiki の目的と初期構成。
- `../raw/papers/graphrag-comparison.pdf` - 最初に取り込んだ論文原資料。参照しやすい英数字ファイル名に変更済み。
- [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) - `raw/` と `wiki/`
  を分離する設計の参照元。
