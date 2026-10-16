---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で進め方に合意し、Autopilot で Issue に基づくフィルターを構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まずプロジェクトに小さな機能を追加しました。しかし、大きな変更にはより堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できるため、適切な方法で必要なものを構築できます。このレッスンから数回にわたり、Issue を基に新機能を実装し、コードの妥当性と機能の動作を確認し、プロジェクトにマージするまでの一般的なエージェント型開発プロセスを実践します。

> [!NOTE]
> 機能の作業では同じ会話とブランチを使い続けます。通常は変更するファイルの種類ごとに別のブランチや PR を用意しますが、ここでは主要な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を進めるなかで、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、利用者がカテゴリとパブリッシャーでゲームを絞り込めるようにします。バックログの Issue に機能の説明はありますが、カテゴリを組み合わせる場合など、コーディングの前に合意しておきたい詳細があります。Plan モードで決定事項を整理し、その後、範囲を決めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ重要性は増しています。多くの開発者は、次のような流れで作業します。

1. 実装内容を説明する Issue から始めます。
2. 構築する内容を計画します。
3. コードを構築してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合は上記の流れを応用したものです。

この標準的なアプローチに沿って進めることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ確認プロセスを経るようにできます。

## 会話モード

**会話モード**によって、エージェントにどこまで自律的に作業させるかを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。計画中はプロジェクト ファイルを編集できません。
- **Autopilot**: エージェントがコードを作成してテストを実行し、タスクが完了するまで自律的に反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルターの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に引き渡す前に機能の実装方法やタスクの進め方を定義できます。複雑な作業では時間をかけて計画するのがお勧めです。Plan モードに切り替えて、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue の参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はまずプロジェクトを調査し、その後、適切なアプローチを検討して計画を作成します。

3. フィルターの動作について Copilot から質問されたら、希望に基づいて回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決定した内容と異なる部分があれば、修正を依頼します。

## Autopilot を承認する

計画を書いてレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot がタスクの完了を判断するまで問題に取り組み、反復できます。

1. **Accept plan and build on autopilot (recommended)**、またはインストールしたバージョンで同等の名前のオプションを選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って作業し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。必ず先に計画を確認してください。Copilot が依存関係の不足やポート競合を報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにレビューしてからマージします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** や **Autopilot** がないことを確認します。
2. `/diff` を実行し、フィルターの実装とテストを確認します。
3. 結果を Issue と計画時の決定事項に照らし合わせます。
4. コードレビューが終わったら、<kbd>Esc</kbd> を押して差分ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば修正を依頼します。

## 新機能を試す

コードは問題なさそうですが、実際に動作するか確認しましょう。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ フィルター、パブリッシャー フィルター、その組み合わせを試します。
4. リセットと結果が空の場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]コードが文書化された規約に沿って生成されるようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
