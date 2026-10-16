---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな変更でも、通常は複数のファイルを更新し、テストなどの作業を行う必要があります。開発者は必要なファイルを探して変更し、テストを実行し、問題をデバッグし、変更し忘れたファイルを見つけてさらに更新するという流れで作業することがあります。作業は次々と続きます。

ここで Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE でより自律的に動作するように設計されています。開発者と同じように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。Agent Mode を使ってサイトに機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる体験を提供します。エージェント モードは IDE 内でタスクを実行し、短いフィードバック サイクルと対話を可能にします。cloud agent はチーム メンバーのように非同期に作業し、Issue とプルリクエストを通じてやり取りするペア プログラマーです。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを見つけて、連携した変更を行います。
- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリとパブリッシャーでゲームを絞り込めるようにします。前の演習でパブリッシャー ヘルパーを追加しました。次は、データ層、UI、テストの残りの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを操作して、現在の機能を確認しましょう。

この Web サイトは、開発者向けテーマのボードゲーム クラウドファンディング プラットフォームです。利用者はゲームの一覧を表示し、各ゲームの詳細を確認できます。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページでは、Drizzle ORM を介してフロントマターからローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後なので、`main` ブランチにいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザー ウィンドウを開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace でターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace で Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動的に作成されます。GitHub Copilot に内容を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、ファイルにまたがる変更を自律的に計画・実装し、ターミナル コマンドやツールを実行します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順でもこのモデルを指定しています。比較的安定した体験を提供できることを期待していますが、LLM は確率的に動作するため、説明と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使う場合がありますが、タスク自体は完了できるはずです。

3. 次のプロンプトを Copilot に送信し、バックログの Issue を確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者に最も役立つと考える項目から順に、優先順位を付けたことを確認してください。

## 指示ファイルを確認する

エージェントにコードを生成してもらう前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトに機能を追加する際の指針が記載された [UI (ユーザー インターフェイス) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - ファイルの種類に応じた指示へのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルからほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。これにより、大きな指示を再利用可能な小さなファイルに分け、例やテンプレートを参照できます。

## フィルター機能を実装する

フィルター機能を完成するには、少なくとも次の 3 つを更新する必要があります。

- データ層 (`src/lib/`) に残りのフィルター ロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

また、すべてをコードベースにマージする前に、テストが成功することを確認します。Copilot Agent Mode にこれらの作業を任せられます。機能を追加しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** を選択し、指示ファイルを追加します。

   ![UI 指示ファイルの選択例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストが見つかり、その後ファイルの変更とテスト実行が始まります。

   ![Copilot がプロジェクトのファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更してテストを実行するなど、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかります。その間、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。終了すると、すべての変更に対して **Keep** または **Undo** を選択できるようになります。

7. Copilot に促されたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテストの実行を確認するダイアログが表示されたスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中にしばらく待たされたり、テストが失敗したりする場合があります。問題ありません。Copilot はエラーを検出しなくなるまで、コード生成とテストの実行を繰り返します。

   ![Copilot Agent Mode の完了した Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を試します。
11. すべて動作することを確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys に機能を追加する方法を学びました。この演習では、次のことを行いました。

- GitHub Copilot Agent Mode で UI とデータ層にまたがる新機能を実装しました。
- Copilot Agent Mode でプロジェクトを調査し、関連ファイルを見つけて、連携した変更を行いました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 – ページネーションを実装する

ゲーム一覧が増えてくると、ページネーションが必要になります。この演習で学んだスキルを使い、サイトにページネーションを実装するよう Copilot に依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、ベスト プラクティスに従います。
- 利用者がページ サイズを選べるようにするか、値を固定するかなど、ページネーションの方法を検討します。
- 希望する実装に必要な情報を Copilot に伝えるプロンプトを作成します。
- GitHub Copilot に変更を依頼し、コンテキストを伝えながら反復する必要があるかもしれません。これは Copilot を使うときの通常の進め方です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
