---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションを少し変更するだけでも、複数のファイルを更新し、テストを実行するなどの作業が必要になることがあります。開発者は通常、必要なファイルを探し、変更し、テストを実行して問題を調査し、変更し忘れたファイルを見つけて、さらに更新するという作業を繰り返します。

このような作業を支援するのが Copilot エージェント モードです。

Copilot エージェント モードは、統合開発環境 (IDE) 内でより自律的に作業します。開発者と同様に、まず既存のプロジェクト構造を調査し、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。ここでは、エージェント モードを使ってサイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で動作し、すばやいフィードバックと対話を可能にします。cloud agent はチームの一員のように非同期で動作し、Issue やプルリクエストを通じて開発者とやり取りします。

この演習では、次の内容を学習します。

- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを見つけて、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot エージェント モードで UI とデータ レイヤーに新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリと発行元で絞り込めるようにします。前の演習では publishers ヘルパーを追加しました。ここでは Copilot エージェント モードを使って、残りのデータ レイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを操作して、現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時に静的な HyperText Markup Language (HTML) としてページを生成します。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド アプリケーション プログラミング インターフェイス (API) やクライアント側ユーザー インターフェイス (UI) フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準の Document Object Model (DOM) API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを実行しやすくするため、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` にいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような Uniform Resource Locator (URL) が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost アドレスのリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace の Web サーバーを公開するプライベート トンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主なページは次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作しますが、さらに機能を追加します。最初にバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されます。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** は複数ファイルの変更を自律的に計画 / 実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。提案されるコードを比較的一貫させて、作業しやすくするためです。ただし、大規模言語モデル (LLM) は確率的に動作するため、手順と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> 大規模言語モデル (LLM) の確率的な性質により、Copilot が別の Model Context Protocol (MCP) コマンドを使う場合がありますが、タスクは完了できるはずです。

3. Copilot に次のプロンプトを送信し、バックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって有益だと判断した項目に基づき、優先順位も付けていることを確認します。

## 指示ファイルを確認する

コード生成をエージェントに依頼する前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する[ユーザー インターフェイス (UI) の指示ファイル][ui-instructions]を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル種別に応じた指示へのリンク。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト ファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、複雑な内容を小さく分け、例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 絞り込み機能を実装する

絞り込みを完成するには、少なくとも 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整する。
- 絞り込み動作のテストを追加または更新する。
- 絞り込み UI を追加するため、ゲーム一覧ページを更新する。

コードベースにマージする前にテストを実行し、すべて成功する必要があります。Copilot エージェント モードにこれらの作業を依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. 次のプロンプトで、バックログの関連 Issue に基づいて機能を実装するよう Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関連するファイルを探す様子を確認します。データ レイヤーのヘルパー、UI、テストを見つけてから、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクト ファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調査し、ファイルを変更してテストを実行します。タスクやコードベースの複雑さによっては数分かかります。作業中、コード エディターに **Keep** と **Undo** ボタンが表示されることがあります。完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待機が発生したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストの実行を繰り返し、タスクの完了とエラーがないことを確認します。

   ![Copilot エージェント モードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの人工知能 (AI) ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを実行しているブラウザーに戻り、新機能を確認します。
11. 動作とコードのレビューが済んだら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードで Tailspin Toys の Web サイトに機能を追加する方法を確認しました。この演習では、次の内容を学習しました。

- GitHub Copilot エージェント モードが UI とデータ レイヤーに新機能を実装する方法。
- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを見つけて、複数ファイルにまたがる変更を行う方法。
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 - ページネーションを実装する

ゲーム一覧が増えてくると、ページネーションが必要になります。この演習で学んだことを使い、Copilot にページネーションの実装を依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従う。
- ページサイズを利用者が選択できるようにするか、固定値にするかなど、ページネーションの方法を検討する。
- 希望どおりの実装に必要なガイダンスをプロンプトに含める。
- 変更を依頼したりコンテキストを追加したりして、Copilot との作業を反復する。これは通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
