---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連の開発ワークフローを実践しました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意された絞り込み Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始め、Plan モードで方針を定め、Autopilot モードで作成し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、unit test、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、coverage、スキルの結果、ブラウザーの根拠を評価しました。
- 絞り込み機能全体をレビューし、絞り込み PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれブランチを作り、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー:** 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連する test を追加します。

絞り込みの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## 検証方法の違い

自動 test、ブラウザーでの手動確認、MCP 経由の Copilot によるブラウザー操作でコードを確認しました。`quality-checks` スキルは unit test、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果を、要件と test coverage のレビューにまとめました。

追加する test は、実際の不足を補うものである必要があります。QA の実行で新しい test が不要という結果になる場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響する根拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでカスタマイズを再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは反復可能な作業を説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の方針決定に、**Interactive** は細かな変更を確認しながら進める場合に、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

コンテキストは、インフラストラクチャと同じくらい重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに確認する

主なワークフローを実践しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビュー エージェントに変更を分析させる。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して進める。
- `/worktree` で別のタスクを分離する。
- `/delegate` でタスクを Copilot cloud agent に送信する。

## 次のステップ

ツールを上達させるには、使い続けるのが一番です。本番コード、個人のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。必要に応じてドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
