---
title: "A Fine-Tuning Approach for T5 Using Knowledge Graphs to Address Complex Tasks"
type: paper
authors:
  - Xiaoxuan Liao
  - Binrong Zhu
  - Jacky He
  - Jia Gao
  - Hongye Zheng
year: 2025
venue: ""
tags: [t5, knowledge-graph, fine-tuning, question-answering, squad]
source_files: [raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf]
last_updated: 2026-05-26
status: ingested
---

# A Fine-Tuning Approach for T5 Using Knowledge Graphs to Address Complex Tasks

## 概要

T5 の fine-tuning 入力に知識グラフ由来のエンティティ埋め込みと関係埋め込みを
統合し、質問応答タスクにおける報告性能を比較した論文である。SQuAD1.1 上の
比較・アブレーション・知識グラフ規模別実験を示す一方、知識グラフの生成方法や
表の評価指標の定義は本文から確認できない。

## 研究上の位置付け

- 問題設定: 複雑なタスクで T5 が外部知識を利用して推論と文脈理解を向上できるかを検討する。
- 提案: 知識グラフのエンティティ埋め込みと関係埋め込みを T5 の fine-tuning に加える。
- 関連する概念: [[concepts/t5-knowledge-graph-finetuning|知識グラフを用いた T5 fine-tuning]]
- 関連する構成: [[architectures/t5-kg-finetuning-architecture|T5 知識グラフ fine-tuning アーキテクチャ]]

## 事実

- 著者らは、外部知識グラフを T5 の fine-tuning に導入する方式を提案し、SQuAD1.1 を用いて検証すると述べる。Abstract、p.1。
- 方法節では、知識グラフを `G=(V,E)` とし、エンティティと関係の埋め込みベクトルを事前学習済み知識グラフ埋め込みモデルから得て、入力質問とともに T5 のエンコーダへ与える。2.2-2.3 節、pp.2-3。
- 学習損失には、通常の出力損失に加えて、エンティティ埋め込みと関係埋め込みの類似度に重み `lambda` を掛けた項を導入すると記載されている。2.4 節、p.3。
- 表1は、`Inference Accuracy` について T5 の `80.1` に対し `Ours (T5+KG)` の `85.2` を報告する。`Contextual understanding` は `79` 対 `83`、`Ability to handle complex problems` は `77` 対 `82` と報告される。p.4。
- 表2のアブレーションは、T5 `80.1 / 79 / 77`、`T5 + Entity Embeddings` `81.5 / 80 / 78`、`T5 + Relation Embeddings` `82.3 / 81 / 79`、`Ours` `85.2 / 83 / 82` を報告する。p.4。
- 表3は、知識グラフ規模を `Small`、`Medium`、`Large` としたとき、3指標がそれぞれ `81.2 / 79 / 77`、`83.5 / 81 / 79`、`85.2 / 83 / 82` となる結果を報告する。pp.4-5。
- 本文の方法節・実験節では、使用した知識グラフの出所、質問から関連エンティティ・関係を得る手順、埋め込みモデル名、表1-3の各指標の算出方法を確認できない。

## 解釈

- 本論文は、知識グラフ情報を検索時ではなく T5 の fine-tuning 入力と損失へ統合する方式の実験報告として位置付けられる。
- 表1-3は知識グラフ情報の追加と指標増加の関係を報告しているが、指標定義と知識グラフ構築条件が明記されないため、再現済みの性能改善とは扱わず、再現検証を要する報告結果として扱う。

## 手法と評価

| 項目 | 内容 | 根拠 |
| --- | --- | --- |
| ベースモデル | T5 | Abstract、2 節、pp.1-3 |
| 外部知識 | エンティティと関係を含む知識グラフ `G=(V,E)` | 2.2 節、p.2 |
| 統合方法 | 質問入力にエンティティ・関係埋め込みを追加して T5 エンコーダへ入力 | 2.3 節、pp.2-3 |
| 学習 | 標準損失に埋め込み類似度項を加え、Adam、Dropout、正則化を用いる | 2.4 節、p.3 |
| データセット | SQuAD1.1 | Abstract、p.1; 3 節、pp.3-4 |
| 比較対象 | GPT-2、RoBERTa、BERT、T5、Ours (T5+KG) | 表1、p.4 |
| 補足実験 | 埋め込みアブレーション、知識グラフ規模別実験 | 表2-3、pp.4-5 |

## 未解決課題

- [[questions/kg-finetuning-evaluation-reproducibility|知識グラフ強化 T5 の性能改善を再現可能に評価できるか]] - 知識グラフ構築、対応付け、指標定義と比較条件の明示を問う。

## 関連ページ

- [[concepts/t5-knowledge-graph-finetuning|知識グラフを用いた T5 fine-tuning]]
- [[architectures/t5-kg-finetuning-architecture|T5 知識グラフ fine-tuning アーキテクチャ]]
- [[comparisons/t5-vs-t5-kg-finetuning|T5 と知識グラフ併用 T5 の比較]]
- [[questions/kg-finetuning-evaluation-reproducibility|知識グラフ強化 T5 の性能改善を再現可能に評価できるか]]

## 矛盾

- 論文は知識グラフ規模の増加に伴う性能改善を報告するが、規模の定義と知識グラフの構築・選択方法は本文で確認できないため、規模増加の一般的効果とは断定しない。
- 論文は `Inference Accuracy` などの改善を報告するが、本文で指標の計算方法を確認できないため、SQuAD1.1 の標準的な評価指標と同一視しない。

## 情報源

- `raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf` - Abstract、2-4 節、図1-2、表1-3、pp.1-5。
