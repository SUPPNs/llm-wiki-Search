---
title: "Chain-of-Thought poisoning"
type: concept
aliases: [CoT poisoning]
tags: [cot-poisoning, llm-security, enterprise-rag, adversarial-context]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: active
---

# Chain-of-Thought poisoning

## 概要

enterprise LLM が信頼して取得する文書コンテキストを操作し、もっともらしい説明を
伴ったままセキュリティ判断を誤らせる攻撃概念である。

## 事実

- 定義: 論文は CoT poisoning を、敵対的コンテキストが LLM の最終 security decision を導く reasoning pathway を corrupt するものとして定義する。Abstract、p.1。
- 攻撃面: knowledge base、internal wiki、SharePoint repository、vector database に注入された文書が、SOC artifact に見える偽 runbook や policy directive として取得されうる。Chapter 1、p.9。
- 影響例: ransomware alert の severity downgrade、data exfiltration の escalation suppression、unauthorized access の routine activity 誤分類が挙げられる。Chapter 1、p.9。
- 評価例: 5種類の controlled poison templates による250試験で `24.8% (62/250)` の behavioral drift が報告された。Sections 5.2-5.3、pp.19-20。

## 解釈

- CoT poisoning は、モデルの学習重みを直接汚染する攻撃ではなく、取得時に与えられる権威的な文脈を利用する runtime attack として本論文では扱われている。
- [[concepts/graphrag|GraphRAG]] で取得される知識構造が運用判断へ使われる場合にも類似の検証課題は考えられるが、本論文による実証対象は enterprise SOC の RAG 文書である。

## 未解決課題

- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]] - semantic manipulation を独立に判定する信頼性が未確定である。
- drift に現れない小さな優先度変化や説明根拠の歪みをどう測るか。

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/rag_security|RAG security]]
- [[concepts/behavioral_drift_detection|Behavioral drift detection]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[comparisons/prompt_injection_vs_cot_poisoning|Prompt injection と CoT poisoning の比較]]
- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]

## 矛盾

- `Chain-of-Thought` という名称を用いるが、論文の実装が観測・評価するものは retrieved context、出力判断、提供された reasoning trace である。内部推論状態への完全なアクセスを実証したものとは扱わない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Abstract、Chapter 1、Sections 5.2-5.3、pp.1, 9-10, 19-20。
