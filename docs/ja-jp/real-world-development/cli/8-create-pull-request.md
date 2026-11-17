---
title: "レッスン 8 - 機能プルリクエストを作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、test をまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込みの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、test は、1 つのブランチに保存されています。まとめてレビューし、プルリクエスト (PR) を作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、絞り込み機能と品質確認の一連の作業を 1 つの会話とブランチで行い、この PR にまとめています。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージ ライフサイクルの自動化方法を理解する。
- 機能全体の変更と検証結果を確認する。
- 絞り込み PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

絞り込み機能の開発を通じて、Copilot を使った計画、実装、検証を行いました。Tailspin Toys は、マージを許可するかどうかの決定権を開発者が持ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストを取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗の修正、レビュー コメントへの対応、必要に応じた rebase など、PR を妨げている問題に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択してきました。Agent Merge にその作業を任せることができますが、有効化する前に内容をレビューし、準備ができていると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りの作業を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 候補から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されるときはレビュー済みの PR を手動でマージします。

## まとめと次のステップ

コード生成、test と検証、プルリクエストの処理など、開発プロセスの一部を自動化しました。このレッスンでは、次の内容を学習しました。

- Agent Merge と、マージ ライフサイクルの自動化方法を理解する。
- 機能全体の変更と検証結果を確認する。
- 絞り込み PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

次は、コンテキスト、モデル、共有、任意のクラウド委任に使える [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
