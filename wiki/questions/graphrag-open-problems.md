---
title: "GraphRAG の実装と評価に残る課題"
type: question
tags: [graphrag, rag, neo4j, langchain, evaluation]
source_files: [raw/blogs/graphrag_qiita_ksonoda_2024.md, raw/papers/graphrag-comparison.pdf]
last_updated: 2026-05-26
status: open
---

# GraphRAG の実装と評価に残る課題

## 概要

Neo4j と LangChain で構成する GraphRAG が、従来 RAG と比べてどの問いで有効であり、
抽出された関係を原文根拠へ追跡可能な形で保持できるかを検証する問いである。

## 重要性

研究テーマ探索に GraphRAG を用いる場合、誤った関係や失われた数値・条件は、
読むべき論文や研究空白の判断を誤らせる。実装可能性と証拠忠実性を同時に評価する
必要がある。

## 事実

- ksonoda (2024) は、Neo4j と LangChain を用いて、グラフ検索結果とベクトル検索結果を最終コンテキストへ統合するサンプルを示す。
- 同記事は、記事作成時点で `LLMGraphTransformer` が experimental であり、グラフ検索用 retriever を自作する必要があると説明する。
- 同記事の例は関係質問への回答を示すが、従来 RAG と同一条件で精度、費用、遅延、根拠忠実性を測る比較実験は提示しない。
- Anaguchi et al. (2025) は、QASPER において GraphRAG の結果が質問型により異なること、知識グラフ構築時に情報欠損が起こりうることを報告する。

## 解釈

- 実装デモを研究利用へ移すには、関係を辿る質問、数値・固有名を求める質問、広域要約を求める質問を分けた評価が必要である。
- グラフ経路が回答へ寄与した場合、参照したノード・関係と元チャンクを提示できる設計にすると、誤った抽出を発見しやすい。
- Neo4j に検索方式を集約する利点がある一方、グラフ構築と更新の費用はベクトル検索のみの基準と比較して測定する必要がある。

## 解決条件

| 項目 | 内容 |
| --- | --- |
| 必要な証拠 | 同一資料・同一質問での RAG と GraphRAG の出力、検索経路、原文根拠、費用・遅延 |
| 候補となる実験 / 調査 | 研究論文コーパスから関係型・局所事実型・要約型質問を作り、Neo4j 型 GraphRAG とベースライン RAG を比較する |
| 関連する評価指標 | 回答正確性、evidence recall、関係抽出精度、根拠忠実性、応答遅延、構築・更新コスト |
| 解決とみなす条件 | GraphRAG が有効となる質問条件と、誤抽出・欠損を検出または抑制する手順が実証される |

## 関係する研究テーマ候補

- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] - 本課題を研究文献探索の評価設計に落とし込む。
- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 内容証拠検索を推薦へ組み込む際にも同じ忠実性課題が生じる。

## 関連ページ

- [[concepts/graphrag|GraphRAG]] - 実装要素と設計上の位置付け。
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - この問いが生じる比較。
- [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較分析]] - 質問型別の性能差と KG 欠損の根拠。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - 根拠保持の詳細課題。

## 未解決課題

- グラフ検索を追加すべき質問を、取得前にどのように判定できるか。
- `LLMGraphTransformer` による関係抽出の誤りと、回答生成段階の誤りをどう分離して測定するか。
- 文献追加・訂正時に、Neo4j 上のどのノードと関係を再生成すればよいか。
- 検索経路が増えることによる費用・遅延を、回答品質の改善とどう比較するか。

## 矛盾

- 実装記事は GraphRAG が文脈取得を補えると説明するが、論文評価は質問型により GraphRAG の優位な構造が変わることを示す。この相違は用途と評価条件を揃えた検証が必要である。

## 情報源

- `raw/blogs/graphrag_qiita_ksonoda_2024.md` - Neo4j・LangChain の実装例と記事記載の未成熟点。
- `raw/papers/graphrag-comparison.pdf` - 質問型別比較と知識グラフ情報欠損の報告。
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - 比較観点の整理。
