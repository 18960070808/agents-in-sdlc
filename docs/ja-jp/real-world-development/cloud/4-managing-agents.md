---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー エクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期で自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理するためのツールはいくつかあります。たとえば、GitHub.com の[agents ページ][agents-page]では、オープンなプルリクエスト (PR) があるエージェント タスクを確認できます。操作内容を調査し、進行中のセッションに指示を追加して、作業を誘導することもできます。

このレッスンでは、次のことを行います。

- agents ページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに誘導を加え、機能を追加するよう依頼します。

## シナリオ

チームはエージェントにハイコントラスト モードを作成するよう依頼した後、ライトモードも追加するとよいと気付きました。サイトのスタイルと切り替え機能をすでに更新するので、ライトモードも一緒に実装するのが合理的です。エージェントの作業を誘導し、ハイコントラスト モードに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左側のペインに表示されるタスクの一覧を確認します。次のように、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを更新するための API を生成する。
   - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーをセットアップする。
   - 割り当てたタスクを完了するために、さまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況やアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを表示し、作業の進捗を監視できます。

## Cloud agent を誘導する

実行中のタスクを確認したので、エージェントがハイコントラスト モードを実装している間にライトモードも追加するよう依頼します。

1. ハイコントラスト モードを追加するセッションを選択します。Copilot が付けたタイトルや作業状況によって、正確なタイトルは異なります。

    ![Mission control に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページの cloud agent タスクと、Steer active session while Copilot is working ダイアログが強調表示された画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に反映する様子を確認します。

## Copilot の作業を待つ

Copilot は、更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業に取り込まれます。

作業には数分かかります。その間、ここまでに学んだことを振り返ってみましょう。

## まとめと次のステップ

Copilot cloud agent タスクを監視し、誘導する中心的な場所である Copilot agents ページについて学びました。この mission control を使うと、進行中と完了済みのタスクを追跡し、作業内容を確認し、進行中のタスクを変更できます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションを誘導して機能を追加するよう依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は[Copilot が作成したプルリクエストを反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
