---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に具体的な情報を伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成の進め方を伝えるために使います。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を適用した状態で、フィルター用の最初のデータ層 (パブリッシャー ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映されることを確認します。
- 次の演習で Copilot が更新済みの指針を使えるように、指示の更新とフィルターの基盤を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力が一字一句一致することではなく、指示の更新後に動作が改善する傾向を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ層には常に単体テストを用意します。
- UI はダークモードで、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルにこれらの情報を記載し、Copilot がプロジェクトの慣習に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。優先するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、またはタスクに応じて特定のファイルに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI リクエストで役立つ技術スタック、開発内容の概要、全体に適用する指針など、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用します。TypeScript や Astro などの言語、UI コンポーネントや単体テストなどのタスクに関するガイドラインを指定できます。

> [!NOTE]
> IDE で指示ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格を使った指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を網羅することはこのワークショップの範囲外ですが、サンプル プロジェクトには一般的な構成例があります。主な方針は次のとおりです。

- `copilot-instructions.md` には、開発内容、プロジェクトの構造、全体に適用するコーディング標準などのプロジェクト レベルの指針を記述します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関する指針を記述します。
- 自然な言葉で、分かりやすく例を示して指示します。

指示ファイルや AI の使い方に唯一の正解はありません。試しながら、自分のプロジェクトやシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトの例には、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクに関するファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。操作は環境によって異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。利用中の環境で使える操作は、該当するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、指示ファイルが用意されています。変更を加える前に、既存の指示を確認して影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングの指針を確認します。この指示は、このリポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示を適用するファイルを決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。大きな指示を小さな再利用可能なファイルに分け、Copilot が参照する例を指定できます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ要件になっていません。次の手順で TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## 変更用のブランチを作成する

ここでは、ベスト プラクティスに沿って変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送信します。後で指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が意図したコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に知らせます。
3. Chat ビューのエージェント ドロップダウンで **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。多くの場合、ヘルパー ファイルとテストの両方が変更されます。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを取り、型付けされたパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の慣習に基づいています。
7. 提案されたコードには TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が改善することが、この演習のポイントです。

## リポジトリ全体に適用する新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コード提案の質を高めるため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールはまだありません。そのため、先ほど生成したヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に次の Markdown を追加し、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前の演習で確認したように、リポジトリ全体の指針には `.github/copilot-instructions.md` を、特定の言語やファイルの種類、タスクには `*.instructions.md` を使います。ドキュメント コメントのルールはプロジェクト全体に適用するため、リポジトリ レベルのファイルに記述します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話をクリアします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同じく、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示の更新とヘルパーの変更があることを確認します。
3. 指示の更新と、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、ドキュメント標準に関するバックログの Issue に関連付けてプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使い、Issue を見つけて PR を作成し、マージするよう Copilot に依頼します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub ツールがすでにワークスペースに接続されているため、セットアップは不要です。初めてツールを使うときに VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、指示の更新と新しいパブリッシャー ヘルパーの基盤を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索して PR を作成します。
5. GitHub の操作を実行するため Copilot が **Allow** を求めたら、コマンドを確認し、適切な場合に選択します。
6. PR が作成されたら、次のプロンプトを使い、PR をマージしてローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行について Copilot が **Allow** を求めたら、コマンドを確認し、適切な場合に選択します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを読み込む方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示を適用した状態でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再送信し、提案されたコードに新しい標準が反映されることを確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤をコミットして `main` にマージしました。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
