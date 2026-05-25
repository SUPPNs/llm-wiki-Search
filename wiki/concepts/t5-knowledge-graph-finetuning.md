---
title: "知識グラフを用いた T5 fine-tuning"
type: concept
tags: [t5, knowledge-graph, fine-tuning, external-knowledge]
source_files: [raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf]
last_updated: 2026-05-26
status: active
---

# 知識グラフを用いた T5 fine-tuning

## 概要

入力テキストに関連する知識グラフのエンティティ・関係埋め込みを T5 の学習過程に
統合し、外部知識を用いた回答生成を目指す fine-tuning 方式である。

## 事実

- Liao et al. は、知識グラフをエンティティ集合 `V` と関係集合 `E` からなる `G=(V,E)` と定義する。2.2 節、p.2。
- 同論文では、事前学習済み知識グラフ埋め込みモデルからエンティティ埋め込みと関係埋め込みを取得し、質問入力とともに T5 のエンコーダへ入力する。2.2-2.3 節、pp.2-3。
- 学習では、生成出力に対する損失に加えて、エンティティ埋め込みと関係埋め込みの類似度に関する項を損失へ導入する。2.4 節、p.3。
- SQuAD1.1 における表1では、同論文の `Ours (T5+KG)` が T5 より高い報告値を3指標で示す。p.4。

## 解釈

- この概念は、質問時に外部グラフを検索して文脈を追加する方式ではなく、モデルの fine-tuning 段階に構造化知識表現を組み込む設計として整理できる。
- 知識グラフの作り方と入力質問への対応付けが実装結果を左右すると考えられるが、当該論文ではその具体手順が本文から確認できない。

## 利用範囲

| 観点 | 内容 |
| --- | --- |
| 対象モデル | Text-to-Text Transformer である T5 |
| 追加情報 | 知識グラフのエンティティ埋め込みと関係埋め込み |
| 学習時の役割 | 入力への統合と補助損失への利用 |
| 報告された対象タスク | SQuAD1.1 の質問応答 |

## 未解決課題

- [[questions/kg-finetuning-evaluation-reproducibility|知識グラフ強化 T5 の性能改善を再現可能に評価できるか]]

## 関連ページ

- [[papers/t5-knowledge-graph-finetuning-complex-tasks-2025|A Fine-Tuning Approach for T5 Using Knowledge Graphs to Address Complex Tasks]]
- [[architectures/t5-kg-finetuning-architecture|T5 知識グラフ fine-tuning アーキテクチャ]]
- [[comparisons/t5-vs-t5-kg-finetuning|T5 と知識グラフ併用 T5 の比較]]

## 矛盾

- 方式の構成は本文に示されているが、知識グラフの出所、関連要素の選定手順、埋め込みモデル名が明示されていないため、概念ページでは実装仕様が確定したものとして扱わない。

## 情報源

- `raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf` - 2 節、図1、表1-3、pp.2-5。
