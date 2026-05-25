---
title: "Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications"
type: paper
authors:
  - Tendai Nemure
year: 2026
venue: "Yeshiva University, Master of Science in Cybersecurity Capstone"
tags: [cot-poisoning, enterprise-rag, llm-security, soc, defense-in-depth]
source_files: [raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf]
last_updated: 2026-05-26
status: ingested
---

# Detecting Chain-of-Thought Poisoning in Enterprise LLM Applications

## 概要

SOC のセキュリティトリアージに用いる enterprise LLM / RAG を対象に、取得された
内部文書が推論判断を誤らせる Chain-of-Thought (CoT) poisoning を検出する
3層防御と ensemble risk scoring を設計・評価した修士 capstone 論文である。

## 研究上の位置付け

- 問題設定: 内部 runbook、policy document、analyst note、過去 incident data を利用する RAG 型トリアージでは、取得コンテキストが攻撃面になりうる。
- 主張される貢献: regex pattern detection、behavioral drift detection、独立した LLM-as-Judge を組み合わせる runtime detection framework の設計と評価。
- 関連する研究テーマ候補: [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]

## 事実

- 著者は CoT poisoning を、敵対的コンテキストがモデルの最終的なセキュリティ判断を導く reasoning pathway を破損させるリスクとして位置付ける。Abstract、p.1; Chapter 1、p.9。
- RAG knowledge base、internal wiki、SharePoint repository、vector database が侵害されると、正当な SOC 文書に似せた runbook や policy directive を注入でき、LLM が危険な誤分類を生成しうると説明される。Chapter 1、p.9。
- 防御は、既知の攻撃パターンを検知する regex 層、clean context と poisoned context の分類差を検出する behavioral drift 層、別の LLM による reasoning validation 層から構成される。Abstract、p.1; Chapter 2、pp.12-14。
- ensemble risk scoring は tiered OR logic を用い、1層が発火すれば medium risk として flag、2層以上が一致すれば high risk として block する。Section 2.5、p.14。
- Layer 2 は、50件の security triage cases と5種類の poison による250試験で、`24.8% (62/250)` の behavioral drift を検出した。Sections 5.2-5.3、pp.19-20。
- Layer 1 は Tensor Trust 由来の4,000攻撃サンプルに対して `58.6%` の detection rate、平均 `0.14 ms`、clean security cases で `2%` 未満の false-positive rate を報告する。Section 5.1、p.19。
- LLM-as-Judge は機能要件表で precision `97.4%`、recall `76.0%`、F1 `85.4%` と報告される。Table 5.4、p.21。
- 最終 ensemble は precision `95.7%`、recall `90.0%`、F1 `92.8%` と報告される。Abstract、p.1; Table 5.4、p.21; Section 6.1、p.23。
- 本システムは NIST AI Risk Management Framework の monitoring、risk response、incident tracking に関連する統制との整合を意図して設計されたと述べられる。Section 3.3、p.16。

## 主張

- enterprise RAG の取得文書を利用した CoT poisoning は、SOC トリアージ判断を変化させうる現実的な攻撃面である。
- 単一検知器ではなく、コンテキスト、行動変化、reasoning integrity を対象にした層状監視が実運用上有用である。

## 手法

| 層 | 対象 | 処理 | 根拠 |
| --- | --- | --- | --- |
| Layer 1 | 入力コンテキスト | Tensor Trust 由来94パターンの regex signature matching | Section 2.1、p.12 |
| Layer 2 | モデル行動 | clean / poisoned context での severity・escalation 判断差を検出 | Section 2.2、pp.12-13 |
| Layer 3 | 推論整合性 | 独立 LLM が alert、retrieved context、reasoning trace を評価 | Sections 2.3-2.4、p.13 |
| Ensemble | 3層の信号 | 1層発火で flag、2層以上で block | Section 2.5、p.14 |

## 評価

| 項目 | 内容 |
| --- | --- |
| トリアージケース | CICIDS2017 由来20件、MITRE ATT&CK 由来20件、edge / ambiguous 10件 |
| poison 種類 | deescalation bias、false positive framing、authority hijacking、policy contradiction、statistical manipulation |
| Layer 2 結果 | `24.8% (62/250)` behavioral drift |
| Layer 3 結果 | precision `97.4%`、recall `76.0%`、F1 `85.4%` |
| Ensemble 結果 | precision `95.7%`、recall `90.0%`、F1 `92.8%` |
| 運用設計 | FastAPI integration gateway、ensemble orchestration、NIST AI RMF alignment documentation |

## 限界

- 攻撃者は knowledge base に文書を注入できるが、モデル、system prompt、検知コードは変更できないという前提に限定される。Section 1.2、p.10。
- dataset の代表性は、50件のケースが一般的な SOC workflow を十分表すという仮定に置かれている。Section 1.2、pp.10-11。
- LLM-as-Judge の ground truth は Layer 2 の drift detection に基づくため、drift を生じない推論操作の評価範囲は不明である。Section 2.4、p.13。
- Layer 1 は単独目標 `>=70%` に達せず `58.6%` であり、ensemble の一要素として許容された。Section 5.1、p.19。

## 解釈

- 本論文は、RAG の検索品質ではなく、取得コンテキストを信用して判断する運用が生む security risk を主題にする。
- [[concepts/graphrag|GraphRAG]] にも外部・内部知識を取得する層があるため、将来はグラフ化された runbook や関係情報の汚染検知が検討対象になりうる。ただし、本論文は GraphRAG を直接評価していない。
- Research LLM Wiki は原資料と解釈を分けて蓄積する仕組みであり、enterprise RAG knowledge base の runtime defense と同一の製品防御機構ではない。

## 今後の研究課題

- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]] - Layer 2 由来ラベルへの依存と独立性を評価する必要がある。
- poison が decision drift を生じさせないまま根拠や調査優先度を変える場合をどう検知するか。
- NIST AI RMF との整合を、SOC における監査可能な運用指標と incident response 手順でどう検証するか。

## 未解決課題

- [[questions/is_llm_as_judge_reliable|CoT poisoning 検知に LLM-as-Judge は信頼できるか]]

## 関連ページ

- [[concepts/chain_of_thought_poisoning|Chain-of-Thought poisoning]]
- [[concepts/behavioral_drift_detection|Behavioral drift detection]]
- [[concepts/llm_as_judge|LLM-as-Judge]]
- [[concepts/rag_security|RAG security]]
- [[architectures/defense_in_depth_rag_security|Enterprise RAG security の3層防御アーキテクチャ]]
- [[comparisons/prompt_injection_vs_cot_poisoning|Prompt injection と CoT poisoning の比較]]
- [[themes/enterprise_rag_security|Enterprise RAG の推論汚染検知と運用ガバナンス]]
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]]

## 矛盾

- 論文は Layer 2 drift detection を Layer 3 評価の ground truth として用いる一方、drift しない reasoning manipulation を捉えられるかは示していない。LLM-as-Judge の信頼性を独立に確定した結果とは扱わない。
- 論文は NIST AI RMF との alignment を述べるが、第三者監査や適合性評価の実施結果は記載していない。

## 情報源

- `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` - Abstract、Chapters 1-6、Tables 5.1-5.5、Appendix A、pp.1, 9-25。
