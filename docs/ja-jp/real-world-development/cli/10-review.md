---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys で一連のワークフローを進めながら、GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと事前登録済みのフィルター Issue を確認しました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの Issue から始めて Plan モードで方法を決め、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、テスト範囲、スキルの実行結果、ブラウザーでの証拠を評価しました。
- フィルターの変更全体をレビューし、Agent Merge によるフィルター PR のマージを承認しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使うスラッシュ コマンドを確認しました。

## リリースしたもの

このワークショップには、更新済みの `main` から作成する別々のブランチで、2 つの PR マイルストーンがあります。

1. **星評価:** 既存の `starRating` と、ゲームカードに評価が設定されていないことを明示する表示を追加します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを追加します。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、作業全体を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を使った Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルでは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はそれらの結果を要件とテスト範囲のレビューとともにまとめました。

追加するテストは、実際の不足を補う必要があります。QA の実行で新しいテストが不要と判断されても、問題とは限りません。マージを承認する前にコードと証拠を確認し、変更後は影響を受ける証拠も更新してください。

## ベスト プラクティス

Copilot に提供するコンテキストとツールが、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは、会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールを接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方法を検討するには **Plan**、範囲を絞った変更で作業を確認しながら進めるには **Interactive**、範囲が明確なタスクを任せるには **Autopilot** を使います。定型的な編集では高速なモデル、複雑な作業ではより高性能なモデルを選びます。

ツールと同じくらい、コンテキストも重要です。*何を*構築するのか、*なぜ*必要なのか、*どのように*進めたいのかを明確に伝えると、出力は大きく変わります。

## さらに試す

コアとなるワークフローを学びました。Copilot CLI で試せる機能をいくつか紹介します。

- `/review` でコードレビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールでも、上達するには使い続けるのが一番です。本番コードや趣味のコード、何年も考えていたけれどまだ作っていない小さなアプリなどで使ってみてください。チームと学びを共有し、チームからも学びましょう。そして、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境と比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
