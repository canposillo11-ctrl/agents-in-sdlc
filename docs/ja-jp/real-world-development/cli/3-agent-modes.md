---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づく絞り込み機能を作成し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能を追加しました。より大きな変更には、堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作できるため、適切なものを適切な方法で作成できます。このレッスンから、Issue を起点に機能を作成し、コードが有効で期待どおりに動作することを確かめ、最終的にプロジェクトへマージする、一般的なエージェント駆動型の開発プロセスを実践します。

> [!NOTE]
> 機能開発を続ける間は、同じ会話とブランチを使います。通常はファイル種別ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため手順を簡略化しています。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能開発を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能の説明がありますが、複数カテゴリを組み合わせるなど、コーディング前に合意すべき詳細があります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に許可します。

## 背景

AI コーディングエージェントを開発フローに加えても、基本は変わりません。むしろ、基本がいっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 実施内容を説明する登録済み Issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. test を実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くは上記の流れを調整したものです。

この標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ審査を受けるようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: 開発者とエージェントが協力して作業します。エージェントは変更を提案し、入力を待ってから次に進みます。
- **Plan**: エージェントが計画を作成します。プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、test を実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能に着手する前に、Codespace とリポジトリ、ターミナルを準備します。

1. Codespace に戻ります。停止中の場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## 絞り込み機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、採用する方法を定義できます。複雑なタスクでは、事前に時間をかけて計画することをお勧めします。Plan モードに切り替えて、Copilot に計画を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、適切な実装方法を判断して計画を作成します。

3. Copilot から絞り込み方法について質問されることがあります。希望に基づいて回答します。ここでは唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 自分の決定と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、Autopilot モードで実装を始めます。Autopilot を使うと、Copilot はタスクが完了したと判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同様のラベルが付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が合意済みの計画に沿って作業し、コードを生成して test を実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。計画を先に確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して、機能が期待どおりに動作することを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、絞り込みの実装と test を確認します。
3. 結果を Issue と計画で決めた内容に照らして確認します。
4. コードのレビューが終わったら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新機能を試す

コードがよさそうでも、実際に動くか確認しましょう。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、発行元、および計画で合意した組み合わせによる絞り込みを試します。
4. リセットと結果が空の場合の動作が、Issue と決定事項に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えながら、機能を作成してレビューしました。このレッスンでは、次の内容を学習しました。

- GitHub Issue から Copilot の会話を開始する。
- Plan モードで要件を定義する。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

次は、[カスタム指示を使って][next-lesson]コードが文書化されたプラクティスに沿うようにし、コード生成の詳細を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
