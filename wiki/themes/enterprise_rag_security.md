---
title: "Enterprise RAG の推論汚染検知と運用ガバナンス"
type: theme
tags: [enterprise-rag, cot-poisoning, soc, nist-ai-rmf, llm-security]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: exploring
---

# Enterprise RAG の推論汚染検知と運用ガバナンス

## 概要

SOC トリアージ向け enterprise RAG で、取得知識の汚染により LLM 判断が操作される
リスクを検知し、analyst review、incident tracking、監査可能性へ接続する
研究テーマ候補である。

## 中心となる研究課題

retrieved context を介した CoT poisoning に対し、provenance 管理と層状 runtime
detection を統合すると、SOC の見逃しリスクと analyst workload を両立して改善
できるか。

## 事実

- Nemure (2026) は enterprise SOC triage の RAG knowledge base に対する poisoned document injection を攻撃モデルとして扱う。Chapter 1、pp.9-10。
- 同論文の5 poison types による250試験では behavioral drift `24.8% (62/250)` が報告された。Tables 5.2-5.3、pp.19-20。
- regex、behavioral drift、LLM-as-Judge を tiered OR logic で統合した ensemble は precision `95.7%`、recall `90.0%`、F1 `92.8%` と報告された。Table 5.4、p.21。
- 著者は system monitoring、risk response、incident tracking に関する NIST AI RMF alignment を設計目的として示す。Section 3.3、p.16。
- 本論文は SOC の RAG pipeline を対象とし、GraphRAG や Research LLM Wiki を評価対象にはしていない。

## 解釈

- 仮説: document provenance と runtime ensemble detection を併用すれば、判断変化の検知だけでなく、どの取得資料が security decision を汚染したかを analyst が追跡しやすくなる可能性がある。
- 貢献候補: CoT poisoning の検知精度だけでなく、SOC review workload、監査証跡、response time を含む運用評価枠組みを提示すること。
- 現時点で注目する理由: 論文は attack surface と検知性能を報告した一方、judge の独立評価、knowledge provenance、SOC における実運用効果は検証余地が残る。
- Research LLM Wiki との接続: LLM Wiki は production defense ではないが、原資料・解釈・矛盾・問いを分離して蓄積し、セキュリティ研究課題の根拠追跡を支援できる。
- GraphRAG との接続: graph-based retrieval の知識要素が判断へ使われる設定では provenance と poisoning detection が課題になりうるが、別途実験が必要である。

## 検証方針

| 項目 | 計画 |
| --- | --- |
| 対象設定 | SOC triage の enterprise RAG、および追加条件として provenance 付き retrieval / graph-based retrieval |
| 提案手法 | 文書 provenance 検査、behavioral drift、独立 judge、tiered escalation の統合 |
| ベースライン | regex のみ、drift のみ、judge のみ、論文の3層 OR ensemble |
| 評価指標 | attack recall、precision、F1、false alarm、latency、analyst review 件数、根拠追跡成功率 |
| 必要なデータ / 資源 | poison 付き SOC triage cases、専門家ラベル、retrieval provenance、複数 judge 条件 |
| 反証条件 | 追加統制が ensemble 単独より見逃しを下げず、false alarm または運用負荷を許容不能に増加させる |
| リスク | judge の共通 failure mode、汚染された baseline、実 SOC データ取得の困難さ |

## NIST AI RMF / SOC workflow との関係

### 事実

- 論文は NIST AI RMF に沿った monitoring、risk response、incident tracking を設計意図として述べる。Section 3.3、p.16。
- gateway は analyst judgment を置き換えるのではなく、手動確認すべき判断の優先付けを支援すると説明される。Section 3.2、p.15。

### 解釈

- 研究では、NIST alignment の宣言に留めず、flag / block の記録、analyst override、poisoned source の隔離、incident resolution までを監査可能に測定する必要がある。

## 未解決課題

- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]] - ensemble の semantic layer を運用へ採用するための前提である。
- RAG knowledge base および GraphRAG の知識構造について、provenance と integrity check をどの粒度で持つべきか。
- NIST AI RMF と SOC playbook の接続を、監査可能な評価指標でどう実証するか。

## 関連ページ

- [[papers/cot_poisoning_enterprise_rag|Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications]]
- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[concepts/rag_security|RAG security]]
- [[concepts/behavioral_drift_detection|Behavioral drift detection]]
- [[concepts/llm_as_judge|LLM-as-Judge]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[comparisons/prompt_injection_vs_cot_poisoning|Prompt injection と CoT poisoning の比較]]
- [[concepts/graphrag|GraphRAG]]
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]]

## 矛盾

- 論文が示す NIST AI RMF alignment は設計上の位置付けであり、運用監査または規格適合の検証結果ではない。
- GraphRAG と Research LLM Wiki への展開は本テーマの解釈・研究計画であり、原論文の実証結果ではない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Abstract、Chapters 1-6、Tables 5.1-5.5、Appendix A、pp.1, 9-25。
