---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債が課題になっています。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残り続ける機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、開発者がレビューできるドラフト プルリクエスト (PR) を作成します。時間のかかるタスクを委任し、開発者がより大きな課題に集中できるようにします。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズした。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを作成した開発者は、ドキュメントを十分に整備しませんでした。まず、アプリケーションのすべての export 関数に TSDoc doc comment を追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示すれば、利用者はほかのゲームも引き続き探せます。まずはユーザー受け入れテストでユーザー体験 (UX) を試せる程度の実装が必要です。これは現在の阻害要因ですが、ほかに優先度の高い Issue もあります。

このようなタスクは優先順位が下がりやすく、Copilot cloud agent に割り当てるのに適しています。エージェントに非同期で作業させれば、開発者は別のタスクに取り組み、後から作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent][copilot-agents] は、人間の開発者と同様に、バックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ます。割り当てると Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境を設定してタスクに取り組みます。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳細を確認すると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。人工知能 (AI) を活用したペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成系人工知能 (生成 AI) を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot を導く方法を確認しました。すでにカスタム指示の演習を完了していれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み取り専用で確認してください。

リポジトリの GitHub Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc doc comment の要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` front matter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc doc comment、TypeScript の規約、コメント ヘッダーが反映されているかを確認します。これらの指示ファイルの効果は、後の演習で PR をレビューするときにもう一度確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成では、通常、必要なツールや依存関係を使えるように環境を準備します。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアのように作業します。

cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に [専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係をインストールできます。ラボの流れを簡単にし、学習に集中できるよう、このファイルはあらかじめ設定されています。単一の Astro アプリ向けに Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザー バイナリ、マイグレーションおよび seed 済みの SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # セットアップ手順に必要な最小限の権限を設定します。Copilot は自身の操作に別のトークンを使います。
    permissions:
      # セットアップ手順でリポジトリを複製する場合（依存関係のインストールなど）は `contents: read` が必要です。複製しない場合、手順の完了後に Copilot が自動で複製します。
      contents: read
    steps:
      - name: Check out code
        uses: actions/checkout@v5

      # Node.js を使ったフロントエンド / アプリのセットアップ（アプリ全体は Astro と Drizzle/libSQL です）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドと test に必要なローカル SQLite データベースをマイグレーションして seed します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローに見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という 1 つの job があります。Copilot が PR の作業を開始する前に、GitHub Actions でこの job が実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブから手動でワークフローを実行して、成功するか確認できます。Copilot が実行するまで待たずにテストできます。

## ドキュメントを追加する

ドキュメントが重要だと理解していても、多くのプロジェクトには古い情報しかないか、ドキュメント自体がありません。このような技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を支援できるため、cloud agent に割り当てるのに適した Issue です。エージェントはバックグラウンドで必要なドキュメントを生成します。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選びます。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR がない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作り、プロジェクトを分析して、作業方法を検討しています。

13. todo リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。更新後のインジケーターが表示されない場合は、ブラウザーを更新します。

   ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

16. ライブ セッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断を下し、コードを作成する様子を確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的は、タスクを非同期で実行し、開発者が別の作業に取り組めるようにすることです。この利点を活かすため、もう 1 つのタスクを cloud agent に割り当てた後、アプリの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分けられることです。デザイン チームのために関連ゲームを追加する作業をエージェントに任せ、別のタスクに取り組みましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

Issue の最初のコメントに目の絵文字が表示され、Copilot が作業を始めたことを示します。

![Copilot が Issue の作業中であることを目の絵文字で示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように動作するため、常に監視する必要はありません。作業が完了したらレビューします。次は、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使って、Issue を非同期で実行するよう Copilot に割り当てられることを確認しました。cloud agent は技術的負債への対応、新機能の作成、フレームワーク間のコード移行などに利用できます。

この演習では、次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認した。
- 範囲を明確に定めた Issue の重要性を理解した。
- Issue を Copilot に割り当てた。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示も読み込みます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
