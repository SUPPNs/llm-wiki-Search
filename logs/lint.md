# Lint Log

wiki-lint の実行範囲、検出事項、修正結果を記録する追記専用ログです。

## 2026-05-26 | schema / logs 再構成後のリンク点検

- Checked: `schema/` と `logs/` の必須ファイル存在確認、`wiki/` の `[[Wikiリンク]]`、`README.md`・`wiki/index.md`・比較ページ・schema 文書の相対 Markdown リンク、旧配置パスの不在
- Findings: 解決不能な Wiki リンクまたは Markdown リンクは確認されなかった。`AGENTS.md`、`wiki/templates/`、`logs/log.md` の旧配置は存在しない。
- Fixed: 旧配置を参照していた現行文書リンクを `schema/` と `logs/ingest.md` へ更新し、Query 履歴を `logs/query.md` に分離した。

## 2026-05-26 | HITL ML ingest 結果の点検

- Checked: `raw/papers/human_in_the_loop_ml_system_architecture_2024.pdf` に対応する論文・概念・アーキテクチャ・未解決課題・テーマ候補ページ、`wiki/index.md` への登録、必須セクション、`[[Wikiリンク]]`、ページ付き引用、MLOps 記載有無
- Findings: 対象5ページは必須セクションを備え、索引に登録済みで、原資料本文に `MLOps` の語は確認されなかった。作業効率について、4.2 節の `67%`・`30%` と結論の `50%` の集計関係が説明されておらず、明示しないまま並記すると同一指標と誤読されうる。
- Fixed: [[papers/shimbo-2024-hitl-ml-system-architecture|論文ページ]]、[[concepts/human_in_the_loop_ml|概念ページ]]、`wiki/index.md` に指標の区別と未説明の集計関係を記録した。`raw/` と `schema/` は変更していない。

## 2026-05-26 | HITL ML ingest 最終再点検

- Checked: Git 差分による変更範囲、`raw/` と `schema/` の非変更、対象 PDF 関連ページへの限定、`logs/ingest.md` と `logs/lint.md` の記録、実体のある `[[Wikiリンク]]` と Markdown リンク、`30%`・`50%`・`67%` の記述
- Findings: `raw/` と `schema/` に差分はなく、wiki の差分は [[papers/shimbo-2024-hitl-ml-system-architecture|論文ページ]]、[[concepts/human_in_the_loop_ml|概念ページ]]、`wiki/index.md` に限定される。コード例・ログ雛形・schema テンプレートのプレースホルダとして表記されたリンク形式を除き、参照リンクの切れは確認されなかった。三つの作業効率値は集計関係が未説明であり、同一指標として断定されていない。
- Fixed: none（点検記録のみ追記）。

## 2026-05-26 | T5 知識グラフ fine-tuning ingest 点検

- Checked: `raw/papers/t5_knowledge_graph_finetuning_complex_tasks_2025.pdf` に対応する論文・概念・アーキテクチャ・比較・未解決課題ページ、`wiki/index.md` の登録、必須セクション、実体を参照する `[[Wikiリンク]]` と Markdown リンク、Git 差分による `raw/` と `schema/` の非変更
- Findings: 本文は図1による構成、表1のベースライン比較、表2のアブレーション、表3の知識グラフ規模別比較を備えるため、作成した5ページの種別判断には原文根拠がある。5ページは必須セクションを備え、今回追加・更新した実リンクにリンク切れは確認されなかった。既存の説明文およびログ雛形のリテラル表示例 `[[Wikiリンク]]`、`[[path/to/page|Page title]]` は参照検査から除外した。取り込み入力である未追跡 PDF は処理開始時から存在しており、`schema/` に差分はない。
- Fixed: `wiki/index.md` に独立領域、各ページへの導線、報告値の留保を追加し、取り込みページ間の相互リンクを整備した。

## 2026-05-26 | Enterprise RAG CoT poisoning ingest 点検

- Checked: `raw/papers/DetectingChainofThoughtPoisoninginLLMEnterpriseSystems.pdf` に対応する論文・概念4件・アーキテクチャ・比較・未解決課題・テーマページ、`wiki/index.md` 登録、必須セクション、実体を参照する `[[Wikiリンク]]`、ローカル Markdown リンク、Git 差分による変更範囲
- Findings: 論文は CoT poisoning の攻撃モデル、3層防御構成、tiered OR logic、評価結果を本文で示すため、作成した9ページの核となる事実に根拠がある。LLM-as-Judge の Layer 2 ラベル依存と NIST AI RMF alignment の検証範囲は `矛盾` および `未解決課題` に分離した。GraphRAG / Research LLM Wiki との接続は解釈・テーマ候補に限定した。
- Fixed: `wiki/index.md` に独立領域と synthesis 導線を追加し、各新規ページを相互リンクした。原資料は実在する `raw/papers/` 配下から読み取り専用で参照した。今回の操作による tracked な `raw/` / `schema/` 差分はない。検証時に表示された未追跡の `schema/templates.md` は本 ingest の編集対象外として触れていない。

## Entry Template

```markdown
## YYYY-MM-DD | <lint scope>

- Checked: <点検対象>
- Findings: <リンク切れ、孤立ページ、必須セクション不足など>
- Fixed: <修正内容、または none>
```
