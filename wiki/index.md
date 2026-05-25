---
title: "Research LLM Wiki Index"
type: index
last_updated: 2026-05-26
---

# Research LLM Wiki Index

論文、概念、比較、問い、研究テーマ候補を横断して、研究の次の一手を見つけるための
統合入口です。ページを追加または大きく更新したときは、このカタログと
[../logs/log.md](../logs/log.md) を更新します。

## 現在の研究地図

| 研究領域 | 中心的な問い | 現在の到達点 | 次に読むページ |
| --- | --- | --- | --- |
| 学術文書 QA の GraphRAG 設計 | 質問型や証拠粒度に応じて、どのグラフ構造・検索経路が適するか | チャンク方式とコミュニティ方式は質問型により優位性が異なり、KG の情報欠損が課題として残る | [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] |
| GraphRAG の実装基盤と検証 | Neo4j と LangChain による GraphRAG を、従来 RAG と公平に比較し LLM Wiki に蓄積できるか | 実装例ではグラフ・全文・ベクトル検索を統合でき、LLM Wiki は成果を保存できるが、研究探索での精度・忠実性・費用評価は未実施である | [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] |
| 説明可能な推薦のグラフ拡張 RAG | 行動関係を使う BGR は推薦理由の説得力を高められるか | 化粧品推薦では VR+BGR が VR より推薦理由の評価で多く選択されたが、忠実性・偏り・専門利用者への適合が未検証である | [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索によるコスメ推薦理由]] |
| 行動証拠と内容証拠の統合 | BGR と知識グラフ型検索を組み合わせると、説得力と検証可能性を両立できるか | 両論文の課題を接続した研究テーマ候補を形成したが、統合効果は未実証である | [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] |
| 産業 ML における Human in the Loop | 運用中の人間による修正と再学習を、業務効果とモデル改善にどう結び付けるか | 腐食診断事例では作業負荷削減が報告されたが、収集した修正データによる再学習の精度改善は確認されなかった | [[architectures/hitl_ml_architecture|HITL ML 運用・開発連携アーキテクチャ]] |

## 読み始める順序

1. [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] で、2つの原資料が扱うグラフと評価目的の違いを把握する。
2. [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] で、検索方式と知識蓄積運用の役割差を確認する。
3. [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] で、文献探索へ適用するための評価条件を確認する。
4. [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] または [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] で、検証可能な研究方法へ落とし込む。
5. [[concepts/human_in_the_loop_ml|Human in the Loop Machine Learning]] と [[architectures/hitl_ml_architecture|HITL ML 運用・開発連携アーキテクチャ]] で、別領域の人間参加型運用課題を確認する。

## 研究領域別カタログ

### 領域 A: 学術文書 QA の GraphRAG 設計

学術論文本文を対象に、文書チャンクの局所証拠とコミュニティ要約の広域文脈を
どのように検索へ用いるかを扱う領域である。

#### concepts

- [[concepts/graphrag|GraphRAG]] - 知識グラフを用いた RAG の設計空間、Neo4j・LangChain 実装要素、BGR との境界を整理する。
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]] - 5W1H と談話関係を保持して局所証拠を辿る方式。
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]] - コミュニティ要約で広い文脈を与える方式。

#### comparisons

- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - QASPER における質問型別の性能差と構造上の制約を整理する。
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - 実装解説と論文評価を区別しながら、検索方式と知識蓄積運用の役割差を整理する。

#### questions

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 局所証拠と広域文脈の検索経路を切り替える条件を問う。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - KG 構築時の根拠喪失と回答忠実性を問う。
- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - Neo4j・LangChain 型の実装を研究探索で評価する条件を問う。

#### themes

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 内容証拠を保持する GraphRAG の課題を、推薦領域の統合テーマへ接続する。
- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] - 関係検索と原文根拠の追跡を同時に評価する。

### 領域 B: GraphRAG の実装基盤と追跡可能な評価

