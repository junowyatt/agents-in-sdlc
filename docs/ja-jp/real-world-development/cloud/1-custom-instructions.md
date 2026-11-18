---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法に決まりがある場合や、Copilot が知っておくべき背景がある場合は、その情報を参照できるようにします。[命令ファイル][instruction-files]を使うと、実行する作業だけでなく、その進め方も Copilot に伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム命令とパス別の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加する。

> [!NOTE]
> VS Code や CLI のワークショップとは異なり、この演習ではプロンプトの実行前と実行後を比較しません。Copilot cloud agent は GitHub Issue を非同期に処理するため、リアルタイムで並べて効果を示すのが難しいためです。後の演習で cloud agent が作成した pull request をレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ず単体テストを用意する。
- UI はダークモードにし、現代的な見た目にする。
- コードには TSDoc のコメントを追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

命令ファイルを使って、Copilot がこのプラクティスに沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝えられます。推奨する規約、ライブラリ、コメントの書き方などを設定し、関連性の高いコードを生成できるようにします。リポジトリ全体、または特定の種類のファイルやタスクに命令を作成できます。

命令ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに共通する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用するガイドラインです。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに利用できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の形式の命令ファイルにも対応しています。詳細は[カスタム命令のサポート][custom-instructions-support]を参照してください。

### 命令ファイルを管理するためのベストプラクティス

命令ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプルプロジェクトには、代表的な構成例が含まれています。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定の作業に関するガイダンスを記載します。
- 自然な言葉で、明確なガイダンスとコードのよい例・悪い例を示します。

命令ファイルに唯一の正解はありません。AI の使い方と同じように、実際に試しながらプロジェクトに合う方法を見つけます。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、十分な命令ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまな作業向けのファイルがあります。
>
> Copilot に命令ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。必要な場合は各ワークショップの手順で説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム命令ファイルを確認する

このリポジトリには、共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注意してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令を適用するファイルをリポジトリルートからの glob パターンで指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトの単体テストを作成する際の命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令を小さな再利用可能なファイルに分け、Copilot に従う例を示せます。ここにあるパスはリポジトリルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は1つです。コード内に TSDoc コメントとファイルレベルのコメントヘッダーを追加する、プロジェクト全体のルールを設定します。cloud agent は、このワークショップの後半で割り当てる Issue に取り組む際に、このルールを読み取ります。

編集の前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチにある命令を読み取りますが、変更を整理してコミットできるようにします。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在の標準は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - エクスポートする関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを付けてください。
   - import 文やコードの前に、ファイルの目的を説明するコメントブロックを追加してください。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のセクションで確認したように、命令ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。今回追加したドキュメントのルールなど、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチから命令ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、命令の変更を cloud agent に使わせるには、`main` にマージする必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で Issue を割り当てる際、cloud agent はこの命令を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、直接 `main` にコミットします。ブランチを使う手順は、実際のプロジェクトに近い方法を体験するためのものです。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのワークショップで cloud agent が作成する pull request に、その標準が反映されます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
