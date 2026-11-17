---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成系人工知能 (AI) を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に背景情報を伝えたい場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、何を実行するかだけでなく、どのように実行するかも Copilot に伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパス単位の指示ファイルを使い、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- `.github/copilot-instructions.md` に新しいリポジトリ全体の標準を追加する。

> [!NOTE]
> VS Code や CLI の実行環境とは異なり、この演習では指示更新の**前後**で同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムに並べて効果を示すのが難しいためです。後続の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず単体テストを追加します。
- ユーザー インターフェイス (UI) はダーク モードにして、モダンな印象にします。
- TSDoc doc comment を使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿うための情報を Copilot に渡します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みのコーディング規約、ライブラリ、追加したいコメントの種類などを指定できます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

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

指示ファイルの作成に関する詳細な説明は、このワークショップの範囲外です。サンプル プロジェクトのファイルは、一般的なアプローチを示しています。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ファイル種別（単体テスト、Astro コンポーネント、データ レイヤー）や特定のタスクに関する具体的な指示を記載します。
- 自然な言葉で、明確なガイダンスを書きます。コードのよい例と避けるべき例を示します。

指示ファイルの作成方法も、AI の使い方も 1 つではありません。実際に試しながら、プロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境での操作は異なります（たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など）。必要な場合は、利用中の環境に対応するレッスンで案内します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイル（中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md`）をエディターまたは GitHub Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データ レイヤー、テストなどの指示があることを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリのルートからの相対パスで、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルを対象にします。
5. このプロジェクトで単体テストを作成するときの指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、一番下までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用しやすいファイルに分け、コード生成の例を Copilot に示せます（ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc doc comment とファイル コメント ヘッダーのルールを追加します。

## リポジトリの新しい標準を追加する

この演習では、コード内に TSDoc doc comment とファイル レベルのコメント ヘッダーを付けるという、プロジェクト全体のルールを追加します。cloud agent は、この実行環境の後半で割り当てる Issue に取り組むときに、この指示を読み取ります。

編集の前に、作業ブランチを作成します。Issue が対象とするブランチの指示を cloud agent が読み取るため、指示の変更は後から `main` にマージします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` をもう一度開きます。
3. **Code formatting requirements** セクション（27 行目付近）を探します。現在のコーディング標準が記載されていますが、コード内ドキュメントのルールがありません。
4. 既存の標準の直後に、次の Markdown を追加します。ファイル コメント ヘッダーと TSDoc doc comment を追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 指示ファイルは、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル種別、タスクごとの `*.instructions.md` のどちらにも作成できます。doc comment の要件のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の更新を commit、push、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み取ります。次の演習で Issue を割り当てると cloud agent は `main` からブランチを作成するため、指示の変更を `main` に取り込む必要があります。

1. 変更を stage して commit します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で Issue を割り当てる際、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチ作成を省略し、`main` に直接 commit します。ここでは実際のプロジェクトに近い手順になるよう、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この実行環境の後半で、cloud agent が作成するプルリクエストにその標準が反映されることを確認します。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
