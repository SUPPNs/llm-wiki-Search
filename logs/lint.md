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

## Entry Template

```markdown
## YYYY-MM-DD | <lint scope>

- Checked: <点検対象>
- Findings: <リンク切れ、孤立ページ、必須セクション不足など>
- Fixed: <修正内容、または none>
```