Neo4j と LangChain による GraphRAG の構築手順を出発点に、文献探索で必要となる
関係検索、根拠追跡、比較評価を扱う領域である。

#### concepts

- [[concepts/graphrag|GraphRAG]] - グラフ検索、Neo4j、LangChain の役割をまとめる。

#### comparisons

- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - RAG と GraphRAG の検索差、および LLM Wiki の蓄積役割を対照する。
- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - GraphRAG 内の表現方式差を評価結果とともに確認する。

#### questions

- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - 実装デモを研究評価へ移す条件を問う。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - 元文書根拠の保持を問う。
- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - グラフ検索を選択する条件を問う。

#### themes

- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] - 関係検索をテーマ探索に利用し、根拠への追跡可能性を評価する候補。

### 領域 C: 説明可能な推薦のためのグラフ拡張 RAG

レビューのベクトル検索に、レビューアーとアイテムの行動関係を加え、推薦理由の
わかりやすさや納得感を改善できるかを扱う領域である。

#### concepts

- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - レビューアーとアイテムの二部グラフから追加レビューを取得する方式。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - わかりやすさ、納得感、参考度を中心とする利用者評価観点。

#### comparisons

- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - BGR と知識グラフ型 GraphRAG が補う証拠と評価目的を対照する。

#### questions

- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]] - 説得力と根拠品質・露出偏りを同時評価する方法を問う。
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]] - レビュー証拠と商品内容証拠の統合を問う。

#### themes

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - BGR と内容証拠検索を併用する研究テーマ候補。

### 領域 D: 行動証拠と内容証拠の統合

上記二領域を接続し、レビュー由来の経験的証拠と、知識グラフ由来の検証可能な
内容証拠を一つの推薦理由生成へ統合する領域である。

#### concepts

- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - 行動証拠の検索経路。
- [[concepts/graphrag|GraphRAG]] - 内容証拠の構造化と検索に関わる設計空間。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - 統合方式を利用者側から評価する観点。

#### comparisons

- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - 統合仮説の根拠となる横断比較。

#### questions

- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]] - 中核となる設計課題。
- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]] - 統合方式の評価課題。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - 内容証拠の欠損課題。
- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 利用者・問いに応じた経路選択課題。

#### themes

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 現時点の中心的な研究テーマ候補。

### 領域 E: 産業 ML における Human in the Loop

化学プラントの腐食診断を事例に、人間の確認・修正を運用中の機械学習と
再学習へ接続し、業務効果とモデル改善を評価する領域である。

#### concepts

- [[concepts/human_in_the_loop_ml|Human in the Loop Machine Learning]] - 推論確認、誤り修正、再学習を結ぶ人間参加型運用。

#### architectures

- [[architectures/hitl_ml_architecture|HITL ML 運用・開発連携アーキテクチャ]] - AI サブシステム、マスタサブシステム、運転員、保守員、エンジニアの機能フロー。

#### comparisons

- 現時点で比較ページはない。資料本文は `MLOps` の語を用いていないため、MLOps との比較は未作成である。

#### questions

- [[questions/hitl-ml-open-problems|HITL ML の運用データと再学習をどう評価するか]] - 修正データ量、ラベル一貫性、画像品質と再学習効果を問う。

#### themes

- [[themes/hitl-industrial-inspection-operation-evaluation|HITL 産業検査における運用データと再学習の評価]] - 論文が記載した将来検討事項をまとめるテーマ候補。

## 論文カタログ

- [[papers/anaguchi-2025-graphrag-comparison|文書のチャンクに基づくGraphRAGと知識グラフのコミュニティ分類に基づくGraphRAGの比較分析]] - 学術論文 QA で2種の GraphRAG と RAG を比較する。
- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索を活用したRAGに基づく推薦理由の説得力の向上]] - レビュー二部グラフで化粧品推薦理由の説得力を評価する。
- [[papers/shimbo-2024-hitl-ml-system-architecture|HITL ML の運用のためのシステムアーキテクチャの定義と実践]] - 腐食診断の人間・AI 協調と再学習を含む運用アーキテクチャを報告する。

