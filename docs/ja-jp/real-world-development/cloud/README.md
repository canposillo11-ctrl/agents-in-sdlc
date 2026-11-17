---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot がクラウドで非同期に作業できるようにします。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を行い、プルリクエストを作成します。その間、開発者は別の作業を続けられます。

演習では、まず cloud agent が使うカスタム指示を追加し、GitHub Issue を割り当てて実装を任せます。カスタム エージェントを確認して利用し、エージェントのダッシュボードからセッションの進行状況を監視して作業を調整します。最後に、作成されたプルリクエストをレビューし、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が使うカスタム指示を追加する |
| [2. GitHub Copilot cloud agent][ex2] | エージェント | Issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの監視と管理][ex4] | 監視 | エージェント セッションを監視して調整する |
| [5. Copilot の作業を反復する][ex5] | レビュー | プルリクエスト (PR) をレビューし、作業を反復して次の手順を選ぶ |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、Model Context Protocol (MCP)、コードレビュー、Copilot CLI の機能が含まれているため、すべての実行環境を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できます。Copilot Business または Enterprise では、管理者が組織に対して有効にする必要があります。

> [!NOTE]
> この実行環境では Model Context Protocol (MCP) を扱いません。Copilot で MCP サーバーを使う方法については、[CLI 実行環境][cli] または [VS Code 実行環境][vscode]を確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli]: ../cli/
[vscode]: ../vscode/
