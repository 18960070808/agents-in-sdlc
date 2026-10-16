---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター操作を確認しました。次に、明確な QA プロセスを持つカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター Issue に照らした結果をレビューします。

## シナリオ

Tailspin Toys はプルリクエスト (PR) を作成する前に、要件、コードの品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントで QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルでは、エージェントの目的、指示、利用可能なツールを説明します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまで使用したカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し可能なチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、Copilot がこれらの機能を使って要件を評価し、結果を報告する方法を指定します。各機能を置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを作成するよう Copilot に依頼します。このプロファイルでは、QA が実行するチェックと、従うべき境界の両方を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA ワークフローと境界を取り込んだか確認します。これにより、検証だけを頼んだつもりなのに、エージェントが機能を変更してしまう事態を防げます。

1. `/diff` を実行し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、追加するとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件から確認を開始し、リポジトリの指示に従い、`quality-checks` スキルを実行して Playwright MCP を使うことを確認します。
4. QA が証拠を示し、実装コードを変更する前に許可を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. いずれかの責務や境界が不足している場合は、続ける前に既定のエージェントにプロファイルの修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクト エージェントを読み込みます。プロファイルを作成した後、フィルターの会話を再開して QA を選択します。これにより、すでに会話のコンテキストにある Issue と計画の決定事項を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されないことがあります。作成は完了しているため、上記のコマンドで有効にできます。

2. 次のプロンプトを使って、QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、レポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門家の役割を追加し、その結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター Issue に照らした結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は[機能 PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
