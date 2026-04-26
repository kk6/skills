# kk6/skills

## 概要

コーディングエージェント用のskill管理リポジトリです。

```bash
gh skill install kk6/skills <skill> --agent claude-code
```

`--agent` 引数のデフォルト値は `github-copilot` です。Claude なら `claude-code` を、Codex なら `codex` を指定してください。

サポートされているエージェントの一覧は公式ドキュメント: [gh skill install](https://cli.github.com/manual/gh_skill_install)を参照してください。

また、`gh skill` の使い方も公式ドキュメントを参照してください。

- [gh skill](https://cli.github.com/manual/gh_skill)

## Skill 一覧

| Skill | Description |
| --- | --- |
| 5w1h-review | 文章をWho/What/When/Where/Why/Howの観点でレビューし、不足要素を表形式で示して具体的な修正案を提示する。進捗報告・議事録・障害報告・PR説明など、読み手向けの文章を送信前にチェックする用途。 |
| intent | Intent-Driven Development (IDD) に基づき、設計判断をADR（Architecture Decision Record）形式で `docs/adr/` 配下に連番で記録する。Why/Whatに整理しつつHow（実装詳細）の混入を検出・分離する。 |
| pr-description | プロジェクトの `.github/PULL_REQUEST_TEMPLATE.md` を読み取り、`git diff` と `git log` から変更内容を分析して、テンプレートに沿ったPR説明文を自動生成する。 |
| python-doc-sync | Pythonコードの変更後にdocstring・インラインコメント・README・`docs/` 配下のMarkdownが実装と乖離していないかをレビューし、最小限の修正を行う。公開API・挙動・設定・制約などの変更後に使用する。 |
