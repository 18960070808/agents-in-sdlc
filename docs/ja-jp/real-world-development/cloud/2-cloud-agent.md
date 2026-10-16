---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残り続けている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行するよう設計されています。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかる作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コードを生成する環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Copilot に Issue を割り当てる方法。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作成した請負業者はドキュメントを十分に整備しておらず、実際にはほとんど存在しません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加したいと考えています。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリのほかのタイトルである関連ゲームを表示し、利用者が引き続きゲームを探せるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現在は作業を妨げている課題ですが、優先度の高い Issue がほかにあります。

どちらも優先順位が下がりやすいタスクの例であり、Copilot cloud agent に割り当てるのに適しています。Copilot cloud agent が非同期に作業している間、開発者はほかの作業に集中できます。後で戻って、成果をレビューし、要件どおりかどうかを確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][start-copilot-sessions]など、複数の方法で作業を依頼できます。割り当て後、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境をセットアップしてタスクに取り組みます。作業中や完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省くことはできません。一見単純な作業でも、詳しく見ると複雑さが隠れている場合があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に設定してください][cloud-agent-best-practices]。AI ペア プログラマーとして段階的に作業し、学び、試し、調整してください。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot のガイドとしての役割を確認しました。カスタム指示を扱うハンズオンをすでに完了している場合は、`.github/copilot-instructions.md` の効果を確認したはずです。まだの場合は、今のうちに簡単に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認すると、後からその影響を見つけやすくなります。

リポジトリの GitHub Web UI、または実行中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメント コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが含まれるか確認します。これらは指示ファイルによって適用されます。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent 用の開発環境を準備する

誰がコードを書く場合でも、通常は特定の環境とセットアップが必要です。依存関係をインストールし、すべてが正常な状態であることを確認します。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア エンジニアと同じように作業します。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、cloud agent が作業を開始する前に実行する開発ツールや依存関係をカスタマイズできます。ラボを進めやすくするため、この設定はあらかじめ用意されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用済みの SQLite データベースを使用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定してください。Copilot は操作用に独自のトークンを受け取ります。
    permissions:
      # 依存関係をインストールするなど、セットアップ手順でリポジトリをクローンする場合は contents: read が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテスト用に、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様に見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストに取り組む前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するまで待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントが重要だと誰もが理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体がなかったりします。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しいチーム メンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で成果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行)、プロジェクトを分析して、問題に取り組むための方法を検討しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、画面を更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot が問題に取り組む様子を確認します。コードを調査して状況を理解し、計画を検討して適切な方法を決め、コードを作成するプロセスを確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を活用し、別のタスクも cloud agent に割り当ててから、アプリケーションに機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームのセクションを追加する

前述のとおり、GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加するのに時間がかからないとしても、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

  ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示されます。Copilot が作業を開始したことを示しています。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように動くため、作業を常時監視する必要はありません。完了した後にレビューできます。ここからはカスタムエージェントの作成と使用を学びます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期に作業を実行させることができます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに利用できます。

この演習では、次のことを確認しました。

- コードを生成する環境をカスタマイズする方法。
- 操作を安全に行う方法。
- Issue の範囲を明確にする重要性。
- Copilot に Issue を割り当てる方法。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用を学びましょう。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も適用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[start-copilot-sessions]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
