---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは効率よく進めるため、フィルターと品質に関する作業全体を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びます。
- 機能全体の変更と検証の証拠を確認します。
- フィルターの PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作業を通じて、Copilot に機能を計画、実装、検証してもらいました。Tailspin Toys では、マージの承認権を開発者が維持しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、プルリクエストをマージするために残っている作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (継続的インテグレーション (CI) チェックの失敗、レビュー コメントへの対応、必要に応じたリベースなど) に対処し、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択しました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に、変更をレビューして準備ができていることを確認してください。

## Agent Merge を使って PR を管理する

すべてのコードを作成したので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を実行してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選択して <kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを実行して PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージが実行されます。
7. Copilot が `"PR #14 was squash-merged successfully."` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は、表示された理由を確認し、リポジトリで許可されている通常のレビュー済みマージを手動で行ってください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する方法を学びました。
- 機能全体の変更と検証の証拠を確認しました。
- フィルターの PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent への任意の委任に使える[便利な Copilot CLI のスラッシュ コマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
