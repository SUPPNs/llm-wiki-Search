---
title: "T5 と知識グラフ併用 T5 の比較"
type: comparison
tags: [t5, knowledge-graph, fine-tuning, comparison, ablation]
source_files: [raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf]
last_updated: 2026-05-26
status: active
---

# T5 と知識グラフ併用 T5 の比較

## 概要

Liao et al. が SQuAD1.1 上で報告した、T5 と知識グラフ埋め込みを追加した T5 の
比較結果、埋め込みアブレーション、知識グラフ規模別結果を整理する。

## 事実

- 表1では、T5 と `Ours (T5+KG)` を含む5モデルが、`Inference Accuracy`、`Contextual understanding`、`Ability to handle complex problems` の3列で比較されている。p.4。
- 表2では、T5 にエンティティ埋め込みのみ、関係埋め込みのみ、両者を含む提案方式を追加した結果が比較されている。p.4。
- 表3では、`Small`、`Medium`、`Large` の知識グラフ規模で結果が比較されている。pp.4-5。
- 本文では、3指標の算出式、評価者または評価手順、知識グラフ規模の定義を確認できない。

## 比較表

### ベースライン比較

| モデル | Inference Accuracy | Contextual understanding | Ability to handle complex problems |
| --- | ---: | ---: | ---: |
| GPT-2 | 72.5 | 70 | 68 |
| RoBERTa | 75.8 | 73 | 72 |
| BERT | 78.3 | 76 | 74 |
| T5 | 80.1 | 79 | 77 |
| Ours (T5+KG) | 85.2 | 83 | 82 |

出典: 表1、p.4。列名と数値は論文の報告値であり、標準 SQuAD 指標との対応は本文で確認できない。

### 埋め込みアブレーション

| モデル | Inference Accuracy | Contextual understanding | Ability to handle complex problems |
| --- | ---: | ---: | ---: |
| T5 | 80.1 | 79 | 77 |
| T5 + Entity Embeddings | 81.5 | 80 | 78 |
| T5 + Relation Embeddings | 82.3 | 81 | 79 |
| Ours | 85.2 | 83 | 82 |

出典: 表2、p.4。

### 知識グラフ規模別結果

| Knowledge Graph Size | Inference Accuracy | Contextual understanding | Ability to handle complex problems |
| --- | ---: | ---: | ---: |
| Small | 81.2 | 79 | 77 |
| Medium | 83.5 | 81 | 79 |
| Large | 85.2 | 83 | 82 |

出典: 表3、pp.4-5。

## 解釈

- 論文が報告した実験範囲では、エンティティと関係の両埋め込みを含む方式が、T5 単独および単一埋め込み条件より高い数値を示している。
- 規模別表は、著者の設定で大きい知識グラフが高い報告値を伴ったことを示すが、規模定義と知識選択手順の不在により一般化は保留する。

## 未解決課題

- [[questions/kg-finetuning-evaluation-reproducibility|知識グラフ強化 T5 の性能改善を再現可能に評価できるか]]

## 関連ページ

- [[papers/t5-knowledge-graph-finetuning-complex-tasks-2025|A Fine-Tuning Approach for T5 Using Knowledge Graphs to Address Complex Tasks]]
- [[concepts/t5-knowledge-graph-finetuning|知識グラフを用いた T5 fine-tuning]]
- [[architectures/t5-kg-finetuning-architecture|T5 知識グラフ fine-tuning アーキテクチャ]]

## 矛盾

- 本文は知識グラフ併用による改善を結論づけるが、比較表の指標定義と評価手順が本文で確認できないため、他研究の数値と直接比較したり一般的改善として断定したりしない。

## 情報源

- `raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf` - 3-4 節、図2、表1-3、pp.3-5。
