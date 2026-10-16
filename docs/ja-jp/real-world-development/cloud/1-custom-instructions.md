---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot にタスクの内容だけでなく進め方も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- リポジトリ全体に適用する新しい標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code と CLI の harness とは異なり、ここでは変更前と変更後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムに並べて比較するのは難しいためです。この harness の後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ層には常に単体テストを用意します。
- UI はダークモードで、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発慣習に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。優先するコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI リクエストで役立つ技術スタック、開発内容の概要、全体に適用する指針など、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に指示を適用します。TypeScript や Astro などの言語、UI コンポーネントや単体テストなどのタスクに関するガイドラインを指定できます。

> [!NOTE]
> IDE で指示ファイルがコード生成に使われるのは Copilot Chat のみです。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと、`applyTo` フロントマターがある `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格を使った指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を網羅することはこのワークショップの範囲外ですが、サンプル プロジェクトには一般的な構成例があります。主な方針は次のとおりです。

- `copilot-instructions.md` には、開発内容、プロジェクトの構造、全体に適用するコーディング標準などのプロジェクト レベルの指針を記述します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関する指針を記述します。
- 自然な言葉で、分かりやすく例を示して指示します。

指示ファイルや AI の使い方に唯一の正解はありません。試しながら、自分のプロジェクトとシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意することをお勧めします。このプロジェクトの例には、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスクに関するファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境で操作は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。利用中の環境で使える操作は、該当するレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、コアとなる `copilot-instructions.md` と、各タスク用の `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、リポジトリのルートからの glob が設定されています。たとえば、`**/*.test.ts` に一致する TypeScript テスト ファイルに指示が適用されます。
5. 単体テストの作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、最後までスクロールします。`unit-tests.instructions.md` など、ほかの指示ファイルやプロジェクト内のファイルへのリンクがあります。これにより、大きな指示を小さな再利用可能なファイルに分け、Copilot が参照する例を指定できます (パスはリポジトリのルートではなく、指示ファイルの場所からの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ要件になっていません。次の手順で TSDoc のドキュメント コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリに新しい標準を追加する

ここで編集するのは、コード内のドキュメントを TSDoc のドキュメント コメントとファイル レベルのコメント ヘッダーとして追加するという、プロジェクト全体のルールです。後の演習で Issue を割り当てたとき、cloud agent はこの指示を読み取ります。

編集を始める前に、作業ブランチを作成します (cloud agent は Issue の対象ブランチにある指示を読み込みますが、ここでも変更を適切にコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えるファイルです。コーディング標準としてドキュメントを追加し、よりよいコード提案につなげます。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、インライン ドキュメントのルールはまだありません。このため、生成されたヘルパーにドキュメント コメントが含まれていませんでした。
3. 既存の標準の直後に、次の Markdown を追加して、ファイル コメント ヘッダーと TSDoc ドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体の指針には `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスクには `*.instructions.md` を使います。ドキュメント コメントのルールはプロジェクト全体に適用するため、リポジトリ レベルのファイルに記述します。

## 指示の更新後にプロンプトを再実行する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
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

新しいプロジェクト標準に従うよう Copilot を誘導し、次の演習で使う実際のコードに適用できました。

## 指示ファイルをコミットしてブランチをプッシュする

指示ファイルもほかのアセットと同様、ソース管理で管理します。指示の更新をリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と、Copilot がヘルパーの基盤として変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連ファイル (テストなど) が表示された場合は、それらも追加します。
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、指示の標準に関する Issue に関連付けたプルリクエストを作成します。手動で作成する代わりに、プロジェクトに接続済みの GitHub ツールを使い、Issue を検索して PR を作成し、Issue を閉じた後にマージするよう Copilot に依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートでは GitHub ツールがすでにワークスペースに接続されているため、セットアップは不要です。初めてツールを使うときに VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従ってください。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の変更と、新しいパブリッシャー ヘルパーの基盤について説明し、該当 Issue を閉じる PR にします。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索して PR を作成します。
5. GitHub の操作を実行するため Copilot が **Allow** を求めたら、コマンドを確認し、適切な場合に選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドの実行について Copilot が **Allow** を求めたら、コマンドを確認し、適切な場合に選択します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを読み込む方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示が適用された状態でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映される様子を確認しました。
- 指示ファイルとヘルパーの基盤をコミットし、次の演習で使えるよう `main` にマージしました。

次は[エージェント モード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使用する指示ファイル][instruction-files]
- [カスタム指示のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[awesome-copilot]: https://github.com/github/awesome-copilot
