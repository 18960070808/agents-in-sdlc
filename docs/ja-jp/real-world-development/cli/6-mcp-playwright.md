---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーでフィルター機能を操作して確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前述のとおり、コードを書くことはコードを入力するだけではありません。データや外部サービスを扱い、Copilot が利用できる追加の自動化も用意します。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、CLI に組み込まれている機能を超えて、Copilot にツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認します。
- Playwright MCP サーバーがまだ使えない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

単体テストとエンドツーエンド テストは重要ですが、UI の更新を検証するには実際に操作してみる必要があります。Copilot が利用者のように Web サイトを操作できるようにし、変更が期待どおりに動作することをより確実にします。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部ツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムに通信できます。最新情報にアクセスし、ユーザーに代わって操作を実行できます。

これらのツールとリソースには MCP サーバーを介してアクセスします。MCP サーバーは AI エージェントと外部ツールやサービスの間の橋渡しをします。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内の MCP サーバーを見つけやすくし、貢献を促すために [MCP registry][mcp-registry] をホストしています。

> [!CAUTION]
> MCP サーバーは、ほかの依存関係と同じように扱ってください。使用する前にソース コードを確認し、発行元を確認して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーをまだ使えない場合は追加し、Copilot が利用者のように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに構成している場合は、セットアップ手順を飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類に **STDIO** を指定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおりに動作するか手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って、同じように確認します。

1. 次のプロンプトを使って、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーの利用方法を判断するため、MCP サーバーを使うように指定する必要はありません。ただし、使うツールが分かっている場合は明示すると、より一貫した結果を得やすくなり、トークンも節約できます。

2. 機能が動作するか確認するため、ブラウザーでさまざまな手順を実行する Copilot の様子を確認します。
3. 報告を読み、すべてが期待どおりに動作することを確かめます。

Copilot はサーバーを起動して Playwright で Web サイトを操作し、サーバーを停止して結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで Playwright MCP サーバーを使い、機能を確認できました。このレッスンでは次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使用方法を確認しました。
- Playwright MCP サーバーがまだ使えない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
