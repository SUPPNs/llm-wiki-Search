---
title: "RAG・GraphRAG・LLM Wiki の比較"
type: comparison
tags: [rag, graphrag, llm-wiki, neo4j, langchain, comparison]
source_files: [raw/blogs/graphrag_qiita_ksonoda_2024.md, raw/papers/graphrag-comparison.pdf]
last_updated: 2026-05-26
status: active
---

# RAG・GraphRAG・LLM Wiki の比較

## 概要

RAG と GraphRAG は、質問時に根拠を取得して回答へ渡す検索・生成方式である。
これに対し、本リポジトリにおける LLM Wiki は、原資料から整理した知識、比較、
未解決課題、研究テーマ候補を Markdown と Wiki リンクで継続的に蓄積する運用である。
三者は競合する単一方式ではなく、研究テーマ探索では組み合わせて使える。

## 比較範囲

- GraphRAG 実装解説: `raw/blogs/graphrag_qiita_ksonoda_2024.md`
- GraphRAG 評価論文: [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較分析]]
- LLM Wiki 運用定義: [../../README.md](../../README.md), [../../AGENTS.md](../../AGENTS.md)
- 関連する概念: [[concepts/graphrag|GraphRAG]]
- 関連する研究テーマ候補: [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]]

## 事実

| 比較軸 | RAG | GraphRAG | Research LLM Wiki |
| --- | --- | --- | --- |
| 主目的 | 質問に関連する原文チャンクを取得して回答に利用する | テキスト検索に加え、抽出した関係を辿って回答コンテキストを補う | 原資料から得た知識、比較、問い、テーマ候補を蓄積し再利用可能にする |
| 処理の中心 | 質問時のベクトル検索、必要に応じた全文検索 | 質問時のベクトル・全文・グラフ検索 | 取り込み時と問い合わせ後の Markdown ページ更新、リンク付け、索引更新 |
| 保存する表現 | チャンクと埋め込みを検索基盤に格納する構成が一般的 | チャンク、埋め込み、ノード、関係をグラフ基盤に格納できる | `raw/` の不変資料と、`wiki/` の人間可読な整理ページ |
| 関係の扱い | 類似チャンクから暗黙に読み取る | ノードとエッジを検索対象として辿る | `[[Wikiリンク]]`、比較表、未解決課題、研究テーマページとして明示する |
| 役立つ場面 | 一つの問いに対し該当記述を取得する | 複数概念・関係をまたぐ問いを検索する | 調査を越えて発見、比較、矛盾、次の研究問いを残す |
| 例となる出力 | 質問への回答と参照チャンク | 質問への回答とグラフ由来の関連コンテキスト | [[concepts/graphrag|概念ページ]]、本比較、[[questions/graphrag-open-problems|未解決課題]]、[[themes/traceable-graphrag-for-research-discovery|研究テーマ候補]] |
| 主な注意点 | 類似検索で必要な関係や文脈を取りこぼす可能性 | KG の誤抽出・欠損、構築費用、質問型による性能差 | 整理が古くなる、事実と解釈が混ざる、リンク切れや索引漏れが生じうる |
| この wiki での根拠 | ksonoda (2024) の RAG 説明、Anaguchi et al. (2025) のベースライン | ksonoda (2024) の実装例、Anaguchi et al. (2025) の比較結果 | `README.md` と `AGENTS.md` に定義された現行運用 |

## 位置付けの違い

| 層 | 役割 | 本 wiki での具体例 |
| --- | --- | --- |
| RAG | 原文をその場で検索して回答を支える | PDF や記事の該当記述を質問に応じて取得する |
| GraphRAG | 関係構造を検索に加え、関連する根拠へ到達する | GraphRAG と BGR、質問型と検索方式の接続を探索する |
| LLM Wiki | 検索・分析の成果を整理して次の調査へ引き継ぐ | 比較を `wiki/comparisons/`、問いを `wiki/questions/`、テーマ候補を `wiki/themes/` に保存する |

## 解釈

- LLM Wiki は RAG や GraphRAG の代替検索エンジンではなく、両者を用いた調査結果を検証可能な形で積み上げる知識層である。
- 研究テーマ探索では、RAG が原文根拠の回収、GraphRAG が概念間関係の発見、LLM Wiki が発見の蓄積と比較・問いへの変換を担う構成が考えられる。
- GraphRAG がなくても LLM Wiki は運用できるが、ページ数が増えるほど、既存ページ間の潜在的な関係や矛盾を見つける補助として GraphRAG 的探索が有用になる可能性がある。
- LLM Wiki の品質は回答精度だけでは測れず、情報源の追跡可能性、リンク接続、矛盾の保持、テーマ候補への発展性で評価する必要がある。

## 未解決課題

- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - LLM Wiki の資料を対象に検索方式を評価できるか。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - wiki ページや原資料の根拠をグラフ化する際の欠損課題。
- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 原文検索と関係探索の切り替え課題。
- LLM Wiki に保存した研究テーマ候補の有用性を、後続調査や専門家判断でどのように評価するか。

## 関連ページ

- [[concepts/graphrag|GraphRAG]] - 定義と Neo4j・LangChain 実装要素。
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - 論文評価に基づく GraphRAG 内部比較。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - QA と推薦におけるグラフの役割。
- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - 三者を接続する際の評価課題。
- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] - LLM Wiki を評価対象にもできる研究テーマ候補。

## 矛盾

- ksonoda (2024) は実装チュートリアルであり、GraphRAG が RAG より高精度であることを定量的に比較した資料ではない。Anaguchi et al. (2025) の質問型別結果をもって、すべての用途における一般的優位性とも解釈しない。
- Research LLM Wiki は本リポジトリの運用設計であり、RAG または GraphRAG と同一の評価タスクを実験比較した結果はまだない。

## 情報源

- `raw/blogs/graphrag_qiita_ksonoda_2024.md` - RAG と GraphRAG の説明、Neo4j・LangChain サンプル、実装上の留保。
- `raw/papers/graphrag-comparison.pdf` - RAG と 2 種の GraphRAG の QASPER 比較結果。
- [../../README.md](../../README.md) - Research LLM Wiki の目的、二層構造、運用サイクル。
- [../../AGENTS.md](../../AGENTS.md) - `raw/` 不変、`wiki/` 更新、事実・解釈・未解決課題の分離とリンク運用規約。
- [Qiita: GraphRAGをわかりやすく解説](https://qiita.com/ksonoda/items/98a6607f31d0bbb237ef) - 保存済み記事の公開元。
