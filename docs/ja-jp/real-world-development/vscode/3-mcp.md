---
title: "演習 3 - Playwright Model Context Protocol (MCP) サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright MCP を使って、ブラウザーで絞り込み機能をテストさせる。
- ブランチを公開し、絞り込み作業のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エージェント モードでエディターの外部にアクセスできると、より多くの作業が可能になります。Copilot が外部のツールやサービスと通信する標準的な方法が Model Context Protocol (MCP) です。

![エージェント モードがコンテキスト、LLM、ツールと連携する仕組みを示す図。MCP サーバーや VS Code 拡張機能が追加したツールを含む。](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を介して、リソースを使った最新情報へのアクセスや、ツールを使った操作を行えます。

ツールやリソースへのアクセスは MCP サーバーを通じて行います。MCP サーバーは AI エージェントと外部のツール / サービス（既存の API や npm package などのローカル ツール）の間の通信を管理します。各 MCP サーバーは、エージェントが利用できるツールやリソースを提供します。

よく使われている MCP サーバーの例を挙げます。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、貢献できるように [MCP registry](https://github.com/mcp) を公開しています。

> [!CAUTION]
> セキュリティに関して、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次のように、`github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できるため、機能をテストできます。

1. `.vscode/mcp.json` を編集し、`github` と並べて `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモート サーバーの `github` と異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグは、表示ウィンドウなしで Playwright を実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド test に Playwright を使うため、通常は Playwright 用のブラウザーがすでにインストールされています。Copilot が後からブラウザー不足を報告した場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** 操作を選択し、サーバーを起動して設定が正しいことを確認します。
2. サーバーを信頼するか VS Code に確認された場合は、設定を確認し、信頼して起動することを選択します。

手動で起動する手順は省略できます。その場合、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って要約します。会話を中断せずに、機能が期待どおりに動作するか確認できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使って動作します。エージェントは、支援技術と同じように、ラベルの付いた構造化要素（ボタン、リンク、リスト項目など）を使って判断します。そのため、簡単な機能確認をアクセシビリティの基本チェックにも活用できます。

サーバーに接続し、アプリを実行した状態で、作成した絞り込み機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして報告を読みます。問題がある場合は、質問を追加するか、プルリクエストを開く前にコードの修正を依頼します。

> [!NOTE]
> このテストには、`http://localhost:4321` で実行中のアプリが必要です。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使う際、ブラウザーをダウンロードする場合があります。ブラウザー不足が報告された場合は、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームにレビューしてもらうためにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使って commit メッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) を使う方法など、PR の作成方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。Copilot は関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub のツールを使えるよう、エージェント ドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために **Continue** を選択して許可します。初めて GitHub ツールを使うときは、GitHub での認証を求められることがあります。画面の案内に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が作成したリンクを選択して PR をレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の内容を学習しました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する仕組みを理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- プルリクエストを作成する前に、ブラウザーで絞り込み機能を手動テストするよう Copilot に依頼する。
- ブランチを公開し、絞り込み作業のプルリクエストを作成する。

MCP サーバーを使って機能を**テスト**しました。MCP は Copilot を拡張する方法の 1 つです。次は、アクセシビリティレビューなど、対象を絞ったタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
