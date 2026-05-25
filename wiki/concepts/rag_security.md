---
title: "Enterprise RAG security"
type: concept
aliases: [RAG security]
tags: [rag, enterprise-security, knowledge-base-poisoning, soc]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: active
---

# Enterprise RAG security

## 概要

内部文書を retrieval context として LLM 判断に利用する enterprise RAG において、
knowledge base の汚染、判断変化、検知・対応の統制を扱うセキュリティ観点である。

## 事実

- 論文は enterprise security operations の LLM が internal knowledge bases、SOC runbooks、policy documents、historical incident data を RAG によって reasoning process に組み込むと説明する。Chapter 1、p.9。
- 攻撃者が knowledge base へ poisoned documents を注入できるがモデル、system prompts、検知コードは変更できないという threat assumption が置かれている。Section 1.2、p.10。
- 提案システムは real-time にトリアージ判断を監視する Python-based LLM Security Gateway として構成され、FastAPI integration gateway、ensemble orchestration、NIST AI RMF alignment documentation を含む。Section 4.1、p.17。
- Section 3.3 は、system monitoring、risk response、incident tracking に関する NIST AI RMF との alignment を目的として記載する。p.16。

## 解釈

- RAG security では、検索精度だけでなく「取得資料を判断根拠として信頼してよいか」を監視する必要がある。
- [[concepts/graphrag|GraphRAG]] は検索表現が異なるが、運用に用いる知識が汚染されるリスクを考える際には、取得ノード・関係・要約の provenance と検知可能性が追加の研究論点になりうる。本論文はこの拡張を実験していない。
- Research LLM Wiki は原資料を不変に保持し、矛盾を記録する知識運用であり、production の RAG gateway ではない。ただし、根拠追跡と矛盾保存という運用思想はセキュリティ調査の整理に有用である。

## 未解決課題

- enterprise knowledge base の provenance、承認履歴、取得時の trust score を runtime detection とどう統合するか。
- GraphRAG のノード・エッジ・community summary が汚染された場合の detection surface をどう定義するか。
- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]]

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]
- [[concepts/graphrag|GraphRAG]]
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]]

## 矛盾

- 本論文の対象は SOC triage 向け RAG であり、GraphRAG や Research LLM Wiki に対する実験結果はない。これらへの接続は研究上の展開候補であり、実証済みの安全性主張ではない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Abstract、Chapter 1、Sections 3.3、4.1、pp.1, 9-10, 16-17。
