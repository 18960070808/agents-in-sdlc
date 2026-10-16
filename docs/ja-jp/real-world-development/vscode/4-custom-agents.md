---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、Copilot が特定の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、領域固有の知識を与えます。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディング慣習に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントと agent skill の比較

カスタムエージェントと [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキル (ほかのエージェントを含む場合もあります) を調整して作業できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI を活用するときと同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるように取り組んでいます。利用者から、現在のダークテーマはテキストと背景のコントラストが十分でなく、読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザイン チームは利用者が切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。そこでカスタムエージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに作成されています。内容を確認して、Copilot の作業への影響を把握しましょう。

Codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチに切り替えます (演習 3 のフィルター PR と分けて作業します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` のフィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と誤り。

## Copilot Chat でカスタムエージェントを使う

VS Code では `.github/agents` に定義された各カスタムエージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。エージェントを選択すると、その指示とツールを使うようにチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** がまだ選択されていない場合は選択します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部のエージェント ドロップダウンを選択します。現在のエージェントが表示されます。既定では **default** です。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに、次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルに適用します。編集内容はファイル パスと差分とともにチャットに表示されます。
6. 変更が加わると **Files changed** インジケーターが更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリに実際の変更を加えます。Astro コンポーネントや CSS、関連するテストなど、既存ファイルも編集します。

次の演習では、作業中の変更をレビューしてエージェントに追加の指示を出します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや領域に合わせた AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot が対象の作業をより効果的に行えるようにします。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェント モードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して誘導し][next-lesson]、進行中の変更をレビューしながら同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
