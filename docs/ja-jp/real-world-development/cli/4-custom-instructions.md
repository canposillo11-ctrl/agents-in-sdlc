---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込みコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行したい場合は、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。絞り込み機能を作成したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示ファイルとパス単位の指示ファイルがエージェントに適用される方法を確認する。
- コーディング標準を守るために指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の export 関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入できる場合は、その説明も含めます。
- 再利用可能な Astro component は `Props` の契約を説明し、関連コードが変わった際にはコメントも最新に保ちます。
- 既存の書式と lint のガイダンスを維持します。

指示ファイルを使い、これらのプラクティスに沿ってタスクを実行するための情報を Copilot に渡します。

## 指示ファイル

カスタム指示では、コーディングスタイルや要件を Copilot に伝え、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクに応じたガイドラインを提供します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが含まれています。変更前に内容を確認し、指示の影響を把握しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディング ガイダンスを確認します。この指示は、リポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開きます。Astro、Drizzle データ レイヤー、test などの指示があることを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob によって、指示を適用するファイルが決まります。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存プロジェクト ファイルへのリンクを確認します。大きな指示セットを小さく再利用しやすいファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルには、TypeScript に新しく追加するコードの TSDoc コメントが必須であることを明記する余地があります。ここでは、共通の `copilot-instructions.md` に標準を追加します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクション（35 行目付近）を探します。
2. 次の項目を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

3. ファイルを保存します。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。更新した指示を使えるように、機能のコンテキストを保ったまま会話を終了して再開します。

1. 絞り込みの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチをチェックアウトした状態から、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

3. 選択を求められた場合は、絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
4. Copilot に更新後のガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を入力して変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、機能に新しい標準を適用しました。このレッスンでは、次の内容を学習しました。

- リポジトリの指示とパス単位の指示ファイルがエージェントに適用される方法を確認する。
- 指示ファイルを更新して、コーディング標準を守るようにする。
- 指示ファイルがコードに与える影響を確認する。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint と test を一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
