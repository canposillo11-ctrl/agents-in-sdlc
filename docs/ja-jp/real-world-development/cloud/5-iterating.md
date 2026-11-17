---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot を使ってユーザー体験 (UX) を改善し、機能を追加する複数のタスクに取り組みました。コードにドキュメントを追加し、デザイン チームが反復できる関連ゲーム機能を作成し、高コントラストとライト モードの切り替えなどのアクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成系人工知能 (生成 AI) を導入しても、ソフトウェア設計と開発運用 (DevOps) の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームのレビューを有効にする前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視を受けずにタスクを実行するため、安全性を保つ目的で一定の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセスは作業に使うブランチ**のみ**です。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローの実行には、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されます](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。Model Context Protocol (MCP) サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面を使います。

> [!NOTE]
> プルリクエスト (PR) を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** または同様のタイトルが付いたプルリクエストを開きます。タイトルに **[WIP]** が含まれている場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. 準備ができたら **Files changed** タブを選び、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc doc comment などのドキュメントを確認します。具体的な変更内容は異なる場合があります。レビュー時は、演習 2 の冒頭で確認した指示ファイルによる TSDoc doc comment、TypeScript の規約、コメント ヘッダーを確認します。
6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあることを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄でワークフローが実行待ちになっていることを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チーム メンバーと同様に、PR やコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確定的ではないため、必ず成功する指示文はありません。Copilot に依頼できる変更の例は次のとおりです。

- 各コード ファイルの先頭に、内容を簡単に説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使って、生成されたドキュメントへの変更を依頼するコメントを追加し、通常のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot が変更を行う様子を確認します。更新のために新しいセッションが始まります。
3. **Back to pull request** を選択して PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が終わったら、PR に新しい commit が追加されていることを確認します。
5. **Files changed** タブを選び、変更をレビューします。

6. 必要に応じてコメントで Copilot に変更を依頼し、更新された差分を再度レビューします。
7. 変更に問題がなければ、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** または同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示すインジケーターがあることを確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄でワークフローが実行待ちになっていることを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名（例: **copilot/fix-8**）に `<branch-name>` を置き換えて、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

10. 必要に応じて **Conversation** タブにコメントを追加し、表示する関連ゲームの数を調整したり、カスタム指示を追加する前に割り当てたタスクにコメント ヘッダーや TSDoc doc comment を追加したりするよう依頼します。Copilot はコメントを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 の Mission control で追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** または同様のタイトルが付いた PR を選択します。タイトルに **[WIP]** が含まれている場合は、Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選び、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - 表示モードを切り替えるユーザー インターフェイス (UI) コンポーネント。
   - ユーザー設定を保存するローカルストレージの使用。
   - 高コントラスト モードとライト モードのカスケーディング スタイル シート (CSS) / スタイル変更。
   - アクセシビリティ属性（Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など）。
   - モード切り替えを管理する JavaScript / TypeScript コード。

6. 更新内容に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示すインジケーターがあることを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄でワークフローが実行待ちになっていることを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動し、ブラウザーで高コントラストとライト モードの切り替えを試します。ページを再読み込みしても設定が保持されることを確認します。

11. アクセシビリティのカスタム エージェントが、アクセシビリティのベスト プラクティスに沿った機能実装を Copilot に促したことを確認します。懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に更新を依頼します。

## 任意の演習 — 作業の委任を続ける

実際のバックログ項目を cloud agent に割り当て、別の作業を並行して進める練習をします。

1. リポジトリに Issue をいくつか作成します。
2. Issue を Copilot に割り当てます。たとえば、次のようなタスクを依頼できます。
   - ゲーム詳細ページに支援者向けの関心フォームを作成する。
   - ゲーム一覧ページにページネーションを実装する。
   - データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の実行環境を完了しました。各レッスンを通じて、次のことを行いました。

- このリポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力に与える影響を確認しました。
- Issue を Copilot cloud agent に割り当て、非同期の環境セットアップ、計画、実行を確認しました。
- アクセシビリティ用カスタム エージェントを作成して使い、高コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページを Mission control として使い、作業中のセッションを監視して調整しました。
- cloud agent のプルリクエストをレビューして反復し、**@copilot** に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent の実行環境を完了しました。ほかの実行環境では、ここで取り組んだ内容をさらに実践できます。

- 🖥️ **[VS Code 実行環境](../../vscode/)** — 統合開発環境 (IDE) で Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI 実行環境](../../cli/)** — Plan と Autopilot モード、agent skill、品質保証 (QA) カスタム エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアを試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
