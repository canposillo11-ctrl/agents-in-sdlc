---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせて、人工知能 (AI) を活用した専門アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、指示、ベスト プラクティス、コーディング パターン、分野固有の知識を定義します。チームの専門知識を再利用可能なエージェントとしてまとめられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、セキュア コーディングを確認するセキュリティ エージェント、一貫したテスト パターンを適用するテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML Ain't Markup Language (YAML) front matter と、エージェントの動作、専門知識、指示を記した Markdown プロンプトが必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には役割が重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一部としてスキル（場合によってはほかのエージェント）を調整できます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエストのワークフロー スキルやテストを実行または管理するスキルを呼び出す場合があります。

> [!NOTE]
> カスタム エージェントを作成する唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントを使って次のことを確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスク割り当て。

## シナリオ

Tailspin Toys は、見え方や好みにかかわらず、すべての利用者がクラウドファンディング プラットフォームを使えるようにしたいと考えています。最近の利用者からのフィードバックによると、文字と背景色のコントラストが不十分で、現在のダーク テーマが読みにくい場合があります。このアクセシビリティの課題に対応するため、デザイン チームは利用者が切り替えられる高コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントは、すでに用意されています。内容を確認し、Copilot をどのように導くか把握しましょう。

エージェントの定義を読むと、Copilot の動作をどのように導くか把握できます。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML front matter を確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含む front matter が必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責任。
   - アクセシビリティのベスト プラクティス。
   - ハイパーテキスト マークアップ言語 (HTML)、カスケーディング スタイル シート (CSS)、JavaScript のコード例。
   - よくある問題と避けるべきこと。

## Issue を作成して割り当てる

エージェントの司令塔となる Mission control では、Copilot cloud agent にタスクを割り当て、監視し、作業中のセッションを別の方向へ導いたり、追加の指示を与えたりできます。高コントラスト モードの Issue を作成して、カスタム エージェントに割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

   ![cloud agent の割り当て画面で、custom agent と accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能なエージェントにまとめ、特定のタスクや分野に応じて Copilot を導けます。

この演習では、次の内容を確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスク割り当て。

Copilot が高コントラスト モードを作成している間に、次は [Mission control からエージェント セッションを監視して調整します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタム エージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
