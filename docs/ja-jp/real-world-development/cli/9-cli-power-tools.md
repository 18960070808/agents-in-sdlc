---
title: "レッスン 9 - GitHub Copilot CLI の便利な機能"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの優れた CLI ツールと同様に、GitHub Copilot CLI には多くのスラッシュ コマンドがあります。高度な機能、内部の動作に関する情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローを完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに近づくことがあります。必要に応じて Copilot CLI は会話を自動的に圧縮します。スラッシュ コマンドを使えば、状態の確認や手動での圧縮もできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル名、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分けられたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が小さい場合は大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウがいっぱいになると、自動的にコンテキストを圧縮します。タイミングを指定する場合は `/compact` を使います。関連のないタスクに切り替え、新しい会話を始める場合は `/clear` または `/new` を使います。

## モデルを選択する

モデルごとに異なる長所があり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択します。画面を閉じるには <kbd>Esc</kbd> を押します。

## セッションを共有する

チームで協力して学びを共有すると、全員が AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストールしたバージョンで `/share` を使う方法を確認します。
2. セッションを共有する場合は、用途に合った出力先を選びます。たとえば `/share file` を使うと Markdown ファイルとしてローカルにエクスポートできます。
3. エクスポートした内容を、誰かに送信したり公開したりする前にレビューします。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## 省略可能: 並行作業や委任を使う

コアとなるワークショップを完了しました。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分けて並行実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期に作業させてプルリクエストを作成できます。

これらのコマンドを使うと、追加の worktree やリモート作業が発生する可能性があるため、任意の内容です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

利用できるスラッシュ コマンドはほかにもあり、Copilot CLI にはまだ多くの機能があります。[学んだ内容を振り返り][next-lesson]、今後さらに学ぶためのステップを確認しましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
