# Lint Log

wiki-lint の実行範囲、検出事項、修正結果を記録する追記専用ログです。

## 2026-05-26 | schema / logs 再構成後のリンク点検

- Checked: `schema/` と `logs/` の必須ファイル存在確認、`wiki/` の `[[Wikiリンク]]`、`README.md`・`wiki/index.md`・比較ページ・schema 文書の相対 Markdown リンク、旧配置パスの不在
- Findings: 解決不能な Wiki リンクまたは Markdown リンクは確認されなかった。`AGENTS.md`、`wiki/templates/`、`logs/log.md` の旧配置は存在しない。
- Fixed: 旧配置を参照していた現行文書リンクを `schema/` と `logs/ingest.md` へ更新し、Query 履歴を `logs/query.md` に分離した。

## Entry Template

```markdown
## YYYY-MM-DD | <lint scope>

- Checked: <点検対象>
- Findings: <リンク切れ、孤立ページ、必須セクション不足など>
- Fixed: <修正内容、または none>
```
