---
title: "Behavioral drift detection"
type: concept
aliases: [decision drift detection]
tags: [behavioral-drift, detection, llm-security, soc-triage]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: active
---

# Behavioral drift detection

## 概要

clean context と疑わしい context の下で LLM の判断を比較し、severity downgrade や
escalation suppression のような行動変化を攻撃兆候として検出する方式である。

## 事実

- Layer 2 は各 security case について original clean context の baseline decision を得た後、knowledge base から取得した potentially poisoned context で分類を再実行する。Section 2.2、pp.12-13。
- poisoned context により `High` から `Low` への severity downgrade や `Escalate` から `No Escalate` への抑制が起きた場合、compromised と flag する。Section 2.2、p.13。
- Layer 2 は既知 signature の有無ではなく poison の behavioral effect を対象にするため、pattern matching を回避する新規攻撃を検出する目的で設計された。Section 2.2、p.13。
- 50件と5 poison type による250試験では `24.8% (62/250)` の drift が報告された。Tables 5.2-5.3、pp.19-20。

## 解釈

- behavior を比較する手法は、文書が悪意あるかを単独で判定するより、実際に意思決定を変えたかを監視する設計である。
- clean baseline の取得・保持が必要なため、更新頻度の高い enterprise RAG では baseline freshness と計算コストが運用課題になる。

## 未解決課題

- baseline context 自体が古い、偏っている、または既に汚染されている場合に drift 判定をどう成立させるか。
- 判断ラベルは変わらないが reasoning や後続調査行動が偏る攻撃をどう捉えるか。
- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]] - drift ラベルを judge 評価の正解とする妥当性を問う。

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[concepts/llm_as_judge|LLM-as-Judge]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]

## 矛盾

- 論文は Layer 2 を Layer 3 の ground truth labeling に利用するが、Layer 2 自身が drift のない semantic manipulation を拾えることは示していない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Sections 2.2、2.4、5.2-5.3、pp.12-13, 19-20。