## 全ページカタログ

### concepts

- [[concepts/graphrag|GraphRAG]] - 知識グラフ検索と Neo4j・LangChain 実装要素を含む設計空間。
- [[concepts/chunk-based-graphrag|文書チャンクに基づく GraphRAG]] - 5W1H と談話関係を保持する方式。
- [[concepts/community-based-graphrag|コミュニティ分類に基づく GraphRAG]] - コミュニティ要約を用いる方式。
- [[concepts/bipartite-graph-retrieval|二部グラフ検索（BGR）]] - 行動関係を辿り推薦理由の証拠を増やす方式。
- [[concepts/persuasive-recommendation-reasons|推薦理由の説得力]] - 推薦理由の利用者評価観点。
- [[concepts/human_in_the_loop_ml|Human in the Loop Machine Learning]] - 人間の確認・修正を機械学習運用へ接続する概念。

### architectures

- [[architectures/hitl_ml_architecture|HITL ML 運用・開発連携アーキテクチャ]] - 腐食診断における人間確認、データ保管、再学習、モデル配置の構成。

### comparisons

- [[comparisons/chunk-vs-community-graphrag|チャンク方式とコミュニティ方式の GraphRAG 比較]] - QASPER における構造と性能差。
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - 回答時の検索方式と調査成果を蓄積する運用の違い。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - QA と推薦におけるグラフ追加の役割を対照する。

### questions

- [[questions/query-adaptive-graphrag-retrieval|質問特性に応じて GraphRAG の検索経路を選択できるか]] - 証拠粒度に応じた検索ルーティングを問う。
- [[questions/faithful-knowledge-graph-construction|GraphRAG の知識グラフ構築で情報欠損をどう測定し抑制するか]] - KG の根拠喪失を問う。
- [[questions/graphrag-open-problems|GraphRAG の実装と評価に残る課題]] - 実装可能性と公平な比較・監査を問う。
- [[questions/evaluating-persuasive-rag-recommendations|説得力の高い RAG 推薦を忠実性と公平性を含めてどう評価するか]] - 説得力と根拠品質・偏りを問う。
- [[questions/integrating-behavior-and-knowledge-graphs|行動二部グラフと知識グラフをどのように統合するか]] - 経験証拠と内容証拠の統合を問う。
- [[questions/hitl-ml-open-problems|HITL ML の運用データと再学習をどう評価するか]] - 修正データによるモデル改善の成立条件を問う。

### themes

- [[themes/hybrid-graph-rag-for-grounded-recommendations|根拠付き推薦のためのハイブリッド Graph-RAG]] - 行動証拠と内容証拠で説得力と忠実性の両立を目指す。
- [[themes/traceable-graphrag-for-research-discovery|研究テーマ探索のための追跡可能な GraphRAG]] - 検索経路と原文根拠を追跡してテーマ探索を評価する。
- [[themes/hitl-industrial-inspection-operation-evaluation|HITL 産業検査における運用データと再学習の評価]] - 論文記載の再学習・再アノテーション等の検討方向を整理する。

## 現時点の synthesis

### 事実

- Anaguchi et al. (2025) は QASPER の QA で、抽出型・要約型ではコミュニティ分類方式、Yes/No 型ではチャンク方式が最良となる結果を報告した。
- Arakaki・Fushimi (2026) は化粧品推薦で、VR+BGR の推薦理由が VR のみより、3例すべてでわかりやすさ・納得感・参考度について多く選択されたと報告した。
- BGR はレビューアーとアイテムの二部グラフを扱い、Anaguchi et al. の GraphRAG は文書由来の知識構造を扱う。
- ksonoda (2024) の解説記事は、Neo4j と LangChain を用い、テキストから抽出した関係のグラフ検索とベクトル検索を統合する GraphRAG サンプルを示す。
- 新保ほか (2024) は、AI 腐食診断システムに HITL の運用・開発連携アーキテクチャを実装し、炭素鋼配管で F 値 `69%`、人間の作業負荷 `50%` 削減を報告した。
- 同論文は、運用中の診断不一致 `110` 件による再学習ではモデル精度の改善が確認されなかったと報告した。

