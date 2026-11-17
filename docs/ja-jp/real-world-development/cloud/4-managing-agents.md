---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent に UX の改善や機能追加を目的とした 3 つのタスクを依頼しました。cloud agent は非同期で自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまな方法で管理できます。Mission control では、開いているプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を与えたりできます。

このレッスンでは、次の内容を学習します。

- エージェント ページを使って、cloud agent のタスクを監視する。
- 作業中のセッションを調整して、追加機能を依頼する。

## シナリオ

高コントラスト モードの作成をエージェントに依頼した後、チームはライト モードも追加するとよいと気づきました。サイトのスタイルと切り替え機能をすでに更新しているため、ライト モードも対象に含めるのが自然です。高コントラストに加えてライト モードを実装するよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域にあるタスク一覧を確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲーム変更用のアプリケーション プログラミング インターフェイス (API) を生成する。
   - Web サイトに高コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリをチェックアウトする。
   - Copilot の作業環境を作成する。
   - Model Context Protocol (MCP) サーバーを設定する。
   - 割り当てられたタスクを完了するための作業を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) のペインも確認します。PR と変更ファイルを確認して、作業の進行状況を監視できます。

## cloud agent の作業を調整する

実行中のタスクを確認したので、高コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラスト モードのセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状態によって異なります。

   ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![cloud agent の作業を調整するダイアログが表示されたエージェント ページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、今後の作業に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業の流れに反映します。

これまでと同様に数分かかります。その間、これまでに学習した内容を振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent タスクを監視して導く中心的な場所である Copilot agents ページを確認しました。Mission control では、実行中と完了済みのタスクを追跡し、作業内容を確認し、進行中の作業を別の方向へ導くことができます。

この演習では、次の内容を学習しました。

- Mission control とエージェント ページを使って cloud agent のタスクを監視する。
- 実行中のセッションを調整して追加機能を依頼する。

Copilot がアクセシビリティ機能を作成している間に、次は作成されたプルリクエストの[作業を反復します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
