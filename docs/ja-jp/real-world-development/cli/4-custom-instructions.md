---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを進めたい場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードの内容だけでなく、構成方法も説明します。フィルターを実装したので、次は Copilot が使った指示を確認し、ドキュメント標準を追加して、自分のコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示がエージェントに読み込まれる仕組みを確認します。
- コーディング標準に従うため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- コメントではコードの内容を繰り返すのではなく、意図や分かりにくい判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その引数も文書化します。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を文書化し、関連するコードが変更されたときはコメントも更新します。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルにこうした情報を記載して、Copilot がプロジェクトの慣習に沿ってタスクを進められるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。優先する規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI リクエストで役立つ技術スタック、開発内容の概要、全体に適用する指針など、プロジェクト全体の情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、タスクに関する指針を指定します。

> [!NOTE]
> ほかの指示形式とサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## プロジェクトの指示ファイルを確認する

スターター プロジェクトには指示ファイルが用意されています。変更を加える前に、現在の指示を確認して影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングの指針を確認します。これらの指示は、このリポジトリでのすべての Copilot とのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドには、指示を適用するファイルを決める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分割し、Copilot が参照できる例を示せます。

## チームの指針に合わせて指示ファイルを更新する

指示ファイルはよい出発点ですが、まだ改善できます。コアとなる `copilot-instructions.md` を変更し、新たに生成する TypeScript に TSDoc コメントを追加するようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します (35 行目あたりです)。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を読み込むため、機能のコンテキストを失わないようにフィルターの会話を終了して再開します。

1. フィルターの会話が開いている場合は `/exit` を実行して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   指示された場合はフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に指針を適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行し、変更された TypeScript ファイルを読みます。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを読み込む方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスに応じた指示がエージェントに読み込まれる仕組みを確認しました。
- コーディング標準に従うため、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
