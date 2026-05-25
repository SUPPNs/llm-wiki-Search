---
title: "Prompt injection と CoT poisoning の比較"
type: comparison
tags: [prompt-injection, cot-poisoning, rag-security, comparison]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: active
---

# Prompt injection と CoT poisoning の比較

## 概要

論文が Chapter 1 で区別する traditional prompt injection と、enterprise RAG の
retrieved context を通じて判断を操作する CoT poisoning の違いを整理する。

## 比較範囲

- 比較対象: traditional prompt injection、Chain-of-Thought poisoning
- 根拠論文: [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- 関連する研究テーマ候補: [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]

## 事実

| 比較軸 | Prompt injection | CoT poisoning | 根拠 |
| --- | --- | --- | --- |
| 攻撃位置 | input layer に指示を与える | RAG で取得される context を介して reasoning mechanism を汚染する | Chapter 1、p.9 |
| 論文が挙げる例 | `ignore previous instructions` のような命令 | 偽 runbook、偽 CISO policy directive、捏造された metrics report | Chapter 1、p.9 |
| 期待する攻撃効果 | instruction override | severity downgrade、escalation suppression、routine activity 誤分類 | Chapter 1、p.9 |
| Layer 1 の扱い | Tensor Trust 由来 regex が既知 technique を検査する | 高度な文書風 poison は signature を回避しうるため Layer 2/3 も用いる | Sections 2.1-2.3、pp.12-13 |
| 評価対象 | Layer 1 は4,000 sampled attacks で評価 | 5 poison types の250試験で drift を評価 | Sections 5.1-5.3、pp.19-20 |

## 解釈

- CoT poisoning は、明示的な命令違反よりも「内部方針らしさ」を利用して判断を変えるため、content filtering 単独より decision monitoring を必要とするという設計課題を示す。
- Prompt injection と CoT poisoning は排他的な分類とは限らない。論文の Layer 1 は prompt injection signatures を CoT poisoning 防御の一層として再利用している。
- RAG / GraphRAG / LLM Wiki の比較においては、検索方式の差に加え、取得知識の信頼性管理を別軸として持つ必要がある。

## 未解決課題

- 文書風 poison と明示的 prompt injection が混在する複合攻撃をどうラベル付けし、比較評価するか。
- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]]

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[concepts/rag_security|RAG security]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]]

## 矛盾

- 論文は両者を攻撃位置で区別する一方、Layer 1 では prompt injection patterns を CoT poisoning detection に用いる。実装上の検知シグナルは概念分類を完全に分離しない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Chapter 1、Sections 2.1-2.3、5.1-5.3、pp.9, 12-13, 19-20。
