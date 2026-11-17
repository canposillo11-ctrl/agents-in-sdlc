---
title: "演習 0: Visual Studio Code の前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Visual Studio Code (VS Code) の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [Codespace][codespaces] を起動して、エディターで GitHub Copilot Chat を使えることを確認します。

## ラボ用リポジトリを設定する

コードを作成するリポジトリのコピーとして、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいインスタンスにはラボに必要なファイルが含まれます。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボ リポジトリの **Use this template** ボタンを選択し、**Create a new repository** を選択してコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えてください。この後の演習で参照します。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使います。自分で Issue を作成する必要はありません。

## Codespace を作成する

次に、ラボの演習で使用する Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードを書き、実行し、デバッグできるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする統合開発環境 (IDE) を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

4. Codespace の作成には数分かかります。次の演習でも使うため、ブラウザーのタブで開いたままにします。

Codespace を使うと、開発サービスを手動でインストールするよりも短時間で準備できます。その間、次の演習で紹介する GitHub Copilot の機能を確認できます。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらの環境にも、演習を円滑に進めるために必要な前提条件が用意されています。

Codespace を使う場合は手順 1～4 を実行します。ローカル環境を使う場合は、手順 5 を実行します。

5. ローカル環境を使う場合は、複製したリポジトリを VS Code で開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が作成されます。

## GitHub Copilot Chat とエージェント モードを使う

GitHub Copilot Chat のエージェント モードを使うには、統合開発環境 (IDE) に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespace を使っている場合は、すでにインストールされています。

拡張機能をインストールした後、GitHub アカウントで認証が必要になる場合があります。

1. GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、Visual Studio Code の Extensions ビューで **GitHub Copilot Chat** を検索し、**Install** を選択します。
2. Codespace に戻ります。
3. エディターの右側に Copilot Chat が表示されていない場合は、Codespace の上部にある **Copilot Chat** アイコンを選択します。
4. Copilot Chat ウィンドウに `Hello world` のようなメッセージを入力し、<kbd>Enter</kbd> を押します。Copilot Chat が起動します。
5. 認証されていない場合は、GitHub アカウントでのサインインを求められます。画面の案内に従って認証します。
6. 認証後、Copilot Chat ウィンドウが表示されていることを確認します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書くために使用する Codespace の作成を始めました。

## 次のステップ

Copilot を使った作業を始めます。次は、プロジェクトの規約を Copilot に伝える[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
