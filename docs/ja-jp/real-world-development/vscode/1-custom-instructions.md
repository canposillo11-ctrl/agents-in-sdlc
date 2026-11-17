---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成系人工知能 (生成 AI) を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、コード生成の指針を Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス単位の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の指示のまま、絞り込み機能用の最初のデータ スライス（publishers ヘルパー）を生成する。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加する。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に沿う様子を確認する。
- 次の演習で更新後の指示を使えるよう、指示と絞り込み用ヘルパーを `main` に commit する。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は非決定的です。この演習では、出力を一字一句一致させるのではなく、指示の更新後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず単体テストを追加します。
- ユーザー インターフェイス (UI) はダーク モードにして、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿うための情報を Copilot に渡します。

### カスタム指示

カスタム指示では、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルは 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべて**のリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中のプロジェクトの概要、ベスト プラクティスなど、ほとんどのチャットや CLI リクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントや単体テストなどの作業に関するガイドラインを提供できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` front matter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準を使った指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの詳しい作成方法は、このワークショップの範囲外です。サンプル プロジェクトのファイルは、一般的なアプローチを示しています。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種別（単体テスト、Astro コンポーネント、データ レイヤー）や特定のタスクに関する具体的な指示を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードのよい例と避けるべき例を示します。

指示ファイルの作り方も人工知能 (AI) の使い方も 1 つではありません。試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルが用意されていると役立ちます。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成させる方法もあります。各環境での操作は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。手順は利用中の環境に対応するレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリに用意された指示ファイルを確認します。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートからの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルを対象にします。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用しやすいファイルに分け、コード生成の例を Copilot に示せます（パスはリポジトリのルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。後の手順で TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベスト プラクティスに沿って、変更用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
カスタム指示の効果を確認するため、まず現在の指示のままプロンプトを送信します。その後、指示を更新して同じプロンプトをもう一度送信し、違いを確認します。

1. Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。
2. 前の演習から開いているエディター タブを閉じ、Copilot に必要なコンテキストだけを残します。
3. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
4. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

5. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

6. Copilot はプロジェクトを調査し、コードを変更します。ヘルパーやテストなど、複数のファイルに変更が及ぶ場合があります。
7. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、publisher の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約によるものです。
8. 提案されたコードに TSDoc doc comment とファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。重要なのは、指示を更新した後の動作が一貫することです。

## リポジトリの新しい標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。ここに標準を追加し、コード提案の改善を促します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクション（27 行目付近）を探します。現在のコーディング標準にコード内ドキュメントのルールがなく、そのため先ほど生成されたヘルパーに doc comment がなかったことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル種別、タスクごとの `*.instructions.md` のどちらにも作成できます。doc comment の要件のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

指示に doc comment のルールを追加しました。Copilot Chat で同じヘルパーの作成プロンプトをもう一度送信します。

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選び、Copilot の対象ファイルとして設定します。
3. 前と同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc comment が追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で使う実際のコードに適用しました。

## 指示とブランチを commit して push する

指示ファイルもほかのファイルと同様に、バージョン管理の手順で管理します。指示とヘルパーの変更を commit して push します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押し、新しいターミナルを開きます。
2. ターミナルで変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と Copilot がヘルパー用に変更したすべてのファイルを stage して commit します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (例: テストファイル) が表示される場合は、そのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したので、バックログのドキュメント標準に関する Issue と関連付けてプルリクエスト (PR) を作成します。Copilot に Issue を探し、Issue を解決する PR を作成して、マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に初めて GitHub 上の操作を任せます。プロジェクト テンプレートでは GitHub のツールがあらかじめワークスペースに設定されています。Copilot が初めてツールを使うときは、VS Code で GitHub へのサインインを求められることがあります。画面の案内に従ってください。後の演習で、この接続の仕組みである Model Context Protocol (MCP) を確認します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイル更新の Issue を探し、現在のブランチから PR を作成して、指示の変更と新しい publishers ヘルパーの土台の両方を説明するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう Copilot に求められた場合は、実行内容を確認してから **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカル ブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェル コマンドを許可するよう求められた場合は、実行内容を確認してから **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージしました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、Copilot Chat で次の作業を行いました。

- プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える、リポジトリのカスタム指示とパス単位の指示ファイルを確認した。
- *既存の*指示でコード生成プロンプトを送信し、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認した。
- 次の演習で土台として使えるよう、指示とヘルパーの変更を commit し、`main` に取り込んだ。

次は、エージェント モードで指示を適用し、Copilot がコードベース全体にわたって機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
