---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は、Copilot CLI を基盤とするデスクトップ アプリケーションです。エージェント型開発のための専用ワークスペースとして、複数のエージェント セッション、切り替え可能なセッション モード、共有 Canvas、GitHub Issue とプルリクエストの管理機能を提供します。**Agent Merge** は、プルリクエストの rebase、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージを支援します。

このワークショップでは、Tailspin Toys の一連の開発ワークフローを進めます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. 絞り込み Issue から始め、**Plan** モードで方針を定め、**Autopilot** モードで作成し、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、絞り込み機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズして、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. 品質保証 (QA) カスタム エージェントを作成し、要件、coverage、検証の根拠をレビューします。
8. 絞り込み機能全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. Canvas の利用可否を確認してから、Database Explorer を使い、リポジトリに保存するトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、その制約を記録し、読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップでは、星評価と絞り込み機能の 2 つの PR を作成します。2 つ目には、指示、スキル、QA プロファイル、test の更新が含まれます。それぞれ更新済みの `main` から開始します。絞り込みと品質確認の一連の作業では 1 つのセッション、worktree、ブランチを共有し、各ツールの使い方を学びながら作業を続けます。最後の Canvas 演習では、そのセッションを継続して、PR ワークフローを繰り返さずに共有スペースの作成とテストに取り組みます。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app をインストールする][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価を追加する: 小さな改善][ex2] | 最初の変更 | 既存の評価と未評価時の表示を追加し、PR 1 をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で作成して Interactive モードでレビューする |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認して更新し、絞り込み機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 反復可能なチェック | 既存スキルを確認し、レポート形式を変えて実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize から MCP を設定し、絞り込みの動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件と coverage | 専門プロファイルを作成して選び、最終的な検証根拠を集める |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | 絞り込み、指示、スキル、QA プロファイル、test をレビューし、2 つ目の PR に Agent Merge を使う |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ Canvas を作成してテストする |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを振り返る |

## 前提条件

ワークショップを始める前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべての実行環境を完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件の演習][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。この設定は Copilot CLI のポリシーとは別です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
