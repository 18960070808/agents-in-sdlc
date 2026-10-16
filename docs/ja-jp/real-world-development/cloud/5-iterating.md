---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot に複数のタスクを依頼しました。コードにドキュメントを追加し、デザイン チームが検討する関連ゲーム機能を構築し、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能を実装しました。コードの変更を確認し、必要に応じて Copilot にフィードバックして改善しましょう。

### シナリオ

前述のとおり、生成 AI を加えてもソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体のレビューを有効にする前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監視を受けずにタスクを実行するため、安全を保つための制約が設けられています。

- Copilot はリポジトリへの読み取りアクセス権を持ち、コード作成に使うブランチに対してのみ書き込みアクセス権があります。
- Cloud agent は GitHub Actions 内で動作し、作業用に一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- 既定では、[外部リソースへのアクセスが制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) で、コードにドキュメントを追加します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの呼び出しを含む外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation**、または同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ってください。数分かかることがあるため、休憩したり、ここまでに学んだことを振り返ったりできます。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく更新されたコードを確認します。TSDoc のドキュメント コメントなどが含まれています。具体的な変更は異なります。

    変更を確認しながら、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルから適用されます。

6. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。実行結果は確率的に異なるため、必ず同じ文面を使う必要はありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、そのファイルの簡単な説明をコメント ヘッダーとして追加します。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. Copilot に変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** とメンションします。上記のいずれかの例や、コードベースに追加してほしいドキュメントの提案を使います。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が変更のために新しいセッションを開始することに注目してください。
3. **Back to pull request** を選択して PR に戻ります。

    ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択し、変更をレビューします。

納得できるまで反復して改善してください。準備ができたら、ドラフト PR をレビュー準備完了にして、`main` ブランチにマージできます。

![PR をレビュー準備完了にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちであることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。先ほどと同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーと TSDoc のドキュメント コメントを追加したりできます (この Issue はカスタム指示を更新する**前**に割り当てました)。前と同じように、**Conversation** タブに新しいコメントを追加すると、Copilot が内容を確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタムエージェントで実装した機能をレビューします。この PR には演習 3 で依頼したハイコントラスト モードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替えるトグル UI コンポーネント。
   - ユーザー設定の保存に使うローカル ストレージ。
   - ハイコントラスト モードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 変更を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちであることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが登録されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラスト モードとライトモードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタムエージェントは、Copilot がアクセシビリティのベスト プラクティスに沿って機能を実装するのに役立ちます。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** に変更を依頼できます。

## 省略可能な演習 — 委任を続ける

実際のバックログ項目をエージェントに任せ、ほかの作業に集中できると cloud agent の効果が高まります。習慣づけのため、リポジトリにいくつか Issue を追加し、Copilot に割り当ててみましょう。次のようなアイデアがあります。

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを追加します。
- データ アクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent harness を完了しました。次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の出力に反映される様子を確認しました。
- **Copilot cloud agent に Issue を割り当て**、環境のセットアップ、計画、非同期の作業を確認しました。
- アクセシビリティ用の**カスタムエージェントを作成して使用し**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使い**、進行中のアクセシビリティ セッションを監視して誘導しました。
- **cloud agent のプルリクエストをレビューして反復的に改善し**、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent harness を完了しました。さらに学びたい場合は、次の harness で同じ内容を別の環境で試せます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan モードと Autopilot モード、agent skill、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る省略可能な `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテスト カバレッジに関する Issue を cloud agent に割り当てます。
- 別の領域を担当するカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
