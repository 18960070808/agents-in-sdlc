---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定タスクや領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、対象の作業をより効果的に行うための指示、ベスト プラクティス、コーディング パターン、領域固有の知識を Copilot に提供します。チームの専門知識を再利用可能なエージェントにまとめることができます。たとえば、[WCAG][wcag] 準拠を確かめるアクセシビリティ エージェント、安全なコーディング慣習に従うセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、またはグローバルな `~/.copilot/agents` に定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントと agent skill の比較

カスタムエージェントと [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義し、AI に操作方法を伝えます。簡潔に区別すると、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを調整して作業できます。このラボのアクセシビリティ カスタムエージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローのスキルやテストを実行・管理するスキルなどを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法に唯一の正解はありません。AI を活用するときと同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを行います。

- カスタムエージェントの定義方法を確認します。
- カスタムエージェントにタスクを割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるように取り組んでいます。利用者から、現在のダークテーマはテキストと背景のコントラストが十分でなく、読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザイン チームは利用者が切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。そこでカスタムエージェントを使って機能を生成します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントがすでに作成されています。内容を確認し、Copilot の動作への影響を把握しましょう。

Codespace に戻り、アクセシビリティ カスタムエージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` のフィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` のフロントマターが必要です。

3. 続くセクションに目を通し、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成するときの主な責務。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と誤り。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、作業中のセッションに追加の指示を出せます。まず、ハイコントラスト モードを作成するタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントと Accessibility が強調表示された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。カスタムエージェントは、特定のタスクや領域に合わせた AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot が対象の作業をより効果的に行えるようにします。

この演習では、次のことを確認しました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て方。

Copilot がハイコントラスト モードを実装している間に、mission control から[エージェント セッションを監視して誘導する][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
