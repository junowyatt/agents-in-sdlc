---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認してドキュメントの標準を追加し、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。作業方法に決まりがある場合は、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も伝えられます。フィルター機能を実装したので、Copilot が参照した命令を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの命令とパス別の命令ファイルがエージェントに適用される仕組みを確認する。
- コーディング標準を守れるよう、命令ファイルを更新する。
- 命令ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントにはコードの内容を繰り返すのではなく、意図や自明でない判断を記載する。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載する。`db` を引数として注入できる場合は、そのことも記載する。
- 再利用可能な Astro コンポーネントには `Props` の契約を記載し、関連するコードを変更した場合はコメントも更新する。
- 既存の書式設定と lint のガイダンスを維持する。

命令ファイルを使い、Copilot がこれらのプラクティスに沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝えられます。リポジトリ全体、または特定の種類のファイルや作業に対して、推奨する規約、ライブラリ、コメントの書き方などを設定できます。

命令ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一のファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、ほとんどの依頼に共通する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、作業に適用するガイドラインです。

> [!NOTE]
> ほかの形式の命令ファイルとそのサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム命令ファイルを確認する

スタータープロジェクトには、すでに命令ファイルが含まれています。変更を加える前に内容を確認し、命令の影響を調べましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。このファイルの命令は、リポジトリ内で Copilot を使うすべての操作に適用されます。
4. `.github/instructions` フォルダーを開き、各ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` フィールドを確認します。このフィールドは、命令を適用するファイルを指定する glob パターンです。
6. `.github/instructions/drizzle.instructions.md` を開き、他の命令ファイルや既存ファイルへのリンクを確認します。これにより、詳細な命令を再利用しやすい小さなファイルに分けたり、Copilot に例を示したりできます。

## チームのガイダンスに合わせて命令を更新する

既存のファイルはよい出発点ですが、追加したい標準があります。`.github/copilot-instructions.md` を変更して、新しく生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きに、次の内容を追加します。

   ```markdown
   - 新しい TypeScript コードには、ドキュメント用の TSDoc コメントを含めてください。
   ```

変更は自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。フィルター機能のコンテキストを保ったまま新しい命令を適用するため、命令を保存した後に会話を終了して再開します。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを使って、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   命令とコードのガイダンスを更新しました。生成したコードをこのガイダンスに合わせて更新してください。
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを得る方法を確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの命令とパス別の命令ファイルがエージェントに適用される仕組みを確認した。
- コーディング標準を守るため命令ファイルを更新した。
- 命令ファイルがコードに与える影響を確認した。

次は、再利用できる [quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して行います。

## リソース

- [Copilot CLI にカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
