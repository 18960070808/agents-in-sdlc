---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、事前登録済みのフィルター Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールとやり取りできます。タスクを任せたり、変更を依頼したりしながら、コマンド ラインから作業に集中できます。まずはツールをインストールしましょう。すでに使い慣れたツールでインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーでフィルターの Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に利用するターミナルでエージェントの機能を使えます。このレッスンでインストールと認証を済ませ、残りのワークショップで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示されたら、デバイス コードを使った認証手順に従い、Copilot CLI を認証して許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認して、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めてください。

## 使い方を確認する

通常のシェル プロンプトで実行したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語の入力がエージェントに送られ、スラッシュ コマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストールしたバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次のような簡単な質問を Copilot に送り、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答の前にリポジトリを調査することを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトで、フィルターの Issue を見つけるよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて Issue を読みます。最初の変更を終えた後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチで作業します。自動的に worktree を作成することはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、既定の許可範囲外のツールを使う前に確認を求めます。このワークショップでは、ユーザーが承認したショートカット `--yolo` を付けて CLI を再起動し、Codespace 内で承認を求めるプロンプトを省略します。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの隔離された環境でのみ使い、日常的な開発の既定値として alias に設定しないでください。詳しくは[ツール使用の許可と拒否][allow-all-warning]を参照してください。

後のレッスンで Issue やプルリクエストに対して作業するために、`--enable-all-github-mcp-tools` を付けると、この Codespace で GitHub MCP の読み取り / 書き込みツールが有効になります。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは引き続き実際のデータです。変更を公開またはマージする前に確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについてもう一度簡単な質問を送り、会話が動作することを確認してから、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に `copilot --resume` を使うと、同じ機能の会話とブランチに戻れます。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーでフィルターの Issue を見つけました。

次は[最初の範囲を絞った変更を行い][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
