---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェント モードでフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで実際に動くことを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エディターの外に接続できると、エージェント モードはさらに強力になります。Model Context Protocol (MCP) は Copilot がその接続に使う標準で、エージェントが外部のツールやサービスと通信する方法を定めています。

![エージェント モードの内部動作と、MCP サーバーや VS Code 拡張機能が提供するツールを含めたコンテキスト、LLM、ツール間の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部のツールやサービスとリアルタイムにやり取りできます。リソースを使って最新情報にアクセスし、ツールを使ってユーザーに代わる操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、エージェントと外部ツール (既存の API や NPM パッケージなどのローカル ツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などをエージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を利用できます。Web ページの移動、フォームへの入力、ボタンの選択などをエージェントに依頼できます。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内の MCP サーバーを見つけやすくし、貢献を促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。使用する前にソース コードをよく確認し、発行元を確認して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する場合は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code が HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能のテストに使います。

1. `.vscode/mcp.json` で、`github` の隣に `playwright` エントリを追加します。ファイルは次のようになります。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してローカル マシン上で起動します。`--headless` フラグを指定すると、Playwright は画面にウィンドウを表示せずブラウザーを実行します。デスクトップ画面のない Codespace ではこの指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストですでに Playwright を使っているため、必要なブラウザーもインストール済みの場合がほとんどです。後で Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再試行します。

## サーバーを起動して信頼する

VS Code は、Copilot がサーバーのツールを初めて必要としたときに MCP サーバーを**オンデマンド**で起動し、そのサーバーを信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認された場合は、構成をレビューして信頼することを選択し、サーバーを起動できるようにします。

手動での起動は省略できます。その場合は次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開き、ページを移動し、フィルターを適用して、結果を読み取ってもらいます。会話を離れずに機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントは、支援技術と同じように、ボタン、リンク、リスト項目などのラベル付き要素を構造的に確認します。そのため、機能の簡易チェックをアクセシビリティの確認にも役立てられます。

サーバーを接続し、アプリを実行した状態で、Copilot に作成したフィルター機能を試してもらいます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーでブラウザーを起動し、各手順を実行して、確認できた内容を報告します。結果の要約を Issue の受け入れ基準と照らし合わせてください。問題があれば、プルリクエストを開く前に質問を続けるか、コードの修正を依頼します。

> [!NOTE]
> このテストではアプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止している場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動くことを確認できたので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot の変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更内容が表示された Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。今回は GitHub Copilot を使い、関連 Issue を見つけて紐づけた PR を作成してもらいます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンから **Agent** を選択し、Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を集めて操作を実行できるようにします。GitHub ツールを初めて使うときは、GitHub への認証を求められる場合があります。画面の指示に従ってください。
5. Copilot が Issue を検索して該当するものを見つけ、PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- リリース前に、Copilot に Playwright MCP を使わせてブラウザーでフィルター機能をテストしました。
- ブランチを公開し、フィルターのプルリクエストを作成しました。

MCP サーバーを機能の**テスト**に使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティのレビューなど、範囲を絞ったタスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
