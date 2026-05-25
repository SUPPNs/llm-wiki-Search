---
title: "LLM-as-Judge による reasoning validation"
type: concept
aliases: [LLM-as-Judge]
tags: [llm-as-judge, reasoning-validation, llm-security, evaluation]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: active
---

# LLM-as-Judge による reasoning validation

## 概要

主モデルとは分離した別の LLM が、alert、retrieved context、主モデルの reasoning
trace を読み、推論が操作されている兆候を分類する検知層である。

## 事実

- 論文の Layer 3 は、主モデルから architecturally isolated された separate LLM instance による reasoning-integrity classifier として設計される。Section 2.3、p.13。
- judge は original alert、retrieved context、primary model reasoning trace を入力として、unsourced statistics、policy を装う embedded directives、logical inconsistencies、alert evidence に反する conclusion を確認する。Section 2.3、p.13。
- Layer 3 の評価用ラベルは Layer 2 の drift detection により割り当てられ、60 reasoning chains が評価対象とされた。Section 2.4、p.13。
- Table 5.4 は LLM-as-Judge の precision `97.4%`、recall `76.0%`、F1 `85.4%` を報告する。p.21。

## 解釈

- 別インスタンスを使う設計は failure mode の共有を低減する狙いを持つが、同系統モデル、同一情報源、同じ評価前提に依存する場合の独立性は追加検証が必要である。
- LLM-as-Judge は単独の真実判定器としてではなく、regex と behavioral drift を補完する runtime monitoring signal として読むのが本論文の位置付けに近い。

## 未解決課題

- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]]
- judge が主モデルと同じ poisoned context に説得される条件をどう評価するか。
- Layer 2 が見逃す操作を含めた独立ラベルセットで性能を測定できるか。

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[concepts/behavioral_drift_detection|Behavioral drift detection]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]]

## 矛盾

- Layer 3 は独立した reasoning validator として記述されるが、その正解ラベルは Layer 2 に依存する。したがって、Layer 2 から独立した検知能力の検証結果とは区別する。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Sections 2.3-2.4、Table 5.4、Sections 6.1-6.2、pp.13, 21, 23-24。
