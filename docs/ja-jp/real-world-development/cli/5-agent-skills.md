---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを入力するだけではありません。これまで、コードを手動で検証し、指示ファイルを使ってコーディング標準に従う方法を確認しました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうしたタスクには、**agent skill** が適しています。スキルを使うと、テストなどの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

Agent skill は、再利用可能なタスク指示、実行可能なスクリプト、サポート リソースをまとめ、必要に応じてエージェントが読み込みます。基本的な構成は、スキル名のフォルダーにある `SKILL.md` ファイルです。Markdown には、スキルの名前と説明を定義するフロントマター、スキルの概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、スキルが使うスクリプトやその他のリソース用のサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに配置すると、リポジトリのアセットとしてチームで共有して再利用できます。また、ユーザーのスキル フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェック用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明を使って、Copilot はスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の進め方が示されていることを確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で依頼することもできます。Copilot に 3 種類のチェックを実行してもらいます。

1. Interactive モードのフィルター会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを確認します。

## レポートをカスタマイズする

実行したチェック、成功したかどうか、ツールが報告した内容を把握できるレポートに改善しましょう。スキルを更新して、この形式のレポートを作成します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、実際に使ってみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを読み、最初のレポートと比較します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更はフィルター機能の PR に含まれます。次は [Playwright MCP サーバーを使って Copilot にサイトを操作してもらいます][next-lesson]。

## スキルの追加例

次のコミュニティの例は参考情報です。追加の演習ではありません。

- [Agent Skills specification][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
