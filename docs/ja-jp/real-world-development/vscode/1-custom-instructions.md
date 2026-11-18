---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業方法に決まりがある場合や、Copilot が知っておくべき背景がある場合は、その情報を参照できるようにします。コンテキストを共有する方法はいくつかあります。特に重要なのが[命令ファイル][instruction-files]です。命令ファイルを使うと、生成するコードだけでなく、その作成方法も Copilot に伝えられます。

この演習では、次の内容を学びます。

- リポジトリのカスタム命令とパス別の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- 現在の命令を使って、フィルター機能の最初のデータ部分 (publishers ヘルパー) を生成する。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認する。
- 命令の更新とフィルター機能のコードを `main` にコミットし、次の演習で Copilot が最新のガイダンスを使えるようにする。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わない場合があります。Copilot の動作は確率的です。この演習では、命令を更新した後の動作の変化を確認します。生成結果が一字一句同じになる必要はありません。

## 命令ファイル

### シナリオ

Tailspin Toys では、開発方法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データレイヤーには、必ず単体テストを用意する。
- UI はダークモードにし、現代的な見た目にする。
- コードには TSDoc のコメントを追加する。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

命令ファイルを使って、Copilot がこのプラクティスに沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝えられます。推奨するコーディング規約、ライブラリ、コメントの書き方などを設定し、関連性の高いコードを生成できるようにします。リポジトリ全体、または特定の種類のファイルやタスクに命令を作成できます。

命令ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、ほとんどのチャットや CLI のリクエストに共通する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別にガイドラインを適用します。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの追加などに利用できます。

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
> Copilot に命令ファイルを生成させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用する環境によって方法が異なります。必要な場合は、各ワークショップの手順で説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム命令ファイルを確認する

プロジェクトには、共通の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがすでに含まれています。エディターまたは GitHub Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスにも注意してください。これらの命令は、Copilot を使うすべての作業に適用されます。
3. `.github/instructions` フォルダーを開いて確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令を適用するファイルをリポジトリルートからの glob パターンで指定します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. このプロジェクトの単体テストを作成する際の命令を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ファイルの末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令を小さな再利用可能なファイルに分け、Copilot に例を示せます。ここにあるパスはリポジトリルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用ブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 次のコマンドでブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の影響を確認するため、まず現在の命令ファイルを使ってプロンプトを送ります。その後、命令を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で使ったエディタータブを閉じ、Copilot が必要なコンテキストだけを参照するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   src/lib/publishers.ts に、各 publisher の name と id を返すデータアクセスヘルパーを作成または更新してください。ファイルに変更を適用してください。
   ```

5. Copilot はプロジェクトを調査し、ヘルパーファイルとテストなどにコードを適用します。
6. 提案されたヘルパーは、最初の引数に型付きの `db` クライアントを受け取り、publisher の型付き配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に従っています。
7. 生成されたコードには、TSDoc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても TSDoc コメントを追加することがあります。その場合も問題ありません。この演習では、命令を更新した後に動作が一貫して改善することを確認します。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を伝えるファイルです。コード提案を改善するため、コーディング標準を追加します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在のコーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイルヘッダーと TSDoc コメントを指示します。

   ```markdown
   - エクスポートする関数には、目的、パラメーター、戻り値を説明する TSDoc コメントを付けてください。
   - import 文やコードの前に、ファイルの目的を説明するコメントブロックを追加してください。
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、命令ファイルはリポジトリ全体に適用する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` のどちらにも作成できます。今回追加したドキュメントのルールなど、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が対象ファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   src/lib/publishers.ts に、各 publisher の name と id を返すデータアクセスヘルパーを作成または更新してください。ファイルに変更を適用してください。
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を導いて新しい標準に従わせ、次の演習で使うコードに適用できました。

## 命令とブランチをコミットしてプッシュする

命令ファイルも他のファイルと同じように、ソース管理で管理します。命令とヘルパーの変更をコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. 次のコマンドで命令の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 命令の更新と、ヘルパーの基盤に関して Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けて pull request を作成します。GitHub ツールはプロジェクトに接続済みなので、Copilot に Issue を探し、Issue を解決する PR を作成してからマージするよう依頼できます。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートには GitHub ツールがすでに接続されています。Copilot が初めてツールを使う際、VS Code から GitHub へのサインインを求められることがあります。画面の指示に従ってください。後の演習で、この接続に使われる Model Context Protocol について学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 命令ファイルの更新と、次のフィルター作業に使う publishers ヘルパーの基盤を説明する PR を現在のブランチから作成するよう、Copilot に依頼します。

   ```plaintext
   命令ファイルの更新に関する Issue を探してください。現在のブランチから pull request を作成し、その Issue を解決する PR であることと、次のフィルター機能のために publishers ヘルパーの基盤も追加したことを記載してください。
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められた場合は、コマンドを確認してから **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   PR を main にマージしてください。その後、ローカルで main に戻り、最新のコードを pull して更新してください。
   ```

7. GitHub の操作やシェルコマンドを許可するよう求められた場合は、コマンドを確認してから **Allow** を選択します。

GitHub Copilot の支援で pull request を作成してマージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の命令を使ってコード生成を依頼し、Copilot の出力を確認した。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加した。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認した。
- 命令の更新とヘルパーの基盤を `main` にコミットし、次の演習に備えた。

次は [agent mode][next-lesson]で命令を使い、Copilot にコードベース全体の新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
