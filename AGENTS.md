# Research LLM Wiki - Agent Instructions

このリポジトリは、論文研究のテーマ探索に使う知識ベースである。LLM は原資料を
保存する担当ではなく、原資料から追跡可能な知識ページを育てる担当として振る舞う。

## Core Layers

```text
raw/       IMMUTABLE: 人間または収集工程が配置した元資料
wiki/      LLM-EDITABLE: 整理、比較、研究テーマ、問いのページ
logs/      APPEND-ONLY: 重要な wiki 更新の記録
scripts/   OPTIONAL: 後日追加する検査・補助スクリプト
```

## Hard Rules

1. `raw/` は変更禁止である。LLM は `raw/` 以下のファイルを作成、編集、移動、
   改名、削除してはならない。読み取りと `Sources` での参照のみ許可する。
2. `wiki/` は LLM が編集してよい。編集前に `wiki/index.md` と関連する既存
   ページを読み、新規作成より既存ページの更新を優先する。
3. 知識ページでは `Facts`、`Interpretations`、`Open Questions` を明確に分ける。
   資料が直接裏付けない主張を `Facts` に書いてはならない。
4. 研究テーマ候補は `wiki/themes/` に保存する。
5. 未解決課題は `wiki/questions/` に保存する。
6. `wiki/` 配下の各知識ページには `## Sources` セクションを必ず置き、
   根拠となる `raw/` のパス、URL、または他の根拠ページを列挙する。
7. 既存の記述と新しい根拠が矛盾する場合、古い記述を黙って削除しない。
   `## Conflicts` に相違、根拠、解消に必要な確認事項を記録する。
8. 重要な追加、統合、矛盾の発見、テーマ候補の状態変更は `logs/log.md` に
   追記する。既存ログは書き換えない。
9. 関連ページへの参照は Obsidian 形式の `[[Wikiリンク]]` を使う。
10. 確認できない内容は断定せず、`Interpretations` または
    `Open Questions` に置き、必要な根拠を示す。

## Repository Map

| Directory | Page purpose |
| --- | --- |
| `wiki/papers/` | 論文ごとの問題設定、方法、結果、限界、研究上の接続 |
| `wiki/concepts/` | 概念、手法、データセット、指標、理論的枠組み |
| `wiki/themes/` | 実行可能性を検討する研究テーマ候補 |
| `wiki/questions/` | 未解決課題、矛盾、追加検証が必要な問い |
| `wiki/comparisons/` | 複数の方法、論文、設定を同一軸で比較する表 |
| `wiki/templates/` | 新しい知識ページを作るときの雛形 |

## Required Page Shape

知識ページには、ページ種別に応じた雛形を使う。最低限、次の意味を持つ
セクションを保つ。

| Section | Meaning |
| --- | --- |
| `Summary` | ページの対象と研究探索上の意味の短い説明 |
| `Facts` | 出典から直接支持できる記述。項目ごとに根拠を対応付ける |
| `Interpretations` | 根拠に基づく整理、含意、仮説。事実として扱わない |
| `Open Questions` | 調査や実験で確かめる必要がある問い。可能なら `wiki/questions/` に接続 |
| `Connections` | `[[Wikiリンク]]` による論文、概念、テーマ、比較への接続 |
| `Conflicts` | 食い違う主張や証拠。なければ `- None identified.` を残す |
| `Sources` | 原資料の場所と、その資料が支える範囲 |

## File Naming And Links

- 新規ファイル名は原則として小文字の `kebab-case.md` を使う。
- 表示名が必要なリンクは
  `[[themes/efficient-evaluation|Efficient Evaluation]]` の形式にする。
- ページのタイトルや用語を変更する場合は、参照元ページのリンクも確認する。
- 同義語で別ページを増やさない。既存ページに別名や用語差分を記録する。

## Workflow: Ingest A Source

1. `AGENTS.md`、`wiki/index.md`、関連しそうな既存ページを読む。
2. 指定された `raw/` 資料を読み、引用可能な事実、LLM の解釈、残る問いを分ける。
3. 該当する既存の論文・概念・テーマ・問い・比較ページを更新する。該当ページが
   ない場合のみ `wiki/templates/` の雛形を基に新規作成する。
4. 新しい関連を `Connections` の `[[Wikiリンク]]` で追加する。
5. 異なる主張が見つかった場合は、関係するページの `Conflicts` に記録する。
6. 新規ページまたは大きく変えたページを `wiki/index.md` に登録する。
7. 重要な知識更新を `logs/log.md` に日付付きで追記する。

## Workflow: Develop Research Themes

1. `wiki/questions/` の未解決課題と `wiki/comparisons/` の差分を確認する。
2. 新しいテーマ候補は `wiki/themes/` に作成し、対象問題、仮説、貢献候補、
   検証方法、必要資源、失敗条件を記す。
3. テーマ候補は必ず根拠となる論文・概念・問いに `[[Wikiリンク]]` で接続する。
4. 実現性や新規性がまだ検証されていない内容は `Interpretations` または
   `Open Questions` として扱う。

## Workflow: Maintenance Check

更新後に可能な範囲で次を確認する。

- 新しい知識ページが `wiki/index.md` に載っているか。
- ページに `Sources`、`Connections`、`Conflicts` があるか。
- `Facts` と `Interpretations` が混同されていないか。
- 追加した `[[Wikiリンク]]` の対象ページが存在するか、今後作成する問いとして
  明示されているか。
- 重要な変更が `logs/log.md` に記録されているか。

## Templates

- 論文ページ: `wiki/templates/paper.md`
- 概念ページ: `wiki/templates/concept.md`
- 研究テーマ候補ページ: `wiki/templates/theme.md`
- 未解決課題ページ: `wiki/templates/question.md`
- 比較ページ: `wiki/templates/comparison.md`

## Design Source

この規約は [Pratiyush/llm-wiki](https://github.com/Pratiyush/llm-wiki) の
`raw/` を不変資料、`wiki/` を LLM が管理する知識、`AGENTS.md` をエージェントの
スキーマとする設計を、研究テーマ探索用のページ種別へ適用したものである。
