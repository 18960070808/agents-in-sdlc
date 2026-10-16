---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共通のバックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成して動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理する反復型の開発に取り組みます。

## シナリオ

あなたは、開発者向けテーマのボードゲーム クラウドファンディングを提供する架空の企業 Tailspin Toys に新しく加わった開発者です。チームのバックログは GitHub Issue として登録済みなので、すぐに着手できます。フィルターやページネーションなどの機能開発に加え、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段使っている作業環境で利用できます。開発スタイルに合った環境を選んでください。各ワークショップには個別のセットアップがあり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、便利なスラッシュ コマンドを活用します。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内し、進捗を監視し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェント モード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
