---
title: "CoT poisoning 検知に LLM-as-Judge は信頼できるか"
type: question
tags: [llm-as-judge, cot-poisoning, reliability, evaluation]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: open
---

# CoT poisoning 検知に LLM-as-Judge は信頼できるか

## 概要

enterprise RAG の推論汚染を検知する独立 judge として別 LLM を利用するとき、
drift 検知に依存しないラベルと adversarial condition の下でも、信頼できる
reasoning-integrity 判定を維持できるかを問う。

## 重要性

Layer 3 は regex と behavioral drift が拾えない semantic manipulation を補完する
ことを期待される。しかし、評価用の正解が Layer 2 の drift 判定に由来するなら、
Layer 2 が見逃す種類の操作に対する judge の有効性は確定しない。

## 事実

- Layer 3 は主モデルと architecturally isolated された separate LLM instance として、retrieved context と reasoning trace の manipulation indicators を判定する。Section 2.3、p.13。
- 評価用 ground truth label は、Layer 2 が behavioral drift を検出した chain を `Compromised`、検出しなかった chain を `Clean` とする方法で付与された。Section 2.4、p.13。
- judge は60 reasoning chains で評価され、Table 5.4 は precision `97.4%`、recall `76.0%`、F1 `85.4%` を報告する。pp.13, 21。
- ensemble は precision `95.7%`、recall `90.0%`、F1 `92.8%` を報告するが、これは judge 単独の結果ではない。Table 5.4、p.21。

## 解釈

- judge の precision が高いことは、Layer 2 が定義した compromise に対する false alarm の少なさを示すが、独立した reasoning integrity の完全性を示すものではない。
- decision label を変えずに根拠・確信度・推奨行動を微妙に偏らせる poison は、別の人手アノテーションまたは policy-based oracle が必要になる可能性がある。

## 解決条件

| 項目 | 内容 |
| --- | --- |
| 必要な証拠 | Layer 2 とは独立した専門家ラベル、judge が読む context の汚染条件、judge model / prompt の仕様 |
| 候補となる実験 / 調査 | drift あり・なしの reasoning manipulation を含む盲検評価、複数 judge 間一致度、human analyst との比較 |
| 関連する評価指標 | precision、recall、F1、false-negative rate、inter-rater agreement、latency、cost |
| 解決とみなす条件 | Layer 2 非依存のラベルセットで、運用許容範囲の見逃し率と false alarm を示し、再現可能な設定を公開できること |

## 関係する研究テーマ候補

- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]] - judge を SOC 統制へ組み込む前に解くべき評価課題である。

## 未解決課題

- judge と primary model が同様の poisoned authority cues に影響される共通 failure mode をどう測るか。
- NIST AI RMF の monitoring / incident tracking に対し、judge 判定を監査証拠として利用できる条件は何か。

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/llm_as_judge|LLM-as-Judge]]
- [[concepts/behavioral_drift_detection|Behavioral drift detection]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]

## 矛盾

- 論文は Layer 3 を独立 reasoning validator と位置付けるが、評価ラベルを Layer 2 が生成するため、Layer 2 から独立した信頼性の根拠としては限定される。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Sections 2.3-2.5、Tables 5.4-5.5、Sections 6.1-6.2、pp.13-14, 21-24。
