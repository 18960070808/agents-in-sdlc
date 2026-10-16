---
title: "レッスン 2 - 星評価を追加する: すぐできる改善"
description: "Copilot CLI でゲームカードを小さく変更し、転送されたブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールして会話を試したので、次はプロジェクトに初めて変更を加えます。変更は小さくしましょう。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- 機能ブランチで Copilot の会話を開始します。
- エージェントに小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを実行し、転送されたブラウザーで確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲームの詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。今回はウォームアップとして、既存の評価を各カードに表示します。最初のセッションに適した、小規模で完結した変更です。

## 会話のしくみ

**会話**は Copilot CLI で作業する場です。Copilot app とは異なり、通常の CLI 会話では、専用の worktree を作るのではなく、ターミナルで現在チェックアウトしているリポジトリと Git ブランチを使います。保存した会話を後から再開できます。ファイルとブランチは通常の Git 状態としてディスクに残ります。

会話では、プロンプトとエージェントの応答、ファイルの調査や編集などのツール操作、`/diff` で確認できる変更の 3 つを扱います。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能の実装を依頼します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. まだの場合は、次のコマンドで Copilot を起動します。

   ```bash
   copilot --yolo
   ```

4. スラッシュ コマンド `/new` を実行し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調査し、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューしてください。Copilot CLI で変更を確認します。

1. `/diff` を実行し、変更されたファイルをすべて確認します。
2. 評価がある場合は数値を表示し、`starRating` が `null` の場合は `No rating yet` を表示することを確認します。
3. 両方の状態をテストで確認していることを確かめます。
4. Copilot が実行したチェックの結果をレビューし、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> Copilot を含む生成 AI ツールは決定論的ではなく確率的に動作するため、コードの詳細は異なる場合があります。特定の実装と一致するかではなく、動作を確認してください。

## 変更を確認する

コードを読むだけで動作すると判断せず、Copilot に Web サイトを起動してもらい、Codespaces から転送したブラウザーで UI を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに 5 段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能ができました。次は、変更をプロジェクトにマージするプルリクエスト (PR) を作成します。

1. 既定のエージェントに、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を実行します。Copilot CLI は PR 作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI に表示された URL を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。
4. 変更されたファイルとチェックをレビューします。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、初めての変更をリリースできました。具体的には、次のことを行いました。

- 機能ブランチで Copilot の会話を開始しました。
- ゲームカードに小さな変更を加えるようエージェントに依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送されたブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は、[フィルター Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