### 解釈

- グラフ拡張 RAG の設計は、単一の最適方式を求めるより、必要とする証拠の種類に応じて検索経路を選択・統合する問題として捉えるとよい。
- 現在の資料からは、BGR による経験的レビュー証拠と、知識グラフ型検索による内容証拠を組み合わせる推薦説明研究が有望な接続点となる。
- 実装例と学術 QA の報告課題を接続すると、文献探索で得た関係を原文根拠へ追跡可能にする GraphRAG の評価が新しいテーマ候補となる。
- Research LLM Wiki は、RAG や GraphRAG で得た比較・問い・テーマ候補を原資料へ結び付けて保存する知識層として位置付けられる。
- HITL 事例では、システム全体の業務効果と再学習によるモデル改善を分けて追跡することが研究上の焦点となる。

### 未解決課題

- 推薦理由の説得力改善を、根拠忠実性や人気偏りを損なわずに評価できるか。
- 行動二部グラフと知識グラフ型検索を、どの順序・粒度・利用者条件で統合すべきか。
- 知識グラフの情報欠損を抑えながら、質問または推薦理由に必要な具体的証拠を取得できるか。
- Neo4j・LangChain 型 GraphRAG が従来 RAG より有用になる質問条件と、追加コストに見合う根拠改善を測定できるか。
- HITL の修正データ量、ラベル一貫性、運用画像品質が、再学習結果と運用価値にどう影響するか。

## 矛盾

- BGR と知識グラフ型 GraphRAG はグラフ対象と評価タスクが異なるため、既存論文の報告数値を直接並べて方式の優劣を断定しない。
- ハイブリッド統合方式は研究テーマ候補であり、現時点では実証結果ではない。
- Qiita 記事の GraphRAG サンプルは実装解説であり、従来 RAG への一般的な性能優位を立証した比較実験とは扱わない。
- HITL 論文は運用と開発の連携を記述するが、`MLOps` の用語や MLOps との比較評価は本文に記載されていない。

## テンプレート

- [論文テンプレート](templates/paper.md)
- [概念テンプレート](templates/concept.md)
- [研究テーマ候補テンプレート](templates/theme.md)
- [未解決課題テンプレート](templates/question.md)
- [比較テンプレート](templates/comparison.md)
- [アーキテクチャテンプレート](templates/architecture.md)

## 情報源

- `../raw/papers/graphrag-comparison.pdf` - 学術文書 QA における GraphRAG 比較の原資料。
- `../raw/papers/bipartite-graph-rag-cosmetics.pdf` - 二部グラフ検索による RAG 推薦の原資料。
- `../raw/blogs/graphrag_qiita_ksonoda_2024.md` - Neo4j・LangChain を用いる GraphRAG 実装解説の保存資料。
- `../raw/papers/human_in_the_loop_ml_system_architecture_2024.pdf` - HITL を適用した AI 腐食診断アーキテクチャと運用結果の原資料。
- [[papers/anaguchi-2025-graphrag-comparison|GraphRAG 比較分析]] - 取り込み済み論文ページ。
- [[papers/arakaki-2026-bipartite-graph-rag-cosmetics|二部グラフ検索によるコスメ推薦理由]] - 取り込み済み論文ページ。
- [[comparisons/graph-augmented-rag-objectives|グラフ拡張 RAG の目的と評価の比較]] - synthesis の中核比較ページ。
- [[comparisons/rag_vs_graphrag|RAG・GraphRAG・LLM Wiki の比較]] - 検索方式と wiki 運用を接続する比較整理。
- [[papers/shimbo-2024-hitl-ml-system-architecture|HITL ML システムアーキテクチャ論文]] - HITL 領域の取り込み済み論文ページ。
