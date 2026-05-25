---
title: "知識グラフ強化 T5 の性能改善を再現可能に評価できるか"
type: question
tags: [t5, knowledge-graph, evaluation, reproducibility, question-answering]
source_files: [raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf]
last_updated: 2026-05-26
status: open
---

# 知識グラフ強化 T5 の性能改善を再現可能に評価できるか

## 概要

知識グラフの埋め込みを T5 fine-tuning に追加したときの性能改善を、知識構築、
入力対応付け、評価指標を明示した条件で再現可能に検証できるかを問う。

## 事実

- Liao et al. は SQuAD1.1 上で、T5 `80.1 / 79 / 77` に対し、知識グラフ併用方式 `85.2 / 83 / 82` を表1で報告する。p.4。
- 同論文は、エンティティ埋め込みと関係埋め込みを個別に追加するアブレーション、および知識グラフ規模別の比較を示す。表2-3、pp.4-5。
- 本文の方法節・実験節では、知識グラフの出所、質問とグラフ要素の対応付け手順、知識グラフ規模の定義、表の3指標の算出方法を確認できない。

## 解釈

- 報告値を再現し、知識グラフ追加そのものの効果を評価するには、モデル構成だけでなく知識の選定経路と評価プロトコルの固定が必要になる。
- 知識グラフ規模を変える研究は候補になりうるが、まず規模の定義と指標の再現性を確立する必要がある。

## 未解決課題

- SQuAD1.1 の各質問に関連するエンティティと関係を、どの知識グラフからどの手続きで選択するのか。
- `Inference Accuracy`、`Contextual understanding`、`Ability to handle complex problems` を、再実行可能な評価手順としてどう定義するのか。
- T5 のモデルサイズ、学習設定、知識グラフ埋め込みモデル、補助損失係数 `lambda` を固定したときにも、報告された改善が維持されるか。
- 知識グラフの規模と品質・関連性を分離して、性能への寄与を評価できるか。

## 検証に必要な条件

| 条件 | 確認すべき内容 |
| --- | --- |
| 知識グラフ仕様 | 出所、構築手順、規模の定義、エンティティ・関係集合 |
| 入力対応付け | 質問から関連ノード・関係を選ぶ規則 |
| モデル仕様 | T5 変種、埋め込み方式、損失、`lambda`、学習設定 |
| 評価仕様 | データ分割、指標定義、計算コードまたは評価プロトコル |
| 比較仕様 | T5 ベースラインと KG 併用条件で共有する設定 |

## 関連ページ

- [[papers/t5-knowledge-graph-finetuning-complex-tasks-2025|A Fine-Tuning Approach for T5 Using Knowledge Graphs to Address Complex Tasks]]
- [[concepts/t5-knowledge-graph-finetuning|知識グラフを用いた T5 fine-tuning]]
- [[architectures/t5-kg-finetuning-architecture|T5 知識グラフ fine-tuning アーキテクチャ]]
- [[comparisons/t5-vs-t5-kg-finetuning|T5 と知識グラフ併用 T5 の比較]]

## 矛盾

- 論文は比較表から改善を主張するが、再現に必要な知識グラフ仕様と評価指標の運用定義は本文で確認できない。したがって、本ページでは改善を確定事実ではなく報告結果として扱う。

## 情報源

- `raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf` - Abstract、2-4 節、表1-3、pp.1-5。
