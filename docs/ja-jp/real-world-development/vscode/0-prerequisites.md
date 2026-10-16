---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code 演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業に使う [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ リポジトリをセットアップする

コードを作成するためのリポジトリのコピーを、[テンプレート][template-repository]から作成します。新しく作成したリポジトリにはラボに必要なファイルが含まれており、演習を通じて使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリのページで **Use this template** を選択して自分用のコピーを作成し、**Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えておきます。ラボの後半で参照します。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップ全体を通じてこれらの Issue を使います。自分で登録する必要はありません。

## Codespace を作成する

次に、ラボの演習を行う codespace を使います。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを書き、実行し、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする、機能が充実した IDE を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する画面](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する画面](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスをすべて手動でインストールするよりずっと短時間で済みます。その間に GitHub Copilot のほかの機能を試してみましょう。次の演習で使います。

> [!CAUTION]
> 今後の演習で codespace に戻ります。今は、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] 内で動作するように作られています。どちらも必要な前提条件がすべてインストールされ、スムーズに進められます。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択してください。VS Code が codespace と同じ dev container を構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能をインストールする必要があります。GitHub Codespace を使っている場合は、すでにインストールされているはずです。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、利用するには GitHub アカウントで認証が必要な場合があります。

1. Codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace の上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに `Hello world` のようなメッセージを入力して Enter キーを押します。Copilot Chat が起動します。
4. 認証されていない場合は、GitHub アカウントへのサインインを求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

ラボ リポジトリのコピーを作成できました。コードを書き始めるときに使う codespace の作成も始めました。

## 次のステップ

Copilot を使って作業を始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、プロジェクトの慣習を Copilot に伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使い方を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
